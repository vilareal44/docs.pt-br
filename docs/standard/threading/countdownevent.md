---
title: CountdownEvent
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- synchronization primitives, CountdownEvent
ms.assetid: eec3812a-e20f-4ecd-bfef-6921d508b708
ms.openlocfilehash: 8ed1414ad377015400d9e126d924bf426fbc753d
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84277850"
---
# <a name="countdownevent"></a>CountdownEvent
<xref:System.Threading.CountdownEvent?displayProperty=nameWithType> é um primitivo de sincronização que desbloqueia seus threads em espera após ser sinalizado um determinado número de vezes. <xref:System.Threading.CountdownEvent> foi projetado para cenários em que, caso contrário, seria necessário usar um <xref:System.Threading.ManualResetEvent> ou um <xref:System.Threading.ManualResetEventSlim> e diminuir manualmente uma variável antes de sinalizar o evento. Por exemplo, em um cenário de bifurcação/junção, basta criar um <xref:System.Threading.CountdownEvent> que tenha uma contagem de sinal de 5 e, em seguida, iniciar cinco itens de trabalho no pool de threads e fazer com que cada item de trabalho chame <xref:System.Threading.CountdownEvent.Signal%2A> quando ele for concluído. Cada chamada para <xref:System.Threading.CountdownEvent.Signal%2A> diminui a contagem de sinal em 1. No thread principal, a chamada para <xref:System.Threading.CountdownEvent.Wait%2A> será bloqueada até que a contagem de sinal seja zero.  
  
> [!NOTE]
> No caso do código que não têm que interagir com as APIs de sincronização herdadas do .NET Framework, considere o uso de <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> objetos ou o método <xref:System.Threading.Tasks.Parallel.Invoke%2A> para uma abordagem ainda mais fácil para expressar o paralelismo bifurcação-junção.  
  
 <xref:System.Threading.CountdownEvent> tem estes recursos adicionais:  
  
- A operação de espera pode ser cancelada usando tokens de cancelamento.  
  
- Sua contagem de sinal pode ser incrementada depois que a instância for criada.  
  
- As instâncias podem ser reutilizadas após <xref:System.Threading.CountdownEvent.Wait%2A> ser retornado ao chamar o método <xref:System.Threading.CountdownEvent.Reset%2A>.  
  
- As instâncias expõem um <xref:System.Threading.WaitHandle> para integração com outra APIs de sincronização do .NET Framework, como <xref:System.Threading.WaitHandle.WaitAll%2A>.  
  
## <a name="basic-usage"></a>Uso básico  
 O exemplo a seguir demonstra como usar um <xref:System.Threading.CountdownEvent> com itens de trabalho <xref:System.Threading.ThreadPool>.  
  
 [!code-csharp[CDS_CountdownEvent#01](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_countdownevent/cs/countdownevent.cs#01)]
 [!code-vb[CDS_CountdownEvent#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_countdownevent/vb/module1.vb#01)]  
  
## <a name="countdownevent-with-cancellation"></a>CountdownEvent com cancelamento  
 O exemplo a seguir mostra como cancelar a operação de espera em <xref:System.Threading.CountdownEvent> usando um token de cancelamento. O padrão básico segue o modelo do cancelamento unificado introduzido no .NET Framework 4. Para obter mais informações, consulte [cancelamento em threads gerenciados](cancellation-in-managed-threads.md).  
  
 [!code-csharp[CDS_CountdownEvent#02](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_countdownevent/cs/countdownevent.cs#02)]
 [!code-vb[CDS_CountdownEvent#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_countdownevent/vb/canceleventwait.vb#02)]  
  
 Observe que a operação de espera não cancela os threads que a estão sinalizando. Normalmente, o cancelamento é aplicado a uma operação lógica e isso pode incluir aguardar o evento e todos os itens de trabalho que a espera está sincronizando. Neste exemplo, cada item de trabalho passa uma cópia do mesmo token de cancelamento para que ele possa responder à solicitação de cancelamento.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Threading.Semaphore?displayProperty=nameWithType>
