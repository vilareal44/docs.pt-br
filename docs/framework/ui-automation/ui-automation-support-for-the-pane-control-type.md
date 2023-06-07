---
title: Suporte de automação de interface de usuário para o Tipo de Controle Pane
description: Obtenha informações sobre o suporte de automação da interface do usuário para o tipo de controle de painel. Aprenda a estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, Pane control type
- Pane control type
- control types, Pane
ms.assetid: 79761191-4449-4630-899c-9cbdb8867d3f
ms.openlocfilehash: 4f7790298054fe947bfa6f7939238ffb956c6d93
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166040"
---
# <a name="ui-automation-support-for-the-pane-control-type"></a>Suporte de automação de interface de usuário para o Tipo de Controle Pane
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico fornece informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o suporte para o tipo de controle de painel. No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , um tipo de controle é um conjunto de condições que um controle deve atender para usar a <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> propriedade. As condições incluem diretrizes específicas para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] valores de propriedade e padrões de controle.  
  
 O tipo de controle de painel é usado para representar um objeto em uma janela de quadro ou de documento. Os usuários podem navegar entre controles de painel e dentro do conteúdo do painel atual, mas não podem navegar entre itens em painéis diferentes. Portanto, os controles de painel representam um nível de agrupamento menor que o Windows ou documentos, mas acima dos controles individuais. O usuário navega entre os painéis pressionando TAB, F6 ou CTRL + TAB, dependendo do contexto. Nenhuma navegação de teclado específica é exigida pelo tipo de controle de painel.  
  
 As seções a seguir definem a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários para o tipo de controle de painel. Os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] requisitos se aplicam a todos os controles de lista, sejam [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] , Win32 ou Windows Forms.  
  
<a name="Required_UI_Automation_Tree_Structure"></a>
## <a name="required-ui-automation-tree-structure"></a>Estrutura de árvore de automação da interface do usuário necessária  
 A tabela a seguir descreve a exibição de controle e a exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore que pertence aos controles de painel e descreve o que pode ser contido em cada exibição. Para obter mais informações sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).  
  
|Exibição de controle|Exibição de conteúdo|  
|------------------|------------------|  
|Painel|Painel|  
  
<a name="Required_UI_Automation_Properties"></a>
## <a name="required-ui-automation-properties"></a>Propriedades de automação da interface do usuário necessárias  
 A tabela a seguir lista as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades cujo valor ou definição é especialmente relevante para controles de painel. Para obter mais informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Propriedade|Valor|Observações|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|Consulte observações.|O valor dessa propriedade precisa ser exclusivo em todos os controles em um aplicativo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|Consulte observações.|O retângulo mais externo que contém o controle inteiro.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|Consulte observações.|Se o controle puder receber o foco do teclado, ele deverá dar suporte a essa propriedade.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Consulte observações.|O valor dessa propriedade sempre deve ser um título claro, conciso e significativo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|Consulte observações.|Essa propriedade expõe um ponto clicável do controle de painel que faz com que o painel se concentre quando é clicado.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|Consulte observações.|Controles de painel normalmente não têm um rótulo estático. Se houver um rótulo de texto estático, ele deverá ser exposto por meio dessa propriedade.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|Painel|Esse valor é o mesmo para todas as [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] estruturas.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|painel|Cadeia de caracteres localizada correspondente ao tipo de controle de painel.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|Os controles de painel são sempre incluídos na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|Os controles de painel são sempre incluídos na exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>|""|O texto de ajuda para controles de painel deve explicar por que a finalidade do quadro e como ele se relaciona com outros quadros. Uma descrição será necessária se a finalidade e a relação de quadros não estiverem claras do valor de `NameProperty` . "|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AccessKeyProperty>|Consulte observações.|Se uma combinação de teclas específica fornecer foco para o painel, essas informações deverão ser expostas por essa propriedade.|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>
## <a name="required-ui-automation-control-patterns"></a>Padrões de controle de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] padrões de controle necessários para serem suportados por todos os controles de painel. Para obter mais informações sobre padrões de controle, consulte [visão geral dos padrões de controle de automação da interface do usuário](ui-automation-control-patterns-overview.md).  
  
|Padrão de controle|Suporte|Observações|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.ITransformProvider>|Depende|Implemente esse padrão de controle se o controle de painel puder ser movido, redimensionado ou girado na tela.|  
|<xref:System.Windows.Automation.Provider.IWindowProvider>|Nunca|Se você precisar implementar esse padrão de controle, seu controle deverá ser baseado no <xref:System.Windows.Automation.ControlType.Window> tipo de controle.|  
|<xref:System.Windows.Automation.Provider.IDockProvider>|Depende|Implemente esse padrão de controle se o controle de painel puder ser encaixado.|  
|<xref:System.Windows.Automation.Provider.IScrollProvider>|Depende|Implemente esse padrão de controle se o controle de painel puder ser rolado.|  
  
<a name="Required_UI_Automation_Events"></a>
## <a name="required-ui-automation-events"></a>Eventos de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos necessários para serem suportados por todos os controles de painel. Para obter mais informações sobre eventos, consulte [visão geral dos eventos de automação da interface do usuário](ui-automation-events-overview.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Circunstância|Suporte/valor|Observações|  
|---------------------------------------------------------------------------------|--------------------|-----------|  
|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowClosedEvent>|Nunca|Nenhum|  
|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowOpenedEvent>|Nunca|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AsyncContentLoadedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalScrollPercentProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalViewSizeProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalScrollPercentProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalViewSizeProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowVisualStateProperty>evento de alteração de propriedade.|Nunca|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|Obrigatório|Nenhum|  
  
<a name="Pane_Control_Type_Example"></a>
## <a name="pane-control-type-example"></a>Exemplo de tipo de controle de painel  
 A imagem a seguir ilustra um controle que implementa o tipo de controle de painel.  
  
 ![Captura de tela da janela do applet com dois painéis](./media/uiauto-pane.GIF "uiauto_pane")  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Exibição de controle de árvore|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Árvore-exibição de conteúdo|  
|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|  
|<ul><li>Painel</li><li>Tree (padrão de rolagem)<br /><br /> <ul><li>TreeItem</li><li>Painel</li><li>Editar (padrão de rolagem</li></ul></li></ul>|-Painel<br />-Tree (padrão de rolagem)<br />- TreeItem<br />- ... Painel<br />-Editar<br />-(Padrão de rolagem)|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.ControlType.Pane>
- [Visão Geral dos Tipos de Controle de Automação de Interface do Usuário](ui-automation-control-types-overview.md)
- [Visão geral de automação da interface do usuário](ui-automation-overview.md)
