---
ms.openlocfilehash: a635e2ed6a735b5234c92fd8f5ffa1685fe9373e
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558160"
---
### <a name="microsoftdotnetplatformabstractions-package-removed"></a>Pacote Microsoft. DotNet. PlatformAbstractions removido

Nenhuma nova versão do [pacote NuGet Microsoft. dotnet. PlatformAbstractions](https://www.nuget.org/packages/Microsoft.DotNet.PlatformAbstractions/) será produzida.

#### <a name="change-description"></a>Descrição da alteração

Anteriormente, novas versões da <xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> biblioteca foram produzidas junto com as novas versões do .NET Core. No futuro, nenhuma nova funcionalidade será adicionada à biblioteca e nenhuma nova versão principal será lançada. No entanto, as versões existentes da biblioteca continuarão funcionando e serão atendidas.

A <xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> biblioteca se sobrepõe a APIs que já estão estabelecidas em System. \* namespaces. Além disso, algumas <xref:Microsoft.DotNet.PlatformAbstractions> APIs não foram projetadas com o mesmo nível de análise e suporte a longo prazo como o restante do sistema. \* API. Por exemplo, <xref:Microsoft.DotNet.PlatformAbstractions> o usa a `Platform` enumeração para descrever a plataforma atual do sistema operacional. Esse design de enumeração foi explicitamente rejeitado quando a <xref:System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform(System.Runtime.InteropServices.OSPlatform)?displayProperty=nameWithType> API foi projetada para permitir novas plataformas e futura flexibilidade.

Os cenários habilitados pela <xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> biblioteca agora são possíveis sem ele. As versões existentes continuarão funcionando, mesmo no .NET 5,0 e posterior, e serão atendidas junto com as versões anteriores do .NET Core. No entanto, a nova funcionalidade não será adicionada à biblioteca. Em vez disso, novas funcionalidades serão adicionadas a outras bibliotecas e APIs.

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 6

#### <a name="recommended-action"></a>Ação recomendada

- Você pode continuar a usar versões mais antigas da biblioteca se elas atenderem às suas necessidades.

- Se as versões mais antigas não atenderem às suas necessidades, substitua os usos das `PlatformAbstractions` APIs pelas substituições recomendadas.

  | `PlatformAbstractions` API | Substituição recomendada |
  |-|-|
  | `ApplicationEnvironment.ApplicationBasePath` | <xref:System.AppContext.BaseDirectory?displayProperty=nameWithType> |
  | <xref:Microsoft.DotNet.PlatformAbstractions.HashCodeCombiner> | <xref:System.HashCode?displayProperty=nameWithType> |
  | `RuntimeEnvironment.GetRuntimeIdentifier()` | <xref:System.Runtime.InteropServices.RuntimeInformation.RuntimeIdentifier?displayProperty=nameWithType> |
  | `RuntimeEnvironment.OperatingSystemPlatform` | <xref:System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform(System.Runtime.InteropServices.OSPlatform)?displayProperty=nameWithType> |
  | `RuntimeEnvironment.RuntimeArchitecture` | <xref:System.Runtime.InteropServices.RuntimeInformation.ProcessArchitecture?displayProperty=nameWithType> |
  | `RuntimeEnvironment.OperatingSystem` | <xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> |
  | `RuntimeEnvironment.OperatingSystemVersion` | <xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> e <xref:System.Environment.OSVersion?displayProperty=nameWithType> |

  > [!NOTE]
  > A maioria dos casos de uso para `RuntimeEnvironment.OperatingSystem` `RuntimeEnvironment.OperatingSystemVersion` o e o são para fins de exibição, por exemplo, exibindo para um usuário, registro em log e telemetria. Não é recomendável tomar decisões de tempo de execução com base em uma versão do sistema operacional (SO). <xref:System.Environment.OSVersion?displayProperty=nameWithType> Agora [retorna a versão correta para os](../../../../docs/core/compatibility/corefx.md#environmentosversion-returns-the-correct-operating-system-version) sistemas operacionais Windows e MacOS. No entanto, para a maioria das distribuições do UNIX, o que é considerado como "versão do sistema operacional" não é tão simples. Por exemplo, pode ser a versão do kernel do Linux ou pode ser a versão distribuição. Para a maioria das plataformas UNIX, <xref:System.Environment.OSVersion?displayProperty=nameWithType> e <xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> retorne a versão retornada pelo `uname` . Para obter as informações de nome e versão do distribuição do Linux, a abordagem recomendada é ler o arquivo */etc/os-Release* .

#### <a name="category"></a>Categoria

Bibliotecas principais do .NET

#### <a name="affected-apis"></a>APIs afetadas

- `Microsoft.DotNet.PlatformAbstractions.ApplicationEnvironment.ApplicationBasePath`
- <xref:Microsoft.DotNet.PlatformAbstractions.HashCodeCombiner?displayProperty=fullName>
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.GetRuntimeIdentifier()`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystem`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemPlatform`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemVersion`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.RuntimeArchitecture`

<!--

#### Affected APIs

- `P:Microsoft.DotNet.PlatformAbstractions.ApplicationEnvironment.ApplicationBasePath`
- `T:Microsoft.DotNet.PlatformAbstractions.HashCodeCombiner`
- `M:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.GetRuntimeIdentifier`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystem`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemPlatform`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemVersion`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.RuntimeArchitecture`

-->
