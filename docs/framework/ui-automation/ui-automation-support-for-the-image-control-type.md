---
title: Suporte de automação de interface de usuário para o Tipo de Controle Image
description: Obtenha informações sobre o suporte de automação da interface do usuário para o tipo de controle imagem. Aprenda a estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, Image control type
- control types, Image
- Image control type
ms.assetid: 4e0eeefb-e09b-46d2-b83b-0a7e35543ab8
ms.openlocfilehash: 97a71b31609566ca081dee1c66b911f0ad534a50
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166943"
---
# <a name="ui-automation-support-for-the-image-control-type"></a>Suporte de automação de interface de usuário para o Tipo de Controle Image
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico fornece informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o suporte para o tipo de controle imagem. No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , um tipo de controle é um conjunto de condições que um controle deve atender para usar a <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> propriedade. As condições incluem diretrizes específicas para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] valores de propriedade e padrões de controle.  
  
 Os controles de imagem usados como ícones, gráficos informativos e gráficos terão suporte para o tipo de controle de imagem. Os controles usados como imagens de plano de fundo ou marca d' água não oferecerão suporte ao tipo de controle Image.  
  
 As seções a seguir definem a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] estrutura de árvore, as propriedades, os padrões de controle e os eventos necessários para o tipo de controle Image. Os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] requisitos se aplicam a todos os controles de imagem, sejam [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)] , Win32 ou Windows Forms.  
  
<a name="Required_UI_Automation_Tree_Structure"></a>
## <a name="required-ui-automation-tree-structure"></a>Estrutura de árvore de automação da interface do usuário necessária  
 A tabela a seguir descreve a exibição de controle e a exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore que pertence a controles de imagem e descreve o que pode ser contido em cada exibição. Para obter mais informações sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore, consulte [visão geral da árvore de automação da interface do usuário](ui-automation-tree-overview.md).  
  
|Exibição de controle|Exibição de conteúdo|  
|------------------|------------------|  
|Imagem|Imagem (depende se a imagem contém informações (com base no valor da `IsContentElement` Propriedade))|  
  
<a name="Required_UI_Automation_Properties"></a>
## <a name="required-ui-automation-properties"></a>Propriedades de automação da interface do usuário necessárias  
 A tabela a seguir lista as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades cujo valor ou definição é especialmente relevante para o tipo de controle de imagem. Para obter mais informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Propriedade|Valor|Observações|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|Consulte observações.|O valor dessa propriedade precisa ser exclusivo em todos os controles em um aplicativo.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|Consulte observações.|O retângulo mais externo que contém o controle inteiro.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|Consulte observações.|O ponto clicável do controle de imagem deve ser um ponto dentro do retângulo delimitador do controle de imagem.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|Consulte observações.|Se o controle puder receber o foco do teclado, ele deverá dar suporte a essa propriedade.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|Consulte observações.|A propriedade Name deve ser exposta para todos os controles de imagem que contêm informações. O acesso programático a essas informações requer que um equivalente textual para o gráfico seja fornecido. Se o controle de imagem for puramente decorativo, ele só deverá aparecer na exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore e não precisará ter um nome. As estruturas de interface do usuário devem oferecer suporte a uma propriedade ALT ou de texto alternativo em imagens que podem ser definidas de dentro de sua estrutura. Essa propriedade será mapeada para a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedade Name.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|Consulte observações.|Se houver um rótulo de texto estático, essa propriedade deverá expor uma referência a esse controle.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|Imagem|Esse valor é o mesmo para todas as estruturas de interface do usuário.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|imagem|Cadeia de caracteres localizada correspondente ao tipo de controle de imagem.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|Consulte observações.|O controle de imagem deve ser incluído na exibição de conteúdo da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore quando ela contém informações significativas ainda não expostas ao usuário final.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|O controle de imagem é sempre incluído na exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.HelpTextProperty>|Consulte observações.|A propriedade HelpText expõe uma cadeia de caracteres localizada que descreve a aparência visual real do controle (por exemplo, um quadrado vermelho com um "X" branco) ou outras informações de dica de ferramenta associadas à imagem.<br /><br /> Essa propriedade deve ter suporte quando uma descrição longa é necessária para transmitir mais informações sobre o controle de imagem. Por exemplo, um gráfico complicado ou diagrama. Essa propriedade é mapeada para a marca HTML LongDesc e a marca Desc (SVG) de gráficos escalonáveis. Os desenvolvedores que trabalham com controles de imagem devem dar suporte a uma propriedade para permitir que a descrição visual seja definida no controle. Essa propriedade deve ser mapeada para a propriedade VisualDescription de automação da interface do usuário.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ItemStatusProperty>|Consulte observações.|Se o controle de imagem representar informações de estado sobre um item específico na tela, o controle deverá estar contido no item. Quando a imagem está contida em um item, o item deve dar suporte à propriedade Status e gerar notificações apropriadas quando o status for alterado.<br /><br /> Se uma imagem for um controle autônomo e estiver transmitindo o status, essa propriedade deverá ter suporte.|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>
## <a name="required-ui-automation-control-patterns"></a>Padrões de controle de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] padrões de controle necessários para serem suportados por todos os controles de imagem. Para obter mais informações sobre padrões de controle, consulte [visão geral dos padrões de controle de automação da interface do usuário](ui-automation-control-patterns-overview.md).  
  
|Padrão de controle|Suporte|Observações|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IGridItemProvider>|Depende|O controle de imagem dá suporte ao padrão de item de grade se o controle estiver dentro de um contêiner de grade.|  
|<xref:System.Windows.Automation.Provider.ITableItemProvider>|Depende|O controle de imagem dá suporte ao padrão de item de tabela se o controle estiver dentro de um contêiner que tem controles de cabeçalho.|  
|<xref:System.Windows.Automation.Provider.IInvokeProvider>|Nunca|Se o controle de imagem contiver uma imagem clicável, o controle deverá dar suporte a um tipo de controle que ofereça suporte ao padrão Invoke, como o tipo de controle Button.|  
|<xref:System.Windows.Automation.Provider.ISelectionItemProvider>|Nunca|Os controles de imagem não devem oferecer suporte ao padrão de item de seleção.|  
  
<a name="Required_UI_Automation_Events"></a>
## <a name="required-ui-automation-events"></a>Eventos de automação da interface do usuário necessários  
 A tabela a seguir lista os [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos necessários para serem suportados por todos os controles de imagem. Para obter mais informações sobre eventos, consulte [visão geral dos eventos de automação da interface do usuário](ui-automation-events-overview.md).  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]Circunstância|Suporte|Observações|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent>|Nunca|Nenhum|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent>|Nunca|Nenhum|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent>|Nunca|Nenhum|  
|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent>|Nunca|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>evento de alteração de propriedade.|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|Obrigatório|Nenhum|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|Obrigatório|Nenhum|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.ControlType.Image>
- [Visão Geral dos Tipos de Controle de Automação de Interface do Usuário](ui-automation-control-types-overview.md)
- [Visão geral de automação da interface do usuário](ui-automation-overview.md)
