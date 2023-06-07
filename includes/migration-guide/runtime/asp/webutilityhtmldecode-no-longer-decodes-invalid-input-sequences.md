---
ms.openlocfilehash: 0b7d6d9543035ab0a8fdda675ae71572ace12a1f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619768"
---
### <a name="webutilityhtmldecode-no-longer-decodes-invalid-input-sequences"></a>WebUtility.HtmlDecode não decodifica mais sequências de entrada inválidas

#### <a name="details"></a>Detalhes

Por padrão, os métodos de decodificação não decodificam mais uma sequência de entrada válida em uma cadeia de caracteres UTF-16 inválida. Em vez disso, eles retornam a entrada original.

#### <a name="suggestion"></a>Sugestão

A alteração na saída do decodificador deve importar somente se você armazenar dados binários em vez de dados UTF-16 em cadeias de caracteres. Para controlar explicitamente esse comportamento, defina o atributo <code>aspnet:AllowRelaxedUnicodeDecoding</code> do elemento [appSettings](~/docs/framework/configure-apps/file-schema/appsettings/index.md) como <code>true</code> para habilitar o comportamento herdado ou como <code>false</code> para habilitar o comportamento atual.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Net.WebUtility.HtmlDecode(System.String)?displayProperty=nameWithType></li><li><xref:System.Net.WebUtility.HtmlDecode(System.String,System.IO.TextWriter)?displayProperty=nameWithType></li><li><xref:System.Net.WebUtility.UrlDecode(System.String)?displayProperty=nameWithType></li></ul>|
