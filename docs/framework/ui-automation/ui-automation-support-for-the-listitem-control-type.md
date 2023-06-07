---
title: Suporte de automação de interface de usuário para o tipo de controle ListItem
description: Obtenha informações sobre o suporte de automação da interface do usuário para o tipo de controle ListItem. Aprenda a estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários.
ms.date: 03/30/2017
helpviewer_keywords:
- control types, List
- List Item control type
- UI Automation, List Item control type
ms.assetid: 34f533bf-fc14-4e78-8fee-fb7107345fab
ms.openlocfilehash: bf1690b094e9d472fd4213f7fa3df545dca6ebac
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166054"
---
# <a name="ui-automation-support-for-the-listitem-control-type"></a>Suporte de automação de interface de usuário para o tipo de controle ListItem
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico fornece informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o suporte para o <xref:System.Windows.Automation.ControlType.ListItem> tipo de controle. No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , um tipo de controle é um conjunto de condições que um controle deve atender para usar a <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> propriedade. As condições incluem diretrizes específicas para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] valores de propriedade e padrões de controle.  
  
 Os controles de item de lista são um exemplo de controles que implementam o tipo de controle ListItem.  
  
 As seções a seguir definem a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários para o tipo de controle ListItem. Os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] requisitos se aplicam a todos os controles de lista, sejam [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] , Win32 ou Windows Forms.  
  
<a name="Required_UI_Automation_Tree_Structure"></a>
## <a name="required-ui-automation-tree-structure"></a>Estrutura de árvore de automação da interface do usuário necessária  
 A tabela a seguir descreve a exibição de controle e a exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore que pertence a controles de item de lista e descreve o que pode ser contido em cada exibição. Para obter mais informações sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).  
  
|Exibição de controle|Exibição de conteúdo|  
|------------------|------------------|  
|Item<br /><br /> -Imagem (0 ou mais)<br />-Texto (0 ou mais)<br />-Editar (0 ou mais)|Item|  
  
 Os filhos de um controle de item de lista na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore devem sempre ser "0". Se a estrutura do controle for tal que outros itens estejam contidos abaixo do item de lista, ele deverá seguir os requisitos para o [suporte de automação da interface do usuário para o tipo de controle do tipo de controle TreeItem](ui-automation-support-for-the-treeitem-control-type.md) .  
  
<a name="Required_UI_Automation_Properties"></a>
## <a name="required-ui-automation-properties"></a>Propriedades de automação da interface do usuário necessárias  
 A tabela a seguir lista as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades cujo valor ou definição é especialmente relevante para controles de item de lista. Para obter mais informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Propriedade|Valor|Observações|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|Consulte observações.|O valor dessa propriedade precisa ser exclusivo em todos os controles em um aplicativo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|Consulte observações.|Esse valor dessa propriedade deve incluir a área do conteúdo de imagem e texto do item de lista.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|Depende|Se o controle de lista tiver um ponto clicável (um ponto que pode ser clicado para fazer com que a lista tenha foco), esse ponto deverá ser exposto por meio dessa propriedade. Se o controle de lista for completamente coberto por itens de lista descendente, ele gerará um <xref:System.Windows.Automation.NoClickablePointException> para indicar que o cliente deve solicitar um item dentro do controle de lista para um ponto clicável.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Consulte observações.|O valor de uma propriedade de nome de controle de item de lista provém do conteúdo de texto do item.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|Consulte observações.|Se houver um rótulo de texto estático, essa propriedade deverá expor uma referência a esse controle.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|Item|Esse valor é o mesmo para todas as estruturas de interface do usuário.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"item de lista"|Cadeia de caracteres localizada correspondente ao tipo de controle ListItem.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|O controle de lista é sempre incluído na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|O controle de lista é sempre incluído na exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|True|Se o contêiner puder aceitar entrada de teclado, esse valor de propriedade deverá ser verdadeiro.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>|""|O texto de ajuda para controles de lista deve explicar por que o usuário está sendo solicitado a fazer uma escolha em uma lista de opções, que geralmente é o mesmo tipo de informações apresentadas por meio de uma dica de ferramenta. Por exemplo, "Selecione um item para definir a resolução de vídeo para o monitor".|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ItemTypeProperty>|Depende|Essa propriedade deve ser exposta para controles de item de lista que estão representando um objeto subjacente. Esses controles de item de lista normalmente têm um ícone associado ao controle que os usuários associam ao objeto subjacente.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>|Depende|Essa propriedade deve retornar um valor para se o item de lista estiver rolado no momento na exibição dentro do contêiner pai que implementa o padrão de controle Scroll.|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>
## <a name="required-ui-automation-control-patterns"></a>Padrões de controle de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] padrões de controle necessários para serem suportados pelos controles de item de lista. Para obter mais informações sobre padrões de controle, consulte [visão geral dos padrões de controle de automação da interface do usuário](ui-automation-control-patterns-overview.md).  
  
|Padrão de controle|Suporte|Observações|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider>|Sim|O controle de item de lista deve implementar esse padrão de controle. Isso permite que os controles de itens de lista sejam transmitidos quando são selecionados.|  
|<xref:System.Windows.Automation.Provider.IScrollItemProvider>|Depende|Se o item de lista estiver contido em um contêiner que é rolável, esse padrão de controle deverá ser implementado.|  
|<xref:System.Windows.Automation.Provider.IToggleProvider>|Depende|Se o item de lista for verificável e a ação não executar uma alteração de estado de seleção, esse padrão de controle deverá ser implementado.|  
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|Depende|Se o item puder ser manipulado para mostrar ou ocultar informações, esse padrão de controle deverá ser implementado.|  
|<xref:System.Windows.Automation.Provider.IValueProvider>|Depende|Se o item puder ser editado, esse padrão de controle deverá ser implementado. As alterações no controle de item de lista causarão alterações nos valores de <xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty> e <xref:System.Windows.Automation.Provider.IValueProvider.Value%2A> .|  
|<xref:System.Windows.Automation.Provider.IGridItemProvider>|Depende|Se houver suporte para a navegação espacial de item para item no contêiner de lista e o contêiner for organizado em linhas e colunas, o padrão de controle de item de grade deverá ser implementado.|  
|<xref:System.Windows.Automation.Provider.IInvokeProvider>|Depende|Se o item tiver um comando que possa ser executado nele, separado da seleção, esse padrão deverá ser implementado. Normalmente, essa é uma ação associada ao clique duplo no controle de item de lista. Os exemplos devem iniciar um documento no Microsoft Windows Explorer ou reproduzir um arquivo de música no Microsoft Windows Media Player.|  
  
<a name="Required_UI_Automation_Events"></a>
## <a name="required-ui-automation-events"></a>Eventos de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos necessários para serem suportados por todos os controles de item de lista. Para obter mais informações sobre eventos, consulte [visão geral dos eventos de automação da interface do usuário](ui-automation-events-overview.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Circunstância|Suporte|Observações|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent>|Depende|Nenhum|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ItemStatusProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.ValuePatternIdentifiers.ValueProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.TogglePatternIdentifiers.ToggleStateProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|Obrigatório|Nenhum|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.ControlType.ListItem>
- [Visão Geral dos Tipos de Controle de Automação de Interface do Usuário](ui-automation-control-types-overview.md)
- [Visão geral de automação da interface do usuário](ui-automation-overview.md)
