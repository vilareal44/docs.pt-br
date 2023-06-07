---
title: Suporte de automação de interface de usuário para o tipo de controle ComboBox
description: Obtenha informações sobre o suporte de automação da interface do usuário para o tipo de controle ComboBox. Aprenda a estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários.
ms.date: 03/30/2017
helpviewer_keywords:
- control types, Combo Box
- UI Automation, Combo Box control type
- ComboBox controls
ms.assetid: bb321126-4770-41da-983a-67b7b89d45dd
ms.openlocfilehash: c708de791056969e281ad1bc223e2a2233fa170b
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166093"
---
# <a name="ui-automation-support-for-the-combobox-control-type"></a>Suporte de automação de interface de usuário para o tipo de controle ComboBox
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico fornece informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o suporte para o tipo de controle ComboBox. No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , um tipo de controle é um conjunto de condições que um controle deve atender para usar a <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> propriedade. As condições incluem diretrizes específicas para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] valores de propriedade, padrões de controle e [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos.  
  
 Uma caixa de combinação é uma caixa de listagem combinada com um controle estático ou um controle de edição que exibe o item atualmente selecionado na parte da caixa de listagem da caixa de combinação. A parte da caixa de listagem do controle é exibida sempre ou só aparece quando o usuário seleciona a seta suspensa (que é um botão de ação) ao lado do controle. Se o campo de seleção for um controle de edição, o usuário poderá inserir informações que não estejam na lista; caso contrário, o usuário só poderá selecionar itens na lista.  
  
 As seções a seguir definem a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários para o tipo de controle ComboBox. Os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] requisitos se aplicam a todos os controles de caixa de combinação, sejam eles, [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] Win32 ou Windows Forms.  
  
<a name="Required_UI_Automation_Tree_Structure"></a>
## <a name="required-ui-automation-tree-structure"></a>Estrutura de árvore de automação da interface do usuário necessária  
 A tabela a seguir descreve a exibição de controle e a exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore que pertence aos controles de caixa de combinação e descreve o que pode ser contido em cada exibição. Para obter mais informações sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).  
  
|Exibição de controle|Exibição de conteúdo|  
|------------------|------------------|  
|ComboBox<br /><br /> -Editar (0 ou 1)<br />-List (1)<br />-Item de lista (filho de lista; 0 a muitos)<br />-Botão (1)|ComboBox<br /><br /> -Item de lista (0 a muitos)|  
  
 O controle de edição no modo de exibição de controle da caixa de combinação será necessário somente se a caixa de combinação puder ser editada para obter qualquer entrada, como é o caso da caixa de combinação na caixa de diálogo Executar.  
  
<a name="Required_UI_Automation_Properties"></a>
## <a name="required-ui-automation-properties"></a>Propriedades de automação da interface do usuário necessárias  
 A tabela a seguir lista as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades cujo valor ou definição é especialmente relevante para os controles de caixa de combinação. Para obter mais informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Propriedade|Valor|Anotações|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|Consulte observações.|O valor dessa propriedade precisa ser exclusivo em todos os controles em um aplicativo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|Consulte observações.|O retângulo mais externo que contém o controle inteiro.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|Consulte observações.|Com suporte se houver um retângulo delimitador. Se nem todos os pontos dentro do retângulo delimitador forem clicáveis e você executar um teste de clique especializado, substitua e forneça um ponto clicável.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|ComboBox|Esse valor é o mesmo para todas as [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] estruturas.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>|Consulte observações.|O texto de ajuda para controles de caixa de combinação deve explicar por que o usuário está sendo solicitado a escolher uma opção na caixa de combinação. O texto é semelhante às informações apresentadas por meio de uma dica de ferramenta. Por exemplo, "Selecione um item para definir a resolução de vídeo do monitor."|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|True|Os controles de caixa de combinação são sempre incluídos na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|Os controles de caixa de combinação são sempre incluídos na exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|True|Os controles de caixa de combinação expõem um conjunto de itens de um contêiner de seleção. O controle caixa de combinação pode receber o foco do teclado, embora quando um cliente de automação da interface do usuário define o foco em uma caixa de combinação, todos os itens na subárvore da caixa de combinação podem receber o foco.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|Consulte observações.|Controles de caixa de combinação normalmente têm um rótulo de texto estático ao qual essa propriedade faz referência.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"caixa de combinação"|Cadeia de caracteres localizada correspondente ao tipo de controle ComboBox.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Consulte observações.|O controle caixa de combinação normalmente obtém seu nome de um controle de texto estático.|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>
## <a name="required-ui-automation-control-patterns"></a>Padrões de controle de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] padrões de controle necessários para serem suportados por todos os controles de caixa de combinação. Para obter mais informações sobre padrões de controle, consulte [visão geral dos padrões de controle de automação da interface do usuário](ui-automation-control-patterns-overview.md).  
  
|Padrão de controle|Suporte|Anotações|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider>|Sim|O controle caixa de combinação sempre deve conter o botão suspenso para ser uma caixa de combinação.|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider>|Sim|Exibe a seleção atual na caixa de combinação. Esse suporte é delegado à caixa de listagem abaixo da caixa de combinação.|  
|<xref:System.Windows.Automation.Provider.IValueProvider>|Depende|Se a caixa de combinação tiver a capacidade de obter valores de texto arbitrários, o padrão de valor deverá ser suportado. Esse padrão fornece a capacidade de definir programaticamente o conteúdo da cadeia de caracteres da caixa de combinação. Se não houver suporte para o padrão de valor, isso indica que o usuário deve fazer uma seleção a partir dos itens da lista na subárvore da caixa de combinação.|  
|<xref:System.Windows.Automation.Provider.IScrollProvider>|Nunca|O padrão de rolagem nunca é suportado em uma caixa de combinação diretamente. Haverá suporte se uma caixa de listagem contida em uma caixa de combinação puder ser rolada. Ele só pode ter suporte quando a caixa de listagem estiver visível na tela.|  
  
<a name="Required_Events"></a>
## <a name="required-events"></a>Eventos necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos necessários para serem suportados por todos os controles de caixa de combinação. Para obter mais informações sobre eventos, consulte [visão geral dos eventos de automação da interface do usuário](ui-automation-events-overview.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Circunstância|Suporte|Anotações|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.ExpandCollapsePatternIdentifiers.ExpandCollapseStateProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.ValuePatternIdentifiers.ValueProperty>evento de alteração de propriedade.|Depende|Se o controle der suporte ao padrão de valor, ele deverá dar suporte a esse evento.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.ControlType.ComboBox>
- [Visão Geral dos Tipos de Controle de Automação de Interface do Usuário](ui-automation-control-types-overview.md)
- [Visão geral de automação da interface do usuário](ui-automation-overview.md)
