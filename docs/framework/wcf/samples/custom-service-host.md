---
title: Host de serviço personalizado
ms.date: 03/30/2017
ms.assetid: fe16ff50-7156-4499-9c32-13d8a79dc100
ms.openlocfilehash: 8302c3c829883da954d200526ca641eb4c169f98
ms.sourcegitcommit: 0edbeb66d71b8df10fcb374cfca4d731b58ccdb2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86052020"
---
# <a name="custom-service-host"></a>Host de serviço personalizado
Este exemplo demonstra como usar um derivado personalizado da <xref:System.ServiceModel.ServiceHost> classe para alterar o comportamento de tempo de execução de um serviço. Essa abordagem fornece uma alternativa reutilizável para configurar um grande número de serviços de maneira comum. O exemplo também demonstra como usar a <xref:System.ServiceModel.Activation.ServiceHostFactory> classe para usar um ServiceHost personalizado no ambiente de hospedagem serviços de informações da Internet (IIS) ou serviço de ativação de processos do Windows (was).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Hosting\CustomServiceHost`  
  
## <a name="about-the-scenario"></a>Sobre o cenário
 Para evitar a divulgação não intencional de metadados de serviço potencialmente confidenciais, a configuração padrão para Windows Communication Foundation (WCF) Services desabilita a publicação de metadados. Esse comportamento é seguro por padrão, mas também significa que você não pode usar uma ferramenta de importação de metadados (como Svcutil.exe) para gerar o código do cliente necessário para chamar o serviço, a menos que o comportamento de publicação de metadados do serviço esteja explicitamente habilitado na configuração.  
  
 A habilitação da publicação de metadados para um grande número de serviços envolve a adição dos mesmos elementos de configuração a cada serviço individual, o que resulta em uma grande quantidade de informações de configuração que é essencialmente a mesma. Como alternativa à configuração de cada serviço individualmente, é possível escrever o código imperativo que permite a publicação de metadados uma vez e, em seguida, reutilizar esse código em vários serviços diferentes. Isso é feito pela criação de uma nova classe que deriva de <xref:System.ServiceModel.ServiceHost> e substitui o `ApplyConfiguration` método () para adicionar imperativamente o comportamento de publicação de metadados.  
  
> [!IMPORTANT]
> Para maior clareza, este exemplo demonstra como criar um ponto de extremidade de publicação de metadados não seguro. Esses pontos de extremidade estão potencialmente disponíveis para consumidores anônimos não autenticados e devem ser levados em vida antes da implantação desses pontos de extremidade para garantir que os metadados de um serviço sejam desmarcados publicamente.  
  
## <a name="implementing-a-custom-servicehost"></a>Implementando um ServiceHost personalizado
 A <xref:System.ServiceModel.ServiceHost> classe expõe vários métodos virtuais úteis que os herdeiros podem substituir para alterar o comportamento de tempo de execução de um serviço. Por exemplo, o `ApplyConfiguration` método () lê as informações de configuração do serviço do repositório de configuração e altera o host de <xref:System.ServiceModel.Description.ServiceDescription> acordo. A implementação padrão lê a configuração do arquivo de configuração do aplicativo. Implementações personalizadas podem substituir `ApplyConfiguration` () para alterar melhor o <xref:System.ServiceModel.Description.ServiceDescription> uso do código imperativo ou até mesmo substituir totalmente o repositório de configuração padrão. Por exemplo, para ler a configuração de ponto de extremidade de um serviço de um banco de dados em vez do arquivo de configuração do aplicativo.  
  
 Neste exemplo, desejamos criar um ServiceHost personalizado que adiciona o ServiceMetadataBehavior (que permite a publicação de metadados) mesmo que esse comportamento não seja explicitamente adicionado ao arquivo de configuração do serviço. Para fazer isso, crie uma nova classe que herda de <xref:System.ServiceModel.ServiceHost> e substitui `ApplyConfiguration` ().  
  
```csharp
class SelfDescribingServiceHost : ServiceHost  
{  
    public SelfDescribingServiceHost(Type serviceType, params Uri[] baseAddresses)  
        : base(serviceType, baseAddresses) { }  
  
    //Overriding ApplyConfiguration() allows us to
    //alter the ServiceDescription prior to opening  
    //the service host.
    protected override void ApplyConfiguration()  
    {  
        //First, we call base.ApplyConfiguration()  
        //to read any configuration that was provided for  
        //the service we're hosting. After this call,  
        //this.Description describes the service  
        //as it was configured.  
        base.ApplyConfiguration();
  
        //(rest of implementation elided for clarity)  
    }  
}  
```  
  
 Como não queremos ignorar nenhuma configuração que tenha sido fornecida no arquivo de configuração do aplicativo, a primeira coisa que nossa substituição de `ApplyConfiguration` () é chamar a implementação de base. Depois que esse método é concluído, podemos adicionar imperativamente o <xref:System.ServiceModel.Description.ServiceMetadataBehavior> à descrição usando o código imperativo a seguir.  
  
```csharp
ServiceMetadataBehavior mexBehavior = this.Description.Behaviors.Find<ServiceMetadataBehavior>();  
if (mexBehavior == null)  
{  
    mexBehavior = new ServiceMetadataBehavior();  
    this.Description.Behaviors.Add(mexBehavior);  
}  
else  
{  
    //Metadata behavior has already been configured,
    //so we do not have any work to do.  
    return;  
}  
```  
  
 A última coisa que nossa `ApplyConfiguration` substituição () deve fazer é adicionar o ponto de extremidade de metadados padrão. Por convenção, um ponto de extremidade de metadados é criado para cada URI na coleção do BaseAddress do host de serviço.  
  
```csharp
//Add a metadata endpoint at each base address  
//using the "/mex" addressing convention  
foreach (Uri baseAddress in this.BaseAddresses)  
{  
    if (baseAddress.Scheme == Uri.UriSchemeHttp)  
    {  
        mexBehavior.HttpGetEnabled = true;  
        this.AddServiceEndpoint(ServiceMetadataBehavior.MexContractName,  
                                MetadataExchangeBindings.CreateMexHttpBinding(),  
                                "mex");  
    }  
    else if (baseAddress.Scheme == Uri.UriSchemeHttps)  
    {  
        mexBehavior.HttpsGetEnabled = true;  
        this.AddServiceEndpoint(ServiceMetadataBehavior.MexContractName,  
                                MetadataExchangeBindings.CreateMexHttpsBinding(),  
                                "mex");  
    }  
    else if (baseAddress.Scheme == Uri.UriSchemeNetPipe)  
    {  
        this.AddServiceEndpoint(ServiceMetadataBehavior.MexContractName,  
                                MetadataExchangeBindings.CreateMexNamedPipeBinding(),  
                                "mex");  
    }  
    else if (baseAddress.Scheme == Uri.UriSchemeNetTcp)  
    {  
        this.AddServiceEndpoint(ServiceMetadataBehavior.MexContractName,  
                                MetadataExchangeBindings.CreateMexTcpBinding(),  
                                "mex");  
    }  
}  
```  
  
## <a name="using-a-custom-servicehost-in-self-host"></a>Usando um ServiceHost personalizado no auto-host  
 Agora que concluímos nossa implementação de ServiceHost personalizada, podemos usá-la para adicionar o comportamento de publicação de metadados a qualquer serviço hospedando esse serviço dentro de uma instância do nosso `SelfDescribingServiceHost` . O código a seguir mostra como usá-lo no cenário de hospedagem interna.  
  
```csharp
SelfDescribingServiceHost host =
         new SelfDescribingServiceHost( typeof( Calculator ) );  
host.Open();  
```  
  
 Nosso host personalizado ainda lê a configuração de ponto de extremidade do serviço do arquivo de configuração do aplicativo, como se tivéssemos usado a <xref:System.ServiceModel.ServiceHost> classe padrão para hospedar o serviço. No entanto, como adicionamos a lógica para habilitar a publicação de metadados dentro de nosso host personalizado, não é mais necessário habilitar explicitamente o comportamento de publicação de metadados na configuração. Essa abordagem tem uma vantagem distinta quando você está criando um aplicativo que contém vários serviços e deseja habilitar a publicação de metadados em cada um deles sem gravar os mesmos elementos de configuração várias vezes.  
  
## <a name="using-a-custom-servicehost-in-iis-or-was"></a>Usando um ServiceHost personalizado no IIS ou WAS  
 Usar um host de serviço personalizado em cenários de hospedagem interna é simples, pois é o código do aplicativo que é, por fim, responsável pela criação e abertura da instância do host de serviço. No entanto, no ambiente IIS ou WAS de hospedagem, a infraestrutura do WCF está instanciando dinamicamente o host do serviço em resposta às mensagens de entrada. Os hosts de serviço personalizados também podem ser usados nesse ambiente de hospedagem, mas exigem um código adicional na forma de um ServiceHostFactory. O código a seguir mostra uma derivada de <xref:System.ServiceModel.Activation.ServiceHostFactory> que retorna instâncias de nosso personalizado `SelfDescribingServiceHost` .  
  
```csharp
public class SelfDescribingServiceHostFactory : ServiceHostFactory  
{  
    protected override ServiceHost CreateServiceHost(Type serviceType,
     Uri[] baseAddresses)  
    {  
        //All the custom factory does is return a new instance  
        //of our custom host class. The bulk of the custom logic should  
        //live in the custom host (as opposed to the factory)
        //for maximum  
        //reuse value outside of the IIS/WAS hosting environment.  
        return new SelfDescribingServiceHost(serviceType,
                                             baseAddresses);  
    }  
}  
```  
  
 Como você pode ver, a implementação de um ServiceHostFactory personalizado é simples. Toda a lógica personalizada reside dentro da implementação do ServiceHost; a fábrica retorna uma instância da classe derivada.  
  
 Para usar uma fábrica personalizada com uma implementação de serviço, devemos adicionar alguns metadados adicionais ao arquivo. svc do serviço.  
  
```aspx-csharp
<% @ServiceHost Service="Microsoft.ServiceModel.Samples.CalculatorService"
               Factory="Microsoft.ServiceModel.Samples.SelfDescribingServiceHostFactory"
               language=c# Debug="true" %>
```
  
 Aqui, adicionamos um `Factory` atributo adicional à `@ServiceHost` diretiva e passamos o nome do tipo CLR de nossa fábrica personalizada como o valor do atributo. Quando o IIS ou o recebe uma mensagem para esse serviço, a infraestrutura de hospedagem do WCF primeiro cria uma instância do ServiceHostFactory e, em seguida, instancia o próprio host do serviço chamando `ServiceHostFactory.CreateServiceHost()` .  
  
## <a name="running-the-sample"></a>Executando o exemplo  
 Embora este exemplo forneça uma implementação de cliente e de serviço totalmente funcional, o ponto do exemplo é ilustrar como alterar o comportamento de tempo de execução de um serviço por meio de um host personalizado. Execute as seguintes etapas:  
  
### <a name="observe-the-effect-of-the-custom-host"></a>Observe o efeito do host personalizado
  
1. Abra o arquivo de Web.config do serviço e observe que não há nenhuma configuração habilitando explicitamente os metadados para o serviço.  
  
2. Abra o arquivo. svc do serviço e observe que sua @ServiceHost diretiva contém um atributo de fábrica que especifica o nome de um ServiceHostFactory personalizado.  
  
### <a name="set-up-build-and-run-the-sample"></a>Configurar, compilar e executar o exemplo
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Depois que a solução tiver sido compilada, execute Setup.bat para configurar o aplicativo ServiceModelSamples no IIS 7,0. O diretório ServiceModelSamples agora deve aparecer como um aplicativo IIS 7,0.

4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

5. Para remover o aplicativo IIS 7,0, execute *Cleanup.bat*.

## <a name="see-also"></a>Confira também

- [Como hospedar um serviço WCF no IIS](../feature-details/how-to-host-a-wcf-service-in-iis.md)
