---
title: Self-Host
ms.date: 03/30/2017
helpviewer_keywords:
- Self hosted service
- Self Host Sample [Windows Communication Foundation]
ms.assetid: 05e68661-1ddf-4abf-a899-9bb1b8272a5b
ms.openlocfilehash: f5c46bc486e03cf86ada3a565a3c282cd81db286
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599939"
---
# <a name="self-host"></a>Self-Host
Este exemplo demonstra como implementar um serviço hospedado automaticamente em um aplicativo de console. Este exemplo é baseado na [introdução](getting-started-sample.md). O arquivo de configuração de serviço foi renomeado de Web. config para app. config e foi modificado para configurar um endereço base, que o host usa. O código-fonte do serviço foi modificado para implementar uma `Main` função estática que cria e abre um host de serviço que fornece o endereço base configurado. A implementação do serviço foi modificada para gravar a saída no console para cada operação. O cliente não foi modificado, exceto para configurar o endereço do ponto de extremidade correto do serviço.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O exemplo implementa uma função principal estática para criar um <xref:System.ServiceModel.ServiceHost> para o `CalculatorService` tipo fornecido, conforme mostrado no código de exemplo a seguir.  
  
```csharp
// Host the service within this EXE console application.  
public static void Main()  
{  
    // Create a ServiceHost for the CalculatorService type.  
    using (ServiceHost serviceHost =
           new ServiceHost(typeof(CalculatorService)))  
    {  
        // Open the ServiceHost to create listeners
        // and start listening for messages.  
        serviceHost.Open();  
  
        // The service can now be accessed.  
        Console.WriteLine("The service is ready.");  
        Console.WriteLine("Press <ENTER> to terminate service.");  
        Console.WriteLine();  
        Console.ReadLine();  
    }  
}  
```  
  
 Quando um serviço é hospedado no Serviços de Informações da Internet (IIS) ou no WAS (serviço de ativação de processos do Windows), o endereço base do serviço é fornecido pelo ambiente de hospedagem. No caso do auto-hospedado, você deve especificar o endereço base por conta própria. Isso é feito usando o `add` elemento, filho de [\<baseAddresses>](../../configure-apps/file-schema/wcf/baseaddresses.md) , filho de [\<host>](../../configure-apps/file-schema/wcf/host.md) , filho de, [\<service>](../../configure-apps/file-schema/wcf/service.md) conforme demonstrado na configuração de exemplo a seguir.  
  
```xml  
<service
    name="Microsoft.ServiceModel.Samples.CalculatorService"  
    behaviorConfiguration="CalculatorServiceBehavior">  
  <host>  
    <baseAddresses>  
      <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>  
    </baseAddresses>  
  </host>  
  ...  
</service>  
```  
  
 Quando você executa o exemplo, as solicitações e respostas da operação são exibidas nas janelas do console do cliente e do serviço. Pressione ENTER em cada janela do console para desligar o serviço e o cliente.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\SelfHost`  
  
## <a name="see-also"></a>Consulte também

- [Hospedagem de AppFabric e persistência Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383418(v=azure.10))
