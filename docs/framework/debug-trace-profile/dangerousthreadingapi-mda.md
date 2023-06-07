---
title: MDA dangerousThreadingAPI
description: Examine o MDA (Assistente de depuração gerenciada) dangerousThreadingAPI, que é ativado quando thread. Suspend é chamado em um thread diferente do thread atual.
ms.date: 03/30/2017
helpviewer_keywords:
- suspending threads
- DangerousThreadingAPI MDA
- managed debugging assistants (MDAs), dangerous threading operations
- threading [.NET Framework], suspending
- MDAs (managed debugging assistants), dangerous threading operations
- Suspend method
- threading [.NET Framework], managed debugging assistants
ms.assetid: 3e5efbc5-92e4-4229-b31f-ce368a1adb96
ms.openlocfilehash: 9069ccb6f106c83db94f88bc464bc0888d28586c
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85415999"
---
# <a name="dangerousthreadingapi-mda"></a>MDA dangerousThreadingAPI
O MDA (assistente para depuração gerenciada) `dangerousThreadingAPI` é ativado quando o método <xref:System.Threading.Thread.Suspend%2A?displayProperty=nameWithType> é chamado em um thread que não seja o thread atual.  
  
## <a name="symptoms"></a>Sintomas  
 Um aplicativo não está respondendo ou para de responder por tempo indefinido. Os dados do sistema ou do aplicativo podem ser deixados em um estado imprevisível temporariamente ou mesmo depois que um aplicativo foi desligado. Algumas operações não são concluídas como esperado.  
  
 Os sintomas podem variar muito devido à aleatoriedade inerente ao problema.  
  
## <a name="cause"></a>Causa  
 Um thread é suspenso de forma assíncrona por outro thread usando o método <xref:System.Threading.Thread.Suspend%2A>. Não há nenhuma maneira de determinar quando é seguro suspender outro thread que pode estar no meio de uma operação. A suspensão do thread pode resultar na corrupção de dados ou na interrupção de invariáveis. Caso um thread precise ser colocado em um estado suspenso e nunca ser retomado usando o método <xref:System.Threading.Thread.Resume%2A>, o aplicativo poderá parar de responder por tempo indefinido e, possivelmente, danificar os dados do aplicativo. Esses métodos foram marcados como obsoletos.  
  
 Se os primitivos de sincronização forem mantidos pelo thread de destino, eles permanecerão mantidos durante a suspensão. Isso pode levar a deadlocks, caso outro thread, por exemplo, o thread que executa o <xref:System.Threading.Thread.Suspend%2A>, tente adquirir um bloqueio no primitivo. Nessa situação, o problema se manifesta como um deadlock.  
  
## <a name="resolution"></a>Resolução  
 Evite designs que exigem o uso de <xref:System.Threading.Thread.Suspend%2A> e <xref:System.Threading.Thread.Resume%2A>. Para cooperação entre threads, use primitivos de sincronização como <xref:System.Threading.Monitor>, <xref:System.Threading.ReaderWriterLock>, <xref:System.Threading.Mutex> ou a instrução `lock` do C#. Se você precisar usar esses métodos, reduza a janela de tempo e minimize a quantidade de código que é executada enquanto o thread está em um estado suspenso.  
  
## <a name="effect-on-the-runtime"></a>Efeito sobre o runtime  
 Esse MDA não tem efeito sobre o CLR. Ele relata apenas os dados sobre operações de threading perigosas.  
  
## <a name="output"></a>Saída  
 O MDA identifica o método de threading perigoso que fez com que ele fosse ativado.  
  
## <a name="configuration"></a>Configuração  
  
```xml  
<mdaConfig>  
  <assistants>  
    <dangerousThreadingAPI />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra uma chamada ao método <xref:System.Threading.Thread.Suspend%2A> que causa a ativação da `dangerousThreadingAPI`.  
  
```csharp
using System.Threading;  
void FireMda()  
{  
Thread t = new Thread(delegate() { Thread.Sleep(1000); });  
    t.Start();  
    // The following line activates the MDA.  
    t.Suspend();
    t.Resume();  
    t.Join();  
}  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Threading.Thread>
- [Diagnosticando erros com assistentes para depuração gerenciada](diagnosing-errors-with-managed-debugging-assistants.md)
- [Instrução lock](../../csharp/language-reference/keywords/lock-statement.md)
