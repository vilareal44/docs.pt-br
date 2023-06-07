---
title: Automação de Interface do usuário e Microsoft Active Accessibility
description: Entenda as diferenças entre a automação da interface do usuário e o Microsoft Acessibilidade Ativa, a solução anterior para tornar os aplicativos acessíveis.
ms.date: 03/30/2017
helpviewer_keywords:
- Active Accessibility
- UI Automation, Active Accessibility compared to
- UI Automation, Microsoft Active Accessibility
- Active Accessibility, UI Automation compared to
ms.assetid: 87bee662-0a3e-4232-a421-20e7a5968321
ms.openlocfilehash: 0685a3f89a6578433641aaf78717f4ff377ff2f9
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164070"
---
# <a name="ui-automation-and-microsoft-active-accessibility"></a>Automação de Interface do usuário e Microsoft Active Accessibility
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 O Microsoft Acessibilidade Ativa foi a solução anterior para tornar os aplicativos acessíveis. [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]é o novo modelo de acessibilidade para o Microsoft Windows e destina-se a atender às necessidades de produtos de tecnologia assistencial e ferramentas de teste automatizadas. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]oferece muitos aprimoramentos em Acessibilidade Ativa.  
  
 Este tópico inclui os principais recursos do [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] e explica como esses recursos são diferentes do acessibilidade ativa.  
  
<a name="Programming_Languages_compare"></a>
## <a name="programming-languages"></a>Linguagens de programação  
O Acessibilidade Ativa é baseado no Component Object Model (COM) com suporte para interfaces duplas e, portanto, é programável em C/C++, Microsoft Visual Basic 6,0 e linguagens de script. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)](incluindo a biblioteca de provedores do lado do cliente para controles padrão) é escrito em código gerenciado e os aplicativos cliente de automação da interface do usuário são programados com mais facilidade usando C# ou Visual Basic .NET. Provedores de automação de interface do usuário, que são implementações de interface, podem ser escritos em código gerenciado ou em C/C++.  
  
<a name="Support_in_Windows_Presentation_Foundation_"></a>
## <a name="support-in-windows-presentation-foundation"></a>Suporte no Windows Presentation Foundation  
 Windows Presentation Foundation (WPF) é o novo modelo para criar interfaces do usuário. Os elementos do WPF não contêm suporte nativo para Acessibilidade Ativa; no entanto, eles oferecem suporte [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , o que inclui suporte a pontes para clientes acessibilidade ativa. Somente os clientes escritos especificamente para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] o podem aproveitar ao máximo os recursos de acessibilidade do WPF, como o suporte avançado para texto.  
  
<a name="Servers_and_Clients_compare"></a>
## <a name="servers-and-clients"></a>Servidores e clientes  
 Em Acessibilidade Ativa, servidores e clientes se comunicam diretamente, amplamente por meio da implementação do servidor do `IAccessible` .  
  
 No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , um serviço principal está entre o servidor (chamado de provedor) e o cliente. O serviço principal faz chamadas para as interfaces implementadas pelos provedores e fornece serviços adicionais, como a geração de identificadores de tempo de execução exclusivos para elementos. Os aplicativos cliente usam funções de biblioteca para chamar o [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] serviço.  
  
 Provedores de automação de interface do usuário podem fornecer informações para Acessibilidade Ativa clientes e Acessibilidade Ativa servidores podem fornecer informações para aplicativos cliente de automação da interface do usuário. No entanto, como Acessibilidade Ativa não expõe tantas informações quanto [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , os dois modelos não são totalmente compatíveis.  
  
<a name="UI_Elements_compare"></a>
## <a name="ui-elements"></a>Elementos da interface do usuário  
 Acessibilidade Ativa apresenta [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] elementos como uma `IAccessible` interface ou como um identificador filho. É difícil comparar dois `IAccessible` ponteiros para determinar se eles se referem ao mesmo elemento.  
  
 No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , cada elemento é representado como um <xref:System.Windows.Automation.AutomationElement> objeto. A comparação é feita usando o operador de igualdade ou o <xref:System.Windows.Automation.AutomationElement.Equals%2A> método, ambos comparam os identificadores de tempo de execução exclusivos dos elementos.  
  
<a name="Tree_Views_and_Navigation_compare"></a>
## <a name="tree-views-and-navigation"></a>Exibições de árvore e navegação  
 Os [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] elementos na tela podem ser vistos como uma estrutura de árvore com a área de trabalho como raiz, janelas de aplicativo como filhos imediatos e elementos dentro de aplicativos como mais descendentes.  
  
 No Acessibilidade Ativa, muitos elementos de automação que são irrelevantes para os usuários finais são expostos na árvore. Os aplicativos cliente precisam examinar todos os elementos para determinar quais são significativos.  
  
 Os aplicativos cliente de automação da interface do usuário veem o [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] por meio de uma exibição filtrada. O modo de exibição contém apenas elementos de interesse: aqueles que fornecem informações ao usuário ou habilitam a interação. Exibições predefinidas de apenas elementos de controle e somente elementos de conteúdo estão disponíveis; Além disso, os aplicativos podem definir modos de exibição personalizados. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]simplifica a tarefa de descrever o [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] para o usuário e ajudar o usuário a interagir com o aplicativo.  
  
 A navegação entre elementos, em Acessibilidade Ativa, é espacial (por exemplo, mover para o elemento que está à esquerda na tela), lógico (por exemplo, mover para o próximo item de menu ou o próximo item na ordem de tabulação dentro de uma caixa de diálogo) ou hierárquico (por exemplo, mover o primeiro filho em um contêiner ou de filho para seu pai). A navegação hierárquica é complicada pelo fato de que os elementos filho nem sempre são objetos que implementam `IAccessible` .  
  
 No [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , todos os [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] elementos são <xref:System.Windows.Automation.AutomationElement> objetos que dão suporte à mesma funcionalidade básica. (Do ponto de vista do provedor, eles são objetos que implementam uma interface herdada do <xref:System.Windows.Automation.Provider.IRawElementProviderSimple> .) A navegação é principalmente hierárquica: de pais a filhos e de um irmão para o próximo. (A navegação entre irmãos tem um elemento lógico, pois ele pode seguir a ordem de tabulação.) Você pode navegar de qualquer ponto de partida, usando qualquer exibição filtrada da árvore, usando a <xref:System.Windows.Automation.TreeWalker> classe. Você também pode navegar para filhos ou descendentes específicos usando <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> e <xref:System.Windows.Automation.AutomationElement.FindAll%2A> ; por exemplo, é muito fácil recuperar todos os elementos dentro de uma caixa de diálogo que ofereça suporte a um padrão de controle especificado.  
  
 A navegação no [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] é mais consistente do que no acessibilidade ativa. Alguns elementos como listas suspensas e janelas pop-up aparecem duas vezes na árvore de Acessibilidade Ativa, e a navegação deles pode ter resultados inesperados. Na verdade, é impossível implementar corretamente Acessibilidade Ativa para um controle rebar. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]habilita a recolocação e o reposicionamento, para que um elemento possa ser colocado em qualquer lugar na árvore, apesar da hierarquia imposta pela propriedade do Windows.  
  
<a name="Roles_and_Control_Types"></a>
## <a name="roles-and-control-types"></a>Funções e tipos de controle  
 Acessibilidade Ativa usa a `accRole` Propriedade ( `IAccessible::get_actRole` ) para recuperar uma descrição da função do elemento no, como [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] ROLE_SYSTEM_SLIDER ou ROLE_SYSTEM_MENUITEM. A função de um elemento é a principal pista para sua funcionalidade disponível. A interação com um controle é obtida usando métodos fixos, como `IAccessible::accSelect` e `IAccessible::accDoDefaultAction` . A interação entre o aplicativo cliente e o [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] é limitada ao que pode ser feito por meio do `IAccessible` .  
  
 Por outro lado, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Basicamente dissocia o tipo de controle do elemento (descrito pela <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.ControlType%2A> Propriedade) de sua funcionalidade esperada. A funcionalidade é determinada pelos padrões de controle que são suportados pelo provedor por meio de sua implementação de interfaces especializadas. Os padrões de controle podem ser combinados para descrever o conjunto completo de funcionalidades com suporte de um determinado [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] elemento. Alguns provedores são necessários para dar suporte a um padrão de controle específico; por exemplo, o provedor de uma caixa de seleção deve oferecer suporte ao padrão de controle de alternância. Outros provedores são necessários para dar suporte a um ou mais de um conjunto de padrões de controle; por exemplo, um botão deve dar suporte a Toggle ou Invoke. Outros ainda não dão suporte a nenhum padrão de controle; por exemplo, um painel que não pode ser movido, redimensionado ou encaixado não tem nenhum padrão de controle.  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]dá suporte a controles personalizados, que são identificados pela <xref:System.Windows.Automation.ControlType.Custom> propriedade e podem ser descritos pela <xref:System.Windows.Automation.AutomationElement.LocalizedControlTypeProperty> propriedade.  
  
 A tabela a seguir mostra o mapeamento de funções de Acessibilidade Ativa para [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] tipos de controle.  
  
|Acessibilidade Ativa função|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]tipo de controle|  
|----------------------------------------------------------------------|----------------------------------------------------------------------------------------|  
|ROLE_SYSTEM_PUSHBUTTON|Botão|  
|ROLE_SYSTEM_CLIENT|Calendário|  
|ROLE_SYSTEM_CHECKBUTTON|Caixa de seleção|  
|ROLE_SYSTEM_COMBOBOX|Caixa de combinação|  
|ROLE_SYSTEM_CLIENT|Personalizado|  
|ROLE_SYSTEM_LIST|Grade de dados|  
|ROLE_SYSTEM_LISTITEM|Item de dados|  
|ROLE_SYSTEM_DOCUMENT|Document|  
|ROLE_SYSTEM_TEXT|Editar|  
|ROLE_SYSTEM_GROUPING|Agrupar|  
|ROLE_SYSTEM_LIST|Cabeçalho|  
|ROLE_SYSTEM_COLUMNHEADER|Item de cabeçalho|  
|ROLE_SYSTEM_LINK|Hyperlink|  
|ROLE_SYSTEM_GRAPHIC|Imagem|  
|ROLE_SYSTEM_LIST|Lista|  
|ROLE_SYSTEM_LISTITEM|Item de lista|  
|ROLE_SYSTEM_MENUPOPUP|Menu|  
|ROLE_SYSTEM_MENUBAR|Barra de menus|  
|ROLE_SYSTEM_MENUITEM|Item de menu|  
|ROLE_SYSTEM_PANE|Painel|  
|ROLE_SYSTEM_PROGRESSBAR|Barra de progresso|  
|ROLE_SYSTEM_RADIOBUTTON|Botão de opção|  
|ROLE_SYSTEM_SCROLLBAR|Barra de rolagem|  
|ROLE_SYSTEM_SEPARATOR|Separador|  
|ROLE_SYSTEM_SLIDER|Controle deslizante|  
|ROLE_SYSTEM_SPINBUTTON|Controle giratório|  
|ROLE_SYSTEM_SPLITBUTTON|Botão de divisão|  
|ROLE_SYSTEM_STATUSBAR|Barra de Status|  
|ROLE_SYSTEM_PAGETABLIST|Tab|  
|ROLE_SYSTEM_PAGETAB|Item de guia|  
|ROLE_SYSTEM_TABLE|Tabela|  
|ROLE_SYSTEM_STATICTEXT|Texto|  
|ROLE_SYSTEM_INDICATOR|Posição|  
|ROLE_SYSTEM_TITLEBAR|Barra de título|  
|ROLE_SYSTEM_TOOLBAR|Barra de ferramentas|  
|ROLE_SYSTEM_TOOLTIP|ToolTip|  
|ROLE_SYSTEM_OUTLINE|Árvore|  
|ROLE_SYSTEM_OUTLINEITEM|Item de árvore|  
|ROLE_SYSTEM_WINDOW|Janela|  
  
 Para obter mais informações sobre os diferentes tipos de controle, consulte [tipos de controle de automação da interface do usuário](ui-automation-control-types.md).  
  
<a name="States_and_Properties"></a>
## <a name="states-and-properties"></a>Estados e propriedades  
 Em Acessibilidade Ativa, os elementos dão suporte a um conjunto comum de propriedades, e algumas propriedades (como `accState` ) devem descrever coisas muito diferentes, dependendo da função do elemento. Os servidores devem implementar todos os métodos de `IAccessible` que retornam uma propriedade, mesmo aqueles que não são relevantes para o elemento.  
  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]define muitas outras propriedades, algumas das quais correspondem a Estados em Acessibilidade Ativa. Algumas são comuns a todos os elementos, mas outras são específicas para controlar tipos e padrões de controle. As propriedades são diferenciadas por identificadores exclusivos e a maioria das propriedades pode ser recuperada usando um único método <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A> ou <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A> . Muitas propriedades também são facilmente recuperáveis dos <xref:System.Windows.Automation.AutomationElement.Current%2A> <xref:System.Windows.Automation.AutomationElement.Cached%2A> acessadores e da propriedade.  
  
 Um provedor de automação de interface do usuário não precisa implementar propriedades irrelevantes, mas pode simplesmente retornar um `null` valor para todas as propriedades para as quais ele não dá suporte. Além disso, o [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] serviço principal pode obter algumas propriedades do provedor de janela padrão, e elas são mesclados com propriedades explicitamente implementadas pelo provedor.  
  
 Além de dar suporte a muitas outras propriedades, o [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] fornece melhor desempenho, permitindo que várias propriedades sejam recuperadas com uma única chamada de processo cruzado.  
  
 A tabela a seguir mostra a correspondência entre as propriedades nos dois modelos.  
  
|Acessador de propriedade Acessibilidade Ativa|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]ID da propriedade|Comentários|  
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|-------------|  
|`get_accKeyboardShortcut`|<xref:System.Windows.Automation.AutomationElement.AccessKeyProperty> ou <xref:System.Windows.Automation.AutomationElement.AcceleratorKeyProperty>|`AccessKeyProperty`terá precedência se ambos estiverem presentes.|  
|`get_accName`|<xref:System.Windows.Automation.AutomationElement.NameProperty>||  
|`get_accRole`|<xref:System.Windows.Automation.AutomationElement.ControlTypeProperty>|Consulte a tabela anterior para mapeamento de funções para tipos de controle.|  
|`get_accValue`|<xref:System.Windows.Automation.ValuePattern.ValueProperty?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.RangeValuePattern.ValueProperty?displayProperty=nameWithType>|Válido somente para tipos de controle que dão suporte a ValuePattern ou RangeValuePattern. Os valores RangeValue são normalizados para 0-100, para serem consistentes com o comportamento de MSAA. Os itens de valor usam uma cadeia de caracteres.|  
|`get_accHelp`|<xref:System.Windows.Automation.AutomationElement.HelpTextProperty>||  
|`accLocation`|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty>||  
|`get_accDescription`|Sem suporte em[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|`accDescription`Não tinha uma especificação clara dentro de MSAA, o que resultou em provedores que estão colocando diferentes partes de informações nessa propriedade.|  
|`get_accHelpTopic`|Sem suporte em[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]||  
  
 A tabela a seguir mostra quais [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades correspondem a acessibilidade ativa constantes de estado.  
  
|Estado de Acessibilidade Ativa|Propriedade [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|A alteração de estado de gatilhos?|  
|-----------------------------------------------------------------------|------------------------------------------------------------------------------------|----------------------------|  
|STATE_SYSTEM_CHECKED|Para caixa de seleção,<xref:System.Windows.Automation.TogglePattern.ToggleStateProperty><br /><br /> Para botão de opção,<xref:System.Windows.Automation.SelectionItemPattern.IsSelectedProperty>|Y|  
|STATE_SYSTEM_COLLAPSED|<xref:System.Windows.Automation.ExpandCollapsePattern.ExpandCollapsePatternInformation.ExpandCollapseState%2A> = <xref:System.Windows.Automation.ExpandCollapseState.Collapsed>|Y|  
|STATE_SYSTEM_EXPANDED|<xref:System.Windows.Automation.ExpandCollapsePattern.ExpandCollapsePatternInformation.ExpandCollapseState%2A> = <xref:System.Windows.Automation.ExpandCollapseState.Expanded> ou <xref:System.Windows.Automation.ExpandCollapseState.PartiallyExpanded>|Y|  
|STATE_SYSTEM_FOCUSABLE|<xref:System.Windows.Automation.AutomationElement.IsKeyboardFocusableProperty>|N|  
|STATE_SYSTEM_FOCUSED|<xref:System.Windows.Automation.AutomationElement.HasKeyboardFocusProperty>|N|  
|STATE_SYSTEM_HASPOPUP|<xref:System.Windows.Automation.ExpandCollapsePattern>para itens de menu|N|  
|STATE_SYSTEM_INVISIBLE|<xref:System.Windows.Automation.AutomationElement.IsOffscreenProperty>= True e <xref:System.Windows.Automation.AutomationElement.GetClickablePoint%2A> causas<xref:System.Windows.Automation.NoClickablePointException>|N|  
|STATE_SYSTEM_LINKED|<xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> =<br /><br /> <xref:System.Windows.Automation.ControlType.Hyperlink>|N|  
|STATE_SYSTEM_MIXED|<xref:System.Windows.Automation.TogglePattern.TogglePatternInformation.ToggleState%2A> = <xref:System.Windows.Automation.ToggleState.Indeterminate>|N|  
|STATE_SYSTEM_MOVEABLE|<xref:System.Windows.Automation.TransformPattern.CanMoveProperty>|N|  
|STATE_SYSTEM_MUTLISELECTABLE|<xref:System.Windows.Automation.SelectionPattern.CanSelectMultipleProperty>|N|  
|STATE_SYSTEM_OFFSCREEN|<xref:System.Windows.Automation.AutomationElement.IsOffscreenProperty>= True|N|  
|STATE_SYSTEM_PROTECTED|<xref:System.Windows.Automation.AutomationElement.IsPasswordProperty>|N|  
|STATE_SYSTEM_READONLY|<xref:System.Windows.Automation.RangeValuePattern.IsReadOnlyProperty?displayProperty=nameWithType> e <xref:System.Windows.Automation.ValuePattern.IsReadOnlyProperty?displayProperty=nameWithType>|N|  
|STATE_SYSTEM_SELECTABLE|<xref:System.Windows.Automation.SelectionItemPattern> é compatível|N|  
|STATE_SYSTEM_SELECTED|<xref:System.Windows.Automation.SelectionItemPattern.IsSelectedProperty>|N|  
|STATE_SYSTEM_SIZEABLE|<xref:System.Windows.Automation.TransformPattern.TransformPatternInformation.CanResize%2A>|N|  
|STATE_SYSTEM_UNAVAILABLE|<xref:System.Windows.Automation.AutomationElement.IsEnabledProperty>|Y|  
  
 Os Estados a seguir não foram implementados pela maioria dos servidores de controle de Acessibilidade Ativa ou não têm nenhum equivalente no [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] .  
  
|Estado de Acessibilidade Ativa|Comentários|  
|-----------------------------------------------------------------------|-------------|  
|STATE_SYSTEM_BUSY|Não disponível em[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
|STATE_SYSTEM_DEFAULT|Não disponível em[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
|STATE_SYSTEM_ANIMATED|Não disponível em[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
|STATE_SYSTEM_EXTSELECTABLE|Não amplamente implementado por servidores Acessibilidade Ativa|  
|STATE_SYSTEM_MARQUEED|Não amplamente implementado por servidores Acessibilidade Ativa|  
|STATE_SYSTEM_SELFVOICING|Não amplamente implementado por servidores Acessibilidade Ativa|  
|STATE_SYSTEM_TRAVERSED|Não disponível em[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
|STATE_SYSTEM_ALERT_HIGH|Não amplamente implementado por servidores Acessibilidade Ativa|  
|STATE_SYSTEM_ALERT_MEDIUM|Não amplamente implementado por servidores Acessibilidade Ativa|  
|STATE_SYSTEM_ALERT_LOW|Não amplamente implementado por servidores Acessibilidade Ativa|  
|STATE_SYSTEM_FLOATING|Não amplamente implementado por servidores Acessibilidade Ativa|  
|STATE_SYSTEM_HOTTRACKED|Não disponível em[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
|STATE_SYSTEM_PRESSED|Não disponível em[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]|  
  
 Para obter uma lista completa de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] identificadores de propriedade, consulte [visão geral das propriedades de automação da interface do usuário](ui-automation-properties-overview.md).  
  
<a name="uiautomation_events_compare"></a>
## <a name="events"></a>Eventos  
 O mecanismo de eventos no [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] , diferentemente do acessibilidade ativa, não depende do roteamento de eventos do Windows (que está fortemente ligado com identificadores de janela) e não requer que o aplicativo cliente configure os ganchos. As assinaturas para eventos podem ser ajustadas não apenas a eventos específicos, mas a partes específicas da árvore. Os provedores também podem ajustar sua geração de eventos mantendo o controle de quais eventos estão sendo ouvidos.  
  
 Também é mais fácil para os clientes recuperarem os elementos que geram eventos, pois eles são passados diretamente para o retorno de chamada do evento. As propriedades do elemento serão previamente buscadas automaticamente se uma solicitação de cache estava ativa quando o cliente assinou o evento.  
  
 A tabela a seguir mostra a correspondência de Acessibilidade Ativa WinEvents e [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] eventos.  
  
|WinEvent|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]identificador de evento|  
|--------------|--------------------------------------------------------------------------------------------|  
|EVENT_OBJECT_ACCELERATORCHANGE|<xref:System.Windows.Automation.AutomationElement.AcceleratorKeyProperty>alteração de propriedade|  
|EVENT_OBJECT_CONTENTSCROLLED|<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty>ou <xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty> alteração de propriedade nas barras de rolagem associadas|  
|EVENT_OBJECT_CREATE|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_DEFACTIONCHANGE|Sem equivalente|  
|EVENT_OBJECT_DESCRIPTIONCHANGE|Nenhum equivalente exato; <xref:System.Windows.Automation.AutomationElement.HelpTextProperty>alteração de talvez ou <xref:System.Windows.Automation.AutomationElement.LocalizedControlTypeProperty> Propriedade|  
|EVENT_OBJECT_DESTROY|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_FOCUS|<xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent>|  
|EVENT_OBJECT_HELPCHANGE|<xref:System.Windows.Automation.AutomationElement.HelpTextProperty>alteração|  
|EVENT_OBJECT_HIDE|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_LOCATIONCHANGE|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty>alteração de propriedade|  
|EVENT_OBJECT_NAMECHANGE|<xref:System.Windows.Automation.AutomationElement.NameProperty>alteração de propriedade|  
|EVENT_OBJECT_PARENTCHANGE|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_REORDER|Não é usado de forma consistente em Acessibilidade Ativa. Nenhum evento correspondente diretamente está definido em [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] .|  
|EVENT_OBJECT_SELECTION|<xref:System.Windows.Automation.SelectionItemPattern.ElementSelectedEvent>|  
|EVENT_OBJECT_SELECTIONADD|<xref:System.Windows.Automation.SelectionItemPattern.ElementAddedToSelectionEvent>|  
|EVENT_OBJECT_SELECTIONREMOVE|<xref:System.Windows.Automation.SelectionItemPattern.ElementRemovedFromSelectionEvent>|  
|EVENT_OBJECT_SELECTIONWITHIN|Sem equivalente|  
|EVENT_OBJECT_SHOW|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent>|  
|EVENT_OBJECT_STATECHANGE|Vários eventos de alteração de propriedade|  
|EVENT_OBJECT_VALUECHANGE|<xref:System.Windows.Automation.RangeValuePattern.ValueProperty?displayProperty=nameWithType>e <xref:System.Windows.Automation.ValuePattern.ValueProperty?displayProperty=nameWithType> alterado|  
|EVENT_SYSTEM_ALERT|Sem equivalente|  
|EVENT_SYSTEM_CAPTUREEND|Sem equivalente|  
|EVENT_SYSTEM_CAPTURESTART|Sem equivalente|  
|EVENT_SYSTEM_CONTEXTHELPEND|Sem equivalente|  
|EVENT_SYSTEM_CONTEXTHELPSTART|Sem equivalente|  
|EVENT_SYSTEM_DIALOGEND|<xref:System.Windows.Automation.WindowPattern.WindowClosedEvent>|  
|EVENT_SYSTEM_DIALOGSTART|<xref:System.Windows.Automation.WindowPattern.WindowOpenedEvent>|  
|EVENT_SYSTEM_DRAGDROPEND|Sem equivalente|  
|EVENT_SYSTEM_DRAGDROPSTART|Sem equivalente|  
|EVENT_SYSTEM_FOREGROUND|<xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent>|  
|EVENT_SYSTEM_MENUEND|<xref:System.Windows.Automation.AutomationElement.MenuClosedEvent>|  
|EVENT_SYSTEM_MENUPOPUPEND|<xref:System.Windows.Automation.AutomationElement.MenuClosedEvent>|  
|EVENT_SYSTEM_MENUPOPUPSTART|<xref:System.Windows.Automation.AutomationElement.MenuOpenedEvent>|  
|EVENT_SYSTEM_MENUSTART|<xref:System.Windows.Automation.AutomationElement.MenuOpenedEvent>|  
|EVENT_SYSTEM_MINIMIZEEND|<xref:System.Windows.Automation.WindowPattern.WindowVisualStateProperty>alteração de propriedade|  
|EVENT_SYSTEM_MINIMIZESTART|<xref:System.Windows.Automation.WindowPattern.WindowVisualStateProperty>alteração de propriedade|  
|EVENT_SYSTEM_MOVESIZEEND|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty>alteração de propriedade|  
|EVENT_SYSTEM_MOVESIZESTART|<xref:System.Windows.Automation.AutomationElement.BoundingRectangleProperty>alteração de propriedade|  
|EVENT_SYSTEM_SCROLLINGEND|<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty>ou <xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty> alteração de propriedade|  
|EVENT_SYSTEM_SCROLLINGSTART|<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty>ou <xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty> alteração de propriedade|  
|EVENT_SYSTEM_SOUND|Sem equivalente|  
|EVENT_SYSTEM_SWITCHEND|Nenhum equivalente, mas um <xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent> evento sinaliza que um novo aplicativo recebeu o foco|  
|EVENT_SYSTEM_SWITCHSTART|Sem equivalente|  
|Sem equivalente|<xref:System.Windows.Automation.MultipleViewPattern.CurrentViewProperty>alteração de propriedade|  
|Sem equivalente|<xref:System.Windows.Automation.ScrollPattern.HorizontallyScrollableProperty>alteração de propriedade|  
|Sem equivalente|<xref:System.Windows.Automation.ScrollPattern.VerticallyScrollableProperty>alteração de propriedade|  
|Sem equivalente|<xref:System.Windows.Automation.ScrollPattern.HorizontalScrollPercentProperty>alteração de propriedade|  
|Sem equivalente|<xref:System.Windows.Automation.ScrollPattern.VerticalScrollPercentProperty>alteração de propriedade|  
|Sem equivalente|<xref:System.Windows.Automation.ScrollPattern.HorizontalViewSizeProperty>alteração de propriedade|  
|Sem equivalente|<xref:System.Windows.Automation.ScrollPattern.VerticalViewSizeProperty>alteração de propriedade|  
|Sem equivalente|<xref:System.Windows.Automation.TogglePattern.ToggleStateProperty>alteração de propriedade|  
|Sem equivalente|<xref:System.Windows.Automation.WindowPattern.WindowVisualStateProperty>alteração de propriedade|  
|Sem equivalente|Evento <xref:System.Windows.Automation.AutomationElement.AsyncContentLoadedEvent>|  
|Sem equivalente|<xref:System.Windows.Automation.AutomationElement.ToolTipOpenedEvent>|  
  
<a name="Security_compare"></a>
## <a name="security"></a>Segurança  
 Alguns `IAccessible` cenários de personalização exigem o encapsulamento de uma base `IAccessible` e a chamada a ele. Isso tem implicações de segurança, pois um componente parcialmente confiável não deve ser um intermediário em um caminho de código.  
  
 O [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] modelo elimina a necessidade de provedores chamarem para outro código de provedor. O [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] serviço principal faz toda a agregação necessária.  
  
## <a name="see-also"></a>Confira também

- [Fundamentos de automação da interface do usuário](index.md)
