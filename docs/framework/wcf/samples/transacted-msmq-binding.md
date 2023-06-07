---
title: Associação transacionada do MSMQ
ms.date: 03/30/2017
ms.assetid: 71f5cb8d-f1df-4e1e-b8a2-98e734a75c37
ms.openlocfilehash: fa53099caba144f321698f180fe18f7614a1fa64
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596559"
---
# <a name="transacted-msmq-binding"></a>Associação transacionada do MSMQ

Este exemplo demonstra como executar a comunicação em fila transacionada usando o MSMQ (enfileiramento de mensagens).

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

Na comunicação em fila, o cliente se comunica com o serviço usando uma fila. Mais precisamente, o cliente envia mensagens para uma fila. O serviço recebe mensagens da fila. O serviço e o cliente, portanto, não precisam estar em execução ao mesmo tempo para se comunicarem usando uma fila.

Quando as transações são usadas para enviar e receber mensagens, na verdade, há duas transações separadas. Quando o cliente envia mensagens dentro do escopo de uma transação, a transação é local para o cliente e o Gerenciador de fila do cliente. Quando o serviço recebe mensagens dentro do escopo da transação, a transação é local para o serviço e o Gerenciador de filas de recebimento. É muito importante lembrar que o cliente e o serviço não estão participando da mesma transação; em vez disso, eles estão usando transações diferentes ao executar suas operações (como enviar e receber) com a fila.

Neste exemplo, o cliente envia um lote de mensagens para o serviço de dentro do escopo de uma transação. As mensagens enviadas para a fila são então recebidas pelo serviço dentro do escopo da transação definido pelo serviço.

O contrato de serviço é `IOrderProcessor` , conforme mostrado no código de exemplo a seguir. A interface define um serviço unidirecional que é adequado para uso com filas do.

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true)]
    void SubmitPurchaseOrder(PurchaseOrder po);
}
```

O comportamento do serviço define um comportamento de operação com `TransactionScopeRequired` definido como `true` . Isso garante que o mesmo escopo de transação usado para recuperar a mensagem da fila seja usado por qualquer Gerenciador de recursos acessado pelo método. Também garante que, se o método lançar uma exceção, a mensagem será retornada para a fila. Sem definir esse comportamento de operação, um canal enfileirado cria uma transação para ler a mensagem da fila e a confirma automaticamente antes da expedição, de modo que, se a operação falhar, a mensagem seja perdida. O cenário mais comum é que as operações de serviço se inscrevam na transação que é usada para ler a mensagem da fila, conforme demonstrado no código a seguir.

```csharp
 // This service class that implements the service contract.
 // This added code writes output to the console window.
 public class OrderProcessorService : IOrderProcessor
 {
     [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
     public void SubmitPurchaseOrder(PurchaseOrder po)
     {
         Orders.Add(po);
         Console.WriteLine("Processing {0} ", po);
     }
  …
}
```

O serviço é hospedado internamente. Ao usar o transporte MSMQ, a fila usada deve ser criada com antecedência. Isso pode ser feito manualmente ou por meio de código. Neste exemplo, o serviço contém o código para verificar a existência da fila e criar a fila, caso ela não exista. O nome da fila é lido no arquivo de configuração. O endereço base é usado pela [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para gerar o proxy para o serviço.

```csharp
// Host the service within this EXE console application.
public static void Main()
{
    // Get the MSMQ queue name from appSettings in configuration.
    string queueName = ConfigurationManager.AppSettings["queueName"];

    // Create the transacted MSMQ queue if necessary.
    if (!MessageQueue.Exists(queueName))
        MessageQueue.Create(queueName, true);

    // Create a ServiceHost for the OrderProcessorService type.
    using (ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService)))
    {
        // Open the ServiceHost to create listeners and start listening for messages.
        serviceHost.Open();

        // The service can now be accessed.
        Console.WriteLine("The service is ready.");
        Console.WriteLine("Press <ENTER> to terminate service.");
        Console.WriteLine();
        Console.ReadLine();

        // Close the ServiceHost to shut down the service.
        serviceHost.Close();
    }
}
```

O nome da fila MSMQ é especificado em uma seção appSettings do arquivo de configuração, conforme mostrado na seguinte configuração de exemplo.

```xml
<appSettings>
    <add key="queueName" value=".\private$\ServiceModelSamplesTransacted" />
</appSettings>
```

> [!NOTE]
> O nome da fila usa um ponto (.) para o computador local e separadores de barra invertida em seu caminho ao criar a fila usando <xref:System.Messaging> . O ponto de extremidade do Windows Communication Foundation (WCF) usa o endereço de fila com o esquema net. MSMQ, usa "localhost" para denotar o computador local e usa barras invertidas em seu caminho.

O cliente cria um escopo de transação. A comunicação com a fila ocorre dentro do escopo da transação, fazendo com que ela seja tratada como uma unidade atômica, onde todas as mensagens são enviadas para a fila ou nenhuma das mensagens é enviada para a fila. A transação é confirmada chamando <xref:System.Transactions.TransactionScope.Complete%2A> o escopo da transação.

```csharp
// Create a client.
OrderProcessorClient client = new OrderProcessorClient();

// Create the purchase order.
PurchaseOrder po = new PurchaseOrder();
po.CustomerId = "somecustomer.com";
po.PONumber = Guid.NewGuid().ToString();

PurchaseOrderLineItem lineItem1 = new PurchaseOrderLineItem();
lineItem1.ProductId = "Blue Widget";
lineItem1.Quantity = 54;
lineItem1.UnitCost = 29.99F;

PurchaseOrderLineItem lineItem2 = new PurchaseOrderLineItem();
lineItem2.ProductId = "Red Widget";
lineItem2.Quantity = 890;
lineItem2.UnitCost = 45.89F;

po.orderLineItems = new PurchaseOrderLineItem[2];
po.orderLineItems[0] = lineItem1;
po.orderLineItems[1] = lineItem2;

// Create a transaction scope.
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
{
    // Make a queued call to submit the purchase order.
    client.SubmitPurchaseOrder(po);
    // Complete the transaction.
    scope.Complete();
}

// Closing the client gracefully closes the connection and cleans up resources.
client.Close();

Console.WriteLine();
Console.WriteLine("Press <ENTER> to terminate client.");
Console.ReadLine();
```

Para verificar se as transações estão funcionando, modifique o cliente comentando o escopo da transação, conforme mostrado no código de exemplo a seguir, recompile a solução e execute o cliente.

```csharp
//scope.Complete();
```

Como a transação não foi concluída, as mensagens não são enviadas para a fila.

Quando você executa o exemplo, as atividades de cliente e serviço são exibidas nas janelas do console do cliente e do serviço. Você pode ver o serviço receber mensagens do cliente. Pressione ENTER em cada janela do console para desligar o serviço e o cliente. Observe que, como o enfileiramento está em uso, o cliente e o serviço não precisam estar em funcionamento ao mesmo tempo. Você pode executar o cliente, desligá-lo e, em seguida, iniciar o serviço e ele ainda receberá as mensagens.

```console
The service is ready.
Press <ENTER> to terminate service.

Processing Purchase Order: 7b31ce51-ae7c-4def-9b8b-617e4288eafd
        Customer: somecustomer.com
        OrderDetails
                Order LineItem: 54 of Blue Widget @unit price: $29.99
                Order LineItem: 890 of Red Widget @unit price: $45.89
        Total cost of this order: $42461.56
        Order status: Pending
```

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Se o serviço for executado primeiro, ele verificará se a fila está presente. Se a fila não estiver presente, o serviço criará uma. Você pode executar o serviço primeiro para criar a fila ou pode criar um por meio do Gerenciador de filas MSMQ. Siga estas etapas para criar uma fila no Windows 2008.

    1. Abra Gerenciador do Servidor no Visual Studio 2012.

    2. Expanda a guia **recursos** .

    3. Clique com o botão direito do mouse em **filas de mensagens particulares**e selecione **nova** **fila privada**.

    4. Marque a caixa **transacional** .

    5. Insira `ServiceModelSamplesTransacted` como o nome da nova fila.

3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

Por padrão, com a <xref:System.ServiceModel.NetMsmqBinding> segurança de transporte está habilitada. Há duas propriedades relevantes para a segurança de transporte do MSMQ <xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A> e <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> . Por padrão, o modo de autenticação é definido como `Windows` e o nível de proteção é definido como `Sign` . Para que o MSMQ forneça o recurso de autenticação e assinatura, ele deve fazer parte de um domínio e a opção de integração de Active Directory para o MSMQ deve ser instalada. Se você executar esse exemplo em um computador que não atenda a esses critérios, você receberá um erro.

### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup-or-without-active-directory-integration"></a>Para executar o exemplo em um computador ingressado em um grupo de trabalho ou sem integração de Active Directory

1. Se o computador não fizer parte de um domínio ou não tiver a integração de Active Directory instalada, desative a segurança de transporte definindo o modo de autenticação e o nível de proteção como `None` mostrado no código de configuração de exemplo a seguir.

    ```xml
    <system.serviceModel>
      <services>
        <service name="Microsoft.ServiceModel.Samples.OrderProcessorService"
                 behaviorConfiguration="OrderProcessorServiceBehavior">
          <host>
            <baseAddresses>
              <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>
            </baseAddresses>
          </host>
          <!-- Define NetMsmqEndpoint. -->
          <endpoint
              address="net.msmq://localhost/private/ServiceModelSamplesTransacted"
              binding="netMsmqBinding"
              bindingConfiguration="Binding1"
           contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
          <!-- The mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex. -->
          <endpoint address="mex"
                    binding="mexHttpBinding"
                    contract="IMetadataExchange" />
        </service>
      </services>

      <bindings>
        <netMsmqBinding>
          <binding name="Binding1">
            <security mode="None" />
          </binding>
        </netMsmqBinding>
      </bindings>

        <behaviors>
          <serviceBehaviors>
            <behavior name="OrderProcessorServiceBehavior">
              <serviceMetadata httpGetEnabled="True"/>
            </behavior>
          </serviceBehaviors>
        </behaviors>

      </system.serviceModel>
    ```

2. Certifique-se de alterar a configuração no servidor e no cliente antes de executar o exemplo.

    > [!NOTE]
    > A configuração `security mode` como `None` é equivalente à configuração <xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A> , <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> e `Message` à segurança para `None` .

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\Transacted`
