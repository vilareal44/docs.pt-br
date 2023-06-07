---
title: 'Transporte: transações personalizadas através de exemplo de UDP'
ms.date: 03/30/2017
ms.assetid: 6cebf975-41bd-443e-9540-fd2463c3eb23
ms.openlocfilehash: ce1e6f0aedff46aaf58e22d8c23c37b03f8789dd
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596533"
---
# <a name="transport-custom-transactions-over-udp-sample"></a>Transporte: transações personalizadas através de exemplo de UDP
Este exemplo baseia-se no exemplo de [transporte: UDP](transport-udp.md) na[extensibilidade de transporte](transport-extensibility.md)do Windows Communication Foundation (WCF). Ele estende o exemplo de transporte UDP para dar suporte ao fluxo de transações personalizadas e demonstra o uso da <xref:System.ServiceModel.Channels.TransactionMessageProperty> propriedade.  
  
## <a name="code-changes-in-the-udp-transport-sample"></a>Alterações de código no exemplo de transporte UDP  
 Para demonstrar o fluxo de transações, o exemplo altera o contrato de serviço para `ICalculatorContract` para exigir um escopo de transação para `CalculatorService.Add()` . O exemplo também adiciona um `System.Guid` parâmetro extra ao contrato da `Add` operação. Esse parâmetro é usado para passar o identificador da transação do cliente para o serviço.  
  
```csharp  
class CalculatorService : IDatagramContract, ICalculatorContract  
{  
    [OperationBehavior(TransactionScopeRequired=true)]  
    public int Add(int x, int y, Guid clientTransactionId)  
    {  
        if(Transaction.Current.TransactionInformation.DistributedIdentifier == clientTransactionId)  
    {  
        Console.WriteLine("The client transaction has flowed to the service");  
    }  
    else  
    {  
     Console.WriteLine("The client transaction has NOT flowed to the service");  
    }  
  
    Console.WriteLine("   adding {0} + {1}", x, y);
    return (x + y);  
    }  
  
    [...]  
}  
```  
  
 O exemplo [Transport: UDP](transport-udp.md) usa pacotes UDP para passar mensagens entre um cliente e um serviço. O [exemplo transporte: transporte personalizado](transport-custom-transactions-over-udp-sample.md) usa o mesmo mecanismo para transportar mensagens, mas quando uma transação flui, ela é inserida no pacote UDP junto com a mensagem codificada.  
  
```csharp  
byte[] txmsgBuffer = TransactionMessageBuffer.WriteTransactionMessageBuffer(txPropToken, messageBuffer);  
  
int bytesSent = this.socket.SendTo(txmsgBuffer, 0, txmsgBuffer.Length, SocketFlags.None, this.remoteEndPoint);  
```  
  
 `TransactionMessageBuffer.WriteTransactionMessageBuffer`é um método auxiliar que contém nova funcionalidade para mesclar o token de propagação para a transação atual com a entidade de mensagem e colocá-lo em um buffer.  
  
 Para o transporte de fluxo de transações personalizado, a implementação do cliente deve saber quais operações de serviço exigem o fluxo de transações e passar essas informações para o WCF. Também deve haver um mecanismo para transmitir a transação do usuário para a camada de transporte. Este exemplo usa "inspetores de mensagem do WCF" para obter essas informações. O Inspetor de mensagem do cliente implementado aqui, que é chamado `TransactionFlowInspector` , executa as seguintes tarefas:  
  
- Determina se uma transação deve ser fluída para uma determinada ação de mensagem (isso ocorre em `IsTxFlowRequiredForThisOperation()` ).  
  
- Anexa a transação de ambiente atual à mensagem usando `TransactionFlowProperty` , se uma transação for necessária para fluir (isso é feito em `BeforeSendRequest()` ).  
  
```csharp  
public class TransactionFlowInspector : IClientMessageInspector  
{  
   void IClientMessageInspector.AfterReceiveReply(ref           System.ServiceModel.Channels.Message reply, object correlationState)  
  {  
  }  
  
   public object BeforeSendRequest(ref System.ServiceModel.Channels.Message request, System.ServiceModel.IClientChannel channel)  
   {  
       // obtain the tx propagation token  
       byte[] propToken = null;
       if (Transaction.Current != null && IsTxFlowRequiredForThisOperation(request.Headers.Action))  
       {  
           try  
           {  
               propToken = TransactionInterop.GetTransmitterPropagationToken(Transaction.Current);  
           }  
           catch (TransactionException e)  
           {  
              throw new CommunicationException("TransactionInterop.GetTransmitterPropagationToken failed.", e);  
           }  
       }  
  
      // set the propToken on the message in a TransactionFlowProperty  
       TransactionFlowProperty.Set(propToken, request);  
  
       return null;
    }  
  }  
  
  static bool IsTxFlowRequiredForThisOperation(String action)  
 {  
       // In general, this should contain logic to identify which operations (actions)      require transaction flow.  
      [...]  
 }  
}  
```  
  
 O `TransactionFlowInspector` próprio é passado para a estrutura usando um comportamento personalizado: o `TransactionFlowBehavior` .  
  
```csharp  
public class TransactionFlowBehavior : IEndpointBehavior  
{  
       public void AddBindingParameters(ServiceEndpoint endpoint,            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)  
      {  
      }  
  
       public void ApplyClientBehavior(ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.ClientRuntime clientRuntime)  
      {  
            TransactionFlowInspector inspector = new TransactionFlowInspector();  
            clientRuntime.MessageInspectors.Add(inspector);  
      }  
  
      public void ApplyDispatchBehavior(ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.EndpointDispatcher endpointDispatcher)  
     {  
     }  
  
      public void Validate(ServiceEndpoint endpoint)  
      {  
      }  
}  
```  
  
 Com o mecanismo anterior em vigor, o código do usuário cria um `TransactionScope` antes de chamar a operação de serviço. O Inspetor de mensagem garante que a transação seja passada para o transporte, caso seja necessário fluir para a operação de serviço.  
  
```csharp  
CalculatorContractClient calculatorClient = new CalculatorContractClient("SampleProfileUdpBinding_ICalculatorContract");  
calculatorClient.Endpoint.Behaviors.Add(new TransactionFlowBehavior());
  
try  
{  
       for (int i = 0; i < 5; ++i)  
      {  
              // call the 'Add' service operation under a transaction scope  
             using (TransactionScope ts = new TransactionScope())  
             {  
        [...]  
        Console.WriteLine(calculatorClient.Add(i, i * 2));  
         }  
      }
       calculatorClient.Close();  
}  
catch (TimeoutException)  
{  
    calculatorClient.Abort();  
}  
catch (CommunicationException)  
{  
    calculatorClient.Abort();  
}  
catch (Exception)  
{  
    calculatorClient.Abort();  
    throw;  
}  
```  
  
 Após receber um pacote UDP do cliente, o serviço o desserializa para extrair a mensagem e possivelmente uma transação.  
  
```csharp  
count = listenSocket.EndReceiveFrom(result, ref dummy);  
  
// read the transaction and message                       TransactionMessageBuffer.ReadTransactionMessageBuffer(buffer, count, out transaction, out msg);  
```  
  
 `TransactionMessageBuffer.ReadTransactionMessageBuffer()`é o método auxiliar que reverte o processo de serialização executado pelo `TransactionMessageBuffer.WriteTransactionMessageBuffer()` .  
  
 Se uma transação tiver sido transformada em fluxo, ela será anexada à mensagem no `TransactionMessageProperty` .  
  
```csharp  
message = MessageEncoderFactory.Encoder.ReadMessage(msg, bufferManager);  
  
if (transaction != null)  
{  
       TransactionMessageProperty.Set(transaction, message);  
}  
```  
  
 Isso garante que o Dispatcher pegue a transação no momento da expedição e a use ao chamar a operação de serviço endereçada pela mensagem.  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
2. O exemplo atual deve ser executado de forma semelhante à amostra [Transport: UDP](transport-udp.md) . Para executá-lo, inicie o serviço com UdpTestService. exe. Se você estiver executando o Windows Vista, deverá iniciar o serviço com privilégios elevados. Para fazer isso, clique com o botão direito do mouse em UdpTestService. exe no explorador de arquivos e clique em **Executar como administrador**.  
  
3. Isso produz a saída a seguir.  
  
    ```console  
    Testing Udp From Code.  
    Service is started from code...  
    Press <ENTER> to terminate the service and start service from config...  
    ```  
  
4. Neste momento, você pode iniciar o cliente executando UdpTestClient. exe. A saída produzida pelo cliente é a seguinte.  
  
    ```console
    0  
    3  
    6  
    9  
    12  
    Press <ENTER> to complete test.  
    ```  
  
5. A saída do serviço é a seguinte:  
  
    ```console
    Hello, world!  
    Hello, world!  
    Hello, world!  
    Hello, world!  
    Hello, world!  
    The client transaction has flowed to the service  
       adding 0 + 0  
    The client transaction has flowed to the service  
       adding 1 + 2  
    The client transaction has flowed to the service  
       adding 2 + 4  
    The client transaction has flowed to the service  
       adding 3 + 6  
    The client transaction has flowed to the service  
       adding 4 + 8  
    ```  
  
6. O aplicativo de serviço exibirá a mensagem `The client transaction has flowed to the service` se ele puder corresponder ao identificador de transação enviado pelo cliente, no `clientTransactionId` parâmetro da `CalculatorService.Add()` operação, para o identificador da transação de serviço. Uma correspondência será obtida somente se a transação do cliente tiver fluído para o serviço.  
  
7. Para executar o aplicativo cliente em pontos de extremidade publicados usando a configuração, pressione ENTER na janela do aplicativo de serviço e execute o cliente de teste novamente. Você deverá ver a seguinte saída no serviço.  
  
    ```console  
    Testing Udp From Config.  
    Service is started from config...  
    Press <ENTER> to terminate the service and exit...  
    ```  
  
8. Executar o cliente em relação ao serviço agora produz uma saída semelhante como antes.  
  
9. Para regenerar o código do cliente e a configuração usando svcutil. exe, inicie o aplicativo de serviço e execute o comando svcutil. exe a seguir do diretório raiz do exemplo.  
  
    ```console  
    svcutil http://localhost:8000/udpsample/ /reference:UdpTransport\bin\UdpTransport.dll /svcutilConfig:svcutil.exe.config  
    ```  
  
10. Observe que svcutil. exe não gera a configuração de extensão de associação para o `sampleProfileUdpBinding` ; você deve adicioná-lo manualmente.  
  
    ```xml  
    <configuration>  
        <system.serviceModel>
            …  
            <extensions>  
                <!-- This was added manually because svcutil.exe does not add this extension to the file -->  
                <bindingExtensions>  
                    <add name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport" />  
                </bindingExtensions>  
            </extensions>  
        </system.serviceModel>  
    </configuration>  
    ```  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Transactions\TransactionMessagePropertyUDPTransport`  
  
## <a name="see-also"></a>Consulte também

- [Transporte: UDP](transport-udp.md)
