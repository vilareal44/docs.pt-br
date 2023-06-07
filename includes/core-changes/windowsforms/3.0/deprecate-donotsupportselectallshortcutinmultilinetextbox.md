---
ms.openlocfilehash: 3fb8807a9b6f0bb0d2bc746f5e89eaa8a81d6aa8
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556126"
---
### <a name="donotsupportselectallshortcutinmultilinetextbox-compatibility-switch-not-supported"></a>Não há suporte para a opção de compatibilidade DoNotSupportSelectAllShortcutInMultilineTextBox

A `Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox` opção de compatibilidade, que foi introduzida no .NET Framework 4.6.1, não tem suporte no Windows Forms no .NET Core e no .net 5,0 e posterior.

#### <a name="change-description"></a>Descrição da alteração

Começando com .NET Framework 4.6.1, selecionando a tecla <kbd>Ctrl</kbd>  +  <kbd>de</kbd> atalho em um <xref:System.Windows.Forms.TextBox> controle selecionar todo o texto. No .NET Framework 4,6 e versões anteriores, a seleção da tecla <kbd>Ctrl</kbd>  +  <kbd>a</kbd> shortcut key falhou em selecionar todo o texto se as propriedades [TextBox. ShortcutsEnabled](xref:System.Windows.Forms.TextBoxBase.ShortcutsEnabled) e <xref:System.Windows.Forms.TextBox.Multiline?displayProperty=nameWithType> foram ambas definidas como `true` . A `Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox` opção de compatibilidade foi introduzida no .NET Framework 4.6.1 para manter o comportamento original. Para obter mais informações, consulte <xref:System.Windows.Forms.TextBox.ProcessCmdKey%2A?displayProperty=nameWithType>.

No .NET Core e no .NET 5,0 e versões posteriores, `Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox` não há suporte para a opção.

#### <a name="version-introduced"></a>Versão introduzida

3.0

#### <a name="recommended-action"></a>Ação recomendada

Remova a opção. Não há suporte para a opção e nenhuma funcionalidade alternativa está disponível.

#### <a name="category"></a>Categoria

Windows Forms

#### <a name="affected-apis"></a>APIs afetadas

- Nenhum

<!-- 

#### Affected APIs

- Not detectable via API analysis

-->
