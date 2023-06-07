---
title: Implementando o padrão de controle SelectionItem de interface de usuário
description: Consulte Diretrizes e convenções para implementar o padrão de controle SelectionItem na automação da interface do usuário. Conheça os membros necessários para a interface ISelectionItemProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- Selection Item control pattern
- UI Automation, Selection Item control pattern
- control patterns, Selection Item
ms.assetid: 76b0949a-5b23-4cfc-84cc-154f713e2e12
ms.openlocfilehash: 441417aa370563a9ce8b513be6ca4507b21e1e4a
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163552"
---
# <a name="implementing-the-ui-automation-selectionitem-control-pattern"></a>Implementando o padrão de controle SelectionItem de interface de usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta as diretrizes e convenções para implementar o <xref:System.Windows.Automation.Provider.ISelectionItemProvider> , incluindo informações sobre propriedades, métodos e eventos. Links para referências adicionais são listados no final da visão geral.  
  
 O <xref:System.Windows.Automation.SelectionItemPattern> padrão de controle é usado para dar suporte a controles que atuam como itens filho selecionáveis individuais de controles de contêiner que implementam <xref:System.Windows.Automation.Provider.ISelectionProvider> . Para obter exemplos de controles que implementam o padrão de controle SelectionItem, consulte [mapeamento de padrão de controle para clientes de automação da interface do usuário](control-pattern-mapping-for-ui-automation-clients.md)  
  
<a name="Implementation_Guidelines_and_Conventions"></a>
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
 Ao implementar o padrão de controle item de seleção, observe as seguintes diretrizes e convenções:  
  
- Controles de seleção única que gerenciam controles filho que implementam <xref:System.Windows.Automation.Provider.IRawElementProviderFragmentRoot> , como o controle deslizante de **resolução de tela** na caixa de diálogo **Propriedades de exibição** , devem implementar <xref:System.Windows.Automation.Provider.ISelectionProvider> e seus filhos devem implementar <xref:System.Windows.Automation.Provider.IRawElementProviderFragment> e <xref:System.Windows.Automation.Provider.ISelectionItemProvider> .  
  
<a name="Required_Members_for_the_IValueProvider_Interface"></a>
## <a name="required-members-for-iselectionitemprovider"></a>Membros necessários para ISelectionItemProvider  
 As propriedades, os métodos e os eventos a seguir são necessários para implementar o <xref:System.Windows.Automation.Provider.ISelectionItemProvider> .  
  
|Membros necessários|Tipo de membro|Observações|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider.CanSelectMultiple%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider.IsSelectionRequired%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider.GetSelection%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent>|Evento|Gerado quando uma seleção em um contêiner é alterada de forma significativa e requer <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent> o envio de eventos mais e <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent> do que a <xref:System.Windows.Automation.Provider.AutomationInteropProvider.InvalidateLimit> constante permite.|  
  
- Se o resultado de um <xref:System.Windows.Automation.SelectionItemPattern.Select%2A> , um <xref:System.Windows.Automation.SelectionItemPattern.AddToSelection%2A> ou um <xref:System.Windows.Automation.SelectionItemPattern.RemoveFromSelection%2A> for um único item selecionado, um <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementSelectedEvent> deve ser gerado; caso contrário, enviar <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementAddedToSelectionEvent> /  <xref:System.Windows.Automation.SelectionItemPatternIdentifiers.ElementRemovedFromSelectionEvent> conforme apropriado.  
  
<a name="Exceptions"></a>
## <a name="exceptions"></a>Exceções  
 Os provedores devem lançar as seguintes exceções.  
  
|Tipo de exceção|Condição|  
|--------------------|---------------|  
|<xref:System.InvalidOperationException>|Quando qualquer uma das seguintes opções for tentada:<br /><br /> -   <xref:System.Windows.Automation.Provider.ISelectionItemProvider.RemoveFromSelection%2A>é chamado em um contêiner de seleção única onde <xref:System.Windows.Automation.SelectionPattern.IsSelectionRequiredProperty>  =  `true` e um elemento já está selecionado.<br />-   <xref:System.Windows.Automation.Provider.ISelectionItemProvider.RemoveFromSelection%2A>é chamado em um contêiner de várias seleções em que <xref:System.Windows.Automation.SelectionPattern.IsSelectionRequiredProperty>  =  `true` e apenas um elemento é selecionado.<br />-   <xref:System.Windows.Automation.Provider.ISelectionItemProvider.AddToSelection%2A>é chamado em um contêiner de seleção única em que <xref:System.Windows.Automation.SelectionPattern.CanSelectMultipleProperty>  =  `false` e outro elemento já está selecionado.|  
  
## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Implementando o padrão de controle Selection de automação de interface do usuário](implementing-the-ui-automation-selection-control-pattern.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
- [Exemplo de provedor de fragmento](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms771502(v=vs.90))
