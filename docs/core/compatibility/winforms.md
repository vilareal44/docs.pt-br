---
title: Windows Forms alterações significativas
description: Lista as alterações significativas no Windows Forms para .NET Core.
ms.date: 01/08/2020
ms.openlocfilehash: beb9a42e4b5007f03480cd74f57bbfbbfc3f48b1
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556127"
---
# <a name="breaking-changes-in-windows-forms"></a>Alterações recentes no Windows Forms

Windows Forms suporte foi adicionado ao .NET Core na versão 3,0. Este artigo lista as alterações significativas para Windows Forms pela versão do .NET Core na qual elas foram introduzidas. Se você estiver atualizando um aplicativo Windows Forms de .NET Framework ou de uma versão anterior do .NET Core (3,0 ou posterior), este artigo será aplicável a você.

As seguintes alterações significativas estão documentadas nesta página:

| Alteração significativa | Versão introduzida |
| - | :-: |
| [Controles da barra de status removidos](#removed-status-bar-controls) | 5.0 |
| [Os métodos WinForms agora lançam ArgumentException](#winforms-methods-now-throw-argumentexception) | 5.0 |
| [Os métodos WinForms agora lançam ArgumentNullException](#winforms-methods-now-throw-argumentnullexception) | 5.0 |
| [As propriedades do WinForms agora geram ArgumentOutOfRangeException](#winforms-properties-now-throw-argumentoutofrangeexception) | 5.0 |
| [Controles removidos](#removed-controls) | 3.1 |
| [Evento CellFormatting não gerado se ToolTip for mostrado](#cellformatting-event-not-raised-if-tooltip-is-shown) | 3.1 |
| [Control. DefaultFont alterado para Segoe UI 9 pt](#default-control-font-changed-to-segoe-ui-9-pt) | 3.0 |
| [Modernização do FolderBrowserDialog](#modernization-of-the-folderbrowserdialog) | 3.0 |
| [SerializableAttribute removido de alguns tipos de Windows Forms](#serializableattribute-removed-from-some-windows-forms-types) | 3.0 |
| [Não há suporte para a opção de compatibilidade AllowUpdateChildControlIndexForTabControls](#allowupdatechildcontrolindexfortabcontrols-compatibility-switch-not-supported) | 3.0 |
| [Não há suporte para a opção de compatibilidade DomainUpDown. UseLegacyScrolling](#domainupdownuselegacyscrolling-compatibility-switch-not-supported) | 3.0 |
| [Não há suporte para a opção de compatibilidade DoNotLoadLatestRichEditControl](#donotloadlatestricheditcontrol-compatibility-switch-not-supported) | 3.0 |
| [Não há suporte para a opção de compatibilidade DoNotSupportSelectAllShortcutInMultilineTextBox](#donotsupportselectallshortcutinmultilinetextbox-compatibility-switch-not-supported) | 3.0 |
| [Não há suporte para a opção de compatibilidade DontSupportReentrantFilterMessage](#dontsupportreentrantfiltermessage-compatibility-switch-not-supported) | 3.0 |
| [Não há suporte para a opção de compatibilidade EnableVisualStyleValidation](#enablevisualstylevalidation-compatibility-switch-not-supported) | 3.0 |
| [Não há suporte para a opção de compatibilidade UseLegacyContextMenuStripSourceControlValue](#uselegacycontextmenustripsourcecontrolvalue-compatibility-switch-not-supported) | 3.0 |
| [Não há suporte para a opção de compatibilidade UseLegacyImages](#uselegacyimages-compatibility-switch-not-supported) | 3.0 |

## <a name="net-50"></a>.NET 5,0

[!INCLUDE [winforms-deprecated-controls](../../../includes/core-changes/windowsforms/5.0/winforms-deprecated-controls.md)]

***

[!INCLUDE [invalid-args-cause-argumentexception](../../../includes/core-changes/windowsforms/5.0/invalid-args-cause-argumentexception.md)]

***

[!INCLUDE [null-args-cause-argumentnullexception](../../../includes/core-changes/windowsforms/5.0/null-args-cause-argumentnullexception.md)]

***

[!INCLUDE [invalid-args-cause-argumentoutofrangeexception](../../../includes/core-changes/windowsforms/5.0/invalid-args-cause-argumentoutofrangeexception.md)]

***

## <a name="net-core-31"></a>.NET Core 3.1

[!INCLUDE[Removed controls](~/includes/core-changes/windowsforms/3.1/remove-controls-3.1.md)]

***

[!INCLUDE[CellFormatting event](~/includes/core-changes/windowsforms/3.1/cellformatting-event-not-raised.md)]

***

## <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE[Control.DefaultFont changed to Segoe UI 9pt](~/includes/core-changes/windowsforms/3.0/control-defaultfont-changed.md)]

***

[!INCLUDE[Modernization of the FolderBrowserDialog](~/includes/core-changes/windowsforms/3.0/modernized-folderbrowserdialog.md)]

***

[!INCLUDE[SerializableAttribute removed from some Windows Forms types](~/includes/core-changes/windowsforms/3.0/remove-serializationattribute.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.AllowUpdateChildControlIndexForTabControls compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-allowupdatechildcontrolindexfortabcontrols.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacyscrolling.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.DoNotLoadLatestRichEditControl compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-donotloadlatestricheditcontrol.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-donotsupportselectallshortcutinmultilinetextbox.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.DontSupportReentrantFilterMessage compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-dontsupportreentrantfiltermessage.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.EnableVisualStyleValidation compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-enablevisualstylevalidation.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacycontextmenustripsourcecontrolvalue.md)]

***

[!INCLUDE[Switch.System.Windows.Forms.UseLegacyImages compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacyimages.md)]

***

## <a name="see-also"></a>Confira também

- [Porta um aplicativo de Windows Forms para o .NET Core](../porting/winforms.md)
