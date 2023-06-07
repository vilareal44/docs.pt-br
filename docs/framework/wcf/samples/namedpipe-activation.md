---
title: NamedPipe Activation
ms.date: 03/30/2017
ms.assetid: f3c0437d-006c-442e-bfb0-6b29216e4e29
ms.openlocfilehash: 8d9a10b94c52514db611144352653b911d109056
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602460"
---
# <a name="namedpipe-activation"></a>NamedPipe Activation

Este exemplo demonstra a hospedagem de um serviço que usa o WAS (serviço de ativação de processos do Windows) para ativar um serviço que se comunica em pipes de nomes. Este exemplo é baseado na [introdução](getting-started-sample.md) e requer que o Windows Vista seja executado.

> [!NOTE]
> Os procedimentos de instalação e as instruções de compilação para esse exemplo estão localizadas no final deste tópico.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\WASHost\NamedPipeActivation`

## <a name="sample-details"></a>Detalhes de exemplo

O exemplo consiste em um programa de console do cliente (. exe) e uma biblioteca de serviços (. dll) hospedados em um processo de trabalho ativado pelo WAS (serviços de ativação de processos do Windows). A atividade do cliente fica visível na janela do console.

O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta. O contrato é definido pela `ICalculator` interface, que expõe operações matemáticas (adicionar, subtrair, multiplicar e dividir), conforme mostrado no código de exemplo a seguir.

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface ICalculator
{
    [OperationContract]
    double Add(double n1, double n2);
    [OperationContract]
    double Subtract(double n1, double n2);
    [OperationContract]
    double Multiply(double n1, double n2);
    [OperationContract]
    double Divide(double n1, double n2);
}
```

O cliente faz solicitações síncronas para uma determinada operação matemática e a implementação do serviço calcula e retorna o resultado apropriado.

```csharp
// Service class that implements the service contract.
public class CalculatorService : ICalculator
{
    public double Add(double n1, double n2)
    {
        return n1 + n2;
    }
    public double Subtract(double n1, double n2)
    {
        return n1 - n2;
    }
    public double Multiply(double n1, double n2)
    {
        return n1 * n2;
    }
    public double Divide(double n1, double n2)
    {
        return n1 / n2;
    }
}
```

O exemplo usa uma associação modificada sem `netNamedPipeBinding` segurança. A associação é especificada nos arquivos de configuração para o cliente e o serviço. O tipo de associação para o serviço é especificado no atributo do elemento do ponto de extremidade `binding` , conforme mostrado na seguinte configuração de exemplo.

Se você quiser usar uma associação de pipe nomeado segura, altere o modo de segurança do servidor para a configuração de segurança desejada e execute svcutil. exe novamente no cliente para obter um arquivo de configuração de cliente atualizado.

```xml
<system.serviceModel>
        <services>
            <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">

        <!-- This endpoint is exposed at the base address provided by host: net.pipe://localhost/servicemodelsamples/service.svc  -->
        <endpoint address=""
                  binding="netNamedPipeBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
        <!-- the mex endpoint is exposed at net.pipe://localhost/servicemodelsamples/service.svc/mex -->
        <endpoint address="mex"
                  binding="mexNamedPipeBinding"
                  contract="IMetadataExchange" />
      </service>
        </services>
        <bindings>
            <netNamedPipeBinding>
                <binding name="Binding1" >
                    <security mode = "None">
                    </security>
                </binding >
            </netNamedPipeBinding>
        </bindings>

    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceMetadata />
          <serviceDebug includeExceptionDetailInFaults="False" />
        </behavior>
      </serviceBehaviors>
    </behaviors>

  </system.serviceModel>
```

As informações de ponto de extremidade do cliente são configuradas conforme mostrado no código de exemplo a seguir.

```xml
<system.serviceModel>

    <client>
      <endpoint name=""
                          address="net.pipe://localhost/servicemodelsamples/service.svc"
                          binding="netNamedPipeBinding"
                          bindingConfiguration="Binding1"
                          contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </client>

    <bindings>

      <!--  Following is the expanded configuration section for a NetNamedPipeBinding.
            Each property is configured with the default value. -->

      <netNamedPipeBinding>
        <binding name="Binding1"
                         maxBufferSize="65536"
                         maxConnections="10">
          <security mode = "None">
          </security>
        </binding >

      </netNamedPipeBinding>
    </bindings>

  </system.serviceModel>
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

1. Verifique se o IIS 7,0 está instalado. O IIS 7,0 é necessário para a ativação do WAS.

2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

    Além disso, você deve instalar os componentes de ativação não HTTP do WCF:

    1. No menu **Iniciar**, selecione **Painel de Controle**.

    2. Selecione **programas e recursos**.

    3. Clique em **Ativar ou desativar componentes do Windows**.

    4. Expanda o nó **Microsoft .NET Framework 3,0** e verifique o Windows Communication Foundation recurso de **ativação não http** .

3. Configure o WAS (serviço de ativação de processos do Windows) para dar suporte à ativação de pipe nomeado.

    Como uma conveniência, as duas etapas a seguir são implementadas em um arquivo em lotes chamado AddNetPipeSiteBinding. cmd localizado no diretório de exemplo.

    1. Para dar suporte à ativação do NET. pipe, o site padrão deve primeiro ser associado ao protocolo net. pipe. Isso pode ser feito usando o Appcmd. exe, que é instalado com o conjunto de ferramentas de gerenciamento do IIS 7,0. Em um prompt de comando elevado (administrador), execute o comando a seguir.

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
        -+bindings.[protocol='net.pipe',bindingInformation='*']
        ```

        > [!NOTE]
        > Esse comando é uma única linha de texto.

        Este comando adiciona uma associação de site net. pipe ao site padrão.

    2. Embora todos os aplicativos em um site compartilhem uma associação net. pipe comum, cada aplicativo pode habilitar o suporte a net. pipe individualmente. Para habilitar net. pipe para o aplicativo/servicemodelsamples, execute o comando a seguir em um prompt de comando elevado.

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http,net.pipe
        ```

        > [!NOTE]
        > Esse comando é uma única linha de texto.

        Esse comando permite que o aplicativo/servicemodelsamples seja acessado usando o `http://localhost/servicemodelsamples` e o `net.tcp://localhost/servicemodelsamples` .

4. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

5. Remova a associação de site net. pipe que você adicionou para este exemplo.

    Como uma conveniência, as duas etapas a seguir são implementadas em um arquivo em lotes chamado RemoveNetPipeSiteBinding. cmd localizado no diretório de exemplo:

    1. Remova net. TCP da lista de protocolos habilitados executando o comando a seguir em um prompt de comandos com privilégios elevados.

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app "Default Web Site/servicemodelsamples" /enabledProtocols:http
        ```

        > [!NOTE]
        > Esse comando deve ser inserido como uma única linha de texto.

    2. Remova a associação do site net. TCP executando o comando a seguir em um prompt de comando elevado.

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" --bindings.[protocol='net.pipe',bindingInformation='*']
        ```

        > [!NOTE]
        > Esse comando deve ser digitado como uma única linha de texto.

## <a name="see-also"></a>Consulte também

- [Hospedagem de AppFabric e persistência Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383418(v=azure.10))
