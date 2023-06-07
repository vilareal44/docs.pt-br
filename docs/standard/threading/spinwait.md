---
title: SpinWait
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- synchronization primitives, SpinWait
ms.assetid: 36012f42-34e5-4f86-adf4-973f433ed6c6
ms.openlocfilehash: 8b98e7d8b8ea4578fb446a0587f9a46ba4271348
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291104"
---
# <a name="spinwait"></a>SpinWait
<xref:System.Threading.SpinWait?displayProperty=nameWithType> é um tipo de sincronização leve que você pode usar em cenários de baixo nível a fim de evitar as alternâncias dispendiosas de contexto e transições de kernel necessárias para eventos de kernel. Em computadores com vários núcleos, quando espera-se que um recurso não seja mantido por muito tempo, talvez seja mais eficiente para um thread em espera executar no modo de usuário durante algumas dezenas ou centenas de ciclos e, em seguida, tentar novamente para adquirir o recurso. Se o recurso estiver disponível após a rotação, você terá salvo vários milhares de ciclos. Se o recurso ainda não estiver disponível, você terá passado apenas alguns ciclos e ainda poderá inserir uma espera baseada em kernel. Essa combinação de rotação e espera é, às vezes, conhecida como uma *operação bifásica de espera*.  
  
 O <xref:System.Threading.SpinWait> foi projetado para ser usado em conjunto com os tipos do .NET Framework que encapsulam os eventos de kernel como <xref:System.Threading.ManualResetEvent>. <xref:System.Threading.SpinWait> também pode ser usado por si só para a funcionalidade básica de rotação em apenas um programa.  
  
 <xref:System.Threading.SpinWait> é mais do que apenas um loop vazio. É implementado com cuidado para fornecer o comportamento de rotação correto para o caso geral, e iniciará por conta própria as alternâncias de contexto se ficar em rotação por um tempo suficiente (aproximadamente o período necessário para uma transição de kernel). Por exemplo, em computadores de núcleo único, <xref:System.Threading.SpinWait> produz imediatamente a fatia de tempo do thread, pois os blocos giratório encaminham progresso em todos os threads. <xref:System.Threading.SpinWait> também dá resultado mesmo em máquinas com vários núcleos, a fim de impedir que o thread de espera bloqueie threads de prioridade mais alta ou o coletor de lixo. Portanto, se você estiver usando um <xref:System.Threading.SpinWait> em uma operação de espera de duas fases, recomendamos que você invoque a espera do kernel antes de o próprio <xref:System.Threading.SpinWait> iniciar uma alternância de contexto. <xref:System.Threading.SpinWait> fornece a propriedade <xref:System.Threading.SpinWait.NextSpinWillYield%2A>, a qual você pode verificar antes de cada chamada para <xref:System.Threading.SpinWait.SpinOnce%2A>. Quando a propriedade retornar `true`, inicie sua própria operação de espera. Por exemplo, confira [Como usar SpinWait para implementar uma operação bifásica de espera](how-to-use-spinwait-to-implement-a-two-phase-wait-operation.md).  
  
 Se você não estiver executando uma operação bifásica de espera, mas estiver apenas em rotação até que alguma condição seja verdadeira, permita que <xref:System.Threading.SpinWait> execute suas alternâncias de contexto para que seja um bom cidadão no ambiente do sistema operacional Windows. O exemplo básico a seguir mostra um <xref:System.Threading.SpinWait> em uma pilha sem bloqueio. Se você precisar de uma pilha de alto desempenho e thread-safe, considere o uso de <xref:System.Collections.Concurrent.ConcurrentStack%601?displayProperty=nameWithType>.  
  
 [!code-csharp[CDS_SpinWait#05](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_spinwait/cs/spinwait.cs#05)]
 [!code-vb[CDS_SpinWait#05](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_spinwait/vb/cds_spinwait1.vb#05)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Threading.Thread.SpinWait%2A>
- [Objetos e recursos de Threading](threading-objects-and-features.md)
