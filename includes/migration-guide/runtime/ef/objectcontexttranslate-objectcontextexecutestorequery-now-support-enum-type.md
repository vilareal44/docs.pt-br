---
ms.openlocfilehash: 6af8e97423c04abca25feb8d77ea9ab6e198a4f0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619800"
---
### <a name="objectcontexttranslate-and-objectcontextexecutestorequery-now-support-enum-type"></a>ObjectContext.Translate e ObjectContext.ExecuteStoreQuery agora dão suporte ao tipo de enumeração

#### <a name="details"></a>Detalhes

No .NET Framework 4.0, o parâmetro genérico <code>T</code> dos métodos <code>ObjectContext.Translate</code> e <code>ObjectContext.ExecuteStoreQuery</code> não pode ser uma enumeração. Agora há suporte para esse cenário.

#### <a name="suggestion"></a>Sugestão

Quando Translate ou ExecuteStoreQuery era chamado em um tipo de enumeração no .NET Framework 4.0, '0' era retornado. Se esse comportamento fosse desejável, as chamadas deveriam ser substituídas por uma constante 0 (ou o equivalente de enumeração dele).

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime|

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Data.Objects.ObjectContext.Translate%60%601(System.Data.Common.DbDataReader)?displayProperty=nameWithType></li><li><xref:System.Data.Objects.ObjectContext.Translate%60%601(System.Data.Common.DbDataReader,System.String,System.Data.Objects.MergeOption)?displayProperty=nameWithType></li><li><xref:System.Data.Objects.ObjectContext.ExecuteStoreQuery%60%601(System.String,System.Object[])?displayProperty=nameWithType></li><li><xref:System.Data.Objects.ObjectContext.ExecuteStoreQuery%60%601(System.String,System.String,System.Data.Objects.MergeOption,System.Object[])?displayProperty=nameWithType></li></ul>|
