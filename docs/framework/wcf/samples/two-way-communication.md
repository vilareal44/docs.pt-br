---
title: Comunicação bidirecional
ms.date: 03/30/2017
ms.assetid: fb64192d-b3ea-4e02-9fb3-46a508d26c60
ms.openlocfilehash: 291380d656b0e22c7fdf1cb291c45d05359a95c8
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591261"
---
# <a name="two-way-communication"></a>Comunicação bidirecional
Este exemplo demonstra como executar a comunicação em fila de duas vias transacionada no MSMQ. Este exemplo usa a `netMsmqBinding` associação. Nesse caso, o serviço é um aplicativo de console auto-hospedado que permite que você observe o serviço que recebe mensagens enfileiradas.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Este exemplo é baseado na [associação MSMQ transacionada](transacted-msmq-binding.md).  
  
 Na comunicação em fila, o cliente se comunica com o serviço usando uma fila. O cliente envia mensagens para uma fila e o serviço recebe mensagens da fila. O serviço e o cliente, portanto, não precisam estar em execução ao mesmo tempo para se comunicarem usando uma fila.  
  
 Este exemplo demonstra a comunicação bidirecional usando filas. O cliente envia pedidos de compra para a fila de dentro do escopo de uma transação. O serviço recebe os pedidos, processa a ordem e, em seguida, chama o cliente com o status da ordem da fila dentro do escopo de uma transação. Para facilitar a comunicação bidirecional, o cliente e o serviço usam filas para enfileirar ordens de compra e status do pedido.  
  
 O contrato de serviço `IOrderProcessor` define operações de serviço unidirecionais que se adaptam ao uso do enfileiramento. A operação de serviço inclui o ponto de extremidade de resposta a ser usado para enviar os status do pedido para. O ponto de extremidade de resposta é o URI da fila para enviar o status do pedido de volta para o cliente. O aplicativo de processamento de pedidos implementa esse contrato.  

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IOrderProcessor  
{  
    [OperationContract(IsOneWay = true)]  
    void SubmitPurchaseOrder(PurchaseOrder po, string
                                  reportOrderStatusTo);  
}
```
  
 O contrato de resposta para enviar o status da ordem é especificado pelo cliente. O cliente implementa o contrato de status do pedido. O serviço usa o proxy gerado deste contrato para enviar o status do pedido de volta para o cliente.  

```csharp
[ServiceContract]  
public interface IOrderStatus  
{  
    [OperationContract(IsOneWay = true)]  
    void OrderStatus(string poNumber, string status);  
}  
```

 A operação de serviço processa a ordem de compra enviada. O <xref:System.ServiceModel.OperationBehaviorAttribute> é aplicado à operação de serviço para especificar a inscrição automática em uma transação que é usada para receber a mensagem da fila e a conclusão automática de transações na conclusão da operação de serviço. A `Orders` classe encapsula a funcionalidade de processamento de pedidos. Nesse caso, ele adiciona a ordem de compra a um dicionário. A transação na qual a operação de serviço inscrito está disponível para as operações na `Orders` classe.  
  
 A operação de serviço, além de processar a ordem de compra enviada, responde de volta ao cliente no status do pedido.  

```csharp
[OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]  
public void SubmitPurchaseOrder(PurchaseOrder po, string reportOrderStatusTo)  
{  
    Orders.Add(po);  
    Console.WriteLine("Processing {0} ", po);  
    Console.WriteLine("Sending back order status information");  
    NetMsmqBinding msmqCallbackBinding = new NetMsmqBinding("ClientCallbackBinding");  
    OrderStatusClient client = new OrderStatusClient(msmqCallbackBinding, new EndpointAddress(reportOrderStatusTo));  
  
    // Please note that the same transaction that is used to dequeue the purchase order is used  
    // to send back order status.  
    using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))  
    {  
        client.OrderStatus(po.PONumber, po.Status);  
        scope.Complete();  
    }  
    //Close the client.  
    client.Close();  
}  
```

 O nome da fila MSMQ é especificado em uma seção appSettings do arquivo de configuração. O ponto de extremidade para o serviço é definido na seção System. ServiceModel do arquivo de configuração.  
  
> [!NOTE]
> O nome da fila MSMQ e o endereço do ponto de extremidade usam convenções de endereçamento ligeiramente diferentes. O nome da fila MSMQ usa um ponto (.) para o computador local e separadores de barra invertida em seu caminho. O endereço do ponto de extremidade do Windows Communication Foundation (WCF) especifica um net. MSMQ: esquema, usa "localhost" para o computador local e usa barras invertidas em seu caminho. Para ler de uma fila hospedada no computador remoto, substitua "." e "localhost" pelo nome do computador remoto.  
  
 O serviço é hospedado internamente. Ao usar o transporte MSMQ, a fila usada deve ser criada com antecedência. Isso pode ser feito manualmente ou por meio de código. Neste exemplo, o serviço verifica a existência da fila e a cria, se necessário. O nome da fila é lido no arquivo de configuração. O endereço base é usado pela [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para gerar o proxy para o serviço.  

```csharp
// Host the service within this EXE console application.  
public static void Main()  
{  
    // Get MSMQ queue name from appSettings in configuration.  
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
    }  
}  
```

 O cliente cria uma transação. A comunicação com a fila ocorre dentro do escopo da transação, fazendo com que ela seja tratada como uma unidade atômica onde todas as mensagens são bem sucedidas ou falham.  

```csharp
// Create a ServiceHost for the OrderStatus service type.  
using (ServiceHost serviceHost = new ServiceHost(typeof(OrderStatusService)))  
{  
  
    // Open the ServiceHostBase to create listeners and start listening for order status messages.  
    serviceHost.Open();  
  
    // Create the purchase order.  
    ...  
  
    // Create a client with given client endpoint configuration.  
    OrderProcessorClient client = new OrderProcessorClient("OrderProcessorEndpoint");  
  
    //Create a transaction scope.  
    using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))  
    {  
        string hostName = Dns.GetHostName();  
  
        // Make a queued call to submit the purchase order.  
        client.SubmitPurchaseOrder(po, "net.msmq://" + hostName + "/private/ServiceModelSamplesTwo-way/OrderStatus");  
  
        // Complete the transaction.  
        scope.Complete();  
    }  
  
    //Close down the client.  
    client.Close();  
  
    Console.WriteLine();  
    Console.WriteLine("Press <ENTER> to terminate client.");  
    Console.ReadLine();  
  
    // Close the ServiceHost to shutdown the service.  
    serviceHost.Close();  
}  
```

 O código do cliente implementa o `IOrderStatus` contrato para receber o status do pedido do serviço. Nesse caso, ele imprime o status do pedido.  

```csharp
[ServiceBehavior]  
public class OrderStatusService : IOrderStatus  
{  
    [OperationBehavior(TransactionAutoComplete = true,
                        TransactionScopeRequired = true)]  
    public void OrderStatus(string poNumber, string status)  
    {  
        Console.WriteLine("Status of order {0}:{1} ", poNumber ,
                                                           status);  
    }  
}  
```

 A fila status do pedido é criada no `Main` método. A configuração do cliente inclui a configuração do serviço de status do pedido para hospedar o serviço de status do pedido, conforme mostrado na seguinte configuração de exemplo.  
  
```xml  
<appSettings>  
  <!-- Use appSetting to configure MSMQ queue name. -->  
  <add key="queueName" value=".\private$\ServiceModelSamplesTwo-way/OrderStatus" />  
</appSettings>  
  
<system.serviceModel>  
  
  <services>  
    <service
       name="Microsoft.ServiceModel.Samples.OrderStatusService">  
      <!-- Define NetMsmqEndpoint -->  
      <endpoint address="net.msmq://localhost/private/ServiceModelSamplesTwo-way/OrderStatus"  
                binding="netMsmqBinding"  
                contract="Microsoft.ServiceModel.Samples.IOrderStatus" />  
    </service>  
  </services>  
  
  <client>  
    <!-- Define NetMsmqEndpoint -->  
    <endpoint name="OrderProcessorEndpoint"  
              address="net.msmq://localhost/private/ServiceModelSamplesTwo-way/OrderProcessor"
              binding="netMsmqBinding"
              contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />  
  </client>  
  
</system.serviceModel>  
```  
  
 Quando você executa o exemplo, as atividades de cliente e serviço são exibidas nas janelas do console do cliente e do serviço. Você pode ver o serviço receber mensagens do cliente. Pressione ENTER em cada janela do console para desligar o serviço e o cliente.  
  
 O serviço exibe as informações da ordem de compra e indica que está enviando de volta o status do pedido para a fila status do pedido.  
  
```console  
The service is ready.  
Press <ENTER> to terminate service.  
  
Processing Purchase Order: 124a1f69-3699-4b16-9bcc-43147a8756fc  
        Customer: somecustomer.com  
        OrderDetails  
                Order LineItem: 54 of Blue Widget @unit price: $29.99  
                Order LineItem: 890 of Red Widget @unit price: $45.89  
        Total cost of this order: $42461.56  
        Order status: Pending  
  
Sending back order status information  
```  
  
 O cliente exibe as informações de status do pedido enviadas pelo serviço.  
  
```console  
Press <ENTER> to terminate client.  
Status of order 124a1f69-3699-4b16-9bcc-43147a8756fc:Pending  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
    > [!NOTE]
    > Se você usar svcutil. exe para regenerar a configuração para este exemplo, certifique-se de modificar os nomes dos pontos de extremidade na configuração do cliente para corresponder ao código do cliente.  
  
 Por padrão, com a <xref:System.ServiceModel.NetMsmqBinding> segurança de transporte está habilitada. Há duas propriedades relevantes para a segurança de transporte do MSMQ <xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A> e, <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> `.` por padrão, o modo de autenticação é definido como `Windows` e o nível de proteção é definido como `Sign` . Para que o MSMQ forneça o recurso de autenticação e assinatura, ele deve fazer parte de um domínio e a opção de integração do Active Directory para o MSMQ deve ser instalada. Se você executar esse exemplo em um computador que não atenda a esses critérios, receberá um erro.  
  
### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup-or-without-active-directory-integration"></a>Para executar o exemplo em um computador ingressado em um grupo de trabalho ou sem integração com o Active Directory  
  
1. Se o seu computador não fizer parte de um domínio ou não tiver a integração do Active Directory instalada, desative a segurança de transporte definindo o modo de autenticação e o nível de proteção como `None` mostrado na seguinte configuração de exemplo:  
  
    ```xml  
    <configuration>  
  
      <appSettings>  
        <!-- Use appSetting to configure MSMQ queue name. -->  
        <add key="queueName" value=".\private$\ServiceModelSamplesTwo-way/OrderProcessor" />  
      </appSettings>  
  
      <system.serviceModel>  
        <services>  
          <service
              name="Microsoft.ServiceModel.Samples.OrderProcessorService">  
            <!-- Define NetMsmqEndpoint -->  
            <endpoint address="net.msmq://localhost/private/ServiceModelSamplesTwo-way/OrderProcessor"  
                      binding="netMsmqBinding"  
                      bindingConfiguration="TransactedBinding"
                      contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />  
          </service>  
        </services>  
  
        <bindings>  
          <netMsmqBinding>  
            <binding name="TransactedBinding" >  
             <security mode="None" />  
            </binding>  
          </netMsmqBinding>  
        </bindings>  
  
      </system.serviceModel>  
  
    </configuration>  
    ```  
  
2. Desativar a segurança para uma configuração de cliente gera o seguinte:  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <appSettings>  
        <!-- Use appSetting to configure MSMQ queue name. -->  
        <add key="queueName" value=".\private$\ServiceModelSamplesTwo-way/OrderStatus" />  
      </appSettings>  
  
      <system.serviceModel>  
  
        <services>  
          <service
             name="Microsoft.ServiceModel.Samples.OrderStatusService">  
            <!-- Define NetMsmqEndpoint -->  
            <endpoint address="net.msmq://localhost/private/ServiceModelSamplesTwo-way/OrderStatus"  
                      binding="netMsmqBinding"  
                      bindingConfiguration="TransactedBinding" contract="Microsoft.ServiceModel.Samples.IOrderStatus" />  
          </service>  
        </services>  
  
        <client>  
          <!-- Define NetMsmqEndpoint -->  
          <endpoint name="OrderProcessorEndpoint"  
                    address="net.msmq://localhost/private/ServiceModelSamplesTwo-way/OrderProcessor"
                    binding="netMsmqBinding"
                    bindingConfiguration="TransactedBinding"  
                    contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />  
        </client>  
  
        <bindings>  
          <netMsmqBinding>  
            <binding name="TransactedBinding" >  
             <security mode="None" />  
            </binding>  
          </netMsmqBinding>  
        </bindings>  
  
      </system.serviceModel>  
  
    </configuration>  
    ```  
  
3. O serviço para este exemplo cria uma associação no `OrderProcessorService` . Adicione uma linha de código depois que a associação é instanciada para definir o modo de segurança como `None` .  
  
    ```csharp
    NetMsmqBinding msmqCallbackBinding = new NetMsmqBinding();  
    msmqCallbackBinding.Security.Mode = NetMsmqSecurityMode.None;  
    ```  
  
4. Certifique-se de alterar a configuração no servidor e no cliente antes de executar o exemplo.  
  
    > [!NOTE]
    > A configuração `security mode` como `None` é equivalente à <xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A> configuração <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> ou `Message` à segurança para `None` .  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Binding\Net\MSMQ\Two-Way`  
