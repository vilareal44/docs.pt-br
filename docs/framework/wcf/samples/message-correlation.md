---
title: Correlação de mensagem
ms.date: 03/30/2017
ms.assetid: 3f62babd-c991-421f-bcd8-391655c82a1f
ms.openlocfilehash: 84b10b507f9fdaa7c53cf937bb132c8cc0aac33f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591626"
---
# <a name="message-correlation"></a>Correlação de mensagem

Este exemplo demonstra como um aplicativo MSMQ (enfileiramento de mensagens) pode enviar uma mensagem MSMQ para um serviço Windows Communication Foundation (WCF) e como as mensagens podem ser correlacionadas entre os aplicativos do remetente e do destinatário em um cenário de solicitação/resposta. Este exemplo usa a associação msmqIntegrationBinding. O serviço, nesse caso, é um aplicativo de console auto-hospedado para permitir que você observe o serviço que recebe mensagens enfileiradas. k

 O serviço processa a mensagem recebida do remetente e envia uma mensagem de resposta de volta para o remetente. O remetente correlaciona a resposta recebida para a solicitação que ele enviou originalmente. As `MessageID` `CorrelationID` Propriedades e da mensagem são usadas para correlacionar as mensagens de solicitação e resposta.

 O `IOrderProcessor` contrato de serviço define uma operação de serviço unidirecional que é adequada para uso com o enfileiramento. Uma mensagem MSMQ não tem um cabeçalho Action, portanto, não é possível mapear diferentes mensagens MSMQ para contratos de operação automaticamente. Portanto, pode haver apenas um contrato de operação nesse caso. Se você quiser definir mais contratos de operação no serviço, o aplicativo deverá fornecer informações sobre qual cabeçalho na mensagem MSMQ (por exemplo, o rótulo ou CorrelationId) pode ser usado para decidir qual contrato de operação deve ser despachado.

 A mensagem MSMQ também não contém informações sobre quais cabeçalhos são mapeados para os diferentes parâmetros do contrato de operação. Portanto, pode haver apenas um parâmetro no contrato de operação. O parâmetro é do tipo <xref:System.ServiceModel.MsmqIntegration.MsmqMessage%601> , que contém a mensagem subjacente do MSMQ. O tipo "T" na `MsmqMessage<T>` classe representa os dados que são serializados no corpo da mensagem MSMQ. Neste exemplo, o `PurchaseOrder` tipo é serializado no corpo da mensagem do MSMQ.

```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]
[ServiceKnownType(typeof(PurchaseOrder))]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true, Action = "*")]
    void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> msg);
}
```

 A operação de serviço processa a ordem de compra e exibe o conteúdo da ordem de compra e seu status na janela do console de serviço. O <xref:System.ServiceModel.OperationBehaviorAttribute> configura a operação para se inscrever em uma transação com a fila e marcar a transação como concluída quando a operação retorna. O `PurchaseOrder` contém os detalhes do pedido que devem ser processados pelo serviço.

```csharp
// Service class that implements the service contract.
public class OrderProcessorService : IOrderProcessor
{
   [OperationBehavior(TransactionScopeRequired = true,
          TransactionAutoComplete = true)]
   public void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> ordermsg)
   {
       PurchaseOrder po = (PurchaseOrder)ordermsg.Body;
       Random statusIndexer = new Random();
       po.Status = PurchaseOrder.OrderStates[statusIndexer.Next(3)];
       Console.WriteLine("Processing {0} ", po);
       //Send a response to the client that the order has been received
         and is pending fullfillment.
       SendResponse(ordermsg);
    }

    private void SendResponse(MsmqMessage<PurchaseOrder> ordermsg)
    {
        OrderResponseClient client = new OrderResponseClient("OrderResponseEndpoint");

        //Set the correlation ID such that the client can correlate the response to the order.
        MsmqMessage<PurchaseOrder> orderResponsemsg = new MsmqMessage<PurchaseOrder>(ordermsg.Body);
        orderResponsemsg.CorrelationId = ordermsg.Id;
        using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
        {
            client.SendOrderResponse(orderResponsemsg);
            scope.Complete();
        }

        client.Close();
    }
}
```

 O serviço usa um cliente personalizado `OrderResponseClient` para enviar a mensagem MSMQ para a fila. Como o aplicativo que recebe e processa a mensagem é um aplicativo MSMQ e não um aplicativo WCF, não há nenhum contrato de serviço implícito entre os dois aplicativos. Portanto, não podemos criar um proxy usando a ferramenta svcutil. exe neste cenário.

 O proxy personalizado é essencialmente o mesmo para todos os aplicativos WCF que usam a `msmqIntegrationBinding` Associação para enviar mensagens. Ao contrário de outros proxies, ele não inclui uma variedade de operações de serviço. É apenas uma operação de envio de mensagem.

```csharp
[System.ServiceModel.ServiceContractAttribute(Namespace = "http://Microsoft.ServiceModel.Samples")]
public interface IOrderResponse
{

    [System.ServiceModel.OperationContractAttribute(IsOneWay = true, Action = "*")]
    void SendOrderResponse(MsmqMessage<PurchaseOrder> msg);
}

public partial class OrderResponseClient : System.ServiceModel.ClientBase<IOrderResponse>, IOrderResponse
{

    public OrderResponseClient()
    { }

    public OrderResponseClient(string configurationName)
        : base(configurationName)
    { }

    public OrderResponseClient(System.ServiceModel.Channels.Binding binding, System.ServiceModel.EndpointAddress address)
        : base(binding, address)
    { }

    public void SendOrderResponse(MsmqMessage<PurchaseOrder> msg)
    {
        base.Channel.SendOrderResponse(msg);
    }
}
```

 O serviço é hospedado internamente. Ao usar o transporte de integração do MSMQ, a fila usada deve ser criada com antecedência. Isso pode ser feito manualmente ou por meio de código. Neste exemplo, o serviço contém o <xref:System.Messaging> código para verificar a existência da fila e criá-la, se necessário. O nome da fila é lido no arquivo de configuração.

```csharp
public static void Main()
{
       // Get the MSMQ queue name from application settings in configuration.
      string queueName =
                ConfigurationManager.AppSettings["orderQueueName"];
      // Create the transacted MSMQ queue if necessary.
      if (!MessageQueue.Exists(queueName))
                MessageQueue.Create(queueName, true);
     // Create a ServiceHost for the OrderProcessorService type.
     using (ServiceHost serviceHost = new
                   ServiceHost(typeof(OrderProcessorService)))
     {
            serviceHost.Open();
            // The service can now be accessed.
            Console.WriteLine("The service is ready.");
            Console.WriteLine("Press <ENTER> to terminate service.");
            Console.ReadLine();
            // Close the ServiceHost to shutdown the service.
            serviceHost.Close();
      }
}
```

 A fila MSMQ para a qual as solicitações de pedido são enviadas é especificada na seção appSettings do arquivo de configuração. Os pontos de extremidade de cliente e de serviço são definidos na seção System. serviceModel do arquivo de configuração. Especifique a `msmqIntegrationbinding` associação.

```xml
<appSettings>
  <add key="orderQueueName" value=".\private$\Orders" />
</appSettings>

<system.serviceModel>
  <client>
    <endpoint    name="OrderResponseEndpoint"
              address="msmq.formatname:DIRECT=OS:.\private$\OrderResponse"
              binding="msmqIntegrationBinding"
              bindingConfiguration="OrderProcessorBinding"
              contract="Microsoft.ServiceModel.Samples.IOrderResponse">
    </endpoint>
  </client>

  <services>
    <service
      name="Microsoft.ServiceModel.Samples.OrderProcessorService">
      <endpoint address="msmq.formatname:DIRECT=OS:.\private$\Orders"
                            binding="msmqIntegrationBinding"
                bindingConfiguration="OrderProcessorBinding"
                contract="Microsoft.ServiceModel.Samples.IOrderProcessor">
      </endpoint>
    </service>
  </services>

  <bindings>
    <msmqIntegrationBinding>
      <binding name="OrderProcessorBinding" >
        <security mode="None" />
      </binding>
    </msmqIntegrationBinding>
  </bindings>

</system.serviceModel>
```

 O aplicativo cliente usa o <xref:System.Messaging> para enviar uma mensagem durável e transacional para a fila. O corpo da mensagem contém a ordem de compra.

```csharp
static void PlaceOrder()
{
    //Connect to the queue
    MessageQueue orderQueue =
            new MessageQueue(
                    ConfigurationManager.AppSettings["orderQueueName"])
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

    Message msg = new Message();
    msg.UseDeadLetterQueue = true;
    msg.Body = po;

    //Create a transaction scope.
    using (TransactionScope scope = new
     TransactionScope(TransactionScopeOption.Required))
    {
        // Submit the purchase order.
        orderQueue.Send(msg, MessageQueueTransactionType.Automatic);
        // Complete the transaction.
        scope.Complete();
    }
    //Save the messageID for order response correlation.
    orderMessageID = msg.Id;
    Console.WriteLine("Placed the order, waiting for response...");
}
```

 A fila MSMQ da qual as respostas de pedidos são recebidas é especificada em uma seção appSettings do arquivo de configuração, conforme mostrado na seguinte configuração de exemplo.

> [!NOTE]
> O nome da fila usa um ponto (.) para o computador local e separadores de barra invertida em seu caminho. O endereço do ponto de extremidade do WCF especifica um esquema MSMQ. FormatName e usa "localhost" para o computador local. Um nome de formato formado corretamente segue MSMQ. FormatName no URI de acordo com as diretrizes do MSMQ.

```xml
<appSettings>
    <add key=" orderResponseQueueName" value=".\private$\Orders" />
</appSettings>
```

 O aplicativo cliente salva a `messageID` mensagem de solicitação de pedido que ele envia para o serviço e aguarda uma resposta do serviço. Quando uma resposta chega na fila, o cliente a correlaciona com a mensagem de pedido enviada usando a `correlationID` propriedade da mensagem, que contém o `messageID` da mensagem de pedido que o cliente enviou para o serviço originalmente.

```csharp
static void DisplayOrderStatus()
{
    MessageQueue orderResponseQueue = new
     MessageQueue(ConfigurationManager.AppSettings
                  ["orderResponseQueueName"]);
    //Create a transaction scope.
    bool responseReceived = false;
    orderResponseQueue.MessageReadPropertyFilter.CorrelationId = true;
    while (!responseReceived)
    {
       Message responseMsg;
       using (TransactionScope scope2 = new
         TransactionScope(TransactionScopeOption.Required))
       {
          //Receive the Order Response message.
          responseMsg =
              orderResponseQueue.Receive
                   (MessageQueueTransactionType.Automatic);
          scope2.Complete();
     }
     responseMsg.Formatter = new
     System.Messaging.XmlMessageFormatter(new Type[] {
         typeof(PurchaseOrder) });
     PurchaseOrder responsepo = (PurchaseOrder)responseMsg.Body;
    //Check if the response is for the order placed.
    if (orderMessageID == responseMsg.CorrelationId)
    {
       responseReceived = true;
       Console.WriteLine("Status of current Order: OrderID-{0},Order
            Status-{1}", responsepo.PONumber, responsepo.Status);
    }
    else
    {
       Console.WriteLine("Status of previous Order: OrderID-{0},Order
            Status-{1}", responsepo.PONumber, responsepo.Status);
    }
  }
}
```

 Quando você executa o exemplo, as atividades de cliente e serviço são exibidas nas janelas do console do cliente e do serviço. Você pode ver o serviço receber mensagens do cliente e enviar uma resposta de volta ao cliente. O cliente exibe a resposta recebida do serviço. Pressione ENTER em cada janela do console para desligar o serviço e o cliente.

> [!NOTE]
> Este exemplo requer a instalação do MSMQ (enfileiramento de mensagens). Consulte as instruções de instalação do MSMQ na seção Consulte também.

## <a name="set-up-build-and-run-the-sample"></a>Configurar, compilar e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Se o serviço for executado primeiro, ele verificará se a fila está presente. Se a fila não estiver presente, o serviço criará uma. Você pode executar o serviço primeiro para criar a fila ou pode criar um por meio do Gerenciador de filas MSMQ. Siga estas etapas para criar uma fila no Windows 2008.

    1. Abra Gerenciador do Servidor no Visual Studio 2012.

    2. Expanda a guia **recursos** .

    3. Clique com o botão direito do mouse em **filas de mensagens particulares**e selecione **nova** **fila privada**.

    4. Marque a caixa **transacional** .

    5. Insira `ServiceModelSamplesTransacted` como o nome da nova fila.

3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Para executar o exemplo em uma configuração de computador único, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

## <a name="run-the-sample-across-computers"></a>Executar o exemplo entre computadores

1. Copie os arquivos de programa do serviço da pasta \service\bin\, na pasta específica do idioma, para o computador do serviço.

2. Copie os arquivos de programas do cliente da pasta \client\bin\, na pasta específica do idioma, para o computador cliente.

3. No arquivo client. exe. config, altere o orderQueueName para especificar o nome do computador de serviço em vez de ".".

4. No arquivo Service. exe. config, altere o endereço do ponto de extremidade do cliente para especificar o nome do computador cliente em vez de ".".

5. No computador do serviço, inicie o Service. exe em um prompt de comando.

6. No computador cliente, inicie o Client. exe em um prompt de comando.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\MSMQIntegration\MessageCorrelation`

## <a name="see-also"></a>Consulte também

- [Enfileiramento no WCF](../feature-details/queuing-in-wcf.md)
- [Enfileiramento de Mensagens](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/ms711472(v=vs.85))
