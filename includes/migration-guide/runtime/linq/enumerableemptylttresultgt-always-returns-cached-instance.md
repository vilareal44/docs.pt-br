---
ms.openlocfilehash: 9131c91b34f4c24653dea37ea39af6be6e072287
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619801"
---
### <a name="enumerableemptylttresultgt-always-returns-cached-instance"></a>Enumerable.Empty&lt;TResult&gt; sempre retorna instância em cache

#### <a name="details"></a>Detalhes

Começando com o .NET Framework 4.5, <xref:System.Linq.Enumerable.Empty%60%601> sempre retorna um <xref:System.Collections.Generic.IEnumerable%601> de instância interna em cache. Anteriormente, o <xref:System.Linq.Enumerable.Empty%60%601> armazenava em cache um <xref:System.Collections.Generic.IEnumerable%601> vazio no momento em que a API era chamada, significando que, em algumas condições nas quais <xref:System.Linq.Enumerable.Empty%60%601> era chamado de forma rápida e simultânea, diferentes instâncias do tipo poderiam ser retornadas para diferentes chamadas à API.

#### <a name="suggestion"></a>Sugestão

Como o comportamento anterior não era determinístico, é improvável que o código dependesse dele. No entanto, na improbabilidade de que enumeráveis vazios estivessem sendo comparados e, às vezes, esperados que fosse diferentes, matrizes vazias explícitas deviam ser criadas (<code>new T[0]</code>) em vez de usar <xref:System.Linq.Enumerable.Empty%60%601>.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Linq.Enumerable.Empty%60%601?displayProperty=nameWithType></li></ul>|
