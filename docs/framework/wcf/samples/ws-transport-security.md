---
title: Segurança de transporte WS
ms.date: 03/30/2017
ms.assetid: 33a20358-5e1b-458a-a6a9-15753bc7b99b
ms.openlocfilehash: d0f357ddcfc355bac8eeb86d57641add0013a052
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596391"
---
# <a name="ws-transport-security"></a>Segurança de transporte WS
Este exemplo demonstra o uso de segurança de transporte SSL com a <xref:System.ServiceModel.WSHttpBinding> associação. Por padrão, a `wsHttpBinding` associação fornece comunicação http. Quando configurado para segurança de transporte, a associação dá suporte à comunicação HTTPS. Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora. O `wsHttpBinding` é especificado e configurado nos arquivos de configuração do aplicativo para o cliente e o serviço.  
  
> [!NOTE]
> Os procedimentos de instalação e as instruções de compilação para esse exemplo estão localizadas no final deste tópico.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\wsTransportSecurity`  
  
 O código do programa no exemplo é idêntico ao do serviço de [introdução](getting-started-sample.md) . Você deve criar um certificado e atribuí-lo usando o assistente de certificado do servidor Web antes de compilar e executar o exemplo. A definição do ponto de extremidade e a definição de associação nas configurações do arquivo de configuração habilitam o `Transport` modo de segurança, conforme mostrado no exemplo de configuração a seguir para o cliente.  
  
```xml  
<system.serviceModel>  
  
    <client>  
      <!-- this endpoint has an https: address -->  
      <endpoint address="https://localhost/servicemodelsamples/service.svc" binding="wsHttpBinding" bindingConfiguration="Binding1" contract="Microsoft.Samples.TransportSecurity.ICalculator"/>  
    </client>  
  
    <bindings>  
      <wsHttpBinding>  
        <!-- configure wsHttpbinding with Transport security mode  
                   and clientCredentialType as None -->  
        <binding name="Binding1">  
          <security mode="Transport">  
            <transport clientCredentialType="None"/>  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
  
  </system.serviceModel>  
```  
  
 O endereço especificado usa o `https://` esquema. A configuração de associação define o modo de segurança como `Transport` . O mesmo modo de segurança deve ser especificado no arquivo Web. config do serviço.  
  
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
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Instale o ASP.NET 4,0 usando o comando a seguir.  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
3. Verifique se você executou as [instruções de instalação do certificado do servidor serviços de informações da Internet (IIS)](iis-server-certificate-installation-instructions.md).  
  
4. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
5. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
