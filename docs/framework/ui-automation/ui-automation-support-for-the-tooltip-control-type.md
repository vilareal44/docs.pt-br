---
title: Suporte de automação de interface de usuário para o tipo de controle ToolTip
description: Obtenha informações sobre o suporte de automação da interface do usuário para o tipo de controle ToolTip. Aprenda a estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, ToolTip control type
- ToolTip control type
- control types, ToolTip
ms.assetid: c3779d78-3164-43ae-8dae-bfaeafffdd65
ms.openlocfilehash: af9c9e0c67026ab339d08876bc5133fa42d1a5b7
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163172"
---
# <a name="ui-automation-support-for-the-tooltip-control-type"></a>Suporte de automação de interface de usuário para o tipo de controle ToolTip
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico fornece informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o suporte para o tipo de controle ToolTip. No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , um tipo de controle é um conjunto de condições que um controle deve atender para usar a <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> propriedade. As condições incluem diretrizes específicas para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] valores de propriedade e padrões de controle.  
  
 Os controles de dica de ferramenta são janelas pop-up que contêm texto.  
  
 As seções a seguir definem a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários para o tipo de controle ToolTip. Os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] requisitos se aplicam a todos os controles de dica de ferramenta, sejam eles, [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] Win32 ou Windows Forms.  
  
<a name="Required_UI_Automation_Tree_Structure"></a>
## <a name="required-ui-automation-tree-structure"></a>Estrutura de árvore de automação da interface do usuário necessária  
 A tabela a seguir descreve a exibição de controle e a exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore que pertence aos controles de dica de ferramenta e descreve o que pode ser contido em cada exibição. Para obter mais informações sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).  
  
|Exibição de controle|Exibição de conteúdo|  
|------------------|------------------|  
|ToolTip<br /><br /> -Texto (0 ou mais)<br />-Imagem (0 ou mais)|ToolTip|  
  
 Os controles de dica de ferramenta aparecem apenas na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore se eles podem receber o foco do teclado. Caso contrário, todas as informações da dica de ferramenta estarão disponíveis no `HelpTextProperty` elemento no [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] qual a dica de ferramenta está se referindo.  
  
 As dicas de ferramenta devem aparecer abaixo do controle ao qual suas informações se referem. Os clientes devem escutar o `ToolTipOpenedEvent` para garantir que eles obtenham consistentemente as informações contidas em dicas de ferramenta.  
  
<a name="Required_UI_Automation_Properties"></a>
## <a name="required-ui-automation-properties"></a>Propriedades de automação da interface do usuário necessárias  
 A tabela a seguir lista as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades cujo valor ou definição é especialmente relevante para controles de dica de ferramenta. Para obter mais informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Propriedade|Valor|Observações|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|Consulte observações.|O valor dessa propriedade precisa ser exclusivo em todos os controles em um aplicativo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|Consulte observações.|O retângulo mais externo que contém o controle inteiro.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|Consulte observações.|O ponto clicável deve ser parte da dica de ferramenta que irá ignorar o controle. Algumas dicas de ferramenta não têm essa capacidade e não terão um ponto clicável.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|Consulte observações.|Se o controle puder receber o foco do teclado, ele deverá dar suporte a essa propriedade.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Consulte observações.|O nome do controle de dica de ferramenta é o texto que é exibido dentro da dica de ferramenta.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|`Null`|Os controles de dica de ferramenta sempre são autorotulados por seu conteúdo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|ToolTip|Esse valor é o mesmo para todas as estruturas de interface do usuário.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"dica de ferramenta"|Cadeia de caracteres localizada correspondente ao tipo de controle ToolTip.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|Depende|Se o controle de dica de ferramenta puder receber o foco do teclado, ele deverá estar na exibição de conteúdo da árvore. Se for somente texto, ele estará disponível como o HelpTextproperty do controle que o gerou.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|O controle de dica de ferramenta sempre deve ser um controle.|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>
## <a name="required-ui-automation-control-patterns"></a>Padrões de controle de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] padrões de controle necessários para serem suportados pelos controles de dica de ferramenta. Para obter mais informações sobre padrões de controle, consulte [visão geral dos padrões de controle de automação da interface do usuário](ui-automation-control-patterns-overview.md).  
  
|Padrão de controle|Suporte|Observações|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IWindowProvider>|Depende|Dicas de ferramenta que podem ser fechadas clicando em um item de interface do usuário devem dar suporte a WindowPattern para que possam ser fechadas automaticamente.|  
|<xref:System.Windows.Automation.Provider.ITextProvider>|Depende|Para melhor acessibilidade, um controle de dica de ferramenta pode dar suporte ao padrão de controle de texto, embora não seja necessário. O padrão de controle de texto é útil quando o texto tem estilo e atributos avançados (por exemplo, cor, negrito e itálico).|  
  
<a name="Required_UI_Automation_Events"></a>
## <a name="required-ui-automation-events"></a>Eventos de automação da interface do usuário necessários  
 Os controles de dica de ferramenta devem gerar o `ToolTipOpenedEvent` quando aparecerem na tela. O Evento incluirá uma referência ao [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] elemento da própria dica de ferramenta.  
  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos necessários para serem suportados por todos os controles de dica de ferramenta. Para obter mais informações sobre eventos, consulte [visão geral dos eventos de automação da interface do usuário](ui-automation-events-overview.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Circunstância|Suporte|Observações|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.TextPatternIdentifiers.TextSelectionChangedEvent>|Depende|Nenhum|  
|<xref:System.Windows.Automation.TextPatternIdentifiers.TextChangedEvent>|Depende|Nenhum|  
|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowClosedEvent>|Depende|Nenhum|  
|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowOpenedEvent>|Depende|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ToolTipOpenedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ToolTipClosedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowVisualStateProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|Obrigatório|Nenhum|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.ControlType.ToolTip>
- [Visão Geral dos Tipos de Controle de Automação de Interface do Usuário](ui-automation-control-types-overview.md)
- [Visão geral de automação da interface do usuário](ui-automation-overview.md)
