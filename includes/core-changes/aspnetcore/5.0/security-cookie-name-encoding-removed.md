---
ms.openlocfilehash: 492d3e61acff31d3d6858a1c27cf353b04a05e36
ms.sourcegitcommit: d4f7ba08f2a45a9dbef53be597eed6d4a9410f29
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/15/2020
ms.locfileid: "86401972"
---
### <a name="security-cookie-name-encoding-removed"></a>Segurança: codificação de nome de cookie removida

O [padrão de cookie http](https://tools.ietf.org/html/rfc6265#section-4.1.1) permite apenas caracteres específicos em valores e nomes de cookie. Para dar suporte a caracteres não permitidos, ASP.NET Core:

* Codifica ao criar um cookie de resposta.
* Decodifica ao ler um cookie de solicitação.

No ASP.NET Core 5,0, esse comportamento de codificação mudou em resposta a uma preocupação de segurança.

Para obter uma discussão, consulte o problema do GitHub [dotnet/aspnetcore # 23578](https://github.com/dotnet/aspnetcore/issues/23578).

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 8

#### <a name="old-behavior"></a>Comportamento antigo

Os nomes de cookies de resposta são codificados. Os nomes de cookie de solicitação são decodificados.

#### <a name="new-behavior"></a>Novo comportamento

A codificação e a decodificação de nomes de cookies foram removidas. Para [versões anteriores com suporte](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) do ASP.NET Core, a equipe planeja reduzir o problema de decodificação in-loco. Além disso, chamar <xref:Microsoft.AspNetCore.Http.IResponseCookies.Append%2A?displayProperty=nameWithType> com um nome de cookie inválido gera uma exceção do tipo <xref:System.ArgumentException> . A codificação e a decodificação de valores de cookies permanecem inalteradas.

#### <a name="reason-for-change"></a>Motivo da alteração

Foi descoberto um problema em [várias estruturas da Web](https://github.com/advisories/GHSA-j6w9-fv6q-3q52). A codificação e a decodificação podem permitir que um invasor ignore um recurso de segurança chamado [prefixos de cookie](https://tools.ietf.org/html/draft-ietf-httpbis-cookie-prefixes-00) , falsificando prefixos reservados como `__Host-` com valores codificados como `__%48ost-` . O ataque requer uma exploração secundária para injetar os cookies falsificados, como uma vulnerabilidade de XSS (script entre sites), no site. Esses prefixos não são usados por padrão em ASP.NET Core ou `Microsoft.Owin` bibliotecas ou modelos.

#### <a name="recommended-action"></a>Ação recomendada

Se você estiver movendo projetos para ASP.NET Core 5,0 ou posterior, verifique se seus nomes de cookie estão em conformidade com os [requisitos de especificação de token](https://tools.ietf.org/html/rfc2616#section-2.2): caracteres ASCII, excluindo controles e separadores `"(" | ")" | "<" | ">" | "@" | "," | ";" | ":" | "\" | <"> | "/" | "[" | "]" | "?" | "=" | "{" | "}" | SP | HT` . O uso de caracteres não ASCII em nomes de cookies ou outros cabeçalhos HTTP pode causar uma exceção do servidor ou ser de ida e volta inadequada pelo cliente.

#### <a name="category"></a>Categoria

ASP.NET Core

#### <a name="affected-apis"></a>APIs afetadas

- <xref:Microsoft.AspNetCore.Http.HttpRequest.Cookies%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Http.HttpResponse.Cookies%2A?displayProperty=nameWithType>
- <xref:Microsoft.Owin.IOwinRequest.Cookies?displayProperty=nameWithType>
- <xref:Microsoft.Owin.IOwinResponse.Cookies?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:Microsoft.AspNetCore.Http.HttpRequest.Cookies`
- `Overload:Microsoft.AspNetCore.Http.HttpResponse.Cookies`
- `P:Microsoft.Owin.IOwinRequest.Cookies`
- `P:Microsoft.Owin.IOwinResponse.Cookies`

-->
