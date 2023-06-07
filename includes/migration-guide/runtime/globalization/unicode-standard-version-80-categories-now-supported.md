---
ms.openlocfilehash: a20fad5f9c95e59c14ffd91f4921cf8bfab443cd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621013"
---
### <a name="unicode-standard-version-80-categories-now-supported"></a>Categorias do padrão Unicode versão 8.0 agora são compatíveis

#### <a name="details"></a>Detalhes

No .NET Framework 4.6.2, os dados Unicode foram atualizados do Unicode Standard versão 6.3 para a versão 8.0.  Ao solicitar a categorias de caracteres Unicode no .NET Framework 4.6.2, alguns resultados poderão não corresponder aos resultados nas versões anteriores do .NET Framework.  Essa alteração afeta principalmente as sílabas Cheroqui, bem como os símbolos vocálicos e as marcas de tom Tai Lue Novo.

#### <a name="suggestion"></a>Sugestão

Examine o código e remova ou altere a lógica que depende de categorias de caractere Unicode embutido em código.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6.2|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Char.GetUnicodeCategory(System.Char)?displayProperty=nameWithType></li><li><xref:System.Globalization.CharUnicodeInfo.GetUnicodeCategory(System.Char)?displayProperty=nameWithType></li><li><xref:System.Globalization.CharUnicodeInfo.GetUnicodeCategory(System.String,System.Int32)?displayProperty=nameWithType></li></ul>|
