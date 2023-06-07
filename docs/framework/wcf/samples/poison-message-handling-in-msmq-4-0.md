---
title: Tratamento de mensagens suspeitas no MSMQ 4.0
ms.date: 03/30/2017
ms.assetid: ec8d59e3-9937-4391-bb8c-fdaaf2cbb73e
ms.openlocfilehash: 54e69d60aabb3793ef4a8d800dd0e6238c28f231
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602434"
---
# <a name="poison-message-handling-in-msmq-40"></a>Tratamento de mensagens suspeitas no MSMQ 4.0
Este exemplo demonstra como executar a manipulação de mensagens suspeitas em um serviço. Este exemplo é baseado no exemplo de [associação MSMQ transacionado](transacted-msmq-binding.md) . Este exemplo usa `netMsmqBinding`. O serviço é um aplicativo de console auto-hospedado para permitir que você observe o serviço que recebe mensagens enfileiradas.

 Na comunicação em fila, o cliente se comunica com o serviço usando uma fila. Mais precisamente, o cliente envia mensagens para uma fila. O serviço recebe mensagens da fila. O serviço e o cliente, portanto, não precisam estar em execução ao mesmo tempo para se comunicarem usando uma fila.

 Uma mensagem suspeita é uma mensagem que é lida repetidamente de uma fila quando o serviço que lê a mensagem não pode processar a mensagem e, portanto, encerra a transação sob a qual a mensagem é lida. Nesses casos, a mensagem é repetida novamente. Teoricamente, isso pode ser usado para sempre se houver um problema com a mensagem. Isso só pode ocorrer quando você usa transações para ler da fila e invocar a operação de serviço.

 Com base na versão do MSMQ, o NetMsmqBinding dá suporte à detecção limitada para detecção completa de mensagens suspeitas. Depois que a mensagem for detectada como inviabilizada, ela poderá ser tratada de várias maneiras. Novamente, com base na versão do MSMQ, o NetMsmqBinding dá suporte ao tratamento limitado para o tratamento total de mensagens suspeitas.

 Este exemplo ilustra as instalações suspeitas limitadas fornecidas no Windows Server 2003 e na plataforma Windows XP e as instalações completas suspeitas fornecidas no Windows Vista. Em ambos os exemplos, o objetivo é mover a mensagem suspeita para fora da fila para outra fila. Essa fila pode então ser atendida por um serviço de mensagens suspeitas.

## <a name="msmq-v40-poison-handling-sample"></a>Exemplo de tratamento inviabilizado do MSMQ v 4.0
 No Windows Vista, o MSMQ fornece um recurso de subfila de suspeita que pode ser usado para armazenar mensagens suspeitas. Este exemplo demonstra a prática recomendada de lidar com mensagens suspeitas usando o Windows Vista.

 A detecção de mensagens suspeitas no Windows Vista é sofisticada. Há três propriedades que ajudam na detecção. O <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A> é o número de vezes que uma determinada mensagem é relida da fila e despachada para o aplicativo para processamento. Uma mensagem é relida da fila quando é colocada de volta na fila porque a mensagem não pode ser expedida para o aplicativo ou o aplicativo reverte a transação na operação de serviço. <xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A>é o número de vezes que a mensagem é movida para a fila de repetição. Quando <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A> for atingido, a mensagem será movida para a fila de repetição. A propriedade <xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A> é o intervalo de tempo após o qual a mensagem é movida da fila de repetição de volta para a fila principal. O <xref:System.ServiceModel.MsmqBindingBase.ReceiveRetryCount%2A> é redefinido como 0. A mensagem é tentada novamente. Se todas as tentativas de ler a mensagem tiverem falhado, a mensagem será marcada como inviabilizada.

 Depois que a mensagem é marcada como inviabilizada, a mensagem é tratada de acordo com as configurações na <xref:System.ServiceModel.MsmqBindingBase.ReceiveErrorHandling%2A> enumeração. Para reiterar os valores possíveis:

- Falha (padrão): para falha no ouvinte e também no host de serviço.

- Drop: para descartar a mensagem.

- Mover: para mover a mensagem para a subfila de mensagens suspeitas. Esse valor está disponível apenas no Windows Vista.

- Rejeitar: para rejeitar a mensagem, enviar a mensagem de volta para a fila de mensagens mortas do remetente. Esse valor está disponível apenas no Windows Vista.

 O exemplo demonstra o uso da `Move` disposição para a mensagem suspeita. `Move`faz com que a mensagem seja movida para a subfila de suspeita.

 O contrato de serviço é `IOrderProcessor` , que define um serviço unidirecional que é adequado para uso com filas.

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true)]
    void SubmitPurchaseOrder(PurchaseOrder po);
}
```

 A operação de serviço exibe uma mensagem informando que está processando a ordem. Para demonstrar a funcionalidade de mensagem suspeita, a `SubmitPurchaseOrder` operação de serviço gera uma exceção para reverter a transação em uma invocação aleatória do serviço. Isso faz com que a mensagem seja colocada de volta na fila. Eventualmente, a mensagem é marcada como suspeita. A configuração é definida para mover a mensagem suspeita para a subfila de suspeita.

```csharp
// Service class that implements the service contract.
// Added code to write output to the console window.
public class OrderProcessorService : IOrderProcessor
{
    static Random r = new Random(137);

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po)
    {

        int randomNumber = r.Next(10);

        if (randomNumber % 2 == 0)
        {
            Orders.Add(po);
            Console.WriteLine("Processing {0} ", po);
        }
        else
        {
            Console.WriteLine("Aborting transaction, cannot process purchase order: " + po.PONumber);
            Console.WriteLine();
            throw new Exception("Cannot process purchase order: " + po.PONumber);
        }
    }

    public static void OnServiceFaulted(object sender, EventArgs e)
    {
        Console.WriteLine("Service Faulted");
    }

    // Host the service within this EXE console application.
    public static void Main()
    {
        // Get MSMQ queue name from app settings in configuration.
        string queueName = ConfigurationManager.AppSettings["queueName"];

        // Create the transacted MSMQ queue if necessary.
        if (!System.Messaging.MessageQueue.Exists(queueName))
            System.Messaging.MessageQueue.Create(queueName, true);

        // Get the base address that is used to listen for WS-MetaDataExchange requests.
        // This is useful to generate a proxy for the client.
        string baseAddress = ConfigurationManager.AppSettings["baseAddress"];

        // Create a ServiceHost for the OrderProcessorService type.
        ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService), new Uri(baseAddress));

        // Hook on to the service host faulted events.
        serviceHost.Faulted += new EventHandler(OnServiceFaulted);

        // Open the ServiceHostBase to create listeners and start listening for messages.
        serviceHost.Open();

        // The service can now be accessed.
        Console.WriteLine("The service is ready.");
        Console.WriteLine("Press <ENTER> to terminate service.");
        Console.WriteLine();
        Console.ReadLine();

        if(serviceHost.State != CommunicationState.Faulted) {
            serviceHost.Close();
        }

    }
}
```

 A configuração de serviço inclui as seguintes propriedades de mensagem suspeita: `receiveRetryCount` , `maxRetryCycles` , `retryCycleDelay` e `receiveErrorHandling` conforme mostrado no arquivo de configuração a seguir.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <!-- Use appSetting to configure MSMQ queue name. -->
    <add key="queueName" value=".\private$\ServiceModelSamplesPoison" />
    <add key="baseAddress" value="http://localhost:8000/orderProcessor/poisonSample"/>
  </appSettings>
  <system.serviceModel>
    <services>
      <service
              name="Microsoft.ServiceModel.Samples.OrderProcessorService">
        <!-- Define NetMsmqEndpoint -->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesPoison"
                  binding="netMsmqBinding"
                  bindingConfiguration="PoisonBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
      </service>
    </services>

    <bindings>
      <netMsmqBinding>
        <binding name="PoisonBinding"
                 receiveRetryCount="0"
                 maxRetryCycles="1"
                 retryCycleDelay="00:00:05"
                 receiveErrorHandling="Move">
        </binding>
      </netMsmqBinding>
    </bindings>
  </system.serviceModel>
</configuration>
```

## <a name="processing-messages-from-the-poison-message-queue"></a>Processando mensagens da fila de mensagens suspeitas
 O serviço de mensagens suspeitas lê mensagens da fila final de mensagens suspeitas e as processa.

 As mensagens na fila de mensagens suspeitas são mensagens endereçadas ao serviço que está processando a mensagem, que pode ser diferente do ponto de extremidade do serviço de mensagens suspeitas. Portanto, quando o serviço de mensagens suspeitas lê mensagens da fila, a camada de canal do WCF localiza a incompatibilidade em pontos de extremidade e não despacha a mensagem. Nesse caso, a mensagem é endereçada ao serviço de processamento de pedidos, mas está sendo recebida pelo serviço de mensagens suspeitas. Para continuar a receber a mensagem mesmo que a mensagem seja endereçada a um ponto de extremidade diferente, devemos adicionar um `ServiceBehavior` para filtrar endereços em que o critério de correspondência é corresponder a qualquer ponto de extremidade de serviço ao qual a mensagem é endereçada. Isso é necessário para processar com êxito as mensagens que você leu da fila de mensagens suspeitas.

 A própria implementação do serviço de mensagens suspeitas é muito semelhante à implementação do serviço. Ele implementa o contrato e processa os pedidos. O exemplo de código é o seguinte.

```csharp
// Service class that implements the service contract.
// Added code to write output to the console window.
[ServiceBehavior(AddressFilterMode=AddressFilterMode.Any)]
public class OrderProcessorService : IOrderProcessor
{
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po)
    {
        Orders.Add(po);
        Console.WriteLine("Processing {0} ", po);
    }

    public static void OnServiceFaulted(object sender, EventArgs e)
    {
        Console.WriteLine("Service Faulted...exiting app");
        Environment.Exit(1);
    }

    // Host the service within this EXE console application.
    public static void Main()
    {

        // Create a ServiceHost for the OrderProcessorService type.
        ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService));

        // Hook on to the service host faulted events.
        serviceHost.Faulted += new EventHandler(OnServiceFaulted);

        serviceHost.Open();

        // The service can now be accessed.
        Console.WriteLine("The poison message service is ready.");
        Console.WriteLine("Press <ENTER> to terminate service.");
        Console.WriteLine();
        Console.ReadLine();

        // Close the ServiceHostBase to shutdown the service.
        if(serviceHost.State != CommunicationState.Faulted)
        {
            serviceHost.Close();
        }
    }
```

 Ao contrário do serviço de processamento de pedidos que lê mensagens da fila de pedidos, o serviço de mensagens suspeitas lê mensagens da subfila suspeita. A fila de suspeitas é uma subfila da fila principal, denominada "suspeita" e é gerada automaticamente pelo MSMQ. Para acessá-lo, forneça o nome da fila principal seguido por um ";" e o nome da subfila, neste caso, "suspeita", conforme mostrado na seguinte configuração de exemplo.

> [!NOTE]
> No exemplo do MSMQ v 3.0, o nome da fila suspeita não é uma subfila, e sim a fila para a qual movemos a mensagem.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.OrderProcessorService">
        <!-- Define NetMsmqEndpoint -->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesPoison;poison"
                  binding="netMsmqBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" >
        </endpoint>
      </service>
    </services>

  </system.serviceModel>
</configuration>
```

 Quando você executa o exemplo, as atividades cliente, serviço e serviço de mensagens suspeitas são exibidas em janelas de console. Você pode ver o serviço receber mensagens do cliente. Pressione ENTER em cada janela do console para desligar os serviços.

 O serviço inicia a execução, processando pedidos e, aleatoriamente, começa a encerrar o processamento. Se a mensagem indicar que ele processou a ordem, você poderá executar o cliente novamente para enviar outra mensagem até ver que o serviço, na verdade, terminou uma mensagem. Com base nas configurações suspeitas configuradas, a mensagem é tentada uma vez para processamento antes de movê-la para a fila de suspeitas final.

```console
The service is ready.
Press <ENTER> to terminate service.

Processing Purchase Order: 0f063b71-93e0-42a1-aa3b-bca6c7a89546
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending

Processing Purchase Order: 5ef9a4fa-5a30-4175-b455-2fb1396095fa
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending

Aborting transaction, cannot process purchase order: 23e0b991-fbf9-4438-a0e2-20adf93a4f89
```

 Inicie o serviço de mensagens suspeitas para ler a mensagem inviabilizada da fila de suspeitas. Neste exemplo, o serviço de mensagens suspeitas lê a mensagem e a processa. Você pode ver que a ordem de compra terminada e inviabilizada é lida pelo serviço de mensagens suspeitas.

```console
The service is ready.
Press <ENTER> to terminate service.

Processing Purchase Order: 23e0b991-fbf9-4438-a0e2-20adf93a4f89
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending
```

#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Se o serviço for executado primeiro, ele verificará se a fila está presente. Se a fila não estiver presente, o serviço criará uma. Você pode executar o serviço primeiro para criar a fila ou pode criar um por meio do Gerenciador de filas MSMQ. Siga estas etapas para criar uma fila no Windows 2008.

    1. Abra Gerenciador do Servidor no Visual Studio 2012.

    2. Expanda a guia **recursos** .

    3. Clique com o botão direito do mouse em **filas de mensagens particulares**e selecione **nova** **fila privada**.

    4. Marque a caixa **transacional** .

    5. Insira `ServiceModelSamplesTransacted` como o nome da nova fila.

3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Para executar o exemplo em uma configuração de computador único ou entre computadores, altere os nomes de fila para refletir o nome de host real em vez de localhost e siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

 Por padrão, com o `netMsmqBinding` transporte de associação, a segurança é habilitada. Duas propriedades `MsmqAuthenticationMode` e `MsmqProtectionLevel` , juntas, determinam o tipo de segurança de transporte. Por padrão, o modo de autenticação é definido como `Windows` e o nível de proteção é definido como `Sign` . Para que o MSMQ forneça o recurso de autenticação e assinatura, ele deve fazer parte de um domínio. Se você executar esse exemplo em um computador que não faz parte de um domínio, receberá o seguinte erro: "certificado interno do serviço de enfileiramento de mensagens não existe".

#### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup"></a>Para executar o exemplo em um computador ingressado em um grupo de trabalho

1. Se o computador não fizer parte de um domínio, desative a segurança de transporte definindo o modo de autenticação e o nível de proteção como `None` mostrado na seguinte configuração de exemplo:

    ```xml
    <bindings>
        <netMsmqBinding>
            <binding name="TransactedBinding">
                <security mode="None"/>
            </binding>
        </netMsmqBinding>
    </bindings>
    ```

     Verifique se o ponto de extremidade está associado à associação definindo o atributo bindingConfiguration do ponto de extremidade.

2. Certifique-se de alterar a configuração no PoisonMessageServer, no servidor e no cliente antes de executar o exemplo.

    > [!NOTE]
    > A configuração `security mode` como `None` é equivalente à configuração `MsmqAuthenticationMode` , `MsmqProtectionLevel` e `Message` à segurança para `None` .  
  
3. Para que o Meta Data Exchange funcione, registramos uma URL com associação http. Isso requer que o serviço seja executado em uma janela de comandos com privilégios elevados. Caso contrário, você obterá uma exceção, como: `Unhandled Exception: System.ServiceModel.AddressAccessDeniedException: HTTP could not register URL http://+:8000/ServiceModelSamples/service/. Your process does not have access rights to this namespace (see https://go.microsoft.com/fwlink/?LinkId=70353 for details). ---> System.Net.HttpListenerException: Access is denied` .  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\Poison\MSMQ4`
