---
ms.openlocfilehash: 9c3c0bf7fbd8d45eba84eaa8634fd2d834195702
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617108"
---
### <a name="systemuri-parsing-adheres-to-rfc-3987"></a>Análise de System.Uri adere à RFC 3987

#### <a name="details"></a>Detalhes

A análise de URI foi alterada de várias maneiras no .NET Framework 4.5. No entanto, observe que essas alterações afetam somente o código direcionado ao .NET Framework 4.5. Se um binário for direcionado ao .NET Framework 4.0, o comportamento antigo será observado. As alterações na análise de URI no .NET Framework 4.5 incluem:

- A análise de URI executará a normalização e a verificação de caracteres de acordo com as regras mais recentes de URI na RFC 3987.
- O formulário C de normalização Unicode só será executado na parte de host do URI.
- Agora, os URIs mailto: inválidos gerarão uma exceção.
- Os pontos à direita no fim de um segmento de linha agora são preservados.
- Os URIs `file://` não escapam o caractere `?`.
- Os caracteres de controle Unicode `U+0080` a `U+009F` não são compatíveis.
- Os caracteres de vírgula `,` ou `%2c` não fogem ao escape automaticamente.

#### <a name="suggestion"></a>Sugestão

Se a semântica de análise de URI antiga do .NET Framework 4.0 for necessária (geralmente não é), ela poderá ser usada direcionando ao .NET Framework 4.0. Isso pode ser feito usando um <xref:System.Runtime.Versioning.TargetFrameworkAttribute?displayProperty=fullName> no assembly ou pela interface do usuário do sistema do projeto no Visual Studio, na página "propriedades do projeto".

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Principal       |
| Versão | 4.5         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Uri.%23ctor(System.String)>
- <xref:System.Uri.%23ctor(System.String,System.Boolean)>
- <xref:System.Uri.%23ctor(System.String,System.UriKind)>
- <xref:System.Uri.%23ctor(System.Uri,System.String)>
- <xref:System.Uri.TryCreate(System.String,System.UriKind,System.Uri@)?displayProperty=nameWithType>
- <xref:System.Uri.TryCreate(System.Uri,System.String,System.Uri@)?displayProperty=nameWithType>
- <xref:System.Uri.TryCreate(System.Uri,System.Uri,System.Uri@)?displayProperty=nameWithType>
