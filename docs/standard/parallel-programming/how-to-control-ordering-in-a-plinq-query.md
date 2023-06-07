---
title: 'Como: Controlar a ordem em uma consulta PLINQ'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to control ordering
ms.assetid: c67eccc7-004d-4b2f-987e-919cbbd62ef7
ms.openlocfilehash: 80e199d75471eba219f1f3da12d307b6cd1d90cf
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84285450"
---
# <a name="how-to-control-ordering-in-a-plinq-query"></a>Como: Controlar a ordem em uma consulta PLINQ
Estes exemplos mostram como controlar a ordem em uma consulta PLINQ usando o método de extensão <xref:System.Linq.ParallelEnumerable.AsOrdered%2A>.  
  
> [!WARNING]
> Esses exemplos têm como objetivo principal demonstrar o uso, e talvez não executem tão rápido quanto a consulta LINQ to Objects sequencial equivalente.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir preserva a ordem da sequência de origem. Às vezes, isso é necessário; por exemplo, alguns operadores de consulta exigem uma sequência de origem ordenada para produzir resultados corretos.  
  
 [!code-csharp[PLINQ#12](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#12)]
 [!code-vb[PLINQ#12](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#12)]  
  
## <a name="example"></a>Exemplo  
 A exemplo a seguir mostra alguns operadores de consulta cuja sequência de origem provavelmente deve ser ordenada. Esses operadores funcionarão em sequências não ordenadas, mas podem produzir resultados inesperados.  
  
 [!code-csharp[PLINQ#14](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#14)]
 [!code-vb[PLINQ#14](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#14)]  
  
 Para executar esse método, cole-o na classe PLINQDataSample no projeto [Exemplo de dados PLINQ](plinq-data-sample.md) e pressione F5.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como preservar a ordem da primeira parte de uma consulta e, depois, remover a ordem para aumentar o desempenho de uma cláusula join. Em seguida, reaplicar a ordem à sequência final de resultados.  
  
 [!code-csharp[PLINQ#15](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#15)]
 [!code-vb[PLINQ#15](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#15)]  
  
 Para executar esse método, cole-o na classe PLINQDataSample no projeto [Exemplo de dados PLINQ](plinq-data-sample.md) e pressione F5.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Linq.ParallelEnumerable>
- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
