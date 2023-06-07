---
title: Implementando o padrão de controle ExpandCollapse de interface de usuário
description: Aprenda as diretrizes e as convenções de implementação para o padrão de controle ExpandCollapse na automação da interface do usuário. Saiba como implementar o IExpandCollapseProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, ExpandCollapse control pattern
- ExpandCollapse control pattern
- control patterns, ExpandCollapse
ms.assetid: 1dbabb8c-0d68-47c1-a35e-1c01cb01af26
ms.openlocfilehash: 525b57816071ba2d879036676201a0506d1a29db
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165861"
---
# <a name="implementing-the-ui-automation-expandcollapse-control-pattern"></a>Implementando o padrão de controle ExpandCollapse de interface de usuário

> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).

Este tópico apresenta as diretrizes e convenções para implementar o <xref:System.Windows.Automation.Provider.IExpandCollapseProvider> , incluindo informações sobre propriedades, métodos e eventos. Links para referências adicionais são listados no final da visão geral.

O <xref:System.Windows.Automation.ExpandCollapsePattern> padrão de controle é usado para dar suporte a controles que expandem visualmente para exibir mais conteúdo e recolher para ocultar o conteúdo. Para obter exemplos de controles que implementam esse padrão de controle, consulte [mapeamento de padrão de controle para clientes de automação da interface do usuário](control-pattern-mapping-for-ui-automation-clients.md).

<a name="Implementation_Guidelines_and_Conventions"></a>

## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação

Ao implementar o padrão de controle ExpandCollapse, observe as seguintes diretrizes e convenções:

- Controles de agregação — criados com objetos filho que fornecem a interface do usuário com funcionalidade de expandir/recolher — devem oferecer suporte ao <xref:System.Windows.Automation.ExpandCollapsePattern> padrão de controle, enquanto seus elementos filho não. Por exemplo, um controle de caixa de combinação é criado com uma combinação de controles de caixa de listagem, botões e edição, mas é apenas a caixa de combinação pai que deve dar suporte ao <xref:System.Windows.Automation.ExpandCollapsePattern> .

  > [!NOTE]
  > Uma exceção é o controle de menu, que é uma agregação de objetos individuais MenuItem. Os objetos MenuItem podem dar suporte ao <xref:System.Windows.Automation.ExpandCollapsePattern> padrão de controle, mas o controle menu pai não pode. Uma exceção semelhante se aplica aos controles de item de árvore e árvore.

- Quando o <xref:System.Windows.Automation.ExpandCollapseState> de um controle é definido como <xref:System.Windows.Automation.ExpandCollapseState.LeafNode> , qualquer <xref:System.Windows.Automation.ExpandCollapsePattern> funcionalidade está inativa no momento para o controle e as únicas informações que podem ser obtidas usando esse padrão de controle é o <xref:System.Windows.Automation.ExpandCollapseState> . Se qualquer objeto filho for adicionado posteriormente, as <xref:System.Windows.Automation.ExpandCollapseState> alterações e a <xref:System.Windows.Automation.ExpandCollapsePattern> funcionalidade serão ativadas.

- <xref:System.Windows.Automation.ExpandCollapseState>refere-se apenas à visibilidade de objetos filho imediatos; Ele não se refere à visibilidade de todos os objetos descendentes.

- A funcionalidade de expandir e recolher é específica ao controle. Veja a seguir exemplos desse comportamento.

  - O menu pessoal do Office pode ser um MenuItem de três Estados <xref:System.Windows.Automation.ExpandCollapseState.Expanded> ( <xref:System.Windows.Automation.ExpandCollapseState.Collapsed> e <xref:System.Windows.Automation.ExpandCollapseState.PartiallyExpanded> ) em que o controle Especifica o estado a ser adotado quando um <xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A> ou <xref:System.Windows.Automation.ExpandCollapsePattern.Collapse%2A> é chamado.

  - Chamar <xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A> em um TreeItem pode exibir todos os descendentes ou apenas filhos imediatos.

  - Se chamar <xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A> ou <xref:System.Windows.Automation.ExpandCollapsePattern.Collapse%2A> em um controle mantiver o estado de seus descendentes, um evento de alteração de visibilidade deverá ser enviado, não um evento de alteração de estado se o controle pai não mantiver o estado de seus descendentes quando recolhido, o controle poderá destruir todos os descendentes que não são mais visíveis e gerar um evento destruído; ou pode alterar o <xref:System.Windows.Automation.Provider.IExpandCollapseProvider.ExpandCollapseState%2A> para cada descendente e gerar um evento de

- Para garantir a navegação, é desejável que um objeto esteja na [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore (com o estado de visibilidade apropriado), independentemente de seus pais <xref:System.Windows.Automation.ExpandCollapseState> . Se os descendentes forem gerados sob demanda, eles só poderão aparecer na [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore depois de serem exibidos pela primeira vez ou somente enquanto estiverem visíveis.

<a name="Required_Members_for_the_IValueProvider_Interface"></a>

## <a name="required-members-for-iexpandcollapseprovider"></a>Membros necessários para IExpandCollapseProvider

As propriedades e os métodos a seguir são necessários para implementar o <xref:System.Windows.Automation.Provider.IExpandCollapseProvider> .

|Membros necessários|Tipo de membro|Observações|
|----------------------|-----------------|-----------|
|<xref:System.Windows.Automation.Provider.IExpandCollapseProvider.ExpandCollapseState%2A>|Propriedade|Nenhum|
|<xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A>|Método|Nenhum|
|<xref:System.Windows.Automation.ExpandCollapsePattern.Collapse%2A>|Método|Nenhum|
|<xref:System.Windows.Automation.AutomationPropertyChangedEventHandler>|Evento|Este controle não tem eventos associados; Use este delegado genérico.|

<a name="Exceptions"></a>

## <a name="exceptions"></a>Exceções

Os provedores devem lançar as seguintes exceções.

|Tipo de exceção|Condição|
|--------------------|---------------|
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.ExpandCollapsePattern.Expand%2A>Ou <xref:System.Windows.Automation.ExpandCollapsePattern.Collapse%2A> é chamado quando o <xref:System.Windows.Automation.ExpandCollapseState>  =  <xref:System.Windows.Automation.ExpandCollapseState.LeafNode> .|

## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Navegar em elementos de automação de interface do usuário com TreeWalker](navigate-among-ui-automation-elements-with-treewalker.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
