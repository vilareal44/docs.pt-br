---
title: Filas de mensagens de inatividade
ms.date: 03/30/2017
ms.assetid: ff664f33-ad02-422c-9041-bab6d993f9cc
ms.openlocfilehash: 8ea2ea530db8745c3802f9f39793ffd77ddd0008
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575284"
---
# <a name="dead-letter-queues"></a>Filas de mensagens de inatividade
Este exemplo demonstra como tratar e processar mensagens que falharam na entrega. Ele se baseia no exemplo de [associação MSMQ transacionado](transacted-msmq-binding.md) . Este exemplo usa a `netMsmqBinding` associação. O serviço é um aplicativo de console auto-hospedado para permitir que você observe o serviço que recebe mensagens enfileiradas.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

> [!NOTE]
> Este exemplo demonstra cada fila de mensagens mortas do aplicativo que está disponível apenas no Windows Vista. O exemplo pode ser modificado para usar as filas padrão de todo o sistema para o MSMQ 3,0 no Windows Server 2003 e no Windows XP.

 Na comunicação em fila, o cliente se comunica com o serviço usando uma fila. Mais precisamente, o cliente envia mensagens para uma fila. O serviço recebe mensagens da fila. O serviço e o cliente, portanto, não precisam estar em execução ao mesmo tempo para se comunicarem usando uma fila.

 Como a comunicação em fila pode envolver uma determinada quantidade de dormência, talvez você queira associar um valor de vida útil na mensagem para garantir que a mensagem não seja entregue ao aplicativo se ela tiver passado o tempo. Também há casos em que um aplicativo deve ser informado se uma mensagem com falha na entrega. Em todos esses casos, como quando a vida útil na mensagem expirou ou a entrega da mensagem falhou, a mensagem é colocada em uma fila de mensagens mortas. O aplicativo de envio pode, então, ler as mensagens na fila de mensagens mortas e tomar medidas corretivas que variam de nenhuma ação para corrigir os motivos da entrega com falha e reenviar a mensagem.

 A fila de mensagens mortas na `NetMsmqBinding` associação é expressa nas seguintes propriedades:

- <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A>a propriedade para expressar o tipo de fila de mensagens mortas exigido pelo cliente. Essa enumeração tem os seguintes valores:

- `None`: Nenhuma fila de mensagens mortas é exigida pelo cliente.

- `System`: A fila de mensagens mortas do sistema é usada para armazenar mensagens mortas. A fila de mensagens mortas do sistema é compartilhada por todos os aplicativos em execução no computador.

- `Custom`: Uma fila de mensagens mortas personalizada especificada usando a <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> propriedade é usada para armazenar mensagens mortas. Esse recurso só está disponível no Windows Vista. Isso é usado quando o aplicativo deve usar sua própria fila de mensagens mortas em vez de compartilhá-lo com outros aplicativos em execução no mesmo computador.

- <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A>Propriedade para expressar a fila específica a ser usada como uma fila de mensagens mortas. Isso está disponível apenas no Windows Vista.

 Neste exemplo, o cliente envia um lote de mensagens para o serviço de dentro do escopo de uma transação e especifica um valor arbitrariamente baixo para "vida útil" para essas mensagens (cerca de 2 segundos). O cliente também especifica uma fila de mensagens mortas personalizada a ser usada para enfileirar as mensagens que expiraram.

 O aplicativo cliente pode ler as mensagens na fila de mensagens mortas e tentar enviar a mensagem novamente ou corrigir o erro que fez com que a mensagem original fosse colocada na fila de mensagens mortas e enviar a mensagem. No exemplo, o cliente exibe uma mensagem de erro.

 O contrato de serviço é `IOrderProcessor` , conforme mostrado no código de exemplo a seguir.

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true)]
    void SubmitPurchaseOrder(PurchaseOrder po);
}
```

 O código de serviço no exemplo é o da [associação MSMQ transacionada](transacted-msmq-binding.md).

 A comunicação com o serviço ocorre dentro do escopo de uma transação. O serviço lê as mensagens da fila, executa a operação e, em seguida, exibe os resultados da operação. O aplicativo também cria uma fila de mensagens mortas para mensagem de inatividade.

```csharp
//The service contract is defined in generatedClient.cs, generated from the service by the svcutil tool.

//Client implementation code.
class Client
{
    static void Main()
    {
        // Get MSMQ queue name from app settings in configuration
        string deadLetterQueueName = ConfigurationManager.AppSettings["deadLetterQueueName"];

        // Create the transacted MSMQ queue for storing dead message if necessary.
        if (!MessageQueue.Exists(deadLetterQueueName))
            MessageQueue.Create(deadLetterQueueName, true);

        // Create a proxy with given client endpoint configuration
        OrderProcessorClient client = new OrderProcessorClient("OrderProcessorEndpoint");

        // Create the purchase order
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

        //Create a transaction scope.
        using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
        {
            // Make a queued call to submit the purchase order
            client.SubmitPurchaseOrder(po);
            // Complete the transaction.
            scope.Complete();
        }

        client.Close();

        Console.WriteLine();
        Console.WriteLine("Press <ENTER> to terminate client.");
        Console.ReadLine();
    }
}
```

 A configuração do cliente especifica uma duração curta para a mensagem alcançar o serviço. Se a mensagem não puder ser transmitida dentro da duração especificada, a mensagem expirará e será movida para a fila de mensagens mortas.

> [!NOTE]
> É possível que o cliente entregue a mensagem à fila de serviço dentro do tempo especificado. Para garantir que você veja o serviço de mensagens mortas em ação, execute o cliente antes de iniciar o serviço. A mensagem atinge o tempo limite e é entregue ao serviço de mensagens mortas.

 O aplicativo deve definir qual fila usar como sua fila de mensagens mortas. Se nenhuma fila for especificada, a fila de mensagens mortas transacionais em todo o sistema padrão será usada para enfileirar mensagens mortas. Neste exemplo, o aplicativo cliente especifica sua própria fila de mensagens mortas do aplicativo.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <appSettings>
    <!-- use appSetting to configure MSMQ Dead Letter queue name -->
    <add key="deadLetterQueueName" value=".\private$\ServiceModelSamplesOrdersAppDLQ"/>
  </appSettings>

  <system.serviceModel>
    <client>
      <!-- Define NetMsmqEndpoint -->
      <endpoint name="OrderProcessorEndpoint"
                address="net.msmq://localhost/private/ServiceModelSamplesDeadLetter"
                binding="netMsmqBinding"
                bindingConfiguration="PerAppDLQBinding"
                contract="IOrderProcessor" />
    </client>

    <bindings>
      <netMsmqBinding>
        <binding name="PerAppDLQBinding"
                 deadLetterQueue="Custom"
                 customDeadLetterQueue="net.msmq://localhost/private/ServiceModelSamplesOrdersAppDLQ"
                 timeToLive="00:00:02"/>
      </netMsmqBinding>
    </bindings>
  </system.serviceModel>

</configuration>
```

 O serviço de mensagens mortas lê mensagens da fila de mensagens mortas. O serviço de mensagem de mensagens mortas implementa o `IOrderProcessor` contrato. No entanto, sua implementação não processa pedidos. O serviço de mensagem de mensagens mortas é um serviço de cliente e não tem o recurso de processar pedidos.

> [!NOTE]
> A fila de mensagens mortas é uma fila de cliente e é local para o Gerenciador de filas de cliente.

 A implementação do serviço de mensagens mortas verifica o motivo da falha na entrega de uma mensagem e faz medidas corretivas. O motivo para uma falha de mensagem é capturado em duas enumerações <xref:System.ServiceModel.Channels.MsmqMessageProperty.DeliveryFailure%2A> e <xref:System.ServiceModel.Channels.MsmqMessageProperty.DeliveryStatus%2A> . Você pode recuperar o <xref:System.ServiceModel.Channels.MsmqMessageProperty> do <xref:System.ServiceModel.OperationContext> conforme mostrado no código de exemplo a seguir:

```csharp
public void SubmitPurchaseOrder(PurchaseOrder po)
{
    Console.WriteLine("Submitting purchase order did not succeed ", po);
    MsmqMessageProperty mqProp =
                  OperationContext.Current.IncomingMessageProperties[
                  MsmqMessageProperty.Name] as MsmqMessageProperty;
    Console.WriteLine("Message Delivery Status: {0} ",
                                                mqProp.DeliveryStatus);
    Console.WriteLine("Message Delivery Failure: {0}",
                                               mqProp.DeliveryFailure);
    Console.WriteLine();
    …
}
```

 As mensagens na fila de mensagens mortas são aquelas que são endereçadas ao serviço que está processando a mensagem. Portanto, quando o serviço de mensagem de inatividade lê mensagens da fila, a camada de canal Windows Communication Foundation (WCF) localiza a incompatibilidade em pontos de extremidade e não despacha a mensagem. Nesse caso, a mensagem é endereçada ao serviço de processamento de pedidos, mas é recebida pelo serviço de mensagem de mensagens mortas. Para receber uma mensagem que é endereçada a um ponto de extremidade diferente, um filtro de endereço para corresponder a qualquer endereço é especificado no `ServiceBehavior` . Isso é necessário para processar com êxito as mensagens que são lidas da fila de mensagens mortas.

 Neste exemplo, o serviço mensagem de mensagens mortas reenvia a mensagem se o motivo da falha é que a mensagem atingiu o tempo limite. Por todos os outros motivos, ele exibe a falha de entrega, conforme mostrado no código de exemplo a seguir:

```csharp
// Service class that implements the service contract.
// Added code to write output to the console window.
[ServiceBehavior(InstanceContextMode=InstanceContextMode.Single, ConcurrencyMode=ConcurrencyMode.Single, AddressFilterMode=AddressFilterMode.Any)]
public class PurchaseOrderDLQService : IOrderProcessor
{
    OrderProcessorClient orderProcessorService;
    public PurchaseOrderDLQService()
    {
        orderProcessorService = new OrderProcessorClient("OrderProcessorEndpoint");
    }

    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po)
    {
        Console.WriteLine("Submitting purchase order did not succeed ", po);
        MsmqMessageProperty mqProp = OperationContext.Current.IncomingMessageProperties[MsmqMessageProperty.Name] as MsmqMessageProperty;

        Console.WriteLine("Message Delivery Status: {0} ", mqProp.DeliveryStatus);
        Console.WriteLine("Message Delivery Failure: {0}", mqProp.DeliveryFailure);
        Console.WriteLine();

        // resend the message if timed out
        if (mqProp.DeliveryFailure == DeliveryFailure.ReachQueueTimeout ||
            mqProp.DeliveryFailure == DeliveryFailure.ReceiveTimeout)
        {
            // re-send
            Console.WriteLine("Purchase order Time To Live expired");
            Console.WriteLine("Trying to resend the message");

            // reuse the same transaction used to read the message from dlq to enqueue the message to app. queue
            orderProcessorService.SubmitPurchaseOrder(po);
            Console.WriteLine("Purchase order resent");
        }
    }

    // Host the service within this EXE console application.
    public static void Main()
    {
        // Create a ServiceHost for the PurchaseOrderDLQService type.
        using (ServiceHost serviceHost = new ServiceHost(typeof(PurchaseOrderDLQService)))
        {
            // Open the ServiceHostBase to create listeners and start listening for messages.
            serviceHost.Open();

            // The service can now be accessed.
            Console.WriteLine("The dead letter service is ready.");
            Console.WriteLine("Press <ENTER> to terminate service.");
            Console.WriteLine();
            Console.ReadLine();
        }
    }
}
```

 O exemplo a seguir mostra a configuração de uma mensagem de mensagens mortas:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.PurchaseOrderDLQService">
        <!-- Define NetMsmqEndpoint in this case, DLQ end point to read messages-->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamplesOrdersAppDLQ"
                  binding="netMsmqBinding"
                  bindingConfiguration="DefaultBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
      </service>
    </services>

    <client>
      <!-- Define NetMsmqEndpoint -->
      <endpoint name="OrderProcessorEndpoint"
                 address="net.msmq://localhost/private/ServiceModelSamplesDeadLetter"
                 binding="netMsmqBinding"
                 bindingConfiguration="SystemDLQBinding"
                 contract="IOrderProcessor" />
    </client>

    <bindings>
      <netMsmqBinding>
        <binding name="DefaultBinding" />
        <binding name="SystemDLQBinding"
                 deadLetterQueue="System"/>
      </netMsmqBinding>
    </bindings>
  </system.serviceModel>
</configuration>
```

 Ao executar o exemplo, há três executáveis a serem executados para ver como a fila de mensagens mortas funciona para cada aplicativo; o cliente, o serviço e um serviço de mensagens mortas que lê da fila de mensagens mortas para cada aplicativo e reenvia a mensagem para o serviço. Todos são aplicativos de console com saída em janelas de console.

> [!NOTE]
> Como o enfileiramento está em uso, o cliente e o serviço não precisam estar em funcionamento ao mesmo tempo. Você pode executar o cliente, desligá-lo e, em seguida, iniciar o serviço e ele ainda receberá suas mensagens. Você deve iniciar o serviço e desligá-lo para que a fila possa ser criada.

 Ao executar o cliente, o cliente exibe a mensagem:

```console
Press <ENTER> to terminate client.
```

 O cliente tentou enviar a mensagem, mas com um tempo limite curto, a mensagem expirou e agora está na fila na fila de mensagens mortas para cada aplicativo.

 Em seguida, você executa o serviço de mensagens mortas, que lê a mensagem e exibe o código de erro e reenvia a mensagem de volta para o serviço.

```console
The dead letter service is ready.
Press <ENTER> to terminate service.

Submitting purchase order did not succeed
Message Delivery Status: InDoubt
Message Delivery Failure: ReachQueueTimeout

Purchase order Time To Live expired
Trying to resend the message
Purchase order resent
```

 O serviço é iniciado e, em seguida, lê a mensagem reenviada e a processa.

```console
The service is ready.
Press <ENTER> to terminate service.

Processing Purchase Order: 97897eff-f926-4057-a32b-af8fb11b9bf9
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

4. Para executar o exemplo em uma configuração de computador único ou entre computadores, altere os nomes da fila adequadamente, substituindo localhost pelo nome completo do computador e siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup"></a>Para executar o exemplo em um computador ingressado em um grupo de trabalho

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

     Verifique se o ponto de extremidade está associado à associação definindo o atributo do ponto de extremidade `bindingConfiguration` .

2. Certifique-se de alterar a configuração no DeadLetterService, no servidor e no cliente antes de executar o exemplo.

    > [!NOTE]
    > A configuração `security mode` como `None` é equivalente à `MsmqAuthenticationMode` configuração `MsmqProtectionLevel` e `Message` à segurança para `None` .

## <a name="comments"></a>Comentários
 Por padrão, com o `netMsmqBinding` transporte de associação, a segurança é habilitada. Duas propriedades `MsmqAuthenticationMode` e `MsmqProtectionLevel` , juntas, determinam o tipo de segurança de transporte. Por padrão, o modo de autenticação é definido como `Windows` e o nível de proteção é definido como `Sign` . Para que o MSMQ forneça o recurso de autenticação e assinatura, ele deve fazer parte de um domínio. Se você executar esse exemplo em um computador que não faz parte de um domínio, receberá o seguinte erro: "certificado interno do serviço de enfileiramento de mensagens não existe".

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\DeadLetter`  
