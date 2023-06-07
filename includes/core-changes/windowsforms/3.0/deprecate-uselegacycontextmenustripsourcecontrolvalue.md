---
ms.openlocfilehash: df6169289fb96df5f7daf8bd58ccaeebc33ad87c
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556123"
---
### <a name="uselegacycontextmenustripsourcecontrolvalue-compatibility-switch-not-supported"></a>Não há suporte para a opção de compatibilidade UseLegacyContextMenuStripSourceControlValue

A `Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue` opção de compatibilidade, que foi introduzida no .NET Framework 4.7.2, não tem suporte no Windows Forms no .NET Core ou no .net 5,0 e posterior.

#### <a name="change-description"></a>Descrição da alteração

A partir do .NET Framework 4.7.2, a `Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue` opção de compatibilidade permite que o desenvolvedor cancele o novo comportamento da <xref:System.Windows.Forms.ContextMenuStrip.SourceControl?displayProperty=nameWithType> propriedade, que agora retorna uma referência ao controle do código-fonte. O comportamento anterior da propriedade era retornar `null` . Para obter mais informações, consulte [ \<AppContextSwitchOverrides> elemento](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md).

No .NET Core e no .NET 5,0 e posterior, `Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue` não há suporte para a opção.

#### <a name="version-introduced"></a>Versão introduzida

3.0

#### <a name="recommended-action"></a>Ação recomendada

Remova a opção. Não há suporte para a opção e nenhuma funcionalidade alternativa está disponível.

#### <a name="category"></a>Categoria

Windows Forms

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Forms.ContextMenuStrip.SourceControl?displayProperty=nameWithType>

<!-- 

#### Affected APIs

- `P:System.Windows.Forms.ContextMenuStrip.SourceControl`

-->
