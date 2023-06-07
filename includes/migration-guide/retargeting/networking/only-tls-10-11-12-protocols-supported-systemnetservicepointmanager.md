---
ms.openlocfilehash: 87dc93ece10eaedbfbabddb5f857d0bcd12e05c4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615595"
---
### <a name="only-tls-10-11-and-12-protocols-supported-in-systemnetservicepointmanager-and-systemnetsecuritysslstream"></a>Somente os protocolos Tls 1.0, 1.1 e 1.2 são compatíveis com System.Net.ServicePointManager e System.Net.Security.SslStream

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6, as classes <xref:System.Net.ServicePointManager> e <xref:System.Net.Security.SslStream> têm permissão para usar somente um dos três protocolos a seguir: Tls1.0, Tls1.1 ou Tls1.2. Não há suporte para o protocolo SSL 3.0 e a criptografia RC4.

#### <a name="suggestion"></a>Sugestão

A mitigação recomendada é fazer upgrade do aplicativo do lado do servidor para Tls1.0, Tls1.1 ou Tls1.2. Se não for viável ou se os aplicativos cliente estiverem desfeitos, a classe <xref:System.AppContext?displayProperty=fullName> poderá ser usada para recusar esse recurso de duas maneiras:

- Configurar de forma programática as opções de compatibilidade na instância <xref:System.AppContext?displayProperty=fullName>, conforme explicado [aqui](https://devblogs.microsoft.com/dotnet/net-announcements-at-build-2015/#dotnet46).
- Adicionando a seguinte linha na seção `<runtime>` do arquivo app.config:

```xml
<AppContextSwitchOverrides value="Switch.System.Net.DontEnableSchUseStrongCrypto=true"/>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Net.SecurityProtocolType.Ssl3?displayProperty=nameWithType>
- <xref:System.Security.Authentication.SslProtocols.None?displayProperty=nameWithType>
- <xref:System.Security.Authentication.SslProtocols.Ssl2?displayProperty=nameWithType>
- <xref:System.Security.Authentication.SslProtocols.Ssl3?displayProperty=nameWithType>
