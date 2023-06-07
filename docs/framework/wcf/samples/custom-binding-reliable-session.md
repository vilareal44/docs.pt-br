---
title: Sessão confiável de associação personalizada
ms.date: 03/30/2017
ms.assetid: c5fcd409-246f-4f3e-b3f1-629506ca4c04
ms.openlocfilehash: bd690f96eea885c4d414f9725125e1918fdffa23
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585137"
---
# <a name="custom-binding-reliable-session"></a>Sessão confiável de associação personalizada

Uma associação personalizada é definida por uma lista ordenada de elementos de associação discretos. Este exemplo demonstra como configurar uma associação personalizada com vários elementos de codificação de mensagens e transporte, especialmente permitindo sessões confiáveis.

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Custom\ReliableSession`

## <a name="sample-details"></a>Detalhes de exemplo

As sessões confiáveis fornecem recursos para mensagens e sessões confiáveis. A Reliable Messaging repete a comunicação em caso de falha e permite que as garantias de entrega, como a chegada da ordem das mensagens a serem especificadas. As sessões mantêm o estado para clientes entre chamadas. O exemplo implementa sessões para manter o estado do cliente e especifica garantias de entrega em ordem. O exemplo se baseia na [introdução](getting-started-sample.md) que implementa um serviço de calculadora. Os recursos de sessão confiáveis são habilitados e configurados nos arquivos de configuração do aplicativo para o cliente e o serviço.

> [!NOTE]
> Os procedimentos de instalação e as instruções de compilação para esse exemplo estão localizadas no final deste tópico.

A ordenação de elementos de associação é importante na definição de uma associação personalizada, pois cada uma representa uma camada na pilha de canais (consulte [associações personalizadas](../extending/custom-bindings.md)).

A configuração de serviço para o exemplo é definida conforme mostrado no exemplo de código a seguir.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.CalculatorService"
          behaviorConfiguration="CalculatorServiceBehavior">
        <!-- This endpoint is exposed at the base address provided by host: http://localhost/servicemodelsamples/service.svc  -->
        <!-- specify customBinding binding and a binding configuration to use -->
        <endpoint address=""
                  binding="customBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
        <!-- The mex endpoint is exposed at http://localhost/servicemodelsamples/service.svc/mex -->
        <endpoint address="mex"
                  binding="mexHttpBinding"
                  contract="IMetadataExchange" />
      </service>
    </services>

    <!-- custom binding configuration - configures HTTP transport, reliable sessions -->
    <bindings>
      <customBinding>
        <binding name="Binding1">
          <reliableSession />
          <security authenticationMode="SecureConversation"
                     requireSecurityContextCancellation="true">
          </security>
          <compositeDuplex />
          <oneWay />
          <textMessageEncoding messageVersion="Soap12WSAddressing10" writeEncoding="utf-8" />
          <httpTransport authenticationScheme="Anonymous" bypassProxyOnLocal="false"
                        hostNameComparisonMode="StrongWildcard"
                        proxyAuthenticationScheme="Anonymous" realm=""
                        useDefaultWebProxy="true" />
        </binding>
      </customBinding>
    </bindings>

    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceMetadata httpGetEnabled="True"/>
          <serviceDebug includeExceptionDetailInFaults="False" />
        </behavior>
      </serviceBehaviors>
    </behaviors>

  </system.serviceModel>

</configuration>
```

Ao executar em um cenário entre máquinas, você deve alterar o endereço do ponto de extremidade do cliente para refletir o nome do host do serviço.

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

3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

    > [!IMPORTANT]
    > Ao executar o cliente em uma configuração entre computadores, certifique-se de substituir "localhost" no `address` atributo do [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-element.md) elemento e no `clientBaseAddress` atributo de [\<compositeDuplex>](../../configure-apps/file-schema/wcf/compositeduplex.md) pelo nome do computador apropriado, conforme mostrado no exemplo a seguir.

    ```xml
    <endpoint name = ""
    address="http://service_machine_name/servicemodelsamples/service.svc" />
    <compositeDuplex clientBaseAddress="http://client_machine_name:8000/myClient/" />
    ```
