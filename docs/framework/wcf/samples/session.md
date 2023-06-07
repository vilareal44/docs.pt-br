---
title: Sessão
ms.date: 03/30/2017
helpviewer_keywords:
- Sessions
ms.assetid: 36e1db50-008c-4b32-8d09-b56e790b8417
ms.openlocfilehash: 283c8b9641dcce8b0207d3be0024b57369d125ff
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591443"
---
# <a name="session"></a>Sessão
O exemplo de sessão demonstra como implementar um contrato que requer uma sessão. Uma sessão fornece contexto para executar várias operações. Isso permite que um serviço associe o estado a uma determinada sessão, de modo que as operações subsequentes possam usar o estado de uma operação anterior. Este exemplo é baseado na [introdução](getting-started-sample.md), que implementa um serviço de calculadora. O `ICalculator` contrato foi modificado para permitir que um conjunto de operações aritméticas seja executado, mantendo um resultado em execução. Essa funcionalidade é definida pelo `ICalculatorSession` contrato. O serviço mantém o estado de um cliente, pois várias operações de serviço são chamadas para executar um cálculo. O cliente pode recuperar o resultado atual chamando `Result()` e limpar o resultado para zero chamando `Clear()` .  
  
 Neste exemplo, o cliente é um aplicativo de console (. exe) e o serviço é hospedado pelo Serviços de Informações da Internet (IIS).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Definindo o <xref:System.ServiceModel.SessionMode> do contrato para `Required` garantir que quando o contrato é exposto em uma ligação específica, a associação dá suporte a sessões. Se a associação não oferecer suporte a sessões, uma exceção será lançada. A `ICalculatorSession` interface é definida de modo que uma ou mais operações possam ser chamadas, o que modifica um resultado em execução, conforme mostrado no código de exemplo a seguir.  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples", SessionMode=SessionMode.Required)]  
public interface ICalculatorSession  
{  
    [OperationContract(IsOneWay=true)]  
    void Clear();  
    [OperationContract(IsOneWay = true)]  
    void AddTo(double n);  
    [OperationContract(IsOneWay = true)]  
    void SubtractFrom(double n);  
    [OperationContract(IsOneWay = true)]  
    void MultiplyBy(double n);  
    [OperationContract(IsOneWay = true)]  
    void DivideBy(double n);  
    [OperationContract]  
    double Result();  
}  
```  
  
 O serviço usa um <xref:System.ServiceModel.InstanceContextMode> de <xref:System.ServiceModel.InstanceContextMode.PerSession> para associar um determinado contexto de instância de serviço a cada sessão de entrada. Isso permite que o serviço Mantenha o resultado em execução para cada sessão em uma variável de membro local.  
  
```csharp
[ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]  
public class CalculatorService : ICalculatorSession  
{  
    double result = 0.0D;  
  
    public void Clear()  
    {  result = 0.0D; }  
  
    public void AddTo(double n)  
    {  result += n;   }  
  
    public void SubtractFrom(double n)  
    {  result -= n;   }  
  
    public void MultiplyBy(double n)  
    {  result *= n;   }  
  
    public void DivideBy(double n)  
    {  result /= n;   }  
  
    public double Result()  
    {  return result; }  
}  
```  
  
 Quando você executa o exemplo, o cliente faz várias solicitações para o servidor e solicita o resultado, que é exibido na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
(((0 + 100) - 50) * 17.65) / 2 = 441.25  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\Session`  
