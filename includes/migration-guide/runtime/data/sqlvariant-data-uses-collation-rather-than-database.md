---
ms.openlocfilehash: d606fbc4048421bc572cfe3db2e06bbcd4529e25
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619792"
---
### <a name="sql_variant-data-uses-sql_variant-collation-rather-than-database-collation"></a>Os dados do Sql_variant usam a ordenação sql_variant em vez da ordenação de banco de dados

#### <a name="details"></a>Detalhes

Os dados do <code>sql_variant</code> usam a ordenação <code>sql_variant</code> em vez da ordenação de banco de dados.

#### <a name="suggestion"></a>Sugestão

Essa alteração aborda os possíveis dados corrompidos caso a ordenação do banco de dados seja diferente da ordenação <code>sql_variant</code>. Os aplicativos que dependem de dados corrompidos podem apresentar falhas.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Transparente|
|Versão|4.5|
|Type|Runtime|
