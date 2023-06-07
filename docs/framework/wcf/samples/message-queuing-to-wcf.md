---
title: Enfileiramento de mensagens para o Windows Communication Foundation
ms.date: 03/30/2017
ms.assetid: 6d718eb0-9f61-4653-8a75-d2dac8fb3520
ms.openlocfilehash: 82e71afc911bff2504be15f7f9f2e736d943972b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584955"
---
# <a name="message-queuing-to-windows-communication-foundation"></a>Enfileiramento de mensagens para o Windows Communication Foundation

Este exemplo demonstra como um aplicativo MSMQ (enfileiramento de mensagens) pode enviar uma mensagem MSMQ para um serviço Windows Communication Foundation (WCF). O serviço é um aplicativo de console auto-hospedado para permitir que você observe o serviço que recebe mensagens enfileiradas.

 O contrato de serviço é `IOrderProcessor` , que define um serviço unidirecional que é adequado para uso com filas. Uma mensagem MSMQ não tem um cabeçalho Action, portanto, não é possível mapear diferentes mensagens MSMQ para contratos de operação automaticamente. Portanto, pode haver apenas um contrato de operação. Se você quiser definir mais de um contrato de operação para o serviço, o aplicativo deverá fornecer informações sobre qual cabeçalho na mensagem MSMQ (por exemplo, o rótulo ou CorrelationId) pode ser usado para decidir qual contrato de operação deve ser redespachado.

 A mensagem MSMQ não contém informações sobre quais cabeçalhos são mapeados para os diferentes parâmetros do contrato de operação. O parâmetro é do tipo <xref:System.ServiceModel.MsmqIntegration.MsmqMessage%601> ( `MsmqMessage<T>` ), que contém a mensagem do MSMQ subjacente. O tipo "T" na <xref:System.ServiceModel.MsmqIntegration.MsmqMessage%601> classe ( `MsmqMessage<T>` ) representa os dados que são serializados no corpo da mensagem MSMQ. Neste exemplo, o `PurchaseOrder` tipo é serializado no corpo da mensagem do MSMQ.

 O código de exemplo a seguir mostra o contrato de serviço do serviço de processamento de pedidos.

```csharp
// Define a service contract.
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]
[ServiceKnownType(typeof(PurchaseOrder))]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true, Action = "*")]
    void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> msg);
}
```

 O serviço é hospedado internamente. Ao usar o MSMQ, a fila usada deve ser criada com antecedência. Isso pode ser feito manualmente ou por meio de código. Neste exemplo, o serviço verifica a existência da fila e a cria, se necessário. O nome da fila é lido no arquivo de configuração.

```csharp
public static void Main()
{
    // Get the MSMQ queue name from the application settings in
    // configuration.
    string queueName = ConfigurationManager.AppSettings["queueName"];
    // Create the MSMQ queue if necessary.
    if (!MessageQueue.Exists(queueName))
        MessageQueue.Create(queueName, true);
    …
}
```

 O serviço cria e abre um <xref:System.ServiceModel.ServiceHost> para o `OrderProcessorService` , conforme mostrado no código de exemplo a seguir.

```csharp
using (ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService)))
{
    serviceHost.Open();
    Console.WriteLine("The service is ready.");
    Console.WriteLine("Press <ENTER> to terminate service.");
    Console.ReadLine();
    serviceHost.Close();
}
```

 O nome da fila MSMQ é especificado em uma seção appSettings do arquivo de configuração, conforme mostrado na seguinte configuração de exemplo.

> [!NOTE]
> O nome da fila usa um ponto (.) para o computador local e separadores de barra invertida em seu caminho. O endereço do ponto de extremidade do WCF especifica um esquema MSMQ. FormatName e usa localhost para o computador local. O endereço da fila de cada diretriz de endereçamento de nome de formato MSMQ segue o esquema MSMQ. FormatName.

```xml
<appSettings>
    <add key="orderQueueName" value=".\private$\Orders" />
</appSettings>
```

 O aplicativo cliente é um aplicativo MSMQ que usa o <xref:System.Messaging.MessageQueue.Send%2A> método para enviar uma mensagem durável e transacional para a fila, conforme mostrado no código de exemplo a seguir.

```csharp
//Connect to the queue.
MessageQueue orderQueue = new MessageQueue(ConfigurationManager.AppSettings["orderQueueName"]);

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

// Submit the purchase order.
Message msg = new Message();
msg.Body = po;
//Create a transaction scope.
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
{

    orderQueue.Send(msg, MessageQueueTransactionType.Automatic);
    // Complete the transaction.
    scope.Complete();

}
Console.WriteLine("Placed the order:{0}", po);
Console.WriteLine("Press <ENTER> to terminate client.");
Console.ReadLine();
```

 Quando você executa o exemplo, as atividades de cliente e serviço são exibidas nas janelas do console do cliente e do serviço. Você pode ver o serviço receber mensagens do cliente. Pressione ENTER em cada janela do console para desligar o serviço e o cliente. Observe que, como o enfileiramento está em uso, o cliente e o serviço não precisam estar em funcionamento ao mesmo tempo. Por exemplo, você pode executar o cliente, desligá-lo e, em seguida, iniciar o serviço e ele ainda receberia suas mensagens.

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

4. No computador do serviço, inicie o Service. exe em um prompt de comando.

5. No computador cliente, inicie o Client. exe em um prompt de comando.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\MSMQIntegration\MsmqToWcf`

## <a name="see-also"></a>Consulte também

- [Filas no WCF](../feature-details/queues-in-wcf.md)
- [Como trocar mensagens com pontos de extremidade do WCF e aplicativos de enfileiramento de mensagens](../feature-details/how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)
- [Enfileiramento de Mensagens](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/ms711472(v=vs.85))
