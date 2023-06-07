---
title: Obtendo elementos da automação interface do usuário
description: Revise várias maneiras de obter objetos de interface de automação da interface do usuário (AutomationElement) para elementos da IU.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, obtaining elements
- elements, UI Automation, obtaining
ms.assetid: c2caaf45-e59c-42a1-bc9b-77a6de520171
ms.openlocfilehash: 2b741dde3b30334ba8fa990d73044da7e3bdd2da
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168166"
---
# <a name="obtaining-ui-automation-elements"></a>Obtendo elementos da automação interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico descreve as várias maneiras de obter <xref:System.Windows.Automation.AutomationElement> objetos para [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] elementos.  
  
> [!CAUTION]
> Se seu aplicativo cliente pode tentar localizar elementos em sua própria interface do usuário, você deve fazer todas as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] chamadas em um thread separado. Para obter mais informações, consulte [problemas de threading de automação da interface do usuário](ui-automation-threading-issues.md).  
  
<a name="The_Root_Element"></a>
## <a name="root-element"></a>Elemento Root  
 Todas as pesquisas de <xref:System.Windows.Automation.AutomationElement> objetos devem ter um local de início. Pode ser qualquer elemento, incluindo a área de trabalho, uma janela de aplicativo ou um controle.  
  
 O elemento raiz para a área de trabalho, do qual todos os elementos são descendentes, é obtido da <xref:System.Windows.Automation.AutomationElement.RootElement%2A?displayProperty=nameWithType> propriedade estática.  
  
> [!CAUTION]
> Em geral, você deve tentar obter apenas filhos diretos do <xref:System.Windows.Automation.AutomationElement.RootElement%2A> . Uma pesquisa por descendentes pode iterar por centenas ou até milhares de elementos, possivelmente resultando em um estouro de pilha. Se você estiver tentando obter um elemento específico em um nível inferior, deverá iniciar a pesquisa na janela do aplicativo ou em um contêiner em um nível inferior.  
  
<a name="Using_Conditions"></a>
## <a name="conditions"></a>Condições  
 Para a maioria das técnicas que você pode usar para recuperar [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] elementos, você deve especificar um <xref:System.Windows.Automation.Condition> , que é um conjunto de critérios que definem quais elementos você deseja recuperar.  
  
 A condição mais simples é <xref:System.Windows.Automation.Condition.TrueCondition> , um objeto predefinido que especifica que todos os elementos dentro do escopo de pesquisa devem ser retornados. <xref:System.Windows.Automation.Condition.FalseCondition>, o converso de <xref:System.Windows.Automation.Condition.TrueCondition> , é menos útil, pois isso impediria que qualquer elemento fosse encontrado.  
  
 Três outras condições predefinidas podem ser usadas sozinhas ou em combinação com outras condições: <xref:System.Windows.Automation.Automation.ContentViewCondition> , <xref:System.Windows.Automation.Automation.ControlViewCondition> e <xref:System.Windows.Automation.Automation.RawViewCondition> . <xref:System.Windows.Automation.Automation.RawViewCondition>, usado por si só, é equivalente a <xref:System.Windows.Automation.Condition.TrueCondition> , porque não filtra elementos por suas <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsControlElement%2A> Propriedades ou <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsContentElement%2A> .  
  
 Outras condições são criadas a partir de um ou mais <xref:System.Windows.Automation.PropertyCondition> objetos, cada um deles especifica um valor de propriedade. Por exemplo, um <xref:System.Windows.Automation.PropertyCondition> pode especificar que o elemento está habilitado ou que ele dá suporte a um determinado padrão de controle.  
  
 As condições podem ser combinadas usando a lógica booliana construindo objetos de tipos <xref:System.Windows.Automation.AndCondition> , <xref:System.Windows.Automation.OrCondition> e <xref:System.Windows.Automation.NotCondition> .  
  
<a name="Search_Scope"></a>
## <a name="search-scope"></a>Escopo de pesquisa  
 Pesquisas feitas usando <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> ou <xref:System.Windows.Automation.AutomationElement.FindAll%2A> devem ter um escopo, bem como um local de início.  
  
 O escopo define o espaço em volta do local inicial que deve ser pesquisado. Isso pode incluir o próprio elemento, seus irmãos, seu pai, seus ancestrais, seus filhos imediatos e seus descendentes.  
  
 O escopo de uma pesquisa é definido por uma combinação de bits de valores da <xref:System.Windows.Automation.TreeScope> enumeração.  
  
<a name="Finding_a_Known_Element"></a>
## <a name="finding-a-known-element"></a>Encontrando um elemento conhecido  
 Para localizar um elemento conhecido, identificado por seu <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name%2A> , <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.AutomationId%2A> ou alguma outra propriedade ou combinação de propriedades, é mais fácil usar o <xref:System.Windows.Automation.AutomationElement.FindFirst%2A> método. Se o elemento procurado for uma janela de aplicativo, o ponto de partida da pesquisa poderá ser o <xref:System.Windows.Automation.AutomationElement.RootElement%2A> .  
  
 Essa forma de localizar [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] elementos é mais útil em cenários de teste automatizados.  
  
<a name="Finding_Elements_in_a_Subtree"></a>
## <a name="finding-elements-in-a-subtree"></a>Localizando elementos em uma subárvore  
 Para localizar todos os elementos que atendem a critérios específicos relacionados a um elemento conhecido, você pode usar <xref:System.Windows.Automation.AutomationElement.FindAll%2A> . Por exemplo, você pode usar esse método para recuperar itens de lista ou itens de menu de uma lista ou menu, ou para identificar todos os controles em uma caixa de diálogo.  
  
<a name="Walking_a_Subtree"></a>
## <a name="walking-a-subtree"></a>Examinando uma subárvore  
 Se você não tiver conhecimento prévio dos aplicativos com os quais seu cliente pode ser usado, poderá construir uma subárvore de todos os elementos de interesse usando a <xref:System.Windows.Automation.TreeWalker> classe. Seu aplicativo pode fazer isso em resposta a um evento de alteração de foco; ou seja, quando um aplicativo ou controle recebe o foco de entrada, o cliente de automação da interface do usuário examina os filhos e talvez todos os descendentes do elemento focado.  
  
 Outra maneira na qual <xref:System.Windows.Automation.TreeWalker> pode ser usada é identificar os ancestrais de um elemento. Por exemplo, ao percorrer a árvore, você pode identificar a janela pai de um controle.  
  
 Você pode usar <xref:System.Windows.Automation.TreeWalker> qualquer um deles criando um objeto da classe (definindo os elementos de interesse passando um <xref:System.Windows.Automation.Condition> ) ou usando um dos seguintes objetos predefinidos que são definidos como campos de <xref:System.Windows.Automation.TreeWalker> .  
  
|||  
|-|-|  
|<xref:System.Windows.Automation.TreeWalker.ContentViewWalker>|Localiza apenas os elementos cuja <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsContentElement%2A> propriedade é `true` .|  
|<xref:System.Windows.Automation.TreeWalker.ControlViewWalker>|Localiza apenas os elementos cuja <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.IsControlElement%2A> propriedade é `true` .|  
|<xref:System.Windows.Automation.TreeWalker.RawViewWalker>|Localiza todos os elementos.|  
  
 Depois de obter um <xref:System.Windows.Automation.TreeWalker> , usá-lo é simples. Basta chamar os `Get` métodos para navegar entre os elementos da subárvore.  
  
 O <xref:System.Windows.Automation.TreeWalker.Normalize%2A> método pode ser usado para navegar para um elemento na subárvore de outro elemento que não faz parte da exibição. Por exemplo, suponha que você tenha criado uma exibição de uma subárvore usando <xref:System.Windows.Automation.TreeWalker.ContentViewWalker> . Em seguida, seu aplicativo recebe uma notificação de que uma barra de rolagem recebeu o foco de entrada. Como uma barra de rolagem não é um elemento de conteúdo, ela não está presente na exibição da subárvore. No entanto, você pode passar o <xref:System.Windows.Automation.AutomationElement> que representa a barra de rolagem para <xref:System.Windows.Automation.TreeWalker.Normalize%2A> e recuperar o ancestral mais próximo que está na exibição de conteúdo.  
  
<a name="Other_Ways_to_Retrieve_an_Element"></a>
## <a name="other-ways-to-retrieve-an-element"></a>Outras maneiras de recuperar um elemento  
 Além de pesquisas e navegação, você pode recuperar um <xref:System.Windows.Automation.AutomationElement> das seguintes maneiras.  
  
### <a name="from-an-event"></a>De um evento  
 Quando seu aplicativo recebe um [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] evento, o objeto de origem passado para o manipulador de eventos é um <xref:System.Windows.Automation.AutomationElement> . Por exemplo, se você assinou os eventos de foco alterado, a origem passada para seu <xref:System.Windows.Automation.AutomationFocusChangedEventHandler> é o elemento que recebeu o foco.  
  
 Para obter mais informações, consulte [assinar eventos de automação da interface do usuário](subscribe-to-ui-automation-events.md).  
  
### <a name="from-a-point"></a>De um ponto  
 Se você tiver coordenadas de tela (por exemplo, uma posição de cursor), poderá recuperar um <xref:System.Windows.Automation.AutomationElement> usando o <xref:System.Windows.Automation.AutomationElement.FromPoint%2A> método estático.  
  
### <a name="from-a-window-handle"></a>De um identificador de janela  
 Para recuperar um <xref:System.Windows.Automation.AutomationElement> de um HWND, use o <xref:System.Windows.Automation.AutomationElement.FromHandle%2A> método estático.  
  
### <a name="from-the-focused-control"></a>Do controle focado  
 Você pode recuperar um <xref:System.Windows.Automation.AutomationElement> que representa o controle focalizado da <xref:System.Windows.Automation.AutomationElement.FocusedElement%2A> propriedade estática.  
  
## <a name="see-also"></a>Confira também

- [Localizar um elemento de automação de interface do usuário com base na condição de uma propriedade](find-a-ui-automation-element-based-on-a-property-condition.md)
- [Navegar em elementos de automação de interface do usuário com TreeWalker](navigate-among-ui-automation-elements-with-treewalker.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
