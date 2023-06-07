---
ms.openlocfilehash: d598d8d3203e804e5e935c3564b0053f9fc2e9a6
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144952"
---
### <a name="shared-framework-assemblies-removed-from-microsoftaspnetcoreapp"></a>Estrutura compartilhada: assemblies removidos do Microsoft. AspNetCore. app

A partir do ASP.NET Core 3,0, a estrutura compartilhada ASP.NET Core ( `Microsoft.AspNetCore.App` ) contém apenas assemblies de terceiros que são totalmente desenvolvidos, com suporte e podem ser atendidos pela Microsoft.

#### <a name="change-description"></a>Descrição da alteração

Imagine a alteração como a redefinição de limites para a ASP.NET Core "plataforma". A estrutura compartilhada será [criada de origem por qualquer pessoa por meio do GitHub](https://github.com/dotnet/source-build) e continuará a oferecer os benefícios existentes das estruturas compartilhadas do .NET Core para seus aplicativos. Alguns benefícios incluem tamanho de implantação menor, aplicação de patch centralizada e tempo de inicialização mais rápido.

Como parte da alteração, algumas alterações significativas importantes são introduzidas no `Microsoft.AspNetCore.App` .

#### <a name="version-introduced"></a>Versão introduzida

3.0

#### <a name="old-behavior"></a>Comportamento antigo

Projetos referenciados `Microsoft.AspNetCore.App` por meio de um `<PackageReference>` elemento no arquivo de projeto.

Além disso, `Microsoft.AspNetCore.App` continha os seguintes subcomponentes:

- Json.NET ( `Newtonsoft.Json` )
- Entity Framework Core (assemblies prefixados com `Microsoft.EntityFrameworkCore.` )
- Roslyn ( `Microsoft.CodeAnalysis` )

#### <a name="new-behavior"></a>Novo comportamento

Uma referência a `Microsoft.AspNetCore.App` não requer mais um `<PackageReference>` elemento no arquivo de projeto. O SDK do .NET Core dá suporte a um novo elemento chamado `<FrameworkReference>` , que substitui o uso de `<PackageReference>` .

Para obter mais informações, consulte [dotnet/aspnetcore # 3612](https://github.com/dotnet/aspnetcore/issues/3612).

Entity Framework Core são fornecidos como pacotes NuGet. Essa alteração alinha o modelo de envio com todas as outras bibliotecas de acesso a dados no .NET. Ele fornece Entity Framework Core caminho mais simples para continuar inovação enquanto dá suporte a várias plataformas .NET. A mudança de Entity Framework Core da estrutura compartilhada não tem impacto sobre seu status como uma biblioteca de serviços desenvolvidos, com suporte e desenvolvido pela Microsoft. A [política de suporte do .NET Core](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) continua a cobrir.

Json.NET e Entity Framework Core continuam trabalhando com ASP.NET Core. No entanto, eles não serão incluídos na estrutura compartilhada.

Para obter mais informações, consulte [o futuro do JSON no .NET Core 3,0](https://github.com/dotnet/announcements/issues/90). Consulte também [a lista completa de binários](https://github.com/dotnet/aspnetcore/issues/3755) removidos da estrutura compartilhada.

#### <a name="reason-for-change"></a>Motivo da alteração

Essa alteração simplifica o consumo de `Microsoft.AspNetCore.App` e reduz a duplicação entre pacotes NuGet e estruturas compartilhadas.

Para obter mais informações sobre a motivação para essa alteração, consulte [esta postagem no blog](https://devblogs.microsoft.com/aspnet/a-first-look-at-changes-coming-in-asp-net-core-3-0/).

#### <a name="recommended-action"></a>Ação recomendada

Não será necessário que os projetos consumam assemblies no `Microsoft.AspNetCore.App` como pacotes NuGet. Para simplificar o direcionamento e o uso da estrutura ASP.NET Core compartilhada, muitos pacotes NuGet fornecidos desde ASP.NET Core 1,0 não são mais produzidos. As APIs que esses pacotes fornecem ainda estão disponíveis para aplicativos usando um `<FrameworkReference>` para `Microsoft.AspNetCore.App` . Exemplos comuns de API incluem Kestrel, MVC e Razor.

Essa alteração não se aplica a todos os binários referenciados por meio `Microsoft.AspNetCore.App` do ASP.NET Core 2. x. As exceções notáveis incluem:

- `Microsoft.Extensions`as bibliotecas que continuam a atingir .NET Standard estarão disponíveis como pacotes NuGet (consulte <https://github.com/dotnet/extensions> ).
- APIs produzidas pela equipe de ASP.NET Core que não fazem parte do `Microsoft.AspNetCore.App` . Por exemplo, os seguintes componentes estão disponíveis como pacotes NuGet:
  - Entity Framework Core
  - APIs que fornecem integração de terceiros
  - Recursos experimentais
  - APIs com dependências que não puderam [atender aos requisitos para estar na estrutura compartilhada](https://github.com/dotnet/aspnetcore/blob/4e44e5bcbedd961cc0d4f6b846699c7c494f5597/docs/SharedFramework.md)
- Extensões para MVC que mantêm suporte para Json.NET. Uma API será fornecida como um pacote NuGet para dar suporte ao uso do Json.NET e MVC.
- O cliente do sinalizador .NET continuará a dar suporte a .NET Standard e enviará como um pacote NuGet. Ele é destinado ao uso em vários tempos de execução do .NET, como Xamarin e UWP.

Para obter mais informações, consulte [parar de produzir pacotes para assemblies de estrutura compartilhada em 3,0](https://github.com/dotnet/aspnetcore/issues/3756). Para obter uma discussão, consulte [dotnet/aspnetcore # 3757](https://github.com/dotnet/aspnetcore/issues/3757).

#### <a name="category"></a>Categoria

ASP.NET Core

#### <a name="affected-apis"></a>APIs afetadas

- <xref:Microsoft.CodeAnalysis?displayProperty=nameWithType>
- <xref:Microsoft.EntityFrameworkCore?displayProperty=nameWithType>

<!--

#### Affected APIs

- `N:Microsoft.CodeAnalysis`
- `N:Microsoft.EntityFrameworkCore`

-->
