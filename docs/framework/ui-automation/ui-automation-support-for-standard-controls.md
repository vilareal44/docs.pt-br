---
title: Suporte de automação de interface do usuário para Controles Padrão
description: Obtenha informações sobre o suporte de automação da interface do usuário para controles padrão em aplicativos desenvolvidos para Windows Presentation Foundation (WPF), Win32 e Windows Forms.
ms.date: 03/30/2017
helpviewer_keywords:
- controls, UI Automation support for
- UI Automation, support for standard controls
ms.assetid: 3770ea8a-2655-4add-9c59-fe0610ad5084
ms.openlocfilehash: 17916a6978008439e91caae00d8b6f26045f9018
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166122"
---
# <a name="ui-automation-support-for-standard-controls"></a>Suporte de automação de interface do usuário para Controles Padrão
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico contém informações sobre [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] o suporte para controles padrão em aplicativos desenvolvidos para as [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] estruturas do, Win32 e Windows Forms.  
  
<a name="Windows_Presentation_Foundation_Controls"></a>
## <a name="windows-presentation-foundation-controls"></a>Controles de Windows Presentation Foundation  
 Todos os [!INCLUDE[TLA2#tla_winclient](../../../includes/tla2sharptla-winclient-md.md)] elementos de controle que fornecem informações ou suporte para interação do usuário têm suporte nativo completo para o [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] . Outros elementos, como painéis, não são visíveis para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] .  
  
<a name="Win32_Controls"></a>
## <a name="win32-controls"></a>Controles do Win32  
 A maioria dos controles do Win32 é exposta a [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] provedores do lado do cliente no UIAutomationClientsideProviders.dll. Esse assembly é registrado automaticamente para uso com aplicativos cliente de automação da interface do usuário.  
  
 O suporte completo é fornecido somente para controles da versão 6 do *ComCtrl32.dll*.  
  
 Há suporte para os controles a seguir.  
  
|Nome da classe|Tipo de controle|  
|----------------|------------------|  
|Botão|Botão|  
|Botão|RadioButton|  
|Botão|Agrupar|  
|Botão|CheckBox|  
|Botão|Hyperlink|  
|Botão|SplitButton|  
|Botão|CheckBox|  
|ComboBoxEx32|ComboBox|  
|ComboBox|ComboBox|  
|Editar|Document|  
|Editar|Editar|  
|SysLink|Hyperlink|  
|Estático|Texto|  
|Estático|Image|  
|SysIPAddress32|Personalizado|  
|SysHeader32|Cabeçalho/HeaderItem|  
|SysListView32|DataGrid|  
|SysListView32|Lista|  
|ListBox|Lista|  
|ListBox|Item|  
|#32768|Menu|  
|#32768|MenuItem|  
|msctls_progress32|ProgressBar|  
|RichEdit|Documento. Veja a observação.|  
|RichEdit20A|Document|  
|RichEdit20W|Document|  
|RichEdit50W|Document|  
|ScrollBar|Controle deslizante|  
|msctls_trackbar32|Controle deslizante|  
|msctls_updown32|Controle giratório|  
|msctls_statusbar32|StatusBar|  
|SysTabControl32|Tab|  
|SysTabControl32|TabItem|  
|ToolbarWindow32|ToolBar|  
|ToolbarWindow32|MenuItem|  
|ToolbarWindow32|Botão|  
|ToolbarWindow32|CheckBox|  
|ToolbarWindow32|RadioButton|  
|ToolbarWindow32|Separador|  
|tooltips_class32|ToolTip|  
|#32774|ToolTip|  
|ReBarWindow32|Barra de ferramentas|  
|SysTreeView32|Árvore|  
|SysTreeView32|TreeItem|  
  
 **Observação** O controle RichEdit tem suporte apenas para versões fornecidas com o Windows Vista (no RichEd20.dll versão 3,1 e posterior e MsftEdit.dll versão 4,1 e posterior).  
  
 Não há suporte para os seguintes controles.  
  
|Nome da classe|Tipo de controle|  
|----------------|------------------|  
|SysAnimate32|Image|  
|SysPager|Controle giratório|  
|SysDateTimePick32|Personalizado|  
|SysMonthCal32|Calendário|  
|MS_WINNOTE|Dica de ferramenta|  
|VBBubble|Dica de ferramenta|  
|ScrollBar (quando usado como um controle autônomo)|Controle deslizante|  
|Subgrade|Personalizado|  
  
<a name="Windows_Forms_Controls"></a>
## <a name="windows-forms-controls"></a>Controles de Windows Forms  
 Os controles de Windows Forms são expostos a [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] por meio de provedores do lado do cliente no UIAutomationClientsideProviders.dll. Esse assembly é registrado automaticamente para uso com aplicativos cliente de automação da interface do usuário.  
  
 Normalmente, Windows Forms controles que são wrappers gerenciados para controles comuns do Win32 são suportados pelo [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] . Há suporte para os controles a seguir.  
  
|Nome da Classe|  
|----------------|  
|Botão|  
|CheckBox|  
|CheckedListBox|  
|ColorDialog|  
|ComboBox|  
|FolderBrowser|  
|FontDialog|  
|GroupBox|  
|HscrollBar|  
|ImageList|  
|Rotular|  
|ListBox|  
|ListView|  
|MainMenu/ContextMenu|  
|MonthCalendar|  
|NotifyIcon|  
|OpenFileDialog|  
|PageSetupDialog|  
|PrintDialog|  
|ProgressBar|  
|RadioButton|  
|RichTextBox|  
|SaveFileDialog|  
|ScrollableControl|  
|SoundPlayer|  
|StatusBar|  
|TabControl/TabPage|  
|TextBox|  
|Timer|  
|Barra de ferramentas|  
|ToolTip|  
|TrackBar|  
|TreeView|  
|VscrollBar|  
|WebBrowser|  
  
 Os controles a seguir são expostos [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] apenas por meio de seu suporte para o Microsoft acessibilidade ativa. Algumas funcionalidades podem não estar disponíveis.  
  
|Nome do controle|  
|------------------|  
|BindingSource|  
|DataGrid|  
|DataGridView|  
|DataNavigator|  
|DomainUpDown|  
|ErrorProvider|  
|FlowLayoutPanel|  
|Formulário|  
|LinkLabel|  
|HelpProvider|  
|MaskedTextBox|  
|MenuStrip/ContextMenuStrip|  
|NumericUpDown|  
|Painel|  
|PictureBox|  
|PrintDocument|  
|Visualização-controle|  
|Caixa de diálogo de visualização prévia|  
|PropertyGrid|  
|UserControl|  
|ToolStrip|  
|TableLayoutPanel|  
|SplitContainer/SplitterPanel|  
|Splitter|  
|RaftingContainer|  
|StatusStrip|  
  
## <a name="see-also"></a>Confira também

- [Tipos de controle de automação de interface do usuário](ui-automation-control-types.md)
