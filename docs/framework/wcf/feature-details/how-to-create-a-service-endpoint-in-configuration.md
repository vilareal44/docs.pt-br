---
title: Como criar um ponto de extremidade de serviço em configuração
description: Saiba como adicionar pontos de extremidade para um serviço WCF usando um arquivo de configuração que contém endereços relativos e absolutos.
ms.date: 06/16/2016
ms.assetid: f474e25d-2a27-4f31-84c5-395c442b8e70
ms.openlocfilehash: 184bcb5f7f3e83f12608757b55bbb4d57be58f7d
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247059"
---
# <a name="how-to-create-a-service-endpoint-in-configuration"></a>Como criar um ponto de extremidade de serviço em configuração
Os pontos de extremidade fornecem aos clientes acesso à funcionalidade oferecida por um serviço Windows Communication Foundation (WCF). Você pode definir um ou mais pontos de extremidade para um serviço usando uma combinação de endereços de ponto de extremidades relativos e absolutos, ou se você não definir pontos de extremidades de serviço, o tempo de execução fornece alguns por padrão para você. Este tópico mostra como adicionar pontos de extremidade usando um arquivo de configuração que contém endereços relativos e absolutos.  
  
## <a name="example"></a>Exemplo  
 A configuração de serviço a seguir especifica um endereço base e cinco pontos de extremidade.  
  
```xml  
<configuration>  
  
  <appSettings>  
    <!-- use appSetting to configure base address provided by host -->  
    <add key="baseAddress" value="http://localhost:8000/servicemodelsamples/service" />  
  </appSettings>  
  
  <system.serviceModel>  
    <services>  
    <!-- This section is optional with the default configuration introduced in .NET Framework 4. -->  
      <service name="Microsoft.ServiceModel.Samples.CalculatorService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>  
          </baseAddresses>  
        </host>  
        <endpoint address=""  
                  binding="wsHttpBinding"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <endpoint address="/test"  
                  binding="wsHttpBinding"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <endpoint address="http://localhost:8001/hello/servicemodelsamples"  
                  binding="wsHttpBinding"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <endpoint address="net.tcp://localhost:9000/servicemodelsamples/service"  
                  binding="netTcpBinding"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <!-- the mex endpoint is another relative address exposed at   
             http://localhost:8000/ServiceModelSamples/service/mex -->  
        <endpoint address="mex"  
                  binding="mexHttpBinding"  
                  contract="IMetadataExchange" />  
      </service>  
    </services>  
  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
  </system.serviceModel>  
  
</configuration>  
```  
  
## <a name="example"></a>Exemplo  
 O endereço base é especificado usando o `add` elemento, em Service/host/baseaddresss, conforme mostrado no exemplo a seguir.  
  
```xml  
<service
    name="Microsoft.ServiceModel.Samples.CalculatorService">  
  <host>  
    <baseAddresses>  
      <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>  
    </baseAddresses>  
  </host>  
```  
  
## <a name="example"></a>Exemplo  
 A primeira definição de ponto de extremidade mostrada no exemplo a seguir especifica um endereço relativo, o que significa que o endereço do ponto de extremidade é uma combinação do endereço base e do endereço relativo seguindo as regras de composição de Uniform Resource Identifier (URI). O endereço relativo está vazio (""), portanto, o endereço do ponto de extremidade é o mesmo que o endereço base. O endereço do ponto de extremidade real é `http://localhost:8000/servicemodelsamples/service` .  
  
```xml  
<endpoint address=""
    binding="wsHttpBinding"  
    contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
## <a name="example"></a>Exemplo  
 A segunda definição de ponto de extremidade também especifica um endereço relativo, conforme mostrado na seguinte configuração de exemplo. O endereço relativo, "Test", é anexado ao endereço base. O endereço do ponto de extremidade real é `http://localhost:8000/servicemodelsamples/service/test` .  
  
```xml  
<endpoint address="/test"  
    binding="wsHttpBinding"  
    contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
## <a name="example"></a>Exemplo  
 A terceira definição de ponto de extremidade especifica um endereço absoluto, conforme mostrado na seguinte configuração de exemplo. O endereço base não desempenha nenhuma função no endereço. O endereço do ponto de extremidade real é `http://localhost:8001/hello/servicemodelsamples` .  
  
```xml  
<endpoint address="http://localhost:8001/hello/servicemodelsamples"  
    binding="wsHttpBinding"  
    contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
## <a name="example"></a>Exemplo  
 O quarto endereço do ponto de extremidade especifica um endereço absoluto e um transporte diferente — TCP. O endereço base não desempenha nenhuma função no endereço. O endereço do ponto de extremidade real é net. TCP://localhost: 9000/servicemodelsamples/Service.  
  
```xml  
<endpoint address="net.tcp://localhost:9000/servicemodelsamples/service"  
    binding="netTcpBinding"  
    contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
## <a name="example"></a>Exemplo  
 Para usar os pontos de extremidade padrão fornecidos pelo tempo de execução, não especifique nenhum ponto de extremidade de serviço no código ou no arquivo de configuração. Neste exemplo, o tempo de execução cria os pontos de extremidade padrão quando o serviço é aberto. Para obter mais informações sobre pontos de extremidade, associações e comportamentos padrão, confira [Configuração simplificada](../simplified-configuration.md) e [Configuração simplificada para serviços WCF](../samples/simplified-configuration-for-wcf-services.md).  
  
```xml  
<configuration>  
  
  <appSettings>  
    <!-- use appSetting to configure base address provided by host -->  
    <add key="baseAddress" value="http://localhost:8000/servicemodelsamples/service" />  
  </appSettings>  
  
  <system.serviceModel>  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
  </system.serviceModel>  
  
</configuration>  
```
