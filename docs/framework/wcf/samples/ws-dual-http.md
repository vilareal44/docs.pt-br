---
title: WS Dual Http
ms.date: 03/30/2017
ms.assetid: 9997eba5-29ec-48db-86f3-fa77b241fb1a
ms.openlocfilehash: 4acf2491c242099f6c8b6c9c01dc18e9c99c9934
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84589718"
---
# <a name="ws-dual-http"></a>WS Dual Http

O exemplo http duplo demonstra como configurar a `WSDualHttpBinding` associação. Este exemplo consiste em um programa de console do cliente (. exe) e uma biblioteca de serviços (. dll) hospedados pelo Serviços de Informações da Internet (IIS). O serviço implementa um contrato duplex. O contrato é definido pela `ICalculatorDuplex` interface, que expõe operações matemáticas (adicionar, subtrair, multiplicar e dividir). Neste exemplo, a `ICalculatorDuplex` interface permite que o cliente execute operações matemáticas, calculando um resultado em execução pela sessão. Independentemente, o serviço retorna resultados na `ICalculatorDuplexCallback` interface. Um contrato duplex requer uma sessão, porque um contexto deve ser estabelecido para correlacionar o conjunto de mensagens que estão sendo enviadas entre o cliente e o serviço. A `WSDualHttpBinding` Associação dá suporte à comunicação duplex.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\DualHttp`

Para configurar um ponto de extremidade de serviço com o `WSDualHttpBinding` , especifique a associação na configuração do ponto de extremidade, conforme mostrado.

```xml
<endpoint address=""
         binding="wsDualHttpBinding"
         contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex" />
```

No cliente, você deve configurar um endereço que o servidor pode usar para se conectar ao cliente, conforme mostrado na seguinte configuração de exemplo.

```xml
<system.serviceModel>
  <client>
    <endpoint address=
         "http://localhost/servicemodelsamples/service.svc"
         binding="wsDualHttpBinding"
         bindingConfiguration="Binding1"
         contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex" />
  </client>

  <bindings>
    <!-- Configure a WSDualHttpBinding that supports duplex -->
    <!-- communication. -->
    <wsDualHttpBinding>
      <binding name="Binding1"
               clientBaseAddress="http://localhost:8000/myClient/"
               useDefaultWebProxy="true"
               bypassProxyOnLocal="false">
      </binding>
    </wsDualHttpBinding>
  </bindings>
</system.serviceModel>
```

Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

```console
Press <ENTER> to terminate client once the output is displayed.

Result(100)
Result(50)
Result(882.5)
Result(441.25)
Equation(0 + 100 - 50 * 17.65 / 2 = 441.25)
```

Ao executar o exemplo, você verá as mensagens retornadas para o cliente na interface de retorno de chamada enviadas do serviço. Cada resultado intermediário é exibido, seguido de toda a equação após a conclusão de todas as operações. Pressione ENTER para desligar o cliente.

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Instale o ASP.NET 4,0 usando o comando a seguir.

    ```console
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable
    ```

2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

    > [!IMPORTANT]
    > Ao executar o cliente em uma configuração de computador cruzado, substitua localhost no `address` atributo [ \<endpoint> \<client> do](../../configure-apps/file-schema/wcf/endpoint-of-client.md) elemento e o `clientBaseAddress` atributo do elemento [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) do [\<wsDualHttpBinding>](../../configure-apps/file-schema/wcf/wsdualhttpbinding.md) elemento pelo nome do computador apropriado, como mostrado a seguir:.

    ```xml
    <client>
        <endpoint name = ""
          address=
         "http://service_machine_name/servicemodelsamples/service.svc"
        />
    </client>
    ...
    <wsDualHttpBinding>
        <binding name="DuplexBinding" clientBaseAddress=
            "http://client_machine_name:8000/myClient/">
        </binding>
    </wsDualHttpBinding>
    ```
