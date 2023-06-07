---
title: Sessões e filas
ms.date: 03/30/2017
ms.assetid: 47d7c5c2-1e6f-4619-8003-a0ff67dcfbd6
ms.openlocfilehash: 425135b533b898cbc75464f50ce8a5e8f6550755
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584162"
---
# <a name="sessions-and-queues"></a>Sessões e filas

Este exemplo demonstra como enviar e receber um conjunto de mensagens relacionadas na comunicação em fila por meio do transporte do MSMQ (enfileiramento de mensagens). Este exemplo usa a `netMsmqBinding` associação. O serviço é um aplicativo de console auto-hospedado para permitir que você observe o serviço que recebe mensagens enfileiradas.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\Session`  
  
 Na comunicação em fila, o cliente se comunica com o serviço usando uma fila. Mais precisamente, o cliente envia mensagens para uma fila. O serviço recebe mensagens da fila. O serviço e o cliente, portanto, não precisam estar em execução ao mesmo tempo para se comunicarem usando uma fila.  
  
 Às vezes, um cliente envia um conjunto de mensagens que estão relacionadas entre si em um grupo. Quando as mensagens devem ser processadas juntas ou em uma ordem especificada, uma fila pode ser usada para agrupá-las, para processamento por um único aplicativo de recebimento. Isso é particularmente importante quando há vários aplicativos de recebimento em um grupo de servidores e é necessário garantir que um grupo de mensagens seja processado pelo mesmo aplicativo de recebimento. Sessões em fila são um mecanismo usado para enviar e receber um conjunto relacionado de mensagens que devem ser processadas de uma só vez. Sessões em fila exigem uma transação para exibir esse padrão.  
  
 No exemplo, o cliente envia várias mensagens para o serviço como parte de uma sessão dentro do escopo de uma única transação.  
  
 O contrato de serviço é `IOrderTaker` , que define um serviço unidirecional que é adequado para uso com filas. O <xref:System.ServiceModel.SessionMode> usado no contrato mostrado no código de exemplo a seguir indica que as mensagens fazem parte da sessão.  

```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples", SessionMode=SessionMode.Required)]  
public interface IOrderTaker  
{  
    [OperationContract(IsOneWay = true)]  
    void OpenPurchaseOrder(string customerId);  
  
    [OperationContract(IsOneWay = true)]  
    void AddProductLineItem(string productId, int quantity);  
  
    [OperationContract(IsOneWay = true)]  
    void EndPurchaseOrder();  
}  
```

 O serviço define as operações de serviço de forma que a primeira operação se inliste em uma transação, mas não conclua automaticamente a transação. As operações subsequentes também são inscritas na mesma transação, mas não são concluídas automaticamente. A última operação na sessão conclui automaticamente a transação. Assim, a mesma transação é usada para várias invocações de operação no contrato de serviço. Se qualquer uma das operações lançar uma exceção, a transação será revertida e a sessão é colocada de volta na fila. Após a conclusão bem-sucedida da última operação, a transação será confirmada. O serviço usa `PerSession` como o <xref:System.ServiceModel.InstanceContextMode> para receber todas as mensagens em uma sessão na mesma instância do serviço.  

```csharp
[ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]  
public class OrderTakerService : IOrderTaker  
{  
    PurchaseOrder po;  
  
    [OperationBehavior(TransactionScopeRequired = true,
                                 TransactionAutoComplete = false)]  
    public void OpenPurchaseOrder(string customerId)  
    {  
        Console.WriteLine("Creating purchase order");  
        po = new PurchaseOrder(customerId);  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true,
                                  TransactionAutoComplete = false)]  
    public void AddProductLineItem(string productId, int quantity)  
    {  
        po.AddProductLineItem(productId, quantity);  
        Console.WriteLine("Product " + productId + " quantity " +
                            quantity + " added to purchase order");  
    }  
  
    [OperationBehavior(TransactionScopeRequired = true,
                                  TransactionAutoComplete = true)]  
    public void EndPurchaseOrder()  
    {  
       Console.WriteLine("Purchase Order Completed");  
       Console.WriteLine();  
       Console.WriteLine(po.ToString());  
    }  
}  
```

 O serviço é hospedado internamente. Ao usar o transporte MSMQ, a fila usada deve ser criada com antecedência. Isso pode ser feito manualmente ou por meio de código. Neste exemplo, o serviço contém o <xref:System.Messaging> código para verificar a existência da fila e a cria, se necessário. O nome da fila é lido no arquivo de configuração usando a <xref:System.Configuration.ConfigurationManager.AppSettings%2A> classe.  

```csharp
// Host the service within this EXE console application.  
public static void Main()  
{  
    // Get MSMQ queue name from app settings in configuration.  
    string queueName = ConfigurationManager.AppSettings["queueName"];  
  
    // Create the transacted MSMQ queue if necessary.  
    if (!MessageQueue.Exists(queueName))  
        MessageQueue.Create(queueName, true);  
  
    // Create a ServiceHost for the OrderTakerService type.  
    using (ServiceHost serviceHost = new ServiceHost(typeof(OrderTakerService)))  
    {  
        // Open the ServiceHost to create listeners and start listening for messages.  
        serviceHost.Open();  
  
        // The service can now be accessed.  
        Console.WriteLine("The service is ready.");  
        Console.WriteLine("Press <ENTER> to terminate service.");  
        Console.WriteLine();  
        Console.ReadLine();  
  
        // Close the ServiceHost to shutdown the service.  
        serviceHost.Close();
    }  
}  
```

 O nome da fila MSMQ é especificado em uma seção appSettings do arquivo de configuração. O ponto de extremidade para o serviço é definido na seção System. serviceModel do arquivo de configuração e especifica a `netMsmqBinding` associação.  
  
```xml  
<appSettings>  
  <!-- Use appSetting to configure MSMQ queue name. -->  
  <add key="queueName" value=".\private$\ServiceModelSamplesSession" />  
</appSettings>  
  
<system.serviceModel>  
  <services>  
    <service name="Microsoft.ServiceModel.Samples.OrderTakerService"  
        behaviorConfiguration="CalculatorServiceBehavior">  
      ...  
      <!-- Define NetMsmqEndpoint -->  
      <endpoint address="net.msmq://localhost/private/ServiceModelSamplesSession"  
                binding="netMsmqBinding"  
                contract="Microsoft.ServiceModel.Samples.IOrderTaker" />  
      ...  
    </service>  
  </services>  
  ...  
</system.serviceModel>  
```  
  
 O cliente cria um escopo de transação. Todas as mensagens na sessão são enviadas para a fila dentro do escopo da transação, fazendo com que ela seja tratada como uma unidade atômica onde todas as mensagens são bem sucedidas ou falham. A transação é confirmada chamando <xref:System.Transactions.TransactionScope.Complete%2A> .  

```csharp
//Create a transaction scope.  
using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))  
{  
    // Create a client with given client endpoint configuration.  
    OrderTakerClient client = new OrderTakerClient("OrderTakerEndpoint");  
    // Open a purchase order.  
    client.OpenPurchaseOrder("somecustomer.com");  
    Console.WriteLine("Purchase Order created");  
  
    // Add product line items.  
    Console.WriteLine("Adding 10 quantities of blue widget");  
    client.AddProductLineItem("Blue Widget", 10);  
  
    Console.WriteLine("Adding 23 quantities of red widget");  
    client.AddProductLineItem("Red Widget", 23);  
  
    // Close the purchase order.  
    Console.WriteLine("Closing the purchase order");  
    client.EndPurchaseOrder();  
  
    //Closing the client gracefully closes the connection and cleans up resources.  
    client.Close();
  
    // Complete the transaction.  
    scope.Complete();  
}  
```

> [!NOTE]
> Você pode usar apenas uma única transação para todas as mensagens na sessão e todas as mensagens na sessão devem ser enviadas antes de confirmar a transação. Fechar o cliente fecha a sessão. Portanto, o cliente precisa ser fechado antes que a transação seja concluída para enviar todas as mensagens na sessão para a fila.  
  
 Quando você executa o exemplo, as atividades de cliente e serviço são exibidas nas janelas do console do cliente e do serviço. Você pode ver o serviço receber mensagens do cliente. Pressione ENTER em cada janela do console para desligar o serviço e o cliente. Como o enfileiramento está em uso, o cliente e o serviço não precisam estar em funcionamento ao mesmo tempo. Você pode executar o cliente, desligá-lo e, em seguida, iniciar o serviço e ele ainda receberá suas mensagens.  
  
 No cliente.  
  
```console  
Purchase Order created  
Adding 10 quantities of blue widget  
Adding 23 quantities of red widget  
Closing the purchase order  
  
Press <ENTER> to terminate client.  
```  
  
 No serviço.  
  
```console  
The service is ready.  
Press <ENTER> to terminate service.  
  
Creating purchase order  
Product Blue Widget quantity 10 added to purchase order  
Product Red Widget quantity 23 added to purchase order  
Purchase Order Completed  
  
Purchase Order: 7c86fef0-2306-4c51-80e6-bcabcc1a6e5e  
        Customer: somecustomer.com  
        OrderDetails  
                Order LineItem: 10 of Blue Widget @unit price: $2985  
                Order LineItem: 23 of Red Widget @unit price: $156  
        Total cost of this order: $33438  
        Order status: Pending  
```  
  
### <a name="set-up-build-and-run-the-sample"></a>Configurar, compilar e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C#, C++ ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
 Por padrão, com a <xref:System.ServiceModel.NetMsmqBinding> segurança de transporte está habilitada. Há duas propriedades relevantes para a segurança de transporte do MSMQ, <xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A> e <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> `.` por padrão, o modo de autenticação é definido como `Windows` e o nível de proteção é definido como `Sign` . Para que o MSMQ forneça o recurso de autenticação e assinatura, ele deve fazer parte de um domínio e a opção de integração do Active Directory para o MSMQ deve ser instalada. Se você executar esse exemplo em um computador que não atenda a esses critérios, você receberá um erro.  
  
### <a name="run-the-sample-on-a-computer-joined-to-a-workgroup"></a>Executar o exemplo em um computador ingressado em um grupo de trabalho  
  
1. Se o seu computador não fizer parte de um domínio ou não tiver a integração do Active Directory instalada, desative a segurança de transporte definindo o modo de autenticação e o nível de proteção como `None` mostrado na seguinte configuração de exemplo.  
  
    ```xml  
    <system.serviceModel>  
      <services>  
        <service name="Microsoft.ServiceModel.Samples.OrderTakerService"  
                 behaviorConfiguration="OrderTakerServiceBehavior">  
          <host>  
            <baseAddresses>  
              <add baseAddress=  
             "http://localhost:8000/ServiceModelSamples/service"/>  
            </baseAddresses>  
          </host>  
          <!-- Define NetMsmqEndpoint -->  
          <endpoint  
              address=  
            "net.msmq://localhost/private/ServiceModelSamplesSession"  
              binding="netMsmqBinding"  
              bindingConfiguration="Binding1"  
           contract="Microsoft.ServiceModel.Samples.IOrderTaker" />  
          <!-- The mex endpoint is exposed at-->
          <!--http://localhost:8000/ServiceModelSamples/service/mex. -->  
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
            <behavior name="OrderTakerServiceBehavior">  
              <serviceMetadata httpGetEnabled="True"/>  
            </behavior>  
          </serviceBehaviors>  
        </behaviors>  
  
      </system.serviceModel>  
    ```  
  
2. Certifique-se de alterar a configuração no servidor e no cliente antes de executar o exemplo.  
  
    > [!NOTE]
    > Definir o modo de segurança como `None` é equivalente a definir <xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A> , <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> e `Message` segurança para `None` .  
