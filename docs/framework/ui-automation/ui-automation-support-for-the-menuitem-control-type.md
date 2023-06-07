---
title: Suporte de automação de interface de usuário para o tipo de controle MenuItem
description: Obtenha informações sobre o suporte de automação da interface do usuário para o tipo de controle MenuItem. Aprenda a estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários.
ms.date: 03/30/2017
helpviewer_keywords:
- control types, Menu Item
- Menu Item control type
- UI Automation, Menu Item control type
ms.assetid: 54bce311-3d23-40b9-ba90-1bdbdaf8fbba
ms.openlocfilehash: 248fdacee42c3a67c6c3b2d5792b774dfdc8408f
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166014"
---
# <a name="ui-automation-support-for-the-menuitem-control-type"></a>Suporte de automação de interface de usuário para o tipo de controle MenuItem

> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).

Este tópico fornece informações sobre [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] o suporte para o tipo de controle MenuItem. Ele descreve a estrutura de [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] árvore do controle e fornece as propriedades e os padrões de controle que são necessários para o tipo de controle MenuItem.

Um controle de menu permite a organização hierárquica de elementos associados a comandos e manipuladores de eventos. Em um aplicativo típico do Microsoft Windows, uma barra de menus contém vários itens de menu (como **arquivo**, **Editar**e **janela**) e cada item de menu exibe um menu. Um menu contém uma coleção de itens de menu (como **novo**, **aberto**e **fechado**), que pode ser expandida para exibir itens de menu adicionais ou executar uma ação específica quando clicado. Um item de menu pode ser hospedado em um menu, barra de menus ou barra de ferramentas.

As seções a seguir definem a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários para o tipo de controle MenuItem. Os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] requisitos se aplicam a todos os controles de lista, sejam [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] , Win32 ou Windows Forms.

<a name="Required_UI_Automation_Tree_Structure"></a>

## <a name="required-ui-automation-tree-structure"></a>Estrutura de árvore de automação da interface do usuário necessária

A tabela a seguir descreve a exibição de controle e a exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore que pertence aos controles de item de menu e descreve o que pode ser contido em cada exibição. Para obter mais informações sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).

|Exibição de controle|Exibição de conteúdo|
|------------------|------------------|
|MenuItem "ajuda"<br /><br /> <ul><li>Menu (submenu do item de menu ajuda)<br /><br /> <ul><li>MenuItem "tópicos da ajuda"</li><li>MenuItem "sobre o bloco de notas"</li></ul></li></ul>|MenuItem "ajuda"<br /><br /> -MenuItem "tópicos da ajuda"<br />-MenuItem "sobre o bloco de notas"|

A exibição de controle do controle de item de menu tem a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore mostrada acima. Observe que o item de menu **ajuda** está incluído para ilustrar melhor a estrutura em um menu típico para hierarquia de submenu.

Para o modo de exibição de conteúdo, o menu está ausente da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore porque não transmite informações significativas para o usuário final.

<a name="Required_UI_Automation_Properties"></a>

## <a name="required-ui-automation-properties"></a>Propriedades de automação da interface do usuário necessárias

A tabela a seguir lista as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades cujo valor ou definição é especialmente relevante para controles de item de menu. Para obter mais informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).

|Propriedade|Valor|Descrição|
|--------------|-----------|-----------------|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|Consulte observações.|O valor dessa propriedade precisa ser exclusivo em todos os controles em um aplicativo.|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|Consulte observações.|O retângulo mais externo que contém o controle inteiro.|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|Consulte observações.|Com suporte se houver um retângulo delimitador. Se nem todos os pontos dentro do retângulo delimitador forem clicáveis e você executar um teste de clique especializado, substitua e forneça um ponto clicável.|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|Consulte observações.|Se o controle puder receber o foco do teclado, ele deverá dar suporte a essa propriedade.|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Consulte observações.|O controle de item de menu está incluído na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore e é rotulado com um nome.|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|`Null`|Sem rótulo.|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|MenuItem|Esse valor é o mesmo para todas as estruturas de interface do usuário.|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"item de menu"|Cadeia de caracteres localizada correspondente ao tipo de controle MenuItem.|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|O controle de item de menu nunca é incluído na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|O controle de item de menu sempre deve ser incluído na exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|

<a name="Required_UI_Automation_Control_Patterns"></a>

## <a name="required-ui-automation-control-patterns"></a>Padrões de controle de automação da interface do usuário necessários

A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] padrões de controle necessários para serem suportados por controles de item de menu. Para obter mais informações sobre padrões de controle, consulte [visão geral dos padrões de controle de automação da interface do usuário](ui-automation-control-patterns-overview.md).

|Propriedade de padrão de controle|Suporte|Observações|
|------------------------------|-------------|-----------|
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|Depende|Se o controle puder ser expandido ou recolhido, implemente <xref:System.Windows.Automation.Provider.IExpandCollapseProvider> .|
|<xref:System.Windows.Automation.Provider.IInvokeProvider>|Depende|Se o controle executar uma única ação ou comando, implemente <xref:System.Windows.Automation.Provider.IInvokeProvider> .|
|<xref:System.Windows.Automation.Provider.IToggleProvider>|Depende|Se o controle representa uma opção que pode ser ativada ou desativada, implemente <xref:System.Windows.Automation.Provider.IToggleProvider> .|
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider>|Depende|Se o controle for usado para selecionar em uma lista de opções entre itens de menu, implemente <xref:System.Windows.Automation.Provider.ISelectionItemProvider> .|

<a name="UI_Automation_Events_for_Menu_Item"></a>

## <a name="ui-automation-events-for-menu-item"></a>Eventos de automação da interface do usuário para item de menu

A tabela a seguir lista os [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] eventos associados ao controle de item de menu.

|Evento|Suporte|Explicação|
|-----------|-------------|-----------------|
|<xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent>|Depende|Deve ser gerado se o controle der suporte ao padrão de controle Invoke.|
|<xref:System.Windows.Automation.TogglePatternIdentifiers.ToggleStateProperty>evento de alteração de propriedade.|Depende|Deve ser gerado se o controle suportar o padrão de controle de alternância.|
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty>evento de alteração de propriedade.|Depende|Deve ser gerado se o controle der suporte ao padrão de controle expandir/recolher.|
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent>|Depende|Nenhum.|

<a name="Required_UI_Automation_Events"></a>

## <a name="required-ui-automation-events"></a>Eventos de automação da interface do usuário necessários

A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos necessários para serem suportados por todos os controles de item de menu. Para obter mais informações sobre eventos, consulte [visão geral dos eventos de automação da interface do usuário](ui-automation-events-overview.md).

|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Circunstância|Suporte/valor|Observações|
|---------------------------------------------------------------------------------|--------------------|-----------|
|<xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent>|Depende|Nenhum|
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent>|Depende|Nenhum|
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent>|Depende|Nenhum|
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent>|Depende|Nenhum|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty>evento de alteração de propriedade.|Depende|Nenhum|
|<xref:System.Windows.Automation.TogglePatternIdentifiers.ToggleStateProperty>evento de alteração de propriedade.|Depende|Nenhum|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|Obrigatório|Nenhum|
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|Obrigatório|Nenhum|

<a name="Legacy_Issues"></a>

## <a name="legacy-issues"></a>Problemas herdados

O padrão de alternância só terá suporte quando o item de menu do Win32 estiver marcado e puder ser determinado de forma programática necessário para dar suporte ao padrão de alternância. Como o item de menu do Win32 não expõe se ele tem a capacidade de ser verificada, o padrão de invocação terá suporte quando o item de menu não estiver marcado. Será feita uma exceção para sempre dar suporte ao padrão Invoke mesmo para itens de menu que só devem dar suporte ao padrão de alternância. Isso é para que os clientes não se confundam que um elemento que estava dando suporte ao padrão Invoke (quando o item de menu estava desmarcado) não dá mais suporte ao padrão quando ele se torna marcado.

## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.ControlType.MenuItem>
- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Visão Geral dos Tipos de Controle de Automação de Interface do Usuário](ui-automation-control-types-overview.md)
- [Visão geral de automação da interface do usuário](ui-automation-overview.md)
