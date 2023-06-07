---
ms.openlocfilehash: d21b2e092d460fdfc367d0f490228ed44ad5c6cc
ms.sourcegitcommit: 63bb83322814f5e5e5c5b69939b14a3139a6ca7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85365631"
---
### <a name="built-in-support-for-winrt-is-removed-from-net"></a>O suporte interno para o WinRT é removido do .NET

O suporte interno para consumo de APIs do [tempo de execução do Windows (WinRT)](/uwp/winrt-cref/winrt-type-system) no .net foi removido.

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 6

#### <a name="change-description"></a>Descrição da alteração

Anteriormente, o CoreCLR poderia consumir [arquivos de metadados do Windows (WinMD)](/uwp/winrt-cref/winmd-files) para ativar e consumir tipos de WinRT. A partir do .NET 5,0, o CoreCLR não pode mais consumir arquivos WinMD diretamente.

Se você tentar fazer referência a um assembly sem suporte, obterá um <xref:System.IO.FileNotFoundException> . Se você ativar uma classe do WinRT, obterá um <xref:System.PlatformNotSupportedException> .

Essa alteração significativa foi feita pelos seguintes motivos:

- Portanto, o WinRT pode ser desenvolvido e aprimorado separadamente do tempo de execução do .NET.
- Para simetria com sistemas de interoperabilidade fornecidos para outros sistemas operacionais, como iOS e Android.
- Para tirar proveito de outros recursos do .NET, como recursos C#, vinculação de IL (linguagem intermediária) e compilação de AOT (antecipadamente de tempo).
- Para simplificar a base de código .NET Runtime.

#### <a name="recommended-action"></a>Ação recomendada

- Remova as referências ao [pacote Microsoft. Windows. Sdk. Contracts](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts) e substitua-as por referências ao [pacote Microsoft.Windows.Sdk.net](https://www.nuget.org/packages/microsoft.windows.sdk.net).

- Use a cadeia de ferramentas [/WinRT do C#](/windows/uwp/csharp-winrt/) para gerar ou personalizar APIs e tipos do WinRT no .NET 5,0 e versões posteriores.

#### <a name="category"></a>Categoria

Interoperabilidade

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.IO.WindowsRuntimeStorageExtensions?displayProperty=fullName>
- <xref:System.IO.WindowsRuntimeStreamExtensions?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.WindowsRuntime?displayProperty=fullName>
- <xref:System.WindowsRuntimeSystemExtensions?displayProperty=fullName>
- <xref:Windows.Foundation.Point?displayProperty=fullName>
- <xref:Windows.Foundation.Size?displayProperty=fullName>
- <xref:Windows.UI.Color?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.IO.WindowsRuntimeStorageExtensions`
- `T: System.IO.WindowsRuntimeStreamExtensions`
- `N:System.Runtime.InteropServices.WindowsRuntime`
- `T:System.WindowsRuntimeSystemExtensions`
- `T:Windows.Foundation.Point`
- `T:Windows.Foundation.Size`
- `T:Windows.UI.Color`

-->
