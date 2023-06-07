---
ms.openlocfilehash: f7dcf9c4c3dc7ea536ddc847769a1a30f1298bb2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617109"
---
### <a name="systemuriiswellformeduristring-method-returns-false-for-relative-uris-with-a-colon-char-in-first-segment"></a>Método System.Uri.IsWellFormedUriString retorna false para URIs com dois-pontos no primeiro segmento

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.5, <xref:System.Uri.IsWellFormedUriString(System.String,System.UriKind)> tratará URIs relativos com um `:` no primeiro segmento como malformados. Essa é uma alteração do comportamento de <xref:System.Uri.IsWellFormedUriString(System.String,System.UriKind)?displayProperty=fullName> no .NET Framework 4.0, realizada para manter a conformidade com a RFC3986.

#### <a name="suggestion"></a>Sugestão

Essa alteração (assim como várias outras alterações de URI) afetará somente os aplicativos do .NET Framework 4.5 ou versões posteriores. Para continuar usando o comportamento antigo, direcione o aplicativo para o .NET Framework 4.0. Como alternativa, verifique se há caracteres `:` no URI que você deseja remover para fins de validação antes de chamar <xref:System.Uri.IsWellFormedUriString(System.String,System.UriKind)?displayProperty=fullName> se quiser usar o comportamento antigo.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.5         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Uri.IsWellFormedUriString(System.String,System.UriKind)?displayProperty=nameWithType>
