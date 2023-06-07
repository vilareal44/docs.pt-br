---
title: 'Como: Especificar opções de mesclagem em PLINQ'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to use merge options
ms.assetid: 0f33b527-e91a-4550-a39a-e63e396fd831
ms.openlocfilehash: 84667fa1fbe2966c580d9c6d32e52ed686af7bb3
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288115"
---
# <a name="how-to-specify-merge-options-in-plinq"></a>Como: Especificar opções de mesclagem em PLINQ
Este exemplo mostra como especificar as opções de mesclagem que se aplicarão a todos os operadores subsequentes em uma consulta PLINQ. Você não precisa definir as opções de mesclagem explicitamente, mas fazer isso pode melhorar o desempenho. Para saber mais sobre opções de mesclagem, veja [Opções de mesclagem no PLINQ](merge-options-in-plinq.md).  
  
> [!WARNING]
> Este exemplo tem como objetivo demonstrar o uso e pode não executar tão rápido quanto a consulta LINQ to Objects sequencial equivalente. Para saber mais sobre agilização, confira [Noções básicas sobre agilização no PLINQ](understanding-speedup-in-plinq.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra o comportamento das opções de mesclagem em um cenário básico que tem uma fonte não ordenada e aplica uma função cara para todo elemento.  
  
 [!code-csharp[PLINQ#23](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#23)]
 [!code-vb[PLINQ#23](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#23)]  
  
 Em casos onde a opção <xref:System.Linq.ParallelMergeOptions.AutoBuffered> resulta em uma latência indesejável antes da produção do primeiro elemento, tente a opção <xref:System.Linq.ParallelMergeOptions.NotBuffered> para produz elementos de resultado mais rápido e mais fácil.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Linq.ParallelMergeOptions>
- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
