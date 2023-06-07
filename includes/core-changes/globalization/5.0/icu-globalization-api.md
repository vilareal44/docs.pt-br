---
ms.openlocfilehash: 74c3d3247912dcd638a9379d54e682967c5e400b
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302693"
---
### <a name="globalization-apis-use-icu-libraries-on-windows"></a>APIs de globalização usam bibliotecas ICU no Windows

O .NET 5,0 e versões posteriores usam bibliotecas [internacionais para](http://site.icu-project.org/home) a funcionalidade de globalização na execução no Windows 10 pode ser 2019 atualização ou posterior.

#### <a name="change-description"></a>Descrição da alteração

Anteriormente, as bibliotecas do .NET usavam APIs [NLS (suporte a idioma nacional)](/windows/win32/intl/national-language-support) para a funcionalidade de globalização. Por exemplo, as funções NLS foram usadas para obter dados de cultura, como padrões de formato de data e hora, comparar cadeias de caracteres e executar maiúsculas e minúsculas na cultura apropriada.

A partir do .NET 5,0, se um aplicativo estiver em execução no Windows 10 pode ser 2019 Update ou posterior, as bibliotecas do .NET usarão APIs de globalização [ICU](http://site.icu-project.org/home) . O Windows 10 pode 2019 atualização e versões posteriores são fornecidas com a biblioteca nativa do ICU. Se o tempo de execução do .NET não puder carregar o ICU, ele usará o NLS.

Essa alteração foi introduzida por dois motivos:

- Os aplicativos têm o mesmo comportamento de globalização entre as plataformas, incluindo Linux, macOS e Windows.
- Os aplicativos podem controlar o comportamento de globalização usando bibliotecas ICU personalizadas.

#### <a name="version-introduced"></a>Versão introduzida

.NET 5,0 Preview 4

#### <a name="recommended-action"></a>Ação recomendada

Nenhuma ação é necessária na parte do desenvolvedor. No entanto, se você quiser continuar usando as APIs de globalização NLS, poderá definir uma [opção de tempo de execução](../../../../docs/core/run-time-config/globalization.md#nls) para reverter para esse comportamento. Para obter mais informações sobre as opções disponíveis, consulte o artigo [globalização .net e ICU](/dotnet/standard/globalization-localization/globalization-icu) .

#### <a name="category"></a>Categoria

Globalização

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Span%601?displayProperty=fullName>
- <xref:System.String?displayProperty=fullName>
- Maioria dos tipos no <xref:System.Globalization?displayProperty=fullName> namespace

<!--

#### Affected APIs

- ``T:System.Span`1``
- `T:System.String`
- `N:System.Globalization`

-->
