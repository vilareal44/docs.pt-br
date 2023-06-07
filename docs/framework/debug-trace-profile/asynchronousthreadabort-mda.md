---
title: MDA asynchronousThreadAbort
description: Examine como o MDA (Assistente de depuração gerenciada) asynchronousThreadAbort é ativado quando um thread tenta colocar uma anulação assíncrona em outro thread.
ms.date: 03/30/2017
helpviewer_keywords:
- asynchronous thread aborts
- AsynchronousThreadAbort MDA
- managed debugging assistants (MDAs), asynchronous thread aborts
- threading [.NET Framework], managed debugging assistants
- MDAs (managed debugging assistants), asynchronous thread aborts
ms.assetid: 9ebe40b2-d703-421e-8660-984acc42bfe0
ms.openlocfilehash: 469372d57d9c21198353d171fec16458691eb25d
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85415661"
---
# <a name="asynchronousthreadabort-mda"></a>MDA asynchronousThreadAbort
O MDA (assistente para depuração gerenciada) `asynchronousThreadAbort` é ativado quando um thread tenta introduzir uma anulação assíncrona em outro thread. Anulações de thread síncronas não ativam o MDA `asynchronousThreadAbort`.

## <a name="symptoms"></a>Sintomas
 Um aplicativo falha com uma <xref:System.Threading.ThreadAbortException> sem tratamento quando o thread do aplicativo principal é anulado. Se o aplicativo precisar continuar sendo executado, as consequências poderão ser piores do que uma falha do aplicativo, possivelmente resultando em mais dados corrompidos.

 As operações que devem ser atômicas provavelmente foram interrompidas após a conclusão parcial, deixando os dados do aplicativo em um estado imprevisível. Uma <xref:System.Threading.ThreadAbortException> pode ser gerada com base em pontos aparentemente aleatórios na execução do código, geralmente em locais dos quais uma exceção não deve surgir. O código pode não ter a capacidade de manipular uma exceção como essa, resultando em um estado corrompido.

 Os sintomas podem variar muito devido à aleatoriedade inerente ao problema.

## <a name="cause"></a>Causa
 O código em um thread chamou o método <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> em um thread de destino para introduzir uma anulação de thread assíncrona. A anulação de thread é assíncrona porque o código que faz a chamada a <xref:System.Threading.Thread.Abort%2A> está em execução em um thread diferente do que o destino da operação de anulação. As anulações de thread síncronas não devem causar um problema porque o thread que executa a <xref:System.Threading.Thread.Abort%2A> deveria ter feito isso somente em um ponto de verificação seguro em que o estado do aplicativo é consistente.

 Anulações de thread assíncrono apresentam um problema porque são processadas em pontos imprevisíveis na execução do thread de destino. Para evitar isso, o código escrito para ser executado em um thread que pode ser anulado dessa maneira precisará manipular uma <xref:System.Threading.ThreadAbortException> em quase todas as linhas de código, tomando cuidado para colocar os dados do aplicativo novamente em um estado consistente. Não é realista esperar que o código seja escrito com esse problema em mente ou escrever um código que proteja contra todas as circunstâncias possíveis.

 As chamadas no código não gerenciado e os blocos `finally` não serão anulados de forma assíncrona, mas imediatamente após a saída de uma dessas categorias.

 A causa pode ser difícil de ser determinada devido à aleatoriedade inerente ao problema.

## <a name="resolution"></a>Resolução
 Evite o design de código que exige o uso de anulações de thread assíncronas. Há várias abordagens mais apropriadas para a interrupção de um thread de destino que não exigem uma chamada a <xref:System.Threading.Thread.Abort%2A>. A mais segura é introduzir um mecanismo, como uma propriedade comum, que sinaliza o thread de destino a solicitar uma interrupção. O thread de destino verifica o sinal em determinados pontos de verificação seguros. Se ele observa que uma interrupção foi solicitada, ele pode ser desligado normalmente.

## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime
 Esse MDA não tem efeito sobre o CLR. Ele apenas relata dados sobre as anulações de thread assíncronas.

## <a name="output"></a>Saída
 O MDA relata a ID do thread que executa a anulação e a ID do thread que é o destino da anulação. Elas nunca serão as mesmas porque isso é limitado a anulações assíncronas.

## <a name="configuration"></a>Configuração

```xml
<mdaConfig>
  <assistants>
    <asynchronousThreadAbort />
  </assistants>
</mdaConfig>
```

## <a name="example"></a>Exemplo
 A ativação do MDA `asynchronousThreadAbort` exige apenas uma chamada a <xref:System.Threading.Thread.Abort%2A> em um thread de execução separado. Considere as consequências se o conteúdo da função de início do thread fosse um conjunto de operações mais complexas que poderiam ser interrompidas em qualquer ponto arbitrário pela anulação.

```csharp
using System.Threading;
void FireMda()
{
    Thread t = new Thread(delegate() { Thread.Sleep(1000); });
    t.Start();
    // The following line activates the MDA.
    t.Abort();
    t.Join();
}
```

## <a name="see-also"></a>Veja também

- <xref:System.Threading.Thread>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
