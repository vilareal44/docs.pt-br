---
ms.openlocfilehash: 53ded5ae6e5a025fc7992da099c3481587bb6f31
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614311"
---
### <a name="privatefontcollectionaddfontfile-method-releases-font-resources"></a>O método PrivateFontCollection.AddFontFile libera os recursos de fonte

#### <a name="details"></a>Detalhes

No .NET Framework 4.7.1 e nas versões anteriores, a classe <xref:System.Drawing.Text.PrivateFontCollection?displayProperty=nameWithType> não libera os recursos de fonte GDI+ depois que a <xref:System.Drawing.Text.PrivateFontCollection> é descartada para os objetos <xref:System.Drawing.Font> que são adicionados a essa coleção usando o método <xref:System.Drawing.Text.PrivateFontCollection.AddFontFile(System.String)>. No .NET Framework 4.7.2 e posterior <xref:System.Drawing.Text.FontCollection.Dispose%2A> libera as fontes GDI+ que foram adicionadas à coleção como arquivos.

#### <a name="suggestion"></a>Sugestão

**Como aceitar ou recusar essas alterações** Para que um aplicativo se beneficie dessas alterações, ele deve ser executado no .NET Framework 4.7.2 ou posterior. O aplicativo pode se beneficiar dessas alterações de uma das seguintes maneiras:

- Ser recompilado para ser direcionado ao .NET Framework 4.7.2. Essa alteração é habilitada por padrão nos aplicativos do Windows Forms direcionados ao .NET Framework 4.7.2 ou posterior.
- Ser direcionado ao .NET Framework 4.7.1 ou a uma versão anterior e recusar os comportamentos de acessibilidade herdados, adicionando a seguinte [Opção AppContext](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element) à seção `<runtime>` do arquivo app.config e definindo-a como `false`, como mostra o exemplo a seguir.

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Drawing.Text.DoNotRemoveGdiFontsResourcesFromFontCollection=false&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

Os aplicativos direcionados ao .NET Framework 4.7.2 ou posterior que desejam preservar o comportamento herdado podem aceitar não liberar os recursos de fonte definindo explicitamente essa opção AppContext como `true`.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Drawing.Text.PrivateFontCollection.AddFontFile(System.String)?displayProperty=nameWithType>
- <xref:System.Drawing.Text.FontCollection.Dispose?displayProperty=nameWithType>
