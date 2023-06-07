---
title: Implementando o padrão de controle GridItem de interface de usuário
description: Conheça as diretrizes e as convenções para implementar o padrão de controle GridItemPattern para itens de grade na automação da interface do usuário. Consulte membros necessários para IGridItemProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, GridItem
- UI Automation GridItem control pattern
- GridItem control pattern
ms.assetid: bffbae08-fe2a-42fd-ab84-f37187518916
ms.openlocfilehash: e0a0c616f3f0cf9bc091e4fbb496d71ab8550bd3
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165820"
---
# <a name="implementing-the-ui-automation-griditem-control-pattern"></a>Implementando o padrão de controle GridItem de interface de usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta diretrizes e convenções para implementação <xref:System.Windows.Automation.Provider.IGridItemProvider> , incluindo informações sobre propriedades. Links para referências adicionais são listados no final da visão geral.  
  
 O <xref:System.Windows.Automation.GridItemPattern> padrão de controle é usado para dar suporte a controles filho individuais de contêineres que implementam <xref:System.Windows.Automation.Provider.IGridProvider> . Para obter exemplos de controles que implementam esse padrão de controle, consulte [mapeamento de padrão de controle para clientes de automação da interface do usuário](control-pattern-mapping-for-ui-automation-clients.md).  
  
<a name="Implementation_Guidelines_and_Conventions"></a>
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
 Ao implementar <xref:System.Windows.Automation.Provider.IGridProvider> , observe as seguintes diretrizes e convenções:  
  
- As coordenadas de grade são baseadas em zero com a célula superior esquerda com coordenadas (0, 0).  
  
- As células mescladas relatarão suas <xref:System.Windows.Automation.Provider.IGridItemProvider.Row%2A> <xref:System.Windows.Automation.Provider.IGridItemProvider.Column%2A> Propriedades e com base em sua célula de âncora subjacente, conforme definido pelo provedor de automação de interface do usuário. Normalmente, será a linha ou coluna mais alta e mais à esquerda.  
  
- <xref:System.Windows.Automation.Provider.IGridItemProvider>não fornece manipulação ativa da grade, como mesclagem ou divisão de células.  
  
- Os controles que implementam <xref:System.Windows.Automation.Provider.IGridItemProvider> normalmente podem ser percorridos (ou seja, um cliente de automação da interface do usuário pode mover para controles adjacentes) usando o teclado.  
  
<a name="Required_Members_for_IGridItemProvider"></a>
## <a name="required-members-for-igriditemprovider"></a>Membros necessários para IGridItemProvider  
 As propriedades e os métodos a seguir são necessários para implementar o <xref:System.Windows.Automation.Provider.IGridItemProvider> .  
  
|Membros necessários|Tipo de membro|Observações|  
|----------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.IGridItemProvider.Row%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IGridItemProvider.Column%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IGridItemProvider.RowSpan%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IGridItemProvider.ColumnSpan%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IGridItemProvider.ContainingGrid%2A>|Propriedade|Nenhum|  
  
 Este padrão de controle não tem nenhum método ou evento associado.  
  
<a name="Exceptions"></a>
## <a name="exceptions"></a>Exceções  
 Este padrão de controle não tem nenhuma exceção associada.  
  
## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Implementando o Padrão de Controle Grid de Automação de Interface de Usuário](implementing-the-ui-automation-grid-control-pattern.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
