---
ms.openlocfilehash: 97299ddb9bee89c792ddb3d2b9c37516180996f7
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614308"
---
### <a name="contextmenustripsourcecontrol-property-contains-a-valid-control-in-the-case-of-nested-toolstripmenuitems"></a>A propriedade ContextMenuStrip.SourceControl contém um controle válido no caso de ToolStripMenuItems aninhados

#### <a name="details"></a>Detalhes

No .NET Framework 4.7.1 e nas versões anteriores, a propriedade <xref:System.Windows.Forms.ContextMenuStrip.SourceControl?displayProperty=nameWithType> retorna nulo incorretamente quando o usuário abre o menu de controles <xref:System.Windows.Forms.ToolStripMenuItem> aninhados. No .NET Framework 4.7.2 e posterior, a propriedade <xref:System.Windows.Forms.ContextMenuStrip.SourceControl> é sempre definida como o controle do código-fonte real.

#### <a name="suggestion"></a>Sugestão

**Como aceitar ou recusar essas alterações** Para que um aplicativo se beneficie dessas alterações, ele deve ser executado no .NET Framework 4.7.2 ou posterior. O aplicativo pode se beneficiar dessas alterações de uma das seguintes maneiras:

- Ser direcionado ao .NET Framework 4.7.2. Essa alteração é habilitada por padrão nos aplicativos do Windows Forms direcionados ao .NET Framework 4.7.2 ou posterior.
- Ser direcionado ao .NET Framework 4.7.1 ou a uma versão anterior e recusar os comportamentos de acessibilidade herdados, adicionando a seguinte [Opção AppContext](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element) à seção `<runtime>` do arquivo app.config e definindo-a como `false`, como mostra o exemplo a seguir.

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue=false"/>
</runtime>
```

Os aplicativos direcionados ao .NET Framework 4.7.2 ou posterior que desejam preservar o comportamento de acessibilidade herdado podem aceitar o uso do controle do código-fonte herdado definindo explicitamente essa opção AppContext como `true`.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Forms.ContextMenuStrip.SourceControl?displayProperty=nameWithType>
