---
ms.openlocfilehash: 17a167e5245914329195d61ab45ae2d8ecb617d6
ms.sourcegitcommit: 3492dafceb5d4183b6b0d2f3bdf4a1abc4d5ed8c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2020
ms.locfileid: "86416248"
---
### <a name="blazor-target-framework-of-nuget-packages-changed"></a>Mais incrivelmente: estrutura de destino de pacotes NuGet alterada

Os projetos Webassembly mais po3,2 foram compilados para o destino .NET Standard 2,1 ( `<TargetFramework>netstandard2.1</TargetFramework>` ). No ASP.NET Core 5,0, os 5,0 projetos de um servidor de Webassembly mais podriveres e mais de um e mais do mais `<TargetFramework>net5.0</TargetFramework>` Para alinhar melhor com a alteração da estrutura de destino, os seguintes pacotes mais eficientes não são mais direcionados .NET Standard 2,1:

* [Microsoft. AspNetCore. Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)
* [Microsoft. AspNetCore. Components. Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization)
* [Microsoft. AspNetCore. Components. Forms](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Forms)
* [Microsoft. AspNetCore. Components. Web](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Web)
* [Microsoft. AspNetCore. Components. Webassembly](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly)
* [Microsoft. AspNetCore. Components. Webassembly. Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication)
* [Interoperabilidade deMicrosoft.JS](https://www.nuget.org/packages/Microsoft.JSInterop)
* [Microsoft.JSInterop. Webassembly](https://www.nuget.org/packages/Microsoft.JSInterop.WebAssembly)
* [Microsoft. Authentication. Webassembly. MSAL](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal)

Para obter uma discussão, consulte o problema do GitHub [dotnet/aspnetcore # 23424](https://github.com/dotnet/aspnetcore/issues/23424).

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 7

#### <a name="old-behavior"></a>Comportamento antigo

No mais 3,1 e 3,2, os pacotes visam .NET Standard 2,1 e o .NET Core 3,1.

#### <a name="new-behavior"></a>Novo comportamento

No ASP.NET Core 5,0, os pacotes são de destino .NET 5,0.

#### <a name="reason-for-change"></a>Motivo da alteração

A alteração foi feita para se alinhar melhor com os requisitos da estrutura de destino do .NET.

#### <a name="recommended-action"></a>Ação recomendada

Os projetos Webassembly mais po3,2 devem ter como destino o .NET 5,0 como parte da atualização de suas referências de pacote para 5. x.x. As bibliotecas que fazem referência a um desses pacotes podem ter como destino o .NET 5,0 ou vários destinos, dependendo de seus requisitos.

#### <a name="category"></a>Categoria

ASP.NET Core

#### <a name="affected-apis"></a>APIs afetadas

Nenhum

<!--

#### Affected APIs

Not detectable via API analysis

-->
