---
title: Pausando e interrompendo threads
description: Saiba como pausar & threads de interrupção no .NET. Saiba como usar métodos como thread. Sleep & thread. Interrupt, & exceções como ThreadInterruptedException.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- interrupting threads
- threading [.NET Framework], pausing
- pausing threads
ms.assetid: 9fce4859-a19d-4506-b082-7dd0792688ca
ms.openlocfilehash: f7f414ec716bac5f1e840c5e8a0946024e059fb6
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769113"
---
# <a name="pausing-and-interrupting-threads"></a>Pausando e interrompendo threads

As formas mais comuns para sincronizar as atividades de threads são segmentos de bloco e versão ou objetos de bloqueio ou regiões de código. Para saber mais sobre esse bloqueio e os mecanismos de bloqueio, confira [Visão geral de primitivos de sincronização](overview-of-synchronization-primitives.md).  
  
 Você também pode fazer com que threads se coloquem no modo de suspensão. Quando os threads estão bloqueados ou suspensos, você pode usar um <xref:System.Threading.ThreadInterruptedException> para removê-los dos estados de espera.  
  
## <a name="the-threadsleep-method"></a>O método Thread.Sleep

 Chamar o método <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> faz com que o thread atual seja bloqueado imediatamente pelo número de milissegundos ou o intervalo de tempo que você passa para o método e produza o resto do intervalo de tempo para outro thread. Depois que o intervalo expira, o thread em suspensão retoma a execução.  
  
 Um thread não pode chamar <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> em outro thread.  <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> é um método estático que sempre faz com que o thread atual entre no modo de suspensão.  
  
 Chamar <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> com um valor <xref:System.Threading.Timeout.Infinite?displayProperty=nameWithType> faz com que um thread seja suspenso até que ele seja interrompido por outro thread que chame o método <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> no thread em suspensão ou até que ele seja encerrado por uma chamada ao método <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>.  O exemplo a seguir ilustra os dois métodos para interromper um thread suspenso.  
  
 [!code-csharp[Conceptual.Threading.Resuming#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.Threading.Resuming/cs/Sleep1.cs#1)]
 [!code-vb[Conceptual.Threading.Resuming#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.Threading.Resuming/vb/Sleep1.vb#1)]  
  
## <a name="interrupting-threads"></a>Interrompendo threads

 Você pode interromper um thread em espera chamando o método <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> no thread bloqueado para lançar um <xref:System.Threading.ThreadInterruptedException>, o que remove o thread da chamada de bloqueio. O thread deve capturar o <xref:System.Threading.ThreadInterruptedException> e fazer o que for apropriado para continuar funcionando. Se o thread ignorar a exceção, o runtime capturará a exceção e interromperá o thread.  
  
> [!NOTE]
> Se o thread de destino não for bloqueado quando <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> for chamado, o thread não será interrompido até ser bloqueado. Se o thread nunca for bloqueado, ele poderá ser concluído sem nunca ser interrompido.  
  
 Se uma espera for uma espera gerenciada, <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> e <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> ativarão o thread imediatamente. Se uma espera for uma espera não gerenciada (por exemplo, uma chamada de invocação de plataforma para a função Win32 [WaitForSingleObject](/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobject)), <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> e <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> não poderão assumir o controle do thread até que ele chame o código gerenciado ou retorne a ele. No código gerenciado, o comportamento é o seguinte:  
  
- <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> ativa um thread de qualquer espera em que ele possa estar e faz com que um <xref:System.Threading.ThreadInterruptedException> seja gerado no thread de destino.  
  
- <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> ativa um thread de qualquer espera em que ele possa estar e faz com que um <xref:System.Threading.ThreadAbortException> seja gerado no thread. Para obter detalhes, confira [Destruindo threads](destroying-threads.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Threading.Thread>
- <xref:System.Threading.ThreadInterruptedException>
- <xref:System.Threading.ThreadAbortException>
- [Threading](index.md)
- [Usando threads e threading](using-threads-and-threading.md)
- [Visão geral de primitivos de sincronização](overview-of-synchronization-primitives.md)
