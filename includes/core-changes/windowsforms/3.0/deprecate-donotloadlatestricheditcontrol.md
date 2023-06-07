---
ms.openlocfilehash: e10b5168d59edd56ff549a3a1e3a09d023fe5e28
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556121"
---
### <a name="donotloadlatestricheditcontrol-compatibility-switch-not-supported"></a>Não há suporte para a opção de compatibilidade DoNotLoadLatestRichEditControl

A `Switch.System.Windows.Forms.UseLegacyImages` opção de compatibilidade, que foi introduzida no .NET Framework 4.7.1, não tem suporte no Windows Forms no .NET Core ou no .net 5,0 e posterior.

#### <a name="change-description"></a>Descrição da alteração

No .NET Framework 4.6.2 e versões anteriores, o <xref:System.Windows.Forms.RichTextBox> controle instancia o controle RichEdit do Win32 v 3.0 e, para aplicativos direcionados .NET Framework 4.7.1, o <xref:System.Windows.Forms.RichTextBox> controle instancia o RichEdit v 4.1 (em *msftedit.dll*). A `Switch.System.Windows.Forms.DoNotLoadLatestRichEditControl` opção de compatibilidade foi introduzida para permitir aplicativos direcionados .NET Framework 4.7.1 e versões posteriores para recusar o novo controle RichEdit v 4.1 e usar o antigo controle RichEdit v3.

No .NET Core e no .NET 5,0 e versões posteriores, `Switch.System.Windows.Forms.DoNotLoadLatestRichEditControl` não há suporte para a opção. Há suporte apenas para novas versões do <xref:System.Windows.Forms.RichTextBox> controle.

#### <a name="version-introduced"></a>Versão introduzida

3.0

#### <a name="recommended-action"></a>Ação recomendada

Remova a opção. Não há suporte para a opção e nenhuma funcionalidade alternativa está disponível.

#### <a name="category"></a>Categoria

Windows Forms

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Forms.RichTextBox?displayProperty=nameWithType>

<!-- 

#### Affected APIs

-  `T:System.Windows.Forms.RichTextBox` 

-->
