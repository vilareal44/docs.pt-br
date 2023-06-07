---
ms.openlocfilehash: a8f51dfa1c82e3b166449d2432dfe8a9b96564b9
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619774"
---
### <a name="blockingcollectionlttgttrytakefromany-does-not-throw-anymore"></a>BlockingCollection&lt;T&gt;.TryTakeFromAny não é mais gerado

#### <a name="details"></a>Detalhes

Se uma das coleções de entrada for marcada como concluída, <xref:System.Collections.Concurrent.BlockingCollection%601.TryTakeFromAny(System.Collections.Concurrent.BlockingCollection{%600}[],%600@)> não retornará mais -1 e <xref:System.Collections.Concurrent.BlockingCollection%601.TakeFromAny(System.Collections.Concurrent.BlockingCollection{%600}[],%600@)> não vai mais gerar uma exceção. Essa alteração possibilita trabalhar com coleções quando uma das coleções está vazia ou concluída, mas a outra coleção ainda possui itens que podem ser recuperados.

#### <a name="suggestion"></a>Sugestão

Se o retorno de -1 de TryTakeFromAny ou a geração de TakeFromAny foram usados para fins de fluxo de controle no caso de conclusão de uma coleção de blocos, tal código agora deverá ser alterado para usar <code>.Any(b =&gt; b.IsCompleted)</code> a fim de detectar essa condição.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Collections.Concurrent.BlockingCollection%601.TakeFromAny(System.Collections.Concurrent.BlockingCollection{%600}[],%600@)?displayProperty=nameWithType></li><li><xref:System.Collections.Concurrent.BlockingCollection%601.TakeFromAny(System.Collections.Concurrent.BlockingCollection{%600}[],%600@,System.Threading.CancellationToken)?displayProperty=nameWithType></li><li><xref:System.Collections.Concurrent.BlockingCollection%601.TryTakeFromAny(System.Collections.Concurrent.BlockingCollection{%600}[],%600@)?displayProperty=nameWithType></li><li><xref:System.Collections.Concurrent.BlockingCollection%601.TryTakeFromAny(System.Collections.Concurrent.BlockingCollection{%600}[],%600@,System.Int32)?displayProperty=nameWithType></li><li><xref:System.Collections.Concurrent.BlockingCollection%601.TryTakeFromAny(System.Collections.Concurrent.BlockingCollection{%600}[],%600@,System.TimeSpan)?displayProperty=nameWithType></li><li><xref:System.Collections.Concurrent.BlockingCollection%601.TryTakeFromAny(System.Collections.Concurrent.BlockingCollection{%600}[],%600@,System.TimeSpan)?displayProperty=nameWithType></li></ul>|
