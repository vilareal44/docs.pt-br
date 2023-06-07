---
title: 'Como: avaliar o desempenho da consulta PLINQ'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to measure performance
ms.assetid: 491ba43b-2c10-473d-9aab-e2cb96446711
ms.openlocfilehash: f240b2c275305aec5699eb42406e0689925490a8
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288180"
---
# <a name="how-to-measure-plinq-query-performance"></a>Como: avaliar o desempenho da consulta PLINQ

Este exemplo mostra como usar a <xref:System.Diagnostics.Stopwatch> classe para medir o tempo necessário para que uma consulta PLINQ seja executada.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa um loop `foreach` vazio (`For Each` no Visual Basic) para medir o tempo necessário de execução da consulta. No código da vida real, o loop normalmente contém etapas de processamento adicionais que se juntam ao tempo de execução total de consultas. Observe que o cronômetro não é iniciado até logo antes do loop, porque é quando a execução da consulta é iniciada. Se você precisar de uma medição mais refinada, use a propriedade `ElapsedTicks` em vez de `ElapsedMilliseconds`.  
  
 [!code-csharp[PLINQ#19](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/measure2.cs#19)]
 [!code-vb[PLINQ#19](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/measure2.vb#19)]  
  
 O tempo de execução total é uma métrica útil quando você está experimentando implementações de consulta, mas nem sempre diz a história inteira. Para obter uma visão mais profunda e mais rica da interação dos threads de consulta entre si e com outros processos em execução, use o [Visualizador de simultaneidade](/visualstudio/profiling/concurrency-visualizer).  
  
## <a name="see-also"></a>Veja também

- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
