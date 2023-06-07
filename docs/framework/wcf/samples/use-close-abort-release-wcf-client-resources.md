---
title: Usar Close e Abort para liberar recursos de cliente do WCF
description: Dispose pode falhar e gerar exceções quando a rede falhar. Isso pode causar comportamento indesejado. Em vez disso, use fechar e anular para liberar recursos do cliente quando a rede tiver falhado.
ms.date: 11/12/2018
ms.assetid: aff82a8d-933d-4bdc-b0c2-c2f7527204fb
ms.openlocfilehash: b338b760f461d7b773f43dd1f5e6dbce98f9e15a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599913"
---
# <a name="close-and-abort-release-resources-safely-when-network-connections-have-dropped"></a>Fechar e anular a liberação de recursos com segurança quando as conexões de rede forem descartadas

Este exemplo demonstra como usar `Close` os `Abort` métodos e para limpar os recursos ao usar um cliente digitado. A `using` instrução causa exceções quando a conexão de rede não é robusta. Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora. Neste exemplo, o cliente é um aplicativo de console (. exe) e o serviço é hospedado pelo Serviços de Informações da Internet (IIS).

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

Este exemplo mostra dois dos problemas comuns que ocorrem ao usar a instrução C# "using" com clientes digitados, bem como o código que limpa corretamente após exceções.

A instrução C# "using" resulta em uma chamada para `Dispose` (). Isso é o mesmo que `Close` (), que pode gerar exceções quando ocorre um erro de rede. Como a chamada para `Dispose` () ocorre implicitamente na chave de fechamento do bloco "using", essa fonte de exceções provavelmente passará despercebida por pessoas que escrevem o código e lendo o código. Isso representa uma possível fonte de erros de aplicativo.

O primeiro problema, ilustrado no `DemonstrateProblemUsingCanThrow` método, é que a chave de fechamento gera uma exceção e o código após a chave de fechamento não é executado:

```csharp
using (CalculatorClient client = new CalculatorClient())
{
    ...
} // <-- this line might throw
Console.WriteLine("Hope this code wasn't important, because it might not happen.");
```

Mesmo que nada dentro do bloco Using gere uma exceção ou todas as exceções dentro do bloco Using sejam detectadas, o `Console.WriteLine` pode não acontecer porque a `Dispose` chamada implícita () na chave de fechamento pode gerar uma exceção.

O segundo problema, ilustrado no `DemonstrateProblemUsingCanThrowAndMask` método, é outra implicação da chave de fechamento lançar uma exceção:

```csharp
using (CalculatorClient client = new CalculatorClient())
{
    ...
    throw new ApplicationException("Hope this exception was not important, because "+
                                   "it might be masked by the Close exception.");
} // <-- this line might throw an exception.
```

Como o `Dispose` () ocorre dentro de um bloco "finally", o `ApplicationException` nunca é visto fora do bloco Using se o `Dispose` () falhar. Se o código fora precisar saber sobre quando `ApplicationException` ocorre, a construção "using" poderá causar problemas mascarando essa exceção.

Por fim, o exemplo demonstra como limpar corretamente quando ocorrem exceções no `DemonstrateCleanupWithExceptions` . Isso usa um bloco try/catch para relatar erros e chamar `Abort` . Consulte o exemplo de [exceções esperadas](expected-exceptions.md) para obter mais detalhes sobre a captura de exceções de chamadas de cliente.

```csharp
try
{
    ...
    client.Close();
}
catch (CommunicationException e)
{
    ...
    client.Abort();
}
catch (TimeoutException e)
{
    ...
    client.Abort();
}
catch (Exception e)
{
    ...
    client.Abort();
    throw;
}
```

> [!NOTE]
> A instrução using e ServiceHost: muitos aplicativos de hospedagem interna fazem pouco mais do que hospedar um serviço, e ServiceHost. Close raramente gera uma exceção, de modo que esses aplicativos podem usar com segurança a instrução using com ServiceHost. No entanto, lembre-se de que ServiceHost. Close pode lançar um `CommunicationException` , portanto, se o seu aplicativo continuar depois de fechar o ServiceHost, você deverá evitar a instrução using e seguir o padrão fornecido anteriormente.

Quando você executa o exemplo, as respostas e as exceções da operação são exibidas na janela do console do cliente.

O processo do cliente executa três cenários, cada um dos quais tenta chamar `Divide` . O primeiro cenário demonstra que o código está sendo ignorado devido a uma exceção de `Dispose` (). O segundo cenário demonstra uma exceção importante sendo mascarada devido a uma exceção de `Dispose` (). O terceiro cenário demonstra a limpeza correta.

A saída esperada do processo do cliente é:

```console
=
= Demonstrating problem:  closing brace of using statement can throw.
=
Got System.ServiceModel.CommunicationException from Divide.
Got System.ServiceModel.Security.MessageSecurityException
=
= Demonstrating problem:  closing brace of using statement can mask other Exceptions.
=
Got System.ServiceModel.CommunicationException from Divide.
Got System.ServiceModel.Security.MessageSecurityException
=
= Demonstrating cleanup with Exceptions.
=
Calling client.Add(0.0, 0.0);
        client.Add(0.0, 0.0); returned 0
Calling client.Divide(0.0, 0.0);
Got System.ServiceModel.CommunicationException from Divide.

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
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Client\UsingUsing`
