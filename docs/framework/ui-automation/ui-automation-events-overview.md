---
title: Visão geral sobre eventos de automação de interface do usuário
description: Consulte uma visão geral da notificação de eventos de automação da interface do usuário da Microsoft. Examine os tipos de eventos, identificadores de evento de automação da interface do usuário e argumentos de evento de automação da interface do usuário.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, providers
- UI Automation, events
- clients, UI Automation
- events, UI Automation
- providers, UI Automation
- UI Automation, clients
ms.assetid: 69eebd8b-39ed-40e7-93cc-4457c4caf746
ms.openlocfilehash: 4f51a31a433986822a9dba22bf8f17ade00bbb76
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168091"
---
# <a name="ui-automation-events-overview"></a>Visão geral sobre eventos de automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]a notificação de eventos é um recurso importante para tecnologias assistenciais, como leitores de tela e ampliadores de tela. Esses clientes de automação da interface do usuário rastreiam eventos que são gerados por provedores de automação de interface do usuário quando algo acontece no [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] e usam as informações para notificar os usuários finais.  
  
 A eficiência é aprimorada permitindo que os aplicativos de provedor gerem eventos de forma seletiva, dependendo se algum cliente estiver inscrito nesses eventos ou não, se nenhum cliente estiver ouvindo nenhum evento.  
  
<a name="Types_of_Events"></a>
## <a name="types-of-events"></a>Tipos de eventos  
 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]os eventos se enquadram nas categorias a seguir.  
  
|Evento|Descrição|  
|-----------|-----------------|  
|Alteração da propriedade|Gerado quando uma propriedade em um [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] padrão de elemento ou de controle é alterada. Por exemplo, se um cliente precisar monitorar o controle de caixa de seleção de um aplicativo, ele poderá se registrar para escutar um evento de alteração de propriedade na <xref:System.Windows.Automation.TogglePattern.TogglePatternInformation.ToggleState%2A> propriedade. Quando o controle caixa de seleção está marcado ou desmarcado, o provedor gera o evento e o cliente pode agir conforme necessário.|  
|Ação de elemento|Gerado quando uma alteração nos [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] resultados de uma atividade programática ou de usuário final, por exemplo, quando um botão é clicado ou invocado por meio do <xref:System.Windows.Automation.InvokePattern> .|  
|Alteração de estrutura|Gerado quando a estrutura da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore é alterada. A estrutura é alterada quando novos [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] itens se tornam visíveis, ocultos ou removidos na área de trabalho.|  
|Alteração global na área de trabalho|Gerado quando ocorrem ações de interesse global para o cliente, como quando o foco muda de um elemento para outro ou quando uma janela é fechada.|  
  
 Alguns eventos não indicam necessariamente que o estado da interface do usuário mudou. Por exemplo, se o usuário tabular para um campo de entrada de texto e clicar em um botão para atualizar o campo, um `TextChangedEvent` será gerado mesmo que o usuário não tenha realmente alterado o texto. Ao processar um evento, pode ser necessário que o aplicativo cliente verifique se algo realmente mudou antes de executar uma ação.  
  
 Os eventos a seguir podem ser gerados mesmo quando o estado da interface do usuário não foi alterado.  
  
- `AutomationPropertyChangedEvent`(dependendo da propriedade que foi alterada)  
  
- `ElementSelectedEvent`  
  
- `InvalidatedEvent`  
  
- `TextChangedEvent`  
  
<a name="UI_Automation_Event_Identifiers"></a>
## <a name="ui-automation-event-identifiers"></a>Identificadores de evento de automação da interface do usuário  
 [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)]os eventos são identificados por <xref:System.Windows.Automation.AutomationEvent> objetos. A <xref:System.Windows.Automation.AutomationIdentifier.Id%2A> propriedade contém um valor que identifica exclusivamente o tipo de evento.  
  
 Os valores possíveis para <xref:System.Windows.Automation.AutomationIdentifier.Id%2A> são fornecidos na tabela a seguir, juntamente com o tipo usado para argumentos de evento. Observe que os identificadores usados por clientes e provedores são campos nomeados de forma idêntica de classes diferentes.  
  
|Identificador do cliente|Identificador do provedor|Tipo de argumentos de evento|  
|-----------------------|-------------------------|--------------------------|  
|<xref:System.Windows.Automation.AutomationElement.AsyncContentLoadedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.AsyncContentLoadedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AsyncContentLoadedEventArgs>|  
|<xref:System.Windows.Automation.SelectionItemPattern.ElementAddedToSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPattern.ElementRemovedFromSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPattern.ElementSelectedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionPattern.InvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.InvokePattern.InvokedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.LayoutInvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.MenuClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.MenuOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPattern.TextChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPattern.TextSelectionChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.ToolTipClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElement.ToolTipOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.WindowPattern.WindowOpenedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.InvokePatternIdentifiers.InvokedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.LayoutInvalidatedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.MenuClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.MenuOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPatternIdentifiers.TextChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.TextPatternIdentifiers.TextSelectionChangedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.ToolTipClosedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.AutomationElementIdentifiers.ToolTipOpenedEvent?displayProperty=nameWithType><br /><br /> <xref:System.Windows.Automation.WindowPatternIdentifiers.WindowOpenedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationEventArgs>|  
|<xref:System.Windows.Automation.AutomationElement.AutomationFocusChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationFocusChangedEventArgs>|  
|<xref:System.Windows.Automation.AutomationElement.AutomationPropertyChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationPropertyChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationPropertyChangedEventArgs>|  
|<xref:System.Windows.Automation.AutomationElement.StructureChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.StructureChangedEventArgs>|  
|<xref:System.Windows.Automation.WindowPattern.WindowClosedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.WindowPatternIdentifiers.WindowClosedEvent?displayProperty=nameWithType>|<xref:System.Windows.Automation.WindowClosedEventArgs>|  
  
<a name="UI_Automation_Event_Arguments"></a>
## <a name="ui-automation-event-arguments"></a>Argumentos de evento de automação da interface do usuário  
 As classes a seguir encapsulam argumentos de evento.  
  
|Classe|Descrição|  
|-----------|-----------------|  
|<xref:System.Windows.Automation.AsyncContentLoadedEventArgs>|Contém informações sobre o carregamento assíncrono de conteúdo, incluindo a porcentagem de carregamento concluído.|  
|<xref:System.Windows.Automation.AutomationEventArgs>|Contém informações sobre um evento simples que não requer dados adicionais.|  
|<xref:System.Windows.Automation.AutomationFocusChangedEventArgs>|Contém informações sobre uma alteração no foco de entrada de um elemento para outro. Os eventos desse tipo são gerados pelo [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] sistema, não pelos provedores.|  
|<xref:System.Windows.Automation.AutomationPropertyChangedEventArgs>|Contém informações sobre uma alteração em um valor de propriedade de um elemento ou padrão de controle.|  
|<xref:System.Windows.Automation.StructureChangedEventArgs>|Contém informações sobre uma alteração na [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore.|  
|<xref:System.Windows.Automation.WindowClosedEventArgs>|Contém informações sobre um fechamento de janela.|  
  
 Todas as classes de argumento de evento contêm um <xref:System.Windows.Automation.AutomationEventArgs.EventId%2A> membro. Esse identificador é encapsulado em um <xref:System.Windows.Automation.AutomationEvent> .  
  
 Os <xref:System.Windows.Automation.AutomationEvent> objetos usados para identificar eventos são obtidos por provedores de campos em <xref:System.Windows.Automation.AutomationElementIdentifiers> e classes de identificador de padrão de controle, como <xref:System.Windows.Automation.DockPatternIdentifiers> . Os campos equivalentes são obtidos por aplicativos cliente de campos no <xref:System.Windows.Automation.AutomationElement> e classes de padrão de controle, como <xref:System.Windows.Automation.DockPattern> .  
  
 Para obter uma lista de identificadores de eventos, consulte [eventos de automação da interface do usuário para clientes](ui-automation-events-for-clients.md).  
  
## <a name="see-also"></a>Confira também

- [Automação de Eventos de Interface de Usuário para Clientes.](ui-automation-events-for-clients.md)
- [Implementação do provedor de automação de interface do usuário no lado do servidor](server-side-ui-automation-provider-implementation.md)
- [Assinar eventos de automação de interface do usuário](subscribe-to-ui-automation-events.md)
