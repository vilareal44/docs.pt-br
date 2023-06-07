---
title: Comunicação em fila volátil
ms.date: 03/30/2017
ms.assetid: 0d012f64-51c7-41d0-8e18-c756f658ee3d
ms.openlocfilehash: a9f7e8a96fd293c7f87cc19846a42a42f28de288
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602330"
---
# <a name="volatile-queued-communication"></a>Comunicação em fila volátil

Este exemplo demonstra como executar comunicação volátil em fila no transporte do MSMQ (enfileiramento de mensagens). Este exemplo usa <xref:System.ServiceModel.NetMsmqBinding> . O serviço, nesse caso, é um aplicativo de console auto-hospedado para permitir que você observe o serviço que recebe mensagens enfileiradas.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

Na comunicação em fila, o cliente se comunica com o serviço usando uma fila. Mais precisamente, o cliente envia mensagens para uma fila. O serviço recebe mensagens da fila. O serviço e o cliente, portanto, não precisam estar em execução ao mesmo tempo para se comunicarem usando uma fila.

Quando você envia uma mensagem sem garantias, o MSMQ só faz um melhor esforço para entregar a mensagem, diferentemente das garantias em que o MSMQ garante que a mensagem seja entregue ou, se não puder ser entregue, permite que você saiba que a mensagem não pode ser entregue.

Em determinados cenários, talvez você queira enviar uma mensagem volátil sem garantias em uma fila, quando a entrega oportuna for mais importante do que a perda de mensagens. As mensagens voláteis não sobrevivem a falhas do Gerenciador de filas. Portanto, se o Gerenciador de filas falhar, a fila não transacional usada para armazenar mensagens voláteis sobreviverá, mas as mensagens não serão armazenadas no disco.

> [!NOTE]
> Não é possível enviar mensagens voláteis sem garantias dentro do escopo de uma transação usando o MSMQ. Você também deve criar uma fila não transacional para enviar mensagens voláteis.

O contrato de serviço neste exemplo é `IStockTicker` que define serviços unidirecionais que são mais adequados para uso com o enfileiramento.

```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]
public interface IStockTicker
{
    [OperationContract(IsOneWay = true)]
    void StockTick(string symbol, float price);
}
```

A operação de serviço exibe o preço e o símbolo da cotação de ações, conforme mostrado no código de exemplo a seguir:

```csharp
public class StockTickerService : IStockTicker
{
    public void StockTick(string symbol, float price)
    {
        Console.WriteLine("Stock Tick {0}:{1} ", symbol, price);
     }
     …
}
```

O serviço é hospedado internamente. Ao usar o transporte MSMQ, a fila usada deve ser criada com antecedência. Isso pode ser feito manualmente ou por meio de código. Neste exemplo, o serviço contém o código para verificar a existência da fila e criá-la, se necessário. O nome da fila é lido no arquivo de configuração. O endereço base é usado pela [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para gerar o proxy para o serviço.

```csharp
// Host the service within this EXE console application.
public static void Main()
{
    // Get MSMQ queue name from app settings in configuration.
    string queueName = ConfigurationManager.AppSettings["queueName"];

    // Create the transacted MSMQ queue if necessary.
    if (!MessageQueue.Exists(queueName))
        MessageQueue.Create(queueName);

    // Create a ServiceHost for the StockTickerService type.
    using (ServiceHost serviceHost = new ServiceHost(typeof(StockTickerService)))
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

O nome da fila MSMQ é especificado na seção appSettings do arquivo de configuração. O ponto de extremidade para o serviço é definido na seção System. serviceModel do arquivo de configuração e especifica a `netMsmqBinding` associação.

> [!NOTE]
> O nome da fila usa um ponto (.) para o computador local e separadores de barra invertida em seu caminho ao criar uma fila usando <xref:System.Messaging> . O endereço do ponto de extremidade do Windows Communication Foundation (WCF) especifica um net. MSMQ: esquema, usa "localhost" para o computador local e barras invertidas em seu caminho.

As garantias e durabilidade ou volatilidade de mensagens também são especificadas na configuração.

```xml
<appSettings>
  <!-- use appSetting to configure MSMQ queue name -->
  <add key="queueName" value=".\private$\ServiceModelSamplesVolatile" />
</appSettings>

<system.serviceModel>
  <services>
    <service name="Microsoft.ServiceModel.Samples.StockTickerService"
             behaviorConfiguration="CalculatorServiceBehavior">
    ...
      <!-- Define NetMsmqEndpoint -->
      <endpoint address="net.msmq://localhost/private/ServiceModelSamplesVolatile"
                binding="netMsmqBinding"
                bindingConfiguration="volatileBinding"
                contract="Microsoft.ServiceModel.Samples.IStockTicker" />
    ...
    </service>
  </services>

  <bindings>
    <netMsmqBinding>
      <binding name="volatileBinding"
             durable="false"
           exactlyOnce="false"/>
    </netMsmqBinding>
  </bindings>
  ...
</system.serviceModel>
```

Como o exemplo envia mensagens em fila usando uma fila não transacional, as mensagens transacionais não podem ser enviadas para a fila.

```csharp
// Create a client.
Random r = new Random(137);

StockTickerClient client = new StockTickerClient();

float price = 43.23F;
for (int i = 0; i < 10; i++)
{
    float increment = 0.01f * (r.Next(10));
    client.StockTick("zzz" + i, price + increment);
}

//Closing the client gracefully cleans up resources.
client.Close();
```

Quando você executa o exemplo, as atividades de cliente e serviço são exibidas nas janelas do console do cliente e do serviço. Você pode ver o serviço receber mensagens do cliente. Pressione ENTER em cada janela do console para desligar o serviço e o cliente. Observe que, como o enfileiramento está em uso, o cliente e o serviço não precisam estar em funcionamento ao mesmo tempo. Você pode executar o cliente, desligá-lo e, em seguida, iniciar o serviço e ele ainda receberá suas mensagens.

```console
The service is ready.
Press <ENTER> to terminate service.

Stock Tick zzz0:43.25
Stock Tick zzz1:43.23
Stock Tick zzz2:43.28
Stock Tick zzz3:43.3
Stock Tick zzz4:43.23
Stock Tick zzz5:43.25
Stock Tick zzz6:43.25
Stock Tick zzz7:43.24
Stock Tick zzz8:43.32
Stock Tick zzz9:43.3
```

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

Por padrão, com a <xref:System.ServiceModel.NetMsmqBinding> segurança de transporte está habilitada. Há duas propriedades pertinentes para a segurança de transporte do MSMQ <xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A> e, <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> `.` por padrão, o modo de autenticação é definido como `Windows` e o nível de proteção é definido como `Sign` . Para que o MSMQ forneça o recurso de autenticação e assinatura, ele deve fazer parte de um domínio e a opção de integração do Active Directory para o MSMQ deve ser instalada. Se você executar esse exemplo em um computador que não atenda a esses critérios, receberá um erro.

### <a name="to-run-the-sample-on-a-computer-joined-to-a-workgroup-or-without-active-directory-integration"></a>Para executar o exemplo em um computador ingressado em um grupo de trabalho ou sem integração com o Active Directory

1. Se o seu computador não fizer parte de um domínio ou não tiver a integração do Active Directory instalada, desative a segurança de transporte definindo o modo de autenticação e o nível de proteção como `None` mostrado no código de configuração de exemplo a seguir:

    ```xml
    <system.serviceModel>
        <services>
          <service name="Microsoft.ServiceModel.Samples.StockTickerService"
                   behaviorConfiguration="StockTickerServiceBehavior">
            <host>
              <baseAddresses>
                <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>
              </baseAddresses>
            </host>

            <!-- Define NetMsmqEndpoint -->
            <endpoint address="net.msmq://localhost/private/ServiceModelSamplesVolatile"
                      binding="netMsmqBinding"
                      bindingConfiguration="volatileBinding"
                      contract="Microsoft.ServiceModel.Samples.IStockTicker" />
            <!-- the mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex -->
            <endpoint address="mex"
                      binding="mexHttpBinding"
                      contract="IMetadataExchange" />

          </service>
        </services>

        <bindings>
          <netMsmqBinding>
            <binding name="volatileBinding"
                  durable="false"
                  exactlyOnce="false">
              <security mode="None" />
            </binding>
          </netMsmqBinding>
        </bindings>

        <behaviors>
          <serviceBehaviors>
            <behavior name="StockTickerServiceBehavior">
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\MSMQ\Volatile`
