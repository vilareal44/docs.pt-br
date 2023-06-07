---
title: Como usar SpinWait para implementar uma operação bifásica de espera
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SpinWait, how to synchronize two-phase wait
ms.assetid: b2ac4e4a-051a-4f65-b4b9-f8e103aff195
ms.openlocfilehash: 4b2bc79a7b652c34334d5a78d9c9af328993ff44
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84279200"
---
# <a name="how-to-use-spinwait-to-implement-a-two-phase-wait-operation"></a>Como usar SpinWait para implementar uma operação bifásica de espera
O exemplo a seguir mostra como usar um objeto <xref:System.Threading.SpinWait?displayProperty=nameWithType> para implementar uma operação de espera de duas fases. Na primeira fase, o objeto de sincronização, um `Latch`, gira por alguns ciclos enquanto verifica se o bloqueio ficou disponível. Na segunda fase, se o bloqueio ficar disponível, o método `Wait` retorna sem usar <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> para executar sua espera; caso contrário, `Wait` executa a espera.  
  
## <a name="example"></a>Exemplo  
 Este exemplo mostra uma implementação muito básica de uma primitivo de sincronização de Trava. Você pode usar essa estrutura de dados quando houver a expectativa de tempos de espera muito curtos. Este exemplo é para fins de demonstração. Se você precisar de funcionalidade do tipo trava em seu programa, considere o uso de <xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType>.  
  
 [!code-csharp[CDS_SpinWait#03](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_spinwait/cs/spinwait03.cs#03)]
 [!code-vb[CDS_SpinWait#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_spinwait/vb/spinwait2.vb#03)]  
  
 A trava usa o objeto <xref:System.Threading.SpinWait> para executar somente no local até que a próxima chamada a `SpinOnce` faça com que <xref:System.Threading.SpinWait> gere a fração de tempo do thread. Nesse ponto, a trava causa sua própria alternância de contexto chamando <xref:System.Threading.WaitHandle.WaitOne%2A> no <xref:System.Threading.ManualResetEvent>, e passando o restante do valor de tempo limite.  
  
 A saída do log mostra com que frequência a trava conseguiu aumentar o desempenho adquirindo o bloqueio sem usar o <xref:System.Threading.ManualResetEvent>.  
  
## <a name="see-also"></a>Veja também

- [SpinWait](spinwait.md)
- [Objetos e recursos de Threading](threading-objects-and-features.md)
