---
title: WSHttpBinding
ms.date: 03/30/2017
helpviewer_keywords:
- WS Profile binding
ms.assetid: 22d85b19-0135-4141-9179-a0e9c343ad73
ms.openlocfilehash: a78eac52095d3f647efdacc9104a75e46651f389
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596365"
---
# <a name="wshttpbinding"></a>WSHttpBinding
Este exemplo demonstra como implementar um serviço típico e um cliente típico usando Windows Communication Foundation (WCF). Este exemplo consiste em um programa de console do cliente (Client. exe) e uma biblioteca de serviços hospedado pelo Serviços de Informações da Internet (IIS). O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta. O contrato é definido pela `ICalculator` interface, que expõe operações matemáticas (adicionar, subtrair, multiplicar e dividir). O cliente faz solicitações síncronas para uma determinada operação matemática e o serviço responde com o resultado. A atividade do cliente fica visível na janela do console.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\wsHttp`  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Este exemplo expõe o `ICalculator` contrato usando o [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) . A configuração dessa associação foi expandida no arquivo Web. config.  
  
```xml
<bindings>  
  <wsHttpBinding>  
    <!--The following is the expanded configuration section for a-->  
    <!--WSHttpBinding. Each property is configured with the default-->
    <!--value. See the ReliableSession, TransactionFlow, -->  
    <!--TransportSecurity, and MessageSecurity samples in the WS -->  
    <!--directory to learn how to configure these features. -->  
    <binding name="Binding1"  
              bypassProxyOnLocal="false"
              transactionFlow="false"
              hostNameComparisonMode="StrongWildcard"  
              maxBufferPoolSize="524288"
              maxReceivedMessageSize="65536"  
              messageEncoding="Text"
              textEncoding="utf-8"
              useDefaultWebProxy="true"  
              allowCookies="false">  
      <reliableSession ordered="true"
                       inactivityTimeout="00:10:00"  
                       enabled="false" />  
      <security mode="Message">  
        <message clientCredentialType="Windows"
                 negotiateServiceCredential="true"  
                 algorithmSuite="Default"
                 establishSecurityContext="true" />  
      </security>  
    </binding>  
  </wsHttpBinding>  
</bindings>  
```  
  
 No elemento base `binding` , o `maxReceivedMessageSize` valor permite que você configure o tamanho máximo de uma mensagem de entrada (em bytes). O `hostNameComparisonMode` valor permite que você configure se o nome de host é considerado ao Desmultiplexar mensagens para o serviço. O `messageEncoding` valor permite que você configure se deseja usar a codificação de texto ou MTOM para mensagens. O `textEncoding` valor permite configurar a codificação de caracteres para mensagens. O `bypassProxyOnLocal` valor permite que você configure se deseja usar um proxy http para comunicação local. O `transactionFlow` valor define se a transação atual está fluindo (se uma operação estiver configurada para o fluxo de transações).  
  
 No [\<reliableSession>](../../configure-apps/file-schema/wcf/reliablesession.md) elemento, o valor booliano habilitado define se as sessões confiáveis estão habilitadas. O `ordered` valor define se a ordenação da mensagem é preservada. O `inactivityTimeout` valor configura quanto tempo uma sessão pode ficar ociosa antes de ter falhado.  
  
 No [\<security>](../../configure-apps/file-schema/wcf/security-of-wshttpbinding.md) , o `mode` valor configura qual modo de segurança deve ser usado. Neste exemplo, a segurança das mensagens está sendo usada, motivo pelo qual o [\<message>](../../configure-apps/file-schema/wcf/message-of-wshttpbinding.md) é especificado dentro do [\<security>](../../configure-apps/file-schema/wcf/security-of-wshttpbinding.md) .  
  
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
  
3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
