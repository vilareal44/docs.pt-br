---
title: Ativação de MSMQ
ms.date: 03/30/2017
ms.assetid: e3834149-7b8c-4a54-806b-b4296720f31d
ms.openlocfilehash: 0dbd24a612d56c0fe88066f625be2a8369b7df5b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602527"
---
# <a name="msmq-activation"></a>Ativação de MSMQ

Este exemplo demonstra como hospedar aplicativos no WAS (serviço de ativação de processos do Windows) que são lidos de uma fila de mensagens. Este exemplo usa o `netMsmqBinding` e é baseado no exemplo de [comunicação bidirecional](two-way-communication.md) . O serviço, nesse caso, é um aplicativo hospedado na Web e o cliente é hospedado internamente e saídas para o console para observar o status dos pedidos de compra enviados.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

> [!NOTE]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> \<InstallDrive>: \ WF_WCF_Samples
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os WCF e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> \<InstallDrive>:\Samples\WCFWFCardSpace\WCF\Basic\Services\Hosting\WASHost\MsmqActivation.

O WAS (serviço de ativação de processos do Windows), o novo mecanismo de ativação de processos do Windows Server 2008, fornece recursos semelhantes ao IIS que estavam disponíveis anteriormente apenas para aplicativos baseados em HTTP para aplicativos que usam protocolos não-HTTP. O Windows Communication Foundation (WCF) usa a interface do adaptador do ouvinte para comunicar solicitações de ativação recebidas em protocolos não HTTP com suporte do WCF, como TCP, pipes nomeados e MSMQ. A funcionalidade para receber solicitações em protocolos não HTTP é hospedada por serviços gerenciados do Windows em execução no SMSvcHost. exe.

O serviço de adaptador de escuta net. MSMQ (NetMsmqActivator) ativa os aplicativos em fila com base nas mensagens na fila.

O cliente envia pedidos de compra para o serviço de dentro do escopo de uma transação. O serviço recebe os pedidos em uma transação e os processa. Em seguida, o serviço chama o cliente com o status do pedido. Para facilitar a comunicação bidirecional, o cliente e o serviço usam filas para enfileirar ordens de compra e status do pedido.

O contrato de serviço `IOrderProcessor` define as operações de serviço unidirecionais que funcionam com o enfileiramento. A operação de serviço usa o ponto de extremidade de resposta para enviar status do pedido para o cliente. O endereço do ponto de extremidade de resposta é o URI da fila usada para enviar o status do pedido de volta para o cliente. O aplicativo de processamento de pedidos implementa esse contrato.

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface IOrderProcessor
{
    [OperationContract(IsOneWay = true)]
    void SubmitPurchaseOrder(PurchaseOrder po,
                                           string reportOrderStatusTo);
}
```

O contrato de resposta para o qual enviar o status do pedido é especificado pelo cliente. O cliente implementa o contrato de status do pedido. O serviço usa o cliente gerado deste contrato para enviar o status do pedido de volta para o cliente.

```csharp
[ServiceContract]
public interface IOrderStatus
{
    [OperationContract(IsOneWay = true)]
    void OrderStatus(string poNumber, string status);
}
```

A operação de serviço processa a ordem de compra enviada. O <xref:System.ServiceModel.OperationBehaviorAttribute> é aplicado à operação de serviço para especificar a inscrição automática na transação que é usada para receber a mensagem da fila e a conclusão automática da transação na conclusão da operação de serviço. A `Orders` classe encapsula a funcionalidade de processamento de pedidos. Nesse caso, ele adiciona a ordem de compra a um dicionário. A transação na qual a operação de serviço inscrito está disponível para as operações na `Orders` classe.

A operação de serviço, além de processar a ordem de compra enviada, responde de volta ao cliente sobre o status do pedido.

```csharp
public class OrderProcessorService : IOrderProcessor
{
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    public void SubmitPurchaseOrder(PurchaseOrder po, string reportOrderStatusTo)
    {
        Orders.Add(po);
        Console.WriteLine("Processing {0} ", po);
        Console.WriteLine("Sending back order status information");
        NetMsmqBinding msmqCallbackBinding = new NetMsmqBinding();
        msmqCallbackBinding.Security.Mode = NetMsmqSecurityMode.None;
        OrderStatusClient client = new OrderStatusClient(msmqCallbackBinding, new EndpointAddress(reportOrderStatusTo));
        // please note that the same transaction that is used to dequeue purchase order is used
        // to send back order status
        using (TransactionScope scope = new TransactionScope(TransactionScopeOption.Required))
        {
            client.OrderStatus(po.PONumber, po.Status);
            scope.Complete();
        }
    }
}
```

A associação de cliente a ser usada é especificada usando um arquivo de configuração.

O nome da fila MSMQ é especificado em uma seção appSettings do arquivo de configuração. O ponto de extremidade para o serviço é definido na seção System. serviceModel do arquivo de configuração.

> [!NOTE]
> O nome da fila MSMQ e o endereço do ponto de extremidade usam convenções de endereçamento ligeiramente diferentes. O nome da fila MSMQ usa um ponto (.) para o computador local e separadores de barra invertida em seu caminho. O endereço do ponto de extremidade do WCF especifica um net. MSMQ: esquema, usa "localhost" para o computador local e usa barras invertidas em seu caminho. Para ler de uma fila que está hospedada no computador remoto, substitua "." e "localhost" pelo nome do computador remoto.

Um arquivo. svc com o nome da classe é usado para hospedar o código do serviço no WAS.

O próprio arquivo Service. svc contém uma diretiva para criar o `OrderProcessorService` .

`<%@ServiceHost language="c#" Debug="true" Service="Microsoft.ServiceModel.Samples.OrderProcessorService"%>`

O arquivo Service. svc também contém uma diretiva de assembly para garantir que System. Transactions. dll seja carregado.

`<%@Assembly name="System.Transactions, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"%>`

O cliente cria um escopo de transação. A comunicação com o serviço ocorre dentro do escopo da transação, fazendo com que ele seja tratado como uma unidade atômica onde todas as mensagens são bem sucedidas ou falham. A transação é confirmada chamando `Complete` o escopo da transação.

```csharp
using (ServiceHost serviceHost = new ServiceHost(typeof(OrderStatusService)))
{
    // Open the ServiceHostBase to create listeners and start listening
    // for order status messages.
    serviceHost.Open();

    // Create a proxy with given client endpoint configuration
    OrderProcessorClient client = new OrderProcessorClient();

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
    using (TransactionScope scope = new
        TransactionScope(TransactionScopeOption.Required))
    {
        // Make a queued call to submit the purchase order
        client.SubmitPurchaseOrder(po,
       "net.msmq://localhost/private/ServiceModelSamplesOrder/OrderStatus");
        // Complete the transaction.
        scope.Complete();
    }

    //Closing the client gracefully closes the connection and cleans up
    //resources
    client.Close();

    Console.WriteLine();
    Console.WriteLine("Press <ENTER> to terminate client.");
    Console.ReadLine();

    // Close the ServiceHostBase to shutdown the service.
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
        Console.WriteLine("Status of order {0}:{1} ",
                                         poNumber , status);
    }
}
```

A fila status do pedido é criada no `Main` método. A configuração do cliente inclui a configuração do serviço de status do pedido para hospedar o serviço de status do pedido, conforme mostrado na seguinte configuração de exemplo.

```xml
<appSettings>
    <!-- use appSetting to configure MSMQ queue name -->
    <add key="targetQueueName" value=".\private$\ServiceModelSamples/service.svc" />
    <add key="responseQueueName" value=".\private$\ServiceModelSamples/OrderStatus" />
  </appSettings>

<system.serviceModel>

    <services>
      <service
         name="Microsoft.ServiceModel.Samples.OrderStatusService">
        <!-- Define NetMsmqEndpoint -->
        <endpoint address="net.msmq://localhost/private/ServiceModelSamples/OrderStatus"
                  binding="netMsmqBinding"
                  contract="Microsoft.ServiceModel.Samples.IOrderStatus" />
      </service>
    </services>

    <client>
      <!-- Define NetMsmqEndpoint -->
      <endpoint name="OrderProcessorEndpoint"
                address="net.msmq://localhost/private/ServiceModelSamples/service.svc"
                binding="netMsmqBinding"
                contract="Microsoft.ServiceModel.Samples.IOrderProcessor" />
    </client>

  </system.serviceModel>
```

Quando você executa o exemplo, as atividades de cliente e serviço são exibidas nas janelas do console do servidor e do cliente. Você pode ver o servidor receber mensagens do cliente. Pressione ENTER em cada janela do console para desligar o servidor e o cliente.

O cliente exibe as informações de status do pedido enviadas pelo servidor:

```console
Press <ENTER> to terminate client.
Status of order 70cf9d63-3dfa-4e69-81c2-23aa4478ebed :Pending
```

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se o IIS 7,0 está instalado, pois ele é necessário para a ativação do WAS.

2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md). Além disso, você deve instalar os componentes de ativação não HTTP do WCF:

    1. No menu **Iniciar**, selecione **Painel de Controle**.

    2. Selecione **programas e recursos**.

    3. Clique em **Ativar ou desativar recursos do Windows**.

    4. Em **Resumo de recursos**, clique em **Adicionar recursos**.

    5. Expanda o nó **Microsoft .NET Framework 3,0** e verifique o Windows Communication Foundation recurso de **ativação não http** .

3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Execute o cliente executando o Client. exe em uma janela de comando. Isso cria a fila e envia uma mensagem a ela. Deixe o cliente em execução para ver o resultado do serviço que está lendo a mensagem

5. Por padrão, o serviço de ativação MSMQ é executado como serviço de rede. Portanto, a fila usada para ativar o aplicativo deve ter permissões RECEIVE e Peek para o serviço de rede. Isso pode ser adicionado usando o MMC do enfileiramento de mensagens:

    1. No menu **Iniciar** , clique em **executar**, digite `Compmgmt.msc` e pressione Enter.

    2. Em **serviços e aplicativos**, expanda **enfileiramento de mensagens**.

    3. Clique em **filas particulares**.

    4. Clique com o botão direito do mouse na fila (servicemodelsamples/Service. svc) e escolha **Propriedades**.

    5. Na guia **segurança** , clique em **Adicionar** e dê permissões de Peek e recebimento ao serviço de rede.

6. Configure o WAS (serviço de ativação de processos do Windows) para dar suporte à ativação do MSMQ.

    Como uma conveniência, as etapas a seguir são implementadas em um arquivo em lotes chamado AddMsmqSiteBinding. cmd localizado no diretório de exemplo.

    1. Para dar suporte à ativação do NET. MSMQ, o site padrão deve primeiro ser associado ao protocolo net. MSMQ. Isso pode ser feito usando o Appcmd. exe, que é instalado com o conjunto de ferramentas de gerenciamento do IIS 7,0. Em um prompt de comando elevado (administrador), execute o comando a seguir.

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
        -+bindings.[protocol='net.msmq',bindingInformation='localhost']
        ```

        > [!NOTE]
        > Esse comando é uma única linha de texto.

        Esse comando adiciona uma associação de site net. MSMQ ao site da Web padrão.

    2. Embora todos os aplicativos em um site compartilhem uma associação net. MSMQ comum, cada aplicativo pode habilitar o suporte net. MSMQ individualmente. Para habilitar net. MSMQ para o aplicativo/servicemodelsamples, execute o comando a seguir em um prompt de comando elevado.

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http,net.msmq
        ```

        > [!NOTE]
        > Esse comando é uma única linha de texto.

        Esse comando permite que o aplicativo/servicemodelsamples seja acessado usando `http://localhost/servicemodelsamples` e `net.msmq://localhost/servicemodelsamples` .

7. Se você não tiver feito isso anteriormente, verifique se o serviço de ativação MSMQ está habilitado. No menu **Iniciar** , clique em **executar**e digite `Services.msc` . Pesquise a lista de serviços para o **adaptador de escuta net. MSMQ**. Clique com o botão direito do mouse e selecione **Propriedades**. Defina o **tipo de inicialização** como **automático**, clique em **aplicar** e clique no botão **Iniciar** . Essa etapa deve ser feita apenas uma vez antes do primeiro uso do serviço do adaptador de escuta net. MSMQ.

8. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md). Além disso, altere o código no cliente que envia a ordem de compra para refletir o nome do computador no URI da fila ao enviar a ordem de compra. Use o seguinte código:

    ```csharp
    client.SubmitPurchaseOrder(po, "net.msmq://localhost/private/ServiceModelSamples/OrderStatus");
    ```

9. Remova a associação de site net. MSMQ que você adicionou para este exemplo.

    Como uma conveniência, as etapas a seguir são implementadas em um arquivo em lotes chamado RemoveMsmqSiteBinding. cmd localizado no diretório de exemplo:

    1. Remova net. MSMQ da lista de protocolos habilitados executando o comando a seguir em um prompt de comandos com privilégios elevados.

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http
        ```

        > [!NOTE]
        > Esse comando é uma única linha de texto.

    2. Remova a associação de site net. MSMQ executando o comando a seguir em um prompt de comando elevado.

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" --bindings.[protocol='net.msmq',bindingInformation='localhost']
        ```

        > [!NOTE]
        > Esse comando é uma única linha de texto.

    > [!WARNING]
    > A execução do arquivo em lotes redefinirá o DefaultAppPool para ser executado usando .NET Framework versão 2,0.

Por padrão, com o `netMsmqBinding` transporte de associação, a segurança é habilitada. Duas propriedades `MsmqAuthenticationMode` e `MsmqProtectionLevel` , juntas, determinam o tipo de segurança de transporte. Por padrão, o modo de autenticação é definido como `Windows` e o nível de proteção é definido como `Sign` . Para que o MSMQ forneça o recurso de autenticação e assinatura, ele deve fazer parte de um domínio. Se você executar esse exemplo em um computador que não faz parte de um domínio, o seguinte erro será recebido: "certificado interno do serviço de enfileiramento de mensagens não existe".

### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup"></a>Para executar o exemplo em um computador ingressado em um grupo de trabalho

1. Se o computador não fizer parte de um domínio, desative a segurança de transporte definindo o modo de autenticação e o nível de proteção como nenhum, conforme mostrado na seguinte configuração de exemplo.

    ```xml
    <bindings>
        <netMsmqBinding>
            <binding configurationName="TransactedBinding">
                <security mode="None"/>
            </binding>
        </netMsmqBinding>
    </bindings>
    ```

2. Altere a configuração no servidor e no cliente antes de executar o exemplo.

    > [!NOTE]
    > A configuração `security mode` como `None` é equivalente à `MsmqAuthenticationMode` configuração `MsmqProtectionLevel` e `Message` à segurança para `None` .

3. Para habilitar a ativação em um computador ingressado em um grupo de trabalho, o serviço de ativação e o processo de trabalho devem ser executados com uma conta de usuário específica (deve ser o mesmo para ambos) e a fila deve ter ACLs para a conta de usuário específica.

     Para alterar a identidade sob a qual o processo de trabalho é executado:

    1. Execute inetmgr. exe.

    2. Em **pools de aplicativos**, clique com o botão direito do mouse no **AppPool** (normalmente **DefaultAppPool**) e escolha **definir padrões do pool de aplicativos...**.

    3. Altere as propriedades de identidade para usar a conta de usuário específica.

     Para alterar a identidade em que o serviço de ativação é executado:

    1. Execute Services. msc.

    2. Clique com o botão direito do mouse no **adaptador net. MsmqListener**e escolha **Propriedades**.

4. Altere a conta na guia **logon** .

5. No grupo de trabalho, o serviço também deve ser executado usando um token irrestrito. Para fazer isso, execute o seguinte em uma janela de comando:

    ```console
    sc sidtype netmsmqactivator unrestricted
    ```

## <a name="see-also"></a>Consulte também

- [Hospedagem de AppFabric e persistência Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383418(v=azure.10))
