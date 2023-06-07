---
ms.openlocfilehash: 1d7dc808d5b514acc582675d6ccdbd5778314624
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619787"
---
### <a name="systemuri-escaping-now-supports-rfc-3986"></a>O escape de System.Uri agora é compatível com RFC 3986

#### <a name="details"></a>Detalhes

O escape do URI foi alterado no .NET Framework 4.5 para atender ao [RFC 3986](https://tools.ietf.org/html/rfc3986). As alterações específicas incluem:<ul><li><xref:System.Uri.EscapeDataString(System.String)?displayProperty=fullName> escapa caracteres reservados com base na RFC 3986.</li><li><xref:System.Uri.EscapeUriString(System.String)?displayProperty=fullName> não escapa caracteres reservados.</li><li><xref:System.Uri.UnescapeDataString(System.String)?displayProperty=fullName> não gerará uma exceção se encontrar uma sequência de escape inválida.</li><li>Os caracteres escapados não reservados não são escapados.</li></ul>

#### <a name="suggestion"></a>Sugestão

<ul><li>Atualize os aplicativos para não dependerem da geração de <xref:System.Uri.UnescapeDataString(System.String)?displayProperty=fullName> no caso de uma sequência de escape inválida. Essas sequências devem ser detectadas diretamente agora.</li><li>Da mesma forma, o URI com escape e sem escape, bem como as cadeias de caracteres de dados, podem variar entre o .NET Framework 4.0 e o .NET Framework 4.5 e não devem ser comparados diretamente entre as versões do .NET. Em vez disso, eles devem ser analisados e normalizados em uma única versão do .NET antes que qualquer comparação seja feita.</li></ul>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Uri.EscapeDataString(System.String)?displayProperty=nameWithType></li><li><xref:System.Uri.EscapeUriString(System.String)?displayProperty=nameWithType></li><li><xref:System.Uri.UnescapeDataString(System.String)?displayProperty=nameWithType></li></ul>|
