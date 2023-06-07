---
title: Exceções esperadas
ms.date: 03/30/2017
ms.assetid: 299a6987-ae6b-43c6-987f-12b034b583ae
ms.openlocfilehash: d8e3c024eb69fe22ec27f3e3697bc4fc7b4ee121
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600523"
---
# <a name="expected-exceptions"></a>Exceções esperadas
Este exemplo demonstra como capturar exceções esperadas ao usar um cliente digitado. Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora. Neste exemplo, o cliente é um aplicativo de console (. exe) e o serviço é hospedado pelo Serviços de Informações da Internet (IIS).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Este exemplo demonstra como capturar e manipular os dois tipos de exceção esperados que os programas corretos devem tratar: `TimeoutException` e `CommunicationException` .  
  
 As exceções que são geradas de métodos de comunicação em um cliente Windows Communication Foundation (WCF) são esperadas ou inesperadas. Exceções inesperadas incluem falhas catastróficas `OutOfMemoryException` , como e erros de programação como `ArgumentNullException` ou `InvalidOperationException` . Normalmente, não há uma maneira útil de lidar com erros inesperados, portanto, normalmente você não deve capturá-los ao chamar um método de comunicação do cliente WCF.  
  
 As exceções esperadas dos métodos de comunicação em um cliente WCF incluem `TimeoutException` , `CommunicationException` e qualquer classe derivada de `CommunicationException` . Eles indicam um problema durante a comunicação que pode ser manipulada com segurança anulando o cliente WCF e relatando uma falha de comunicação. Como fatores externos podem causar esses erros em qualquer aplicativo, os aplicativos corretos devem capturar essas exceções e recuperar quando ocorrerem.  
  
 Há várias classes derivadas de `CommunicationException` que um cliente pode lançar. Em alguns casos, os aplicativos também capturam alguns deles para fazer uma manipulação especial, mas permitem que os outros sejam tratados como um `CommunicationException` . Isso pode ser feito com a captura do tipo de exceção mais específico primeiro e, em seguida, `CommunicationException` a captura de uma cláusula catch posterior.  
  
 O código que chama um método de comunicação do cliente deve capturar `TimeoutException` e `CommunicationException` . Uma maneira de lidar com esses erros é anular o cliente e relatar a falha de comunicação.  
  
```csharp
try  
{  
    ...  
    double result = client.Add(value1, value2);  
    ...  
    client.Close();  
}  
catch (TimeoutException exception)  
{  
    Console.WriteLine("Got {0}", exception.GetType());  
    client.Abort();  
}  
catch (CommunicationException exception)  
{  
    Console.WriteLine("Got {0}", exception.GetType());  
    client.Abort();  
}  
```  
  
 Se ocorrer uma exceção esperada, o cliente poderá ou não poderá ser usado posteriormente. Para determinar se o cliente ainda pode ser usado, verifique se a `State` propriedade é `CommunicationState` . Feito. Se ele ainda estiver aberto, ele ainda será utilizável. Caso contrário, você deve anular o cliente e liberar todas as referências a ele.  
  
> [!CAUTION]
> Você pode observar que os clientes que têm uma sessão geralmente não podem mais ser usados após uma exceção, e os clientes que não têm uma sessão geralmente ainda podem ser usados após uma exceção. No entanto, nenhuma delas é garantida, portanto, se você quiser tentar continuar usando o cliente após uma exceção, seu aplicativo deverá verificar a `State` propriedade para verificar se o cliente ainda está aberto.  
  
 Quando você executa o exemplo, as respostas e as exceções da operação são exibidas na janela do console do cliente.  
  
 O processo do cliente executa dois cenários, cada um dos quais tenta chamar `Add` seguido por `Divide` . O primeiro cenário simula um problema de rede anulando o cliente antes de fazer a chamada para `Divide` . O segundo cenário causa uma condição de tempo limite definindo o tempo limite muito curto para que o método seja concluído. A saída esperada do processo do cliente é:  
  
```output
Add(100,15.99) = 115.99  
Simulated network problem occurs...  
Got System.ServiceModel.CommunicationObjectAbortedException  
Add(100,15.99) = 115.99  
Set timeout too short for method to complete...  
Got System.TimeoutException  
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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Client\ExpectedExceptions`  
