---
title: Sessão confiável de associação personalizada através de HTTPS
ms.date: 03/30/2017
ms.assetid: 16aaa80d-3ffe-47c4-8b16-ec65c4d25f8d
ms.openlocfilehash: ab2dd4725879ba969afdae8a6423a920a9786125
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585292"
---
# <a name="custom-binding-reliable-session-over-https"></a>Sessão confiável de associação personalizada através de HTTPS
Este exemplo demonstra o uso de segurança de transporte SSL com sessões confiáveis. As sessões confiáveis implementam o protocolo de mensagens WS-Reliable. Você pode ter uma sessão confiável segura, compondo o WS-Security em sessões confiáveis. Mas, às vezes, você pode optar por usar a segurança de transporte HTTP com SSL.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Custom\ReliableSessionOverHttps`  
  
## <a name="sample-details"></a>Detalhes de exemplo  
 O SSL garante que os próprios pacotes sejam protegidos. É importante observar que isso é diferente de proteger a sessão confiável usando a conversa WS-Secure.  
  
 Para usar uma sessão confiável via HTTPS, você deve criar uma associação personalizada. Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora. Uma associação personalizada é criada usando o elemento de associação de sessão confiável e o [\<httpsTransport>](../../configure-apps/file-schema/wcf/httpstransport.md) . A configuração a seguir é da associação personalizada.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service
          name="Microsoft.ServiceModel.Samples.CalculatorService"  
          behaviorConfiguration="CalculatorServiceBehavior">  
        <!-- use base address provided by host -->  
        <endpoint address=""  
                  binding="customBinding"  
                  bindingConfiguration="reliableSessionOverHttps"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <!-- the mex endpoint is exposed as http://localhost/servicemodelsamples/service.svc/mex-->  
        <endpoint address="mex"  
                  binding="mexHttpBinding"  
                  contract="IMetadataExchange"/>  
      </service>  
    </services>  
  
    <bindings>  
      <customBinding>  
        <binding name="reliableSessionOverHttps">  
          <reliableSession />  
          <httpsTransport />  
        </binding>  
      </customBinding>  
    </bindings>  
  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="CalculatorServiceBehavior">  
          <serviceMetadata httpGetEnabled="true" />  
          <serviceDebug includeExceptionDetailInFaults="False" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
  </system.serviceModel>  
  
</configuration>  
```  
  
 O código do programa no exemplo é idêntico ao do serviço de [introdução](getting-started-sample.md) . Você deve criar um certificado e atribuí-lo usando o assistente de certificado do servidor Web antes de compilar e executar o exemplo. A definição do ponto de extremidade e a definição de associação nas configurações do arquivo de configuração habilitam o uso de associação personalizada, conforme mostrado no exemplo de configuração a seguir para o cliente.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <system.serviceModel>  
  
    <client>  
      <!-- this endpoint has an https: address -->  
      <endpoint name=""  
                address="https://localhost/servicemodelsamples/service.svc"
                binding="customBinding"
                bindingConfiguration="reliableSessionOverHttps"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />  
    </client>  
  
      <bindings>  
        <customBinding>  
          <binding name="reliableSessionOverHttps">  
            <reliableSession />  
            <httpsTransport />  
          </binding>  
        </customBinding>
    </bindings>  
  
  </system.serviceModel>  
  
</configuration>  
```  
  
 O endereço especificado usa o `https://` esquema.  
  
 Como o certificado usado neste exemplo é um certificado de teste criado com MakeCert. exe, um alerta de segurança é exibido quando você tenta acessar um https: address, como `https://localhost/servicemodelsamples/service.svc` , no seu navegador. Para permitir que o cliente do Windows Communication Foundation (WCF) funcione com um certificado de teste em vigor, um código adicional foi adicionado ao cliente para suprimir o alerta de segurança. Esse código e a classe que o acompanham não são necessários ao usar certificados de produção.  

```csharp
// This code is required only for test certificates like those created by Makecert.exe.  
PermissiveCertificatePolicy.Enact("CN=ServiceModelSamples-HTTPS-Server");  
```

 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Instale o ASP.NET 4,0 usando o comando a seguir.  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
3. Verifique se você executou as [instruções de instalação do certificado do servidor serviços de informações da Internet (IIS)](iis-server-certificate-installation-instructions.md).  
  
4. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
5. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
