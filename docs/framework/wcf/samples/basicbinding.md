---
title: BasicBinding
ms.date: 03/30/2017
ms.assetid: 86fbeb87-4d89-4b61-9577-867e0ac12945
ms.openlocfilehash: 84bfe78aa9e82b9600c48e0a32514f669fcc7d77
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575648"
---
# <a name="basicbinding"></a>BasicBinding

Este exemplo demonstra o uso do `basicHttpBinding` que fornece comunicação http e a interoperabilidade máxima com serviços da Web de primeira e segunda geração.

> [!NOTE]
> Os procedimentos de instalação e as instruções de compilação para esse exemplo estão localizadas no final deste tópico.

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Basic\Http`

## <a name="sample-details"></a>Detalhes de exemplo

Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora.

Para usar a associação básica com o comportamento padrão, somente o nome da seção de associação é necessário. Se você quiser configurar a ligação básica e alterar algumas de suas configurações, será necessário definir uma configuração de associação. O ponto de extremidade deve referenciar a configuração de associação por nome usando o `bindingConfiguration` atributo do `endpoint` elemento <>, conforme mostrado no código de exemplo a seguir.

```xml
<services>
    <service
        type="Microsoft.ServiceModel.Samples.CalculatorService"
        behaviorConfiguration="CalculatorServiceBehavior">
       <endpoint address=""
             binding="basicHttpBinding"
             bindingConfiguration="Binding1"
             contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
</services>
```

Neste exemplo, a configuração de associação é nomeada `"Binding1"` e é definida conforme mostrado no exemplo de código a seguir.

```xml
<bindings>
   <basicHttpBinding>
      <binding name="Binding1"
               hostNameComparisonMode="StrongWildcard"
               receiveTimeout="00:10:00"
               sendTimeout="00:10:00"
               openTimeout="00:10:00"
               closeTimeout="00:10:00"
               maxMessageSize="65536"
               maxBufferSize="65536"
               maxBufferPoolSize="524288"
               transferMode="Buffered"
               messageEncoding="Text"
               textEncoding="utf-8"
               bypassProxyOnLocal="false"
               useDefaultWebProxy="true" >
         <security mode="None" />
      </binding>
   </basicHttpBinding>
</bindings>
```

O elemento Binding fornece atributos para definir o modo de comparação de nomes de host, tamanho máximo da mensagem, opções de proxy, tempos limite, codificação de mensagens e outras opções.

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

3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).
