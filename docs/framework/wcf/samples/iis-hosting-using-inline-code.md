---
title: Hospedagem do IIS utilizando código embutido
ms.date: 03/30/2017
helpviewer_keywords:
- Web hosted service
- IIS Hosting Using Inline Code Sample [Windows Communication Foundation]
ms.assetid: 56fe3687-a34b-4661-8e30-b33770f413fa
ms.openlocfilehash: 47d056e35b92654c8e47647c7273c5d69b37bd97
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594641"
---
# <a name="iis-hosting-using-inline-code"></a>Hospedagem do IIS utilizando código embutido

Este exemplo demonstra como implementar um serviço hospedado pelo Serviços de Informações da Internet (IIS), onde o código de serviço está contido em linha em um arquivo. svc e é compilado sob demanda. O código de serviço também pode ser implementado diretamente em arquivos de código-fonte localizados no diretório \ App_Code do aplicativo ou compilado no assembly implantado no \bin. Este exemplo não demonstra essas técnicas.

> [!NOTE]
> Os procedimentos de instalação e as instruções de compilação para esse exemplo estão localizadas no final deste tópico.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Hosting\WebHost\InlineCode`

O exemplo demonstra um serviço típico que implementa um contrato que define um padrão de comunicação de solicitação-resposta. O serviço é hospedado no IIS e o código do serviço está totalmente contido no arquivo Service. svc. O serviço é ativado pelo host e compilado sob demanda pela primeira mensagem enviada ao serviço. Não há necessidade de pré-compilação. O serviço implementa um `ICalculator` contrato, conforme definido no código a seguir:

```csharp
// Define a service contract.
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

A implementação do serviço calcula e retorna o resultado apropriado.

`<%@ServiceHost language=c# Debug="true" Service="Microsoft.ServiceModel.Samples.CalculatorService" %>`

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

Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

```console
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Depois que a solução tiver sido criada, execute Setup. bat para configurar o aplicativo ServiceModelSamples no IIS 7,0. O diretório ServiceModelSamples agora deve aparecer como um aplicativo IIS 7,0.

4. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md). Para obter um exemplo de como criar um aplicativo cliente que pode chamar esse serviço, consulte [como: criar um cliente](../how-to-create-a-wcf-client.md).

## <a name="see-also"></a>Confira também

- [Hospedagem de AppFabric e persistência Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383418(v=azure.10))
