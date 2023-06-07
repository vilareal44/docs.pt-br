---
title: Unidirecional
ms.date: 03/30/2017
ms.assetid: 74e3e03d-cd15-4191-a6a5-1efa2dcb9e73
ms.openlocfilehash: 07fc4ecf981acbad577758c943aa22405f528a52
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575246"
---
# <a name="one-way"></a>Unidirecional
Este exemplo demonstra um contato de serviço com operações de serviço unidirecionais. O cliente não aguarda que as operações de serviço sejam concluídas como é o caso com operações de serviço bidirecionais. Este exemplo é baseado no [introdução](getting-started-sample.md) e usa a `wsHttpBinding` associação. O serviço neste exemplo é um aplicativo de console auto-hospedado para permitir que você observe o serviço que recebe e processa solicitações. O cliente também é um aplicativo de console.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Para criar um contrato de serviço unidirecional, defina seu contrato de serviço, aplique a <xref:System.ServiceModel.OperationContractAttribute> classe a cada operação e <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A> defina `true` como, conforme mostrado no código de exemplo a seguir:  
  
```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]  
public interface IOneWayCalculator  
{  
    [OperationContract(IsOneWay=true)]  
    void Add(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Subtract(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Multiply(double n1, double n2);  
    [OperationContract(IsOneWay = true)]  
    void Divide(double n1, double n2);  
}  
```  
  
 Para demonstrar que o cliente não aguarda a conclusão das operações de serviço, o código de serviço neste exemplo implementa um atraso de cinco segundos, conforme mostrado no código de exemplo a seguir:  
  
```csharp
// This service class implements the service contract.  
// This code writes output to the console window.  
 [ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Multiple,  
    InstanceContextMode = InstanceContextMode.PerCall)]  
public class CalculatorService : IOneWayCalculator  
{  
    public void Add(double n1, double n2)  
    {  
        Console.WriteLine("Received Add({0},{1}) - sleeping", n1, n2);  
        System.Threading.Thread.Sleep(1000 * 5);  
        double result = n1 + n2;  
        Console.WriteLine("Processing Add({0},{1}) - result: {2}", n1, n2, result);  
    }  
    ...  
}  
```  
  
 Quando o cliente chama o serviço, a chamada retorna sem esperar que a operação de serviço seja concluída.  
  
 Quando você executa o exemplo, as atividades de cliente e serviço são exibidas nas janelas do console do cliente e do serviço. Você pode ver o serviço receber mensagens do cliente. Pressione ENTER em cada janela do console para desligar o serviço e o cliente.  
  
 O cliente é concluído antes do serviço, demonstrando que um cliente não aguarda a conclusão de operações de serviço unidirecionais. A saída do cliente é a seguinte:  
  
```console  
Add(100,15.99)  
Subtract(145,76.54)  
Multiply(9,81.25)  
Divide(22,7)  
  
Press <ENTER> to terminate client.  
```  
  
 A saída de serviço a seguir é mostrada:  
  
```console  
The service is ready.  
Press <ENTER> to terminate service.  
  
Received Add(100,15.99) - sleeping  
Received Subtract(145,76.54) - sleeping  
Received Multiply(9,81.25) - sleeping  
Received Divide(22,7) - sleeping  
Processing Add(100,15.99) - result: 115.99  
Processing Subtract(145,76.54) - result: 68.46  
Processing Multiply(9,81.25) - result: 731.25  
Processing Divide(22,7) - result: 3.14285714285714  
```  
  
> [!NOTE]
> O HTTP é, por definição, um protocolo de solicitação/resposta; Quando uma solicitação é feita, uma resposta é retornada. Isso é verdadeiro mesmo para uma operação de serviço unidirecional que é exposta por HTTP. Quando a operação é chamada, o serviço retorna um código de status HTTP de 202 antes da execução da operação de serviço. Esse código de status significa que a solicitação foi aceita para processamento, mas o processamento ainda não foi concluído. O cliente que chamou a operação é bloqueado até receber a resposta 202 do serviço. Isso pode causar algum comportamento inesperado quando várias mensagens unidirecionais são enviadas usando uma associação configurada para usar sessões. A `wsHttpBinding` associação usada neste exemplo é configurada para usar sessões por padrão para estabelecer um contexto de segurança. Por padrão, as mensagens em uma sessão têm garantia de chegar na ordem em que são enviadas. Por isso, quando a segunda mensagem em uma sessão é enviada, ela não é processada até que a primeira mensagem seja processada. O resultado disso é que o cliente não recebe a resposta 202 para uma mensagem até que o processamento da mensagem anterior tenha sido concluído. O cliente, portanto, parece bloquear em cada chamada de operação subsequente. Para evitar esse comportamento, esta amostra configura o tempo de execução para distribuir mensagens simultaneamente para instâncias distintas para processamento. O exemplo define <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode%2A> como para `PerCall` que cada mensagem possa ser processada por uma instância diferente. <xref:System.ServiceModel.ServiceBehaviorAttribute.ConcurrencyMode%2A>é definido como `Multiple` para permitir que mais de um thread envie mensagens por vez.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!NOTE]
> Execute o serviço antes de executar o cliente e desligue o cliente antes de desligar o serviço. Isso evita uma exceção de cliente quando o cliente não pode fechar a sessão de segurança corretamente porque o serviço não existe mais.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\Oneway`  
