---
title: Endereçando
ms.date: 03/30/2017
ms.assetid: d438e6f2-d0f3-43aa-b259-b51b5bda2e64
ms.openlocfilehash: 3221a12a21aebe20e0f6822554937623dc3fbb8d
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575960"
---
# <a name="addressing"></a>Endereçando
O exemplo de endereçamento demonstra vários aspectos e recursos de endereços de ponto de extremidade. O exemplo é baseado na [introdução](getting-started-sample.md). Neste exemplo, o serviço é auto-hospedado. O serviço e o cliente são aplicativos de console. O serviço define vários pontos de extremidade usando uma combinação de endereços de ponto de extremidades relativos e absolutos.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O arquivo de configuração de serviço especifica um endereço base e quatro pontos de extremidade. O endereço base é especificado usando o elemento add, em Service/host/baseAddresss, conforme demonstrado na configuração de exemplo a seguir.  
  
```xml  
<service name="Microsoft.ServiceModel.Samples.CalculatorService"  
         behaviorConfiguration="CalculatorServiceBehavior">  
  <host>  
    <baseAddresses>  
      <add baseAddress="http://localhost:8000/ServiceModelSamples/service" />  
    </baseAddresses>  
  </host>  
</service>  
```  
  
 A primeira definição de ponto de extremidade mostrada na seguinte configuração de exemplo especifica um endereço relativo, o que significa que o endereço do ponto de extremidade é uma combinação do endereço base e do endereço relativo seguindo as regras de composição de URI.  
  
```xml
<!-- Empty relative address specified:   
     use the base address provided by the host. -->  
<!-- The endpoint address is  
     http://localhost:8000/ServiceModelSamples/service. -->  
<endpoint address=""  
          binding="wsHttpBinding"  
          contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
 Nesse caso, o endereço relativo está vazio (""), portanto, o endereço do ponto de extremidade é o mesmo que o endereço base. O endereço do ponto de extremidade real é `http://localhost:8000/servicemodelsamples/service` .
  
 A segunda definição de ponto de extremidade também especifica um endereço relativo, conforme mostrado na seguinte configuração de exemplo.  
  
```xml  
<!-- The relative address specified: use the base address -->  
<!-- provided by the host + path. The endpoint address is -->  
<!-- http://localhost:8000/servicemodelsamples/service/test. -->  
<endpoint address="/test"  
          binding="wsHttpBinding"  
          contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
 O endereço relativo, "Test", é anexado ao endereço base. O endereço do ponto de extremidade real é `http://localhost:8000/servicemodelsamples/service/test` .
  
 A terceira definição de ponto de extremidade especifica um endereço absoluto, conforme mostrado na seguinte configuração de exemplo.  
  
```xml  
<endpoint address="http://localhost:8001/hello/servicemodelsamples"  
          binding="wsHttpBinding"  
          contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
 O endereço base não desempenha nenhuma função no endereço. O endereço do ponto de extremidade real é `http://localhost:8001/hello/servicemodelsamples` .
  
 O quarto endereço do ponto de extremidade especifica um endereço absoluto e um transporte diferente — TCP. O endereço base não desempenha nenhuma função no endereço. O endereço do ponto de extremidade real é `net.tcp://localhost:9000/servicemodelsamples/service` .
  
```xml  
<!-- The absolute address specified, different transport: -->  
<!-- use the specified address, and ignore the base address. -->  
<!-- The endpoint address is -->  
<!-- net.tcp://localhost:9000/servicemodelsamples/service. -->  
<endpoint address=  
          "net.tcp://localhost:9000/servicemodelsamples/service"  
          binding="netTcpBinding"  
          contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
 O cliente acessa apenas um dos quatro pontos de extremidade de serviço, mas todos os quatro são definidos em seu arquivo de configuração. O cliente seleciona um ponto de extremidade quando cria o `CalculatorProxy` objeto. Ao alterar o nome da configuração de `CalculatorEndpoint1` até o `CalculatorEndpoint4` , você pode exercitar cada um dos pontos de extremidade.  
  
 Quando você executa o exemplo, o serviço enumera o endereço, o nome da associação e o nome do contrato para cada um de seus pontos de extremidade. O ponto de extremidade de intercâmbio de metadados (MEX) é apenas outro ponto de extremidade da perspectiva do ServiceHost, então ele aparece na lista.  
  
```console  
Service endpoints:  
Endpoint - address:  http://localhost:8000/ServiceModelSamples/service  
           binding:  WSHttpBinding  
           contract: ICalculator  
Endpoint - address:  http://localhost:8000/ServiceModelSamples/service/test  
           binding:  WSHttpBinding  
           contract: ICalculator  
Endpoint - address:  http://localhost:8001/hello/servicemodelsamples  
           binding:  WSHttpBinding  
           contract: ICalculator  
Endpoint - address:  net.tcp://localhost:9000/servicemodelsamples/service  
           binding:  NetTcpBinding  
           contract: ICalculator  
Endpoint - address:  http://localhost:8000/ServiceModelSamples/service/mex  
           binding:  MetadataExchangeHttpBinding  
           contract: IMetadataExchange  
  
The service is ready.  
Press <ENTER> to terminate service.  
```  
  
 Quando você executa o cliente do, as solicitações de operação e as respostas são exibidas nas janelas do console do cliente e do serviço. Pressione ENTER em cada janela do console para desligar o serviço e o cliente.  
  
```console  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
    > [!NOTE]
    > Se você usar svcutil. exe para regenerar a configuração para este exemplo, certifique-se de modificar o nome do ponto de extremidade na configuração do cliente para corresponder ao código do cliente.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Addressing`  
