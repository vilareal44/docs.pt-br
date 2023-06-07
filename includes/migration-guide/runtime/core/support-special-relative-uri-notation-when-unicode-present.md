---
ms.openlocfilehash: 08a9292c5a41e7b9b7c1bcc18ec96460de19863f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621012"
---
### <a name="support-special-relative-uri-notation-when-unicode-is-present"></a>Permitir notação de URI relativo especial quando o Unicode estiver presente

#### <a name="details"></a>Detalhes

<xref:System.Uri>não gerará mais um <xref:System.NullReferenceException> ao chamar <xref:System.Uri.TryCreate%2A> em determinados URIs relativos que contenham Unicode. A reprodução mais simples do <xref:System.NullReferenceException> está abaixo, com as duas instruções sendo equivalentes:<pre><code class="lang-csharp">bool success = Uri.TryCreate(&quot;http:%C3%A8&quot;, UriKind.RelativeOrAbsolute, out Uri href);&#13;&#10;bool success = Uri.TryCreate(&quot;http:&#232;&quot;, UriKind.RelativeOrAbsolute, out Uri href);&#13;&#10;</code></pre>Para reproduzir a <xref:System.NullReferenceException>, os itens a seguir precisam ser verdadeiros:<ul><li>O URI precisa ser especificado como relativo acrescentando 'http:' a ele e não inserindo '//' depois dele.</li><li>O URI precisa conter Unicode codificado por porcentagem ou símbolos não reservados.</li></ul>

#### <a name="suggestion"></a>Sugestão

Os usuários que dependem desse comportamento para não permitir URIs relativos precisam especificar <xref:System.UriKind.Absolute?displayProperty=nameWithType> durante a criação de um URI.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.7.2|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Uri.TryCreate(System.Uri,System.Uri,System.Uri@)?displayProperty=nameWithType></li><li><xref:System.Uri.TryCreate(System.String,System.UriKind,System.Uri@)?displayProperty=nameWithType></li><li><xref:System.Uri.TryCreate(System.Uri,System.String,System.Uri@)?displayProperty=nameWithType></li></ul>|
