---
title: 'Como: Cancelar um loop Parallel.For ou ForEach'
description: Cancele um loop Parallel. for ou Parallel. ForEach no .NET fornecendo um objeto de token de cancelamento para o método no parâmetro ParallelOptions.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel foreach loop, how to cancel
- parallel for loops, how to cancel
ms.assetid: 9d19b591-ea95-4418-8ea7-b6266af9905b
ms.openlocfilehash: 0a22794f3c45e685a80d36a42ecd849461936c7b
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768983"
---
# <a name="how-to-cancel-a-parallelfor-or-foreach-loop"></a>Como: Cancelar um loop Parallel.For ou ForEach
Os métodos <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> oferecem suporte ao cancelamento por meio do uso de tokens de cancelamento. Para saber mais sobre cancelamento em geral, confira [Cancelamento](../threading/cancellation-in-managed-threads.md). Em um loop paralelo, você fornece <xref:System.Threading.CancellationToken> para o método no parâmetro <xref:System.Threading.Tasks.ParallelOptions> e, em seguida, coloca a chamada paralela em um bloco try-catch.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como cancelar uma chamada para <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType>. Você pode aplicar a mesma abordagem a uma chamada <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType>.  
  
 [!code-csharp[TPL_Parallel#29](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/parallel_cancel.cs#29)]
 [!code-vb[TPL_Parallel#29](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/cancelloop.vb#29)]  
  
 Se o token que sinaliza o cancelamento for o mesmo token que é especificado na instância <xref:System.Threading.Tasks.ParallelOptions>, o loop paralelo lançará uma única <xref:System.OperationCanceledException> no cancelamento. Se algum outro token causar o cancelamento, o loop lançará uma <xref:System.AggregateException> com uma <xref:System.OperationCanceledException> como um InnerException.  
  
## <a name="see-also"></a>Veja também

- [Paralelismo de dados](data-parallelism-task-parallel-library.md)
- [Expressões lambda em PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md)
