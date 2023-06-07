---
title: Compatibilidade ASP.NET
ms.date: 03/30/2017
ms.assetid: c8b51f1e-c096-4c42-ad99-0519887bbbc5
ms.openlocfilehash: 23930e0756d3fbefc28a8f650b5a056106145a50
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594706"
---
# <a name="aspnet-compatibility"></a>Compatibilidade ASP.NET

Este exemplo demonstra como habilitar o modo de compatibilidade ASP.NET no Windows Communication Foundation (WCF). Os serviços em execução no modo de compatibilidade ASP.NET participam totalmente do pipeline de aplicativo do ASP.NET e podem fazer uso de recursos do ASP.NET, como autorização de arquivo/URL, estado de sessão e a <xref:System.Web.HttpContext> classe. A <xref:System.Web.HttpContext> classe permite acesso a cookies, sessões e outros recursos do ASP.net. Esse modo requer que as associações usem o transporte HTTP e o próprio serviço deve ser hospedado no IIS.

Neste exemplo, o cliente é um aplicativo de console (um executável) e o serviço é hospedado no Serviços de Informações da Internet (IIS).

> [!NOTE]
> Os procedimentos de instalação e as instruções de compilação para esse exemplo estão localizadas no final deste tópico.

Este exemplo exige um pool de aplicativos .NET Framework 4 para ser executado. Para criar um novo pool de aplicativos ou para modificar o pool de aplicativos padrão, siga estas etapas.

1. Abra o **Painel de Controle**.  Abra o miniaplicativo **Ferramentas administrativas** no título **sistema e segurança** . Abra o miniaplicativo **Gerenciador do serviços de informações da Internet (IIS)** .

2. Expanda o modo de exibição de árvore no painel **conexões** . Selecione o nó **pools de aplicativos** .

3. Para definir o pool de aplicativos padrão a ser usado .NET Framework 4 (o que pode causar problemas de incompatibilidade com os sites existentes), clique com o botão direito do mouse no item de lista **DefaultAppPool** e selecione **configurações básicas..**.. Defina o pull-down da **versão do .NET Framework** para o **.NET Framework v 4.0.30128** (ou posterior).

4. Para criar um novo pool de aplicativos que usa o .NET Framework 4 (para preservar a compatibilidade de outros aplicativos), clique com o botão direito do mouse no nó **pools de aplicativos** e selecione **Adicionar pool de aplicativos...**. Nomeie o novo pool de aplicativos e defina o pull-down da **versão do .NET Framework** para o **.NET Framework v 4.0.30128** (ou posterior). Depois de executar as etapas de configuração abaixo, clique com o botão direito do mouse no aplicativo **ServiceModelSamples** e selecione **gerenciar aplicativo**, **Configurações avançadas...**. Defina o **pool de aplicativos** para o novo pool de aplicativos.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\WebHost\ASPNetCompatibility`

Este exemplo é baseado na [introdução](getting-started-sample.md), que implementa um serviço de calculadora. O `ICalculator` contrato foi modificado como o `ICalculatorSession` contrato para permitir que um conjunto de operações seja executado, ao mesmo tempo em que mantém um resultado em execução.

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface ICalculatorSession
{
    [OperationContract]
    void Clear();
    [OperationContract]
    void AddTo(double n);
    [OperationContract]
    void SubtractFrom(double n);
    [OperationContract]
    void MultiplyBy(double n);
    [OperationContract]
    void DivideBy(double n);
    [OperationContract]
    double Result();
}
```

O serviço mantém o estado, usando o recurso, para cada cliente, uma vez que várias operações de serviço são chamadas para executar um cálculo. O cliente pode recuperar o resultado atual chamando `Result` e pode limpar o resultado para zero chamando `Clear` .

O serviço usa a sessão ASP.NET para armazenar o resultado para cada sessão de cliente. Isso permite que o serviço Mantenha o resultado em execução para cada cliente em várias chamadas para o serviço.

> [!NOTE]
> O estado de sessão do ASP.NET e as sessões do WCF são coisas muito diferentes. Consulte a [sessão](session.md) para obter detalhes sobre as sessões do WCF.

O serviço tem uma dependência profunda no estado de sessão ASP.NET e requer que o modo de compatibilidade ASP.NET funcione corretamente. Esses requisitos são expressos declarativamente aplicando-se o `AspNetCompatibilityRequirements` atributo.

```csharp
[AspNetCompatibilityRequirements(RequirementsMode =
                       AspNetCompatibilityRequirementsMode.Required)]
public class CalculatorService : ICalculatorSession
{
    double Result
    {  // store result in AspNet Session
       get {
          if (HttpContext.Current.Session["Result"] != null)
             return (double)HttpContext.Current.Session["Result"];
          return 0.0D;
       }
       set
       {
          HttpContext.Current.Session["Result"] = value;
       }
    }
    public void Clear()
    {
        Result = 0.0D;
    }
    public void AddTo(double n)
    {
        Result += n;
    }
    public void SubtractFrom(double n)
    {
        Result -= n;
    }
    public void MultiplyBy(double n)
    {
        Result *= n;
    }
    public void DivideBy(double n)
    {
        Result /= n;
    }
    public double Result()
    {
        return Result;
    }
}
```

Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

```console
0, + 100, - 50, * 17.65, / 2 = 441.25
Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Certifique-se de ter executado o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Depois que a solução tiver sido criada, execute Setup. bat para configurar o aplicativo ServiceModelSamples no IIS 7,0. O diretório ServiceModelSamples agora deve aparecer como um aplicativo IIS 7,0.

4. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

## <a name="see-also"></a>Confira também

- [Hospedagem de AppFabric e persistência Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383418(v=azure.10))
