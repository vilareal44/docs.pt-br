---
ms.openlocfilehash: 02a15f6b9c02002b60c568b9e1d871af49744092
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621923"
---
### <a name="concurrentqueuelttgttrypeek-can-return-an-erroneous-null-via-its-out-parameter"></a>ConcurrentQueue&lt;T&gt;.TryPeek pode retornar um nulo errado por meio de seus parâmetros de saída

#### <a name="details"></a>Detalhes

Em alguns cenários multithread, <xref:System.Collections.Concurrent.ConcurrentQueue%601.TryPeek(%600@)?displayProperty=fullName> pode retornar true, mas popular o parâmetro de saída com um valor nulo (em vez do valor correto, inspecionado).

#### <a name="suggestion"></a>Sugestão

Esse problema foi corrigido no .NET Framework 4.5.1. O upgrade para esse Framework resolverá o problema.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Principal|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Collections.Concurrent.ConcurrentQueue%601.TryPeek(%600@)?displayProperty=nameWithType></li></ul>|
