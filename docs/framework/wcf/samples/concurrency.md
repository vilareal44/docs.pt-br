---
title: Simultaneidade
ms.date: 03/30/2017
helpviewer_keywords:
- service behaviors, concurency sample
- Concurrency Sample [Windows Communication Foundation]
ms.assetid: f8dbdfb3-6858-4f95-abe3-3a1db7878926
ms.openlocfilehash: 393c8a79cb60a33203b41a0778176a4d78a9b6ee
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585305"
---
# <a name="concurrency"></a>Simultaneidade
O exemplo de simultaneidade demonstra o uso do <xref:System.ServiceModel.ServiceBehaviorAttribute> com a <xref:System.ServiceModel.ConcurrencyMode> enumeração, que controla se uma instância de um serviço processa as mensagens sequencialmente ou simultaneamente. O exemplo se baseia na [introdução](getting-started-sample.md), que implementa o `ICalculator` contrato de serviço. Este exemplo define um novo contrato, `ICalculatorConcurrency` , que é herdado do `ICalculator` , fornecendo duas operações adicionais para inspecionar o estado da simultaneidade do serviço. Alterando a configuração de simultaneidade, você pode observar a alteração no comportamento executando o cliente.  
  
 Neste exemplo, o cliente é um aplicativo de console (. exe) e o serviço é hospedado pelo Serviços de Informações da Internet (IIS).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Há três modos de simultaneidade disponíveis:  
  
- `Single`: Cada instância de serviço processa uma mensagem por vez. Este é o modo de simultaneidade padrão.  
  
- `Multiple`: Cada instância de serviço processa várias mensagens simultaneamente. A implementação do serviço deve ser thread-safe para usar esse modo de simultaneidade.  
  
- `Reentrant`: Cada instância de serviço processa uma mensagem por vez, mas aceita chamadas reentrantes. O serviço só aceita essas chamadas quando está chamando. Reentrante é demonstrado na amostra [ConcurrencyMode. reentrante](concurrencymode-reentrant.md) .  
  
 O uso da simultaneidade está relacionado ao modo de instanciação. Em <xref:System.ServiceModel.InstanceContextMode.PerCall> instanciação, a simultaneidade não é relevante, pois cada mensagem é processada por uma nova instância de serviço. Em <xref:System.ServiceModel.InstanceContextMode.Single> instanciação, <xref:System.ServiceModel.ConcurrencyMode.Single> ou <xref:System.ServiceModel.ConcurrencyMode.Multiple> a simultaneidade é relevante, dependendo se a única instância processa as mensagens sequencialmente ou simultaneamente. Na <xref:System.ServiceModel.InstanceContextMode.PerSession> instanciação, qualquer um dos modos de simultaneidade pode ser relevante.  
  
 A classe de serviço especifica o comportamento de simultaneidade com o `[ServiceBehavior(ConcurrencyMode=<setting>)]` atributo, conforme mostrado no exemplo de código a seguir. Alterando quais linhas são comentadas, você pode experimentar os `Single` modos de `Multiple` simultaneidade e. Lembre-se de recompilar o serviço depois de alterar o modo de simultaneidade.  
  
```csharp
// Single allows a single message to be processed sequentially by each service instance.  
//[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Single, InstanceContextMode = InstanceContextMode.Single)]  
  
// Multiple allows concurrent processing of multiple messages by a service instance.  
// The service implementation should be thread-safe. This can be used to increase throughput.  
[ServiceBehavior(ConcurrencyMode=ConcurrencyMode.Multiple, InstanceContextMode = InstanceContextMode.Single)]  
  
// Uses Thread.Sleep to vary the execution time of each operation.  
public class CalculatorService : ICalculatorConcurrency  
{  
    int operationCount;  
  
    public double Add(double n1, double n2)  
    {  
        operationCount++;  
        System.Threading.Thread.Sleep(180);  
        return n1 + n2;  
    }  
  
    public double Subtract(double n1, double n2)  
    {  
        operationCount++;  
        System.Threading.Thread.Sleep(100);  
        return n1 - n2;  
    }  
  
    public double Multiply(double n1, double n2)  
    {  
        operationCount++;  
        System.Threading.Thread.Sleep(150);  
        return n1 * n2;  
    }  
  
    public double Divide(double n1, double n2)  
    {  
        operationCount++;  
        System.Threading.Thread.Sleep(120);  
        return n1 / n2;  
    }  
  
    public string GetConcurrencyMode()  
    {
        // Return the ConcurrencyMode of the service.  
        ServiceHost host = (ServiceHost)OperationContext.Current.Host;  
        ServiceBehaviorAttribute behavior = host.Description.Behaviors.Find<ServiceBehaviorAttribute>();  
        return behavior.ConcurrencyMode.ToString();  
    }  
  
    public int GetOperationCount()  
    {
        // Return the number of operations.  
        return operationCount;  
    }  
}  
```  
  
 O exemplo usa a <xref:System.ServiceModel.ConcurrencyMode.Multiple> simultaneidade com <xref:System.ServiceModel.InstanceContextMode.Single> instanciação por padrão. O código do cliente foi modificado para usar um proxy assíncrono. Isso permite que o cliente faça várias chamadas para o serviço sem esperar uma resposta entre cada chamada. Você pode observar a diferença no comportamento do modo de simultaneidade do serviço.  
  
 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. O modo de simultaneidade em que o serviço está sendo executado é exibido, cada operação é chamada e, em seguida, a contagem de operações é exibida. Observe que, quando o modo de simultaneidade é `Multiple` , os resultados são retornados em uma ordem diferente da forma como eles foram chamados, pois o serviço processa várias mensagens simultaneamente. Ao alterar o modo de simultaneidade para `Single` , os resultados são retornados na ordem em que foram chamados, porque o serviço processa cada mensagem sequencialmente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Se você usar svcutil. exe para gerar o cliente proxy, certifique-se de incluir a `/async` opção.  
  
3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Behaviors\Concurrency`  
