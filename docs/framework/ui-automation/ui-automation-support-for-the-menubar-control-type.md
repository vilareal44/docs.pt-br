---
title: Suporte de automação de interface de usuário para o tipo de controle MenuBar
description: Obtenha informações sobre o suporte de automação da interface do usuário para o tipo de controle MenuBar. Aprenda a estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, Menu Bar control type
- control types, Menu Bar
- Menu Bar control type
ms.assetid: c1202b21-c1f0-4560-853c-7b99bd73ad97
ms.openlocfilehash: 923f8d9dc62a7175b2bcfe2b0839b1435bf83132
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166028"
---
# <a name="ui-automation-support-for-the-menubar-control-type"></a>Suporte de automação de interface de usuário para o tipo de controle MenuBar
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico fornece informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o suporte para o <xref:System.Windows.Automation.ControlType.MenuBar> tipo de controle. No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , um tipo de controle é um conjunto de condições que um controle deve atender para usar a <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> propriedade. As condições incluem diretrizes específicas para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] valores de propriedade e padrões de controle.  
  
 Os controles de barra de menus são um exemplo de controles que implementam o tipo de controle MenuBar. As barras de menu fornecem um meio para os usuários ativarem comandos e opções contidas em um aplicativo.  
  
 As seções a seguir definem a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários para o tipo de controle MenuBar. Os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] requisitos se aplicam a todos os controles de lista, sejam [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] , Win32 ou Windows Forms.  
  
<a name="Required_UI_Automation_Tree_Structure"></a>
## <a name="required-ui-automation-tree-structure"></a>Estrutura de árvore de automação da interface do usuário necessária  
 A tabela a seguir descreve a exibição de controle e a exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore que pertence aos controles de barra de menus e descreve o que pode ser contido em cada exibição. Para obter mais informações sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).  
  
|Exibição de controle|Exibição de conteúdo|  
|------------------|------------------|  
|MenuBar<br /><br /> -MenuItem (1 ou mais)<br />-Outros controles (0 ou muitos)|MenuBar<br /><br /> -MenuItem (1 ou mais)<br />-Outros controles (0 ou muitos)|  
  
 Os controles de barra de menus podem conter outros controles, como controles de edição e caixas de combinação dentro de sua estrutura. Esses controles adicionais correspondem aos "outros controles" listados acima nas exibições de controle e conteúdo.  
  
<a name="Required_UI_Automation_Properties"></a>
## <a name="required-ui-automation-properties"></a>Propriedades de automação da interface do usuário necessárias  
 A tabela a seguir lista as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades cujo valor ou definição é especialmente relevante para os controles de barra de menus. Para obter mais informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Propriedade|Valor|Observações|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|Consulte observações.|O valor exposto por essa propriedade deve incluir todos os controles contidos nela.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Consulte observações.|O controle de barra de menus não precisa de um nome, a menos que um aplicativo tenha mais de uma barra de menus. Se houver mais de uma barra de menus em um aplicativo, essa propriedade deverá ser usada para expor nomes de distinção, como "formatação" ou "estrutura de tópicos".|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|`Null`|Controles de barra de menu nunca têm um rótulo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|MenuBar|Esse valor é o mesmo para todas as estruturas de interface do usuário.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"barra de menus"|Cadeia de caracteres localizada correspondente ao tipo de controle MenuBar.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|O controle barra de menus sempre é incluído na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|O controle barra de menus sempre é incluído na exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>|Consulte observações.|O valor dessa propriedade depende de se o controle é visível na tela.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.OrientationProperty>|Depende|Essa propriedade expõe se o controle de barra de menus é horizontal ou vertical.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|True|Os controles de barra de menus têm foco no teclado, pois os controles que eles contêm podem ter o foco do teclado.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>|Consulte observações.|Nenhum cenário para quando o texto de ajuda é necessário para um controle de barra de menus.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AcceleratorKeyProperty>|`Null`|Barras de menu nunca têm teclas de aceleração.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AccessKeyProperty>|PRESSIONANDO|Pressionar a tecla ALT deve sempre colocar o foco na barra de menus dentro do aplicativo.|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>
## <a name="required-ui-automation-control-patterns"></a>Padrões de controle de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] padrões de controle necessários para serem suportados por controles de barra de menu. Para obter mais informações sobre padrões de controle, consulte [visão geral dos padrões de controle de automação da interface do usuário](ui-automation-control-patterns-overview.md).  
  
|Padrão de controle|Suporte|Observações|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|Depende|Se o controle puder ser expandido ou recolhido, implemente <xref:System.Windows.Automation.Provider.IExpandCollapseProvider> .|  
|<xref:System.Windows.Automation.Provider.IDockProvider>|Depende|Se o controle puder ser encaixado em partes diferentes da tela, implemente <xref:System.Windows.Automation.Provider.IDockProvider> .|  
|<xref:System.Windows.Automation.Provider.ITransformProvider>|Depende|Se o controle puder ser redimensionado, girado ou movido, ele deverá implementar <xref:System.Windows.Automation.Provider.ITransformProvider> .|  
  
<a name="Required_UI_Automation_Events"></a>
## <a name="required-ui-automation-events"></a>Eventos de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos necessários para serem suportados por todos os controles de barra de menus. Para obter mais informações sobre eventos, consulte [visão geral dos eventos de automação da interface do usuário](ui-automation-events-overview.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Circunstância|Suporte/valor|Observações|  
|---------------------------------------------------------------------------------|--------------------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|Obrigatório|Nenhum|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.ControlType.MenuBar>
- [Visão Geral dos Tipos de Controle de Automação de Interface do Usuário](ui-automation-control-types-overview.md)
- [Visão geral de automação da interface do usuário](ui-automation-overview.md)
