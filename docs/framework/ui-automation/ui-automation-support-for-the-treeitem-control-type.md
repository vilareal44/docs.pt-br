---
title: Suporte de automação de interface de usuário para o tipo de controle TreeItem
description: Obtenha informações sobre o suporte de automação da interface do usuário para o tipo de controle TreeItem. Aprenda a estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários.
ms.date: 03/30/2017
helpviewer_keywords:
- control types, Tree Item
- Tree Item control type
- UI Automation, Tree Item control type
ms.assetid: 229f341a-477f-434e-b877-4db9973068eb
ms.openlocfilehash: 6b20e2ee6a54ad4f59f0f7d491c019bd8db24a80
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167828"
---
# <a name="ui-automation-support-for-the-treeitem-control-type"></a>Suporte de automação de interface de usuário para o tipo de controle TreeItem
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico fornece informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o suporte para o tipo de controle TreeItem. No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , um tipo de controle é um conjunto de condições que um controle deve atender para usar a <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> propriedade. As condições incluem diretrizes específicas para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] valores de propriedade e padrões de controle.  
  
 O tipo de controle TreeItem representa um nó dentro de um contêiner de árvore. Cada nó pode conter outros nós, chamados nós filho. Nós pai ou nós que contêm nós filhos podem ser exibidos como expandidos ou recolhidos.  
  
 As seções a seguir definem a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários para o tipo de controle TreeItem. Os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] requisitos se aplicam a todos os controles de item de árvore, sejam [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] , Win32 ou Windows Forms.  
  
<a name="Required_UI_Automation_Tree_Structure"></a>
## <a name="required-ui-automation-tree-structure"></a>Estrutura de árvore de automação da interface do usuário necessária  
 A tabela a seguir descreve a exibição de controle e a exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore que pertence aos controles de item de árvore e descreve o que pode ser contido em cada exibição. Para obter mais informações sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).  
  
|Exibição de controle|Exibição de conteúdo|  
|------------------|------------------|  
|TreeItem<br /><br /> -Caixa de seleção (0 ou 1)<br />-Imagem (0 ou 1)<br />-Botão (0 ou 1)<br />-TreeItem (0 ou mais)|TreeItem<br /><br /> -TreeItem (0 ou mais)|  
  
 Os controles de item de árvore podem ter zero ou mais filhos de item de árvore na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore. Se o controle de item de árvore tiver funcionalidade além do que é exposto nos padrões de controle listados abaixo, o controle deverá ser baseado no tipo de controle de item de dados.  
  
 Os itens de árvore recolhidos não serão exibidos no modo de exibição de controle ou de conteúdo até que se tornem expandidos e visíveis (ou possam ser rolados para a exibição).  
  
 O modo de exibição de controle pode conter detalhes adicionais para um controle, incluindo uma imagem associada ou um botão. Por exemplo, um item em um modo de exibição de estrutura de tópicos pode conter uma imagem, bem como um botão para expandir ou recolher a estrutura de tópicos. Esses objetos de detalhes não aparecem no modo de exibição de conteúdo porque as informações já estão representadas pelo item de árvore pai. Os itens de árvore que são rolados para fora da tela aparecerão nas exibições de controle e conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore e devem ter o <xref:System.Windows.Automation.AutomationElement.IsOffscreenProperty> conjunto como verdadeiro.  
  
<a name="Required_UI_Automation_Properties"></a>
## <a name="required-ui-automation-properties"></a>Propriedades de automação da interface do usuário necessárias  
 A tabela a seguir lista as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades cujo valor ou definição é especialmente relevante para controles de lista. Para obter mais informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Propriedade|Valor|Anotações|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|Consulte observações.|O valor dessa propriedade precisa ser exclusivo em todos os controles em um aplicativo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|Consulte observações.|O retângulo mais externo que contém o controle inteiro.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|Consulte observações.|Essa propriedade deve retornar um local do item que fará com que o item altere o estado de seleção ou se torne focado.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|TreeItem|Esse valor é o mesmo para todas as estruturas de interface do usuário.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|O controle de lista é sempre incluído na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|O controle de lista é sempre incluído na exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>|Consulte observações.|Essa propriedade é definida para indicar quando um controle de item de árvore é rolado para fora da tela.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|Consulte observações.|Se o controle puder receber o foco do teclado, ele deverá dar suporte a essa propriedade.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ItemTypeProperty>|Consulte observações.|Se o controle de item de árvore usar um ícone de Visual para indicar que é um tipo específico de objeto, essa propriedade deverá ter suporte e indicar o que é o objeto.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|`Null`|Os controles de item de árvore são autorotulados.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"item de árvore"|Cadeia de caracteres localizada correspondente ao tipo de controle TreeItem.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Consulte observações.|Essa propriedade expõe o texto exibido para cada controle de item de árvore.|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>
## <a name="required-ui-automation-control-patterns"></a>Padrões de controle de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] padrões de controle necessários para serem suportados por controles de lista. Para obter mais informações sobre padrões de controle, consulte [visão geral dos padrões de controle de automação da interface do usuário](ui-automation-control-patterns-overview.md).  
  
|Propriedade padrão de controle/padrão|Suporte/valor|Anotações|  
|---------------------------------------|--------------------|-----------|  
|<xref:System.Windows.Automation.Provider.IInvokeProvider>|Depende|Implemente esse padrão de controle se o item de árvore tiver um comando separado e acionável.|  
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|Sim|Todos os itens de árvore podem ser expandidos ou recolhidos.|  
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider.ExpandCollapseState%2A>|Nó expandido, recolhido ou folha|Os itens de árvore serão nós folha quando não forem expandidos ou recolhidos.|  
|<xref:System.Windows.Automation.Provider.IScrollItemProvider>|Depende|Implemente esse padrão de controle se o contêiner de árvore oferecer suporte ao padrão de controle Scroll.|  
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider>|Depende|Implemente esse padrão de controle se for possível ter uma seleção ativa mantida quando o usuário retornar ao contêiner de árvore.|  
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider.SelectionContainer%2A>|Sim|Essa propriedade irá expor o mesmo contêiner para todos os itens dentro do contêiner.|  
|<xref:System.Windows.Automation.Provider.IToggleProvider>|Depende|Implemente esse padrão de controle se o item de árvore tiver uma caixa de seleção associada.|  
  
<a name="Required_UI_Automation_Events"></a>
## <a name="required-ui-automation-events"></a>Eventos de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos necessários para serem suportados por todos os controles de item de árvore. Para obter mais informações sobre eventos, consulte [visão geral dos eventos de automação da interface do usuário](ui-automation-events-overview.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Circunstância|Suporte|Anotações|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ItemStatusProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent>|Depende|Nenhum|  
|<xref:System.Windows.Automation.MultipleViewPatternIdentifiers.CurrentViewProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent>|Depende|Nenhum|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent>|Depende|Nenhum|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent>|Depende|Nenhum|  
|<xref:System.Windows.Automation.TogglePatternIdentifiers.ToggleStateProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.ValuePatternIdentifiers.ValueProperty>evento de alteração de propriedade.|Depende|Nenhum|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.ControlType.TreeItem>
- [Visão Geral dos Tipos de Controle de Automação de Interface do Usuário](ui-automation-control-types-overview.md)
- [Visão geral de automação da interface do usuário](ui-automation-overview.md)
