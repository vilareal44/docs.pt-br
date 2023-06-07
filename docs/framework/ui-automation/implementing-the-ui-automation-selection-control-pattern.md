---
title: Implementando o padrão de controle Selection de automação de interface do usuário
description: Examine as diretrizes e convenções para implementar o padrão de controle de seleção na automação da interface do usuário. Consulte membros necessários para a interface ISelectionProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- Selection control pattern
- UI Automation, Selection control pattern
- control patterns, Selection
ms.assetid: 449c3068-a5d6-4f66-84c6-1bcc7dd4d209
ms.openlocfilehash: d3854a401ae6179be4e4e75d86964108d83b0ccf
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163584"
---
# <a name="implementing-the-ui-automation-selection-control-pattern"></a>Implementando o padrão de controle Selection de automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta diretrizes e convenções para implementação <xref:System.Windows.Automation.Provider.ISelectionProvider> , incluindo informações sobre eventos e propriedades. Links para referências adicionais são listados no final do tópico.  
  
 O <xref:System.Windows.Automation.SelectionPattern> padrão de controle é usado para dar suporte a controles que atuam como contêineres para uma coleção de itens filho selecionáveis. Os filhos deste elemento devem implementar <xref:System.Windows.Automation.Provider.ISelectionItemProvider> . Para obter exemplos de controles que implementam esse padrão de controle, consulte [mapeamento de padrão de controle para clientes de automação da interface do usuário](control-pattern-mapping-for-ui-automation-clients.md).  
  
<a name="Implementation_Guidelines_and_Conventions"></a>
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
 Ao implementar o padrão de controle de seleção, observe as seguintes diretrizes e convenções:  
  
- Os controles que implementam <xref:System.Windows.Automation.Provider.ISelectionProvider> permitem que um único ou vários itens filho sejam selecionados. Por exemplo, caixa de listagem, exibição de lista e exibição de árvore dão suporte a várias seleções, enquanto que a caixa de combinação, o controle deslizante e o grupo de botões de opção dão suporte à seleção única.  
  
- Controles que têm um intervalo mínimo, máximo e contínuo, como o controle deslizante de **volume** , devem implementar <xref:System.Windows.Automation.Provider.IRangeValueProvider> em vez de <xref:System.Windows.Automation.Provider.ISelectionProvider> .  
  
- Controles de seleção única que gerenciam controles filho que implementam <xref:System.Windows.Automation.Provider.IRawElementProviderFragmentRoot> , como o controle deslizante de **resolução de tela** na caixa de diálogo **Propriedades de exibição** ou o controle seleção de **seletor de cores** do Microsoft Word (ilustrado abaixo), devem implementar <xref:System.Windows.Automation.Provider.ISelectionProvider> ; seus filhos devem implementar <xref:System.Windows.Automation.Provider.IRawElementProviderFragment> e <xref:System.Windows.Automation.Provider.ISelectionItemProvider> .  
  
 ![Seletor de cores com amarelo realçado.](./media/uia-valuepattern-colorpicker.png "UIA_ValuePattern_ColorPicker")  
Exemplo de mapeamento de cadeia de caracteres de amostra de cor  
  
- Os menus não dão suporte ao <xref:System.Windows.Automation.SelectionPattern> . Se você estiver trabalhando com itens de menu que incluem elementos gráficos e texto (como os itens do **painel de visualização** no menu **Exibir** no Microsoft Outlook) e precisar transmitir o estado, você deverá implementar o <xref:System.Windows.Automation.Provider.IToggleProvider> .  
  
<a name="Required_Members_for_ISelectionProvider"></a>
## <a name="required-members-for-iselectionprovider"></a>Membros necessários para ISelectionProvider  
 As propriedades, os métodos e os eventos a seguir são necessários para a <xref:System.Windows.Automation.Provider.ISelectionProvider> interface.  
  
|Membros necessários|Type|Observações|  
|----------------------|----------|-----------|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider.CanSelectMultiple%2A>|Propriedade|Deve oferecer suporte a eventos de alteração de propriedade usando <xref:System.Windows.Automation.Automation.AddAutomationPropertyChangedEventHandler%2A> e <xref:System.Windows.Automation.Automation.RemoveAutomationPropertyChangedEventHandler%2A> .|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider.IsSelectionRequired%2A>|Propriedade|Deve oferecer suporte a eventos de alteração de propriedade usando <xref:System.Windows.Automation.Automation.AddAutomationPropertyChangedEventHandler%2A> e <xref:System.Windows.Automation.Automation.RemoveAutomationPropertyChangedEventHandler%2A> .|  
|<xref:System.Windows.Automation.Provider.ISelectionProvider.GetSelection%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.SelectionPatternIdentifiers.InvalidatedEvent>|Evento|Gerado quando uma seleção em um contêiner é alterada significativamente e requer o envio de mais eventos de adição e remoção do que a <xref:System.Windows.Automation.Provider.AutomationInteropProvider.InvalidateLimit> constante permite.|  
  
 As <xref:System.Windows.Automation.Provider.ISelectionProvider.IsSelectionRequired%2A> <xref:System.Windows.Automation.Provider.ISelectionProvider.CanSelectMultiple%2A> Propriedades e podem ser dinâmicas. Por exemplo, o estado inicial de um controle pode não ter nenhum item selecionado por padrão, indicando que <xref:System.Windows.Automation.Provider.ISelectionProvider.IsSelectionRequired%2A> é `false` . No entanto, depois que um item é selecionado, o controle sempre deve ter pelo menos um item selecionado. Da mesma forma, em casos raros, um controle pode permitir que vários itens sejam selecionados na inicialização, mas posteriormente permitir que apenas seleções únicas sejam feitas.  
  
<a name="Exceptions"></a>
## <a name="exceptions"></a>Exceções  
 Os provedores devem lançar as seguintes exceções.  
  
|Tipo de exceção|Condição|  
|--------------------|---------------|  
|<xref:System.Windows.Automation.ElementNotEnabledException>|Se o controle não está habilitado.|  
|<xref:System.InvalidOperationException>|Se o controle estiver oculto.|  
  
## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Implementando o padrão de controle SelectionItem de interface de usuário](implementing-the-ui-automation-selectionitem-control-pattern.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
