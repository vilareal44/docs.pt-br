---
title: Windows Communication Foundation para enfileiramento de mensagens
ms.date: 03/30/2017
ms.assetid: 78d0d0c9-648e-4d4a-8f0a-14d9cafeead9
ms.openlocfilehash: 872632dc7d0a8a94f8829ffb3fe8eea2607697c8
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602331"
---
# <a name="windows-communication-foundation-to-message-queuing"></a>Windows Communication Foundation para enfileiramento de mensagens

Este exemplo demonstra como um aplicativo Windows Communication Foundation (WCF) pode enviar uma mensagem para um aplicativo de enfileiramento de mensagens (MSMQ). O serviço é um aplicativo de console auto-hospedado para permitir que você observe o serviço que recebe mensagens enfileiradas. O serviço e o cliente não precisam estar em execução ao mesmo tempo.

 O serviço recebe mensagens da fila e processa os pedidos. O serviço cria uma fila transacional e configura um manipulador de mensagens recebidas de mensagem, como mostrado no código de exemplo a seguir.

```csharp
static void Main(string[] args)
{
    if (!MessageQueue.Exists(
              ConfigurationManager.AppSettings["queueName"]))
       MessageQueue.Create(
           ConfigurationManager.AppSettings["queueName"], true);
        //Connect to the queue
        MessageQueue Queue = new
    MessageQueue(ConfigurationManager.AppSettings["queueName"]);
    Queue.ReceiveCompleted +=
                 new ReceiveCompletedEventHandler(ProcessOrder);
    Queue.BeginReceive();
    Console.WriteLine("Order Service is running");
    Console.ReadLine();
}
```

 Quando uma mensagem é recebida na fila, o manipulador de mensagens `ProcessOrder` é invocado.

```csharp
public static void ProcessOrder(Object source,
    ReceiveCompletedEventArgs asyncResult)
{
    try
    {
        // Connect to the queue.
        MessageQueue Queue = (MessageQueue)source;
        // End the asynchronous receive operation.
        System.Messaging.Message msg =
                     Queue.EndReceive(asyncResult.AsyncResult);
        msg.Formatter = new System.Messaging.XmlMessageFormatter(
                                new Type[] { typeof(PurchaseOrder) });
        PurchaseOrder po = (PurchaseOrder) msg.Body;
        Random statusIndexer = new Random();
        po.Status = PurchaseOrder.OrderStates[statusIndexer.Next(3)];
        Console.WriteLine("Processing {0} ", po);
        Queue.BeginReceive();
    }
    catch (System.Exception ex)
    {
        Console.WriteLine(ex.Message);
    }

}
```

 O serviço extrai o `ProcessOrder` do corpo da mensagem MSMQ e processa o pedido.

 O nome da fila MSMQ é especificado em uma seção appSettings do arquivo de configuração, conforme mostrado na seguinte configuração de exemplo.

```xml
<appSettings>
    <add key="orderQueueName" value=".\private$\Orders" />
</appSettings>
```

> [!NOTE]
> O nome da fila usa um ponto (.) para o computador local e separadores de barra invertida em seu caminho.

 O cliente cria uma ordem de compra e envia a ordem de compra dentro do escopo de uma transação, conforme mostrado no código de exemplo a seguir.

```csharp
// Create the purchase order
PurchaseOrder po = new PurchaseOrder();
// Fill in the details
...

OrderProcessorClient client = new OrderProcessorClient("OrderResponseEndpoint");

MsmqMessage<PurchaseOrder> ordermsg = new MsmqMessage<PurchaseOrder>(po);
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
{
    client.SubmitPurchaseOrder(ordermsg);
    scope.Complete();
}
Console.WriteLine("Order has been submitted:{0}", po);

//Closing the client gracefully closes the connection and cleans up resources
client.Close();
```

 O cliente usa um cliente personalizado em ordem para enviar a mensagem MSMQ para a fila. Como o aplicativo que recebe e processa a mensagem é um aplicativo MSMQ e não um aplicativo WCF, não há nenhum contrato de serviço implícito entre os dois aplicativos. Portanto, não podemos criar um proxy usando a ferramenta svcutil. exe neste cenário.

 O cliente personalizado é essencialmente o mesmo para todos os aplicativos WCF que usam a `MsmqIntegration` Associação para enviar mensagens. Ao contrário de outros clientes, ele não inclui uma variedade de operações de serviço. É apenas uma operação de envio de mensagem.

```csharp
[System.ServiceModel.ServiceContractAttribute(Namespace = "http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true, Action = "*")]
    void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> msg);
}

public partial class OrderProcessorClient : System.ServiceModel.ClientBase<IOrderProcessor>, IOrderProcessor
{
    public OrderProcessorClient(){}

    public OrderProcessorClient(string configurationName)
        : base(configurationName)
    { }

    public OrderProcessorClient(System.ServiceModel.Channels.Binding binding, System.ServiceModel.EndpointAddress address)
        : base(binding, address)
    { }

    public void SubmitPurchaseOrder(MsmqMessage<PurchaseOrder> msg)
    {
        base.Channel.SubmitPurchaseOrder(msg);
    }
}
```

 Quando você executa o exemplo, as atividades de cliente e serviço são exibidas nas janelas do console do cliente e do serviço. Você pode ver o serviço receber mensagens do cliente. Pressione ENTER em cada janela do console para desligar o serviço e o cliente. Observe que, como o enfileiramento está em uso, o cliente e o serviço não precisam estar em funcionamento ao mesmo tempo. Por exemplo, você pode executar o cliente, desligá-lo e, em seguida, iniciar o serviço e ele ainda receberia suas mensagens.

> [!NOTE]
> Este exemplo requer a instalação do serviço de enfileiramento de mensagens. Consulte as instruções de instalação no [enfileiramento de mensagens](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/ms711472(v=vs.85)).

## <a name="set-up-build-and-run-the-sample"></a>Configurar, compilar e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Se o serviço for executado primeiro, ele verificará se a fila está presente. Se a fila não estiver presente, o serviço criará uma. Você pode executar o serviço primeiro para criar a fila ou pode criar um por meio do Gerenciador de filas MSMQ. Siga estas etapas para criar uma fila no Windows 2008.

    1. Abra Gerenciador do Servidor no Visual Studio 2012.

    2. Expanda a guia **recursos** .

    3. Clique com o botão direito do mouse em **filas de mensagens particulares**e selecione **nova**  >  **fila particular**.

    4. Marque a caixa **transacional** .

    5. Insira `ServiceModelSamplesTransacted` como o nome da nova fila.

3. Para criar a edição C# ou Visual Basic da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Para executar o exemplo em uma configuração de computador único, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

## <a name="run-the-sample-across-computers"></a>Executar o exemplo entre computadores

1. Copie os arquivos de programa do serviço da pasta \service\bin\, na pasta específica do idioma, para o computador do serviço.

2. Copie os arquivos de programas do cliente da pasta \client\bin\, na pasta específica do idioma, para o computador cliente.

3. No arquivo client. exe. config, altere o endereço do ponto de extremidade do cliente para especificar o nome do computador de serviço em vez de ".".

4. No computador do serviço, inicie o Service. exe em um prompt de comando.

5. No computador cliente, inicie o Client. exe em um prompt de comando.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\MSMQIntegration\WcfToMsmq`

## <a name="see-also"></a>Consulte também

- [Como trocar mensagens com pontos de extremidade do WCF e aplicativos de enfileiramento de mensagens](../feature-details/how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)
- [Enfileiramento de Mensagens](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/ms711472(v=vs.85))
