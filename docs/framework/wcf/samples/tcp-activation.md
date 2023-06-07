---
title: Ativação TCP
ms.date: 03/30/2017
ms.assetid: bf8c215c-0228-4f4f-85c2-e33794ec09a7
ms.openlocfilehash: 0fa737adbdc7acc51511557877799c89849149bc
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598652"
---
# <a name="tcp-activation"></a>Ativação TCP

Este exemplo demonstra a hospedagem de um serviço que usa o WAS (serviços de ativação de processos do Windows) para ativar um serviço que se comunica através do protocolo net. TCP. Este exemplo é baseado na [introdução](getting-started-sample.md).

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\WASHost\TCPActivation`

O exemplo consiste em um programa de console do cliente (. exe) e uma biblioteca de serviços (. dll) hospedada em um processo de trabalho ativado pelo WAS. A atividade do cliente fica visível na janela do console.

O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta. O contrato é definido pela `ICalculator` interface, que expõe operações matemáticas (adicionar, subtrair, multiplicar e dividir), conforme mostrado no código de exemplo a seguir:

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

A implementação do serviço calcula e retorna o resultado apropriado:

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

O exemplo usa uma variante da Associação net. TCP com compartilhamento de porta TCP habilitada e segurança desativada. Se você quiser usar uma associação TCP segura, altere o modo de segurança do servidor para a configuração desejada e execute novamente svcutil. exe no cliente para gerar um arquivo de configuração de cliente de atualização.

O exemplo a seguir mostra a configuração para o serviço:

```xml
<system.serviceModel>

    <services>
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">
        <!-- This endpoint is exposed at the base address provided by host: net.tcp://localhost/servicemodelsamples/service.svc  -->
        <endpoint binding="netTcpBinding" bindingConfiguration="PortSharingBinding"
          contract="Microsoft.ServiceModel.Samples.ICalculator" />
        <!-- the mex endpoint is exposed at net.tcp://localhost/servicemodelsamples/service.svc/mex -->
        <endpoint address="mex"
                  binding="mexTcpBinding"
                  contract="IMetadataExchange" />
      </service>
    </services>
    <bindings>
      <netTcpBinding>
        <binding name="PortSharingBinding" portSharingEnabled="true">
          <security mode="None" />
        </binding>
      </netTcpBinding>
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

O ponto de extremidade do cliente é configurado conforme mostrado no código de exemplo a seguir:

```xml
<system.serviceModel>
    <bindings>
        <netTcpBinding>
          <binding name="NetTcpBinding_ICalculator">
            <security mode="None"/>
          </binding>
        </netTcpBinding>
    </bindings>
    <client>
        <endpoint address="net.tcp://localhost/servicemodelsamples/service.svc"
            binding="netTcpBinding" bindingConfiguration="NetTcpBinding_ICalculator"
            contract="Microsoft.ServiceModel.Samples.ICalculator" name="NetTcpBinding_ICalculator" />
    </client>
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

2. Certifique-se de ter executado o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

    Além disso, você deve instalar os componentes de ativação não HTTP do WCF:

    1. No menu **Iniciar**, selecione **Painel de Controle**.

    2. Selecione **programas e recursos**.

    3. Clique em **Ativar ou desativar componentes do Windows**.

    4. Expanda o nó **Microsoft .NET Framework 3,0** e verifique o Windows Communication Foundation recurso de **ativação não http** .

3. Configurar o WAS para dar suporte à ativação de TCP.

    Como uma conveniência, as duas etapas a seguir são implementadas em um arquivo em lotes chamado AddNetTcpSiteBinding. cmd localizado no diretório de exemplo.

    1. Para dar suporte à ativação net. TCP, o site padrão deve primeiro ser associado a uma porta Net. TCP. Isso pode ser feito usando o Appcmd. exe, que é instalado com o conjunto de ferramentas de gerenciamento do Serviços de Informações da Internet 7,0 (IIS). Em um prompt de comando de nível de administrador, execute o seguinte comando:

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site" -+bindings.[protocol='net.tcp',bindingInformation='808:*']
        ```

        > [!TIP]
        > Esse comando é uma única linha de texto. Esse comando adiciona uma associação de site net. TCP ao site padrão escutando na porta TCP 808 com qualquer nome de host.

    2. Embora todos os aplicativos em um site compartilhem uma associação net. TCP comum, cada aplicativo pode habilitar o suporte a net. TCP individualmente. Para habilitar net. TCP para o aplicativo/servicemodelsamples, execute o seguinte comando em um prompt de comando de nível de administrador:

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app
        "Default Web Site/servicemodelsamples" /enabledProtocols:http,net.tcp
        ```

        > [!NOTE]
        > Esse comando é uma única linha de texto. Esse comando permite que o aplicativo/servicemodelsamples seja acessado usando o `http://localhost/servicemodelsamples` e o `net.tcp://localhost/servicemodelsamples` .

4. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

5. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

    Remova a associação de site net. TCP que você adicionou para este exemplo.

    Como uma conveniência, as duas etapas a seguir são implementadas em um arquivo em lotes chamado RemoveNetTcpSiteBinding. cmd localizado no diretório de exemplo.

    1. Remova net. TCP da lista de protocolos habilitados executando o seguinte comando de um prompt de comando de nível de administrador:

        ```console
        %windir%\system32\inetsrv\appcmd.exe set app
        "Default Web Site/servicemodelsamples" /enabledProtocols:http
        ```

        > [!NOTE]
        > Esse comando deve ser inserido como uma única linha de texto.

    2. Remova a associação de site net. TCP executando o seguinte comando de um prompt de comando de nível de administrador:

        ```console
        %windir%\system32\inetsrv\appcmd.exe set site "Default Web Site"
        --bindings.[protocol='net.tcp',bindingInformation='808:*']
        ```

        > [!NOTE]
        > Esse comando deve ser digitado como uma única linha de texto.

## <a name="see-also"></a>Consulte também

- [Hospedagem de AppFabric e persistência Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383418(v=azure.10))
