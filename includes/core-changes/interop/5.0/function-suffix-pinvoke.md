---
ms.openlocfilehash: e128bdb5735b3e4844356ad4807cc092f6f2b105
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062413"
---
### <a name="no-aw-suffix-probing-on-non-windows-platforms"></a>Nenhuma investigação de sufixo A/W em plataformas não Windows

Os tempos de execução do .NET não adicionam mais um `A` `W` sufixo ou aos nomes de exportação de função durante a investigação de P/Invokes em plataformas não Windows.

#### <a name="version-introduced"></a>Versão introduzida

5,0 versão prévia 4

#### <a name="change-description"></a>Descrição da alteração

O [Windows tem uma convenção](/windows/win32/intl/conventions-for-function-prototypes) de adicionar `A` um `W` sufixo ou para SDK do Windows nomes de função, que correspondem à página de código do Windows e às versões Unicode, respectivamente.

Nas versões anteriores do .NET, os tempos de execução do CoreCLR e do mono adicionam um `A` `W` sufixo ou ao nome de exportação durante a descoberta de exportação para P/Invokes *em todas as plataformas*.

No .NET 5,0 e versões posteriores, `A` um `W` sufixo ou é adicionado ao nome de exportação durante a descoberta *de exportação somente no Windows*. Em plataformas UNIX, o sufixo não é adicionado. A semântica de ambos os tempos de execução na plataforma Windows permanece inalterada.

#### <a name="reason-for-change"></a>Motivo da alteração

Essa alteração foi feita para simplificar a investigação de plataforma cruzada. É uma sobrecarga que não deve ser incorrida, Considerando que plataformas não Windows não contêm essa semântica.

#### <a name="recommended-action"></a>Ação recomendada

Para atenuar a alteração, você pode adicionar manualmente o sufixo desejado em plataformas que não são do Windows. Por exemplo:

```csharp
[DllImport(...)]
extern static void SetWindowTextW();
```

#### <a name="category"></a>Categoria

Interoperabilidade

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Runtime.InteropServices.DllImportAttribute?displayProperty=fullName>

<!--

#### Affected APIs

- `T:System.Runtime.InteropServices.DllImportAttribute`

-->
