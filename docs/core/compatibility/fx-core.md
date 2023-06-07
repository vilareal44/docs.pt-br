---
title: Alterações recentes – .NET Framework para o .NET Core
titleSuffix: ''
description: Lista as alterações significativas de .NET Framework para o .NET Core.
ms.date: 05/05/2020
ms.openlocfilehash: 5f7424fdd959044b729dfb04f4f0147fbc946bfd
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556300"
---
# <a name="breaking-changes-for-migration-from-net-framework-to-net-core"></a>Alterações recentes de migração do .NET Framework para o .NET Core

Se você estiver migrando um aplicativo do .NET Framework para o .NET Core, as alterações significativas listadas neste artigo poderão afetar você. As alterações significativas são agrupadas por categoria e dentro dessas categorias, pela versão do .NET Core na qual elas foram introduzidas.

> [!NOTE]
> Este artigo não é uma lista completa de alterações significativas entre o .NET Framework e o .NET Core. As alterações significativas mais importantes são adicionadas aqui, pois estamos cientes delas.

## <a name="core-net-libraries"></a>Bibliotecas principais do .NET

- [Alteração no valor padrão de UseShellExecute](#change-in-default-value-of-useshellexecute)
- [UnauthorizedAccessException gerado por FileSystemInfo. Attributes](#unauthorizedaccessexception-thrown-by-filesysteminfoattributes)
- [Tratamento de exceções de estado de processo corrompido não é suportado](#handling-corrupted-state-exceptions-is-not-supported)
- [As propriedades UriBuilder não precedem mais os caracteres à esquerda](#uribuilder-properties-no-longer-prepend-leading-characters)
- [Process. StartInfo gera InvalidOperationException para processos que você não iniciou](#processstartinfo-throws-invalidoperationexception-for-processes-you-didnt-start)

### <a name="net-core-21"></a>.NET Core 2.1

[!INCLUDE[Process.Start changes](~/includes/core-changes/corefx/2.1/process-start-changes.md)]

***

### <a name="net-core-10"></a>.NET Core 1.0

[!INCLUDE [UnauthorizedAccessException thrown by FileSystemInfo.Attributes](~/includes/core-changes/corefx/1.0/filesysteminfo-attributes-exceptions.md)]

***

[!INCLUDE [corrupted-state-exceptions](~/includes/core-changes/corefx/1.0/corrupted-state-exceptions.md)]

***

[!INCLUDE [uribuilder-behavior-changes](../../../includes/core-changes/corefx/1.0/uribuilder-behavior-changes.md)]

***

[!INCLUDE [startinfo-throws-exception](../../../includes/core-changes/corefx/1.0/startinfo-throws-exception.md)]

***

## <a name="cryptography"></a>Criptografia

- [O parâmetro booliano de SignedCms. ComputeSignature é respeitado](#boolean-parameter-of-signedcmscomputesignature-is-respected)

### <a name="net-core-21"></a>.NET Core 2.1

[!INCLUDE [Boolean parameter of SignedCms.ComputeSignature is respected](~/includes/core-changes/cryptography/2.1/compute-signature-silent-parameter.md)]

***

## <a name="msbuild"></a>MSBuild

- [Alteração de nome de arquivo de manifesto de recurso](#resource-manifest-file-name-change)

### <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE[Resource file names](~/includes/core-changes/msbuild/3.0/resource-manifest-name.md)]

***

## <a name="networking"></a>Rede

- [WebClient. CancelAsync nem sempre cancela imediatamente](#webclientcancelasync-doesnt-always-cancel-immediately)

### <a name="net-core-20"></a>.NET Core 2.0

[!INCLUDE [behavior-change-webclient-cancelasync](../../../includes/core-changes/networking/2.0/behavior-change-webclient-cancelasync.md)]

***

## <a name="windows-forms"></a>Windows Forms

Windows Forms suporte foi adicionado ao .NET Core na versão 3,0. Se você estiver migrando um aplicativo Windows Forms do .NET Framework para o .NET Core, as alterações significativas listadas aqui podem afetar seu aplicativo.

- [Controles removidos](#removed-controls)
- [Evento CellFormatting não gerado se ToolTip for mostrado](#cellformatting-event-not-raised-if-tooltip-is-shown)
- [Control. DefaultFont alterado para Segoe UI 9 pt](#default-control-font-changed-to-segoe-ui-9-pt)
- [Modernização do FolderBrowserDialog](#modernization-of-the-folderbrowserdialog)
- [SerializableAttribute removido de alguns tipos de Windows Forms](#serializableattribute-removed-from-some-windows-forms-types)
- [Não há suporte para a opção de compatibilidade AllowUpdateChildControlIndexForTabControls](#allowupdatechildcontrolindexfortabcontrols-compatibility-switch-not-supported)
- [Não há suporte para a opção de compatibilidade DomainUpDown. UseLegacyScrolling](#domainupdownuselegacyscrolling-compatibility-switch-not-supported)
- [Não há suporte para a opção de compatibilidade DoNotLoadLatestRichEditControl](#donotloadlatestricheditcontrol-compatibility-switch-not-supported)
- [Não há suporte para a opção de compatibilidade DoNotSupportSelectAllShortcutInMultilineTextBox](#donotsupportselectallshortcutinmultilinetextbox-compatibility-switch-not-supported)
- [Não há suporte para a opção de compatibilidade DontSupportReentrantFilterMessage](#dontsupportreentrantfiltermessage-compatibility-switch-not-supported)
- [Não há suporte para a opção de compatibilidade EnableVisualStyleValidation](#enablevisualstylevalidation-compatibility-switch-not-supported)
- [Não há suporte para a opção de compatibilidade UseLegacyContextMenuStripSourceControlValue](#uselegacycontextmenustripsourcecontrolvalue-compatibility-switch-not-supported)
- [Não há suporte para a opção de compatibilidade UseLegacyImages](#uselegacyimages-compatibility-switch-not-supported)

### <a name="net-core-31"></a>.NET Core 3.1

[!INCLUDE[Removed controls](~/includes/core-changes/windowsforms/3.1/remove-controls-3.1.md)]

***

[!INCLUDE[CellFormatting event](~/includes/core-changes/windowsforms/3.1/cellformatting-event-not-raised.md)]

***

### <a name="net-core-30"></a>.NET Core 3.0

[!INCLUDE[Control.DefaultFont changed to Segoe UI 9 pt](~/includes/core-changes/windowsforms/3.0/control-defaultfont-changed.md)]

***

[!INCLUDE[Modernization of the FolderBrowserDialog](~/includes/core-changes/windowsforms/3.0/modernized-folderbrowserdialog.md)]

***

[!INCLUDE[SerializableAttribute removed from some Windows Forms types](~/includes/core-changes/windowsforms/3.0/remove-serializationattribute.md)]

***

[!INCLUDE[AllowUpdateChildControlIndexForTabControls compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-allowupdatechildcontrolindexfortabcontrols.md)]

***

[!INCLUDE[DomainUpDown.UseLegacyScrolling compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacyscrolling.md)]

***

[!INCLUDE[DoNotLoadLatestRichEditControl compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-donotloadlatestricheditcontrol.md)]

***

[!INCLUDE[DoNotSupportSelectAllShortcutInMultilineTextBox compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-donotsupportselectallshortcutinmultilinetextbox.md)]

***

[!INCLUDE[DontSupportReentrantFilterMessage compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-dontsupportreentrantfiltermessage.md)]

***

[!INCLUDE[EnableVisualStyleValidation compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-enablevisualstylevalidation.md)]

***

[!INCLUDE[UseLegacyContextMenuStripSourceControlValue compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacycontextmenustripsourcecontrolvalue.md)]

***

[!INCLUDE[UseLegacyImages compatibility switch not supported](~/includes/core-changes/windowsforms/3.0/deprecate-uselegacyimages.md)]

***

## <a name="see-also"></a>Confira também

- [APIs que sempre lançam exceções no .NET Core](unsupported-apis.md)
- [Tecnologias do .NET Framework não disponíveis no .NET Core](../porting/net-framework-tech-unavailable.md)
