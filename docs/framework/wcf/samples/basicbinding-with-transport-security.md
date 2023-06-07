---
title: BasicBinding com segurança de transporte
ms.date: 03/30/2017
ms.assetid: f49b1de6-0254-4362-8ef2-fccd8ff9688b
ms.openlocfilehash: 822a7dcb20c6559a70ba77719b6e7a62633bb31c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575661"
---
# <a name="basicbinding-with-transport-security"></a>BasicBinding com segurança de transporte

Este exemplo demonstra o uso de segurança de transporte SSL com a associação básica. Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora.

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Basic\TransportSecurity`

## <a name="sample-details"></a>Detalhes de exemplo

Por padrão, a associação básica dá suporte à comunicação HTTP. O exemplo mostra como habilitar a segurança de transporte para a associação básica. Antes de executar o exemplo, você deve criar um certificado e atribuí-lo usando o assistente de certificado do servidor Web.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

O código do programa no exemplo é idêntico ao do serviço de [introdução](getting-started-sample.md) . A definição de associação e definição de ponto de extremidade nas configurações do arquivo de configuração são modificadas para habilitar a comunicação segura, conforme mostrado na seguinte configuração de exemplo.

```xml
<system.serviceModel>
  <services>
    <service type="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <endpoint address=""
                binding="basicHttpBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
   </services>
  <bindings>
    <basicHttpBinding>
      <!-- Configure basicHttpBinding with Transport security -->
      <!-- mode and clientCredentialType set to None. -->
      <binding name="Binding1">
        <security mode="Transport">
          <transport clientCredentialType="None"/>
        </security>
      </binding>
    </basicHttpBinding>
  </bindings>
</system.serviceModel>
```

Como o certificado usado neste exemplo é um certificado de teste criado com MakeCert. exe, um alerta de segurança é exibido quando você tenta acessar um endereço HTTPS: em seu navegador, como `https://localhost/servicemodelsamples/service.svc` . Para permitir que o cliente do Windows Communication Foundation (WCF) funcione com um certificado de teste, algum código adicional é adicionado ao cliente para suprimir o alerta de segurança. Esse código e a classe que o acompanham não são necessários ao usar certificados reais.

```csharp
// This code is required only for test certificates such as those
// created by Makecert.exe.
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

1. Instale o ASP.NET 4,0 usando o seguinte comando:

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

3. Verifique se você executou as [instruções de instalação do certificado do servidor serviços de informações da Internet (IIS)](iis-server-certificate-installation-instructions.md).

4. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

5. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).
