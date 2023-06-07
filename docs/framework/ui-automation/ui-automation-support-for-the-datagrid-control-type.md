---
title: Suporte de automação de interface de usuário para o tipo de controle DataGrid
description: Obtenha informações sobre o suporte de automação da interface do usuário para o tipo de controle DataGrid. Aprenda a estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários.
ms.date: 03/30/2017
helpviewer_keywords:
- Data Grid control type
- control types, Data Grid
- UI Automation, Data Grid control type
ms.assetid: a3db4a3f-feb5-4e5f-9b42-aae7fa816e8a
ms.openlocfilehash: 13f265e414383e646f3e138feb890661fa01e2de
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167991"
---
# <a name="ui-automation-support-for-the-datagrid-control-type"></a>Suporte de automação de interface de usuário para o tipo de controle DataGrid
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico fornece informações sobre [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] o suporte para o tipo de controle DataGrid. No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , um tipo de controle é um conjunto de condições que um controle deve atender para usar a `ControlType` propriedade. As condições incluem diretrizes específicas para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] valores de propriedade e padrões de controle.  
  
 O tipo de controle DataGrid permite que um usuário trabalhe facilmente com itens que contêm metadados representados em colunas. Os controles de grade de dados têm linhas de itens e colunas de informações sobre esses itens. Um controle de exibição de lista no Microsoft Vista Explorer é um exemplo que dá suporte ao tipo de controle DataGrid.  
  
 As seções a seguir definem a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários para o tipo de controle DataGrid. Os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] requisitos se aplicam a todos os controles de grade de dados, sejam [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] , Win32 ou Windows Forms.  
  
## <a name="required-ui-automation-tree-structure"></a>Estrutura de árvore de automação da interface do usuário necessária  
 A tabela a seguir descreve a exibição de controle e a exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore que pertence a controles de grade de dados e descreve o que pode ser contido em cada exibição. Para obter mais informações sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Exibição de controle de árvore|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Árvore-exibição de conteúdo|  
|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|  
|DataGrid<br /><br /> <ul><li>Cabeçalho (0, 1 ou 2)<br /><br /> <ul><li>HeaderItem (número de colunas ou linhas)</li></ul></li><li>DataItem (0 ou mais; pode ser estruturado na hierarquia)</li></ul>|DataGrid<br /><br /> -DataItem (0 ou mais; pode ser estruturado na hierarquia)|  
  
<a name="Required_UI_Automation_Properties"></a>
## <a name="required-ui-automation-properties"></a>Propriedades de automação da interface do usuário necessárias  
 A tabela a seguir lista as propriedades cujo valor ou definição é especialmente relevante para os controles de grade de dados. Para obter mais informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).  
  
|Propriedade|Valor|Anotações|  
|--------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|Consulte observações.|O valor dessa propriedade precisa ser exclusivo em todos os controles em um aplicativo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|Consulte observações.|O retângulo mais externo que contém o controle inteiro.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|Consulte observações.|Com suporte se houver um retângulo delimitador. Se nem todos os pontos dentro do retângulo delimitador forem clicáveis e você executar um teste de clique especializado, substitua e forneça um ponto clicável.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|DataGrid|Esse valor é o mesmo para todas as estruturas de interface do usuário.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|O valor dessa propriedade sempre deve ser verdadeiro. Isso significa que o controle de grade de dados sempre deve estar na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|O valor dessa propriedade sempre deve ser verdadeiro. Isso significa que o controle de grade de dados sempre deve estar na exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|Consulte observações.|Se o controle puder receber o foco do teclado, ele deverá dar suporte a essa propriedade.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|Consulte observações.|Se houver um rótulo de texto estático, essa propriedade deverá expor uma referência a esse controle.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"grade de dados"|Cadeia de caracteres localizada correspondente ao tipo de controle DataGrid.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Consulte observações.|O controle de grade de dados normalmente Obtém o valor de sua `Name` propriedade de um rótulo de texto estático. Se não houver um rótulo de texto estático, um desenvolvedor de aplicativo deverá atribuir um valor para a `Name` propriedade. O valor da `Name` Propriedade nunca deve ser o conteúdo textual do controle de edição.|  
  
## <a name="required-ui-automation-control-patterns"></a>Padrões de controle de automação da interface do usuário necessários  
 A tabela a seguir lista os padrões de controle necessários para serem suportados por todos os controles de grade de dados. Para obter mais informações sobre padrões de controle, consulte [visão geral dos padrões de controle de automação da interface do usuário](ui-automation-control-patterns-overview.md).  
  
|Padrão de controle|Suporte|Anotações|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IGridProvider>|Sim|O próprio controle de grade de dados sempre dá suporte ao padrão de controle de grade porque os itens que ele contém metadados que são dispostos em uma grade.|  
|<xref:System.Windows.Automation.Provider.IScrollProvider>|Depende|A capacidade de rolar a grade de dados depende do conteúdo e se as barras de rolagem estão presentes.|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider>|Depende|A capacidade de selecionar a grade de dados depende do conteúdo.|  
|<xref:System.Windows.Automation.Provider.ITableProvider>|Sim|O controle de grade de dados sempre tem um cabeçalho dentro de sua subárvore, de modo que o padrão de controle de tabela deve ser suportado.|  
  
 Os itens de dados dentro dos contêineres de grade de dados terão suporte no mínimo:  
  
- Padrão de controle de item de seleção (se a grade de dados for selecionável)  
  
- Padrão de controle de item de rolagem (se a grade de dados for rolável)  
  
- Padrão de controle de item de grade  
  
- padrão de controle Item de Tabela  
  
<a name="Required_UI_Automation_Events"></a>
## <a name="required-ui-automation-events"></a>Eventos de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos necessários para serem suportados por todos os controles de grade de dados. Para obter mais informações sobre eventos, consulte [visão geral dos eventos de automação da interface do usuário](ui-automation-events-overview.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Circunstância|Suporte|Anotações|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LayoutInvalidatedEvent>|Depende|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.MultipleViewPatternIdentifiers.CurrentViewProperty>evento de alteração de propriedade.|Depende|Nenhum|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontallyScrollableProperty>evento de alteração de propriedade.|Depende|Se o controle oferecer suporte ao padrão de rolagem, ele deverá dar suporte a esse evento.|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalScrollPercentProperty>evento de alteração de propriedade.|Depende|Se o controle oferecer suporte ao padrão de rolagem, ele deverá dar suporte a esse evento.|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.HorizontalViewSizeProperty>evento de alteração de propriedade.|Depende|Se o controle oferecer suporte ao padrão de rolagem, ele deverá dar suporte a esse evento.|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalScrollPercentProperty>evento de alteração de propriedade.|Depende|Se o controle oferecer suporte ao padrão de rolagem, ele deverá dar suporte a esse evento.|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticallyScrollableProperty>evento de alteração de propriedade.|Depende|Se o controle oferecer suporte ao padrão de rolagem, ele deverá dar suporte a esse evento.|  
|<xref:System.Windows.Automation.ScrollPatternIdentifiers.VerticalViewSizeProperty>evento de alteração de propriedade.|Depende|Se o controle oferecer suporte ao padrão de rolagem, ele deverá dar suporte a esse evento.|  
|<xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent>|Obrigatório|Nenhum|  
  
## <a name="date-grid-control-type-example"></a>Exemplo de tipo de controle de grade de data  
 A imagem a seguir ilustra um controle de exibição de lista que implementa o tipo de controle DataGrid.  
  
 ![Gráfico de um controle de exibição de lista com dois itens de dados](./media/uiauto-data-grid-detailed.GIF "uiauto_data_grid_detailed")  
  
 A exibição de controle e a exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore que pertencem ao controle de exibição de lista são exibidas abaixo. Os padrões de controle para cada elemento de automação são mostrados entre parênteses.  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Exibição de controle de árvore|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Árvore-exibição de conteúdo|  
|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|  
|<ul><li>DataGrid (tabela, grade, seleção)</li><li>Cabeçalho<br /><br /> <ul><li>HeaderItem "Name" (Invoke)</li><li>HeaderItem "data de modificação" (invocar)</li><li>HeaderItem "Size" (Invoke)</li></ul></li><li>Grupo "contoso" (TableItem, GridItem, SelectionItem, tabela *, grade \* )<br /><br /> <ul><li>DataItem "contas Receivable.doc" (SelectionItem, Invoke, TableItem \* , GridItem \* )</li><li>DataItem "contas Payable.doc" (SelectionItem, Invoke, TableItem \* , GridItem \* )</li></ul></li></ul>|<ul><li>DataGrid (tabela, grade, seleção)</li><li>Grupo "contoso" (TableItem, GridItem, SelectionItem, tabela *, grade \* )<br /><br /> <ul><li>DataItem "contas Receivable.doc" (SelectionItem, Invoke, TableItem \* , GridItem \* )</li><li>DataItem "contas Payable.doc" (SelectionItem, Invoke, TableItem \* , GridItem \* )</li></ul></li></ul>|  
  
 \*O exemplo anterior mostra um DataGrid que contém vários níveis de controles. O controle Group ("contoso") contém dois controles DataItem ("accounts Receivable.doc" e "accounts Payable.doc"). Um par DataGrid/GridItem é independente de um par em outro nível. Os controles de DataItem em um grupo também podem ser expostos como um tipo de controle ListItem, permitindo que eles sejam apresentados mais claramente como objetos selecionáveis, e não como elementos de dados simples. Este exemplo não inclui os subelementos dos itens de dados agrupados.  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.ControlType.DataGrid>
- [Visão Geral dos Tipos de Controle de Automação de Interface do Usuário](ui-automation-control-types-overview.md)
- [Visão geral de automação da interface do usuário](ui-automation-overview.md)
