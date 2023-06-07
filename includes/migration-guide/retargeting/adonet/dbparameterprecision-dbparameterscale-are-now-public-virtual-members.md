---
ms.openlocfilehash: 063e10b0310880af255793215a80a5529a5db0ff
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616019"
---
### <a name="dbparameterprecision-and-dbparameterscale-are-now-public-virtual-members"></a>DbParameter.Precision e DbParameter.Scale agora são membros virtuais públicos

#### <a name="details"></a>Detalhes

<xref:System.Data.Common.DbParameter.Precision> e <xref:System.Data.Common.DbParameter.Scale> são implementados como propriedades virtuais públicas. Elas substituem as implementações de interface explícitas correspondentes, <xref:System.Data.Common.DbParameter.System%23Data%23IDbDataParameter%23Precision> e <xref:System.Data.Common.DbParameter.System%23Data%23IDbDataParameter%23Scale>.

#### <a name="suggestion"></a>Sugestão

Ao recompilar um provedor de banco de dados ADO.NET, essas diferenças exigirão que a palavra-chave "override" seja aplicada às propriedades Precision e Scale. Isso é necessário somente na recompilação dos componentes; binários existentes continuarão funcionando.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.5.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Data.Common.DbParameter.Precision?displayProperty=nameWithType>
- <xref:System.Data.Common.DbParameter.Scale?displayProperty=nameWithType>
