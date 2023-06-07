---
ms.openlocfilehash: 9a747d8fc15ca8fa2b915f89291f118d7344d172
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325204"
---
### <a name="httpsys-client-certificate-renegotiation-disabled-by-default"></a>HttpS: renegociação de certificado de cliente desabilitada por padrão

A opção para renegociar uma conexão e solicitar um certificado de cliente foi desabilitada por padrão. Para obter uma discussão, veja Issue [dotnet/aspnetcore # 23181](https://github.com/dotnet/aspnetcore/issues/23181).

#### <a name="version-introduced"></a>Versão introduzida

ASP.NET Core 5,0

#### <a name="old-behavior"></a>Comportamento antigo

A conexão pode ser renegociada para solicitar um certificado de cliente.

#### <a name="new-behavior"></a>Novo comportamento

Certificados de cliente só podem ser solicitados durante o handshake de conexão inicial. Para obter mais informações, consulte solicitação pull [dotnet/aspnetcore # 23162](https://github.com/dotnet/aspnetcore/pull/23162).

#### <a name="reason-for-change"></a>Motivo da alteração

A renegociação causou um número de problemas de desempenho e deadlock. Também não tem suporte no HTTP/2. Para obter o contexto adicional de quando a opção para controlar esse comportamento foi introduzida no ASP.NET Core 3,1, veja o problema [dotnet/aspnetcore # 14806](https://github.com/dotnet/aspnetcore/issues/14806).

#### <a name="recommended-action"></a>Ação recomendada

Aplicativos que exigem certificados de cliente devem usar *netsh.exe* para definir a `clientcertnegotiation` opção como `enabled` . Para obter mais informações, consulte [netsh http Commands](/windows-server/networking/technologies/netsh/netsh-http).

Se você quiser que os certificados de cliente estejam habilitados somente para algumas partes do seu aplicativo, consulte as diretrizes em [certificados de cliente opcionais](/aspnet/core/security/authentication/certauth?view=aspnetcore-3.1#optional-client-certificates).

Se você precisar do comportamento antigo de renegociação, defina `HttpSysOptions.ClientCertificateMethod` como o valor antigo `ClientCertificateMethod.AllowRenegotiate` . Isso não é recomendado pelos motivos descritos acima e nas diretrizes vinculadas.

#### <a name="category"></a>Categoria

ASP.NET Core

#### <a name="affected-apis"></a>APIs afetadas

- <xref:Microsoft.AspNetCore.Http.ConnectionInfo.ClientCertificate%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Http.ConnectionInfo.GetClientCertificateAsync%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ClientCertificateMethod%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

- `Overload:Microsoft.AspNetCore.Http.ConnectionInfo.ClientCertificate`
- `Overload:Microsoft.AspNetCore.Http.ConnectionInfo.GetClientCertificateAsync`
- `Overload:Microsoft.AspNetCore.Server.HttpSys.HttpSysOptions.ClientCertificateMethod`

-->
