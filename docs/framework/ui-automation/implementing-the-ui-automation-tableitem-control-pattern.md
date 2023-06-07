---
title: Implementando o padrão de controle TableItem de interface de usuário
description: Examine as diretrizes e convenções para implementar o padrão de controle TableItem na automação da interface do usuário. Conheça os membros necessários para a interface ITableItemProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, Table Item
- UI Automation, Table Item control pattern
- TableItem control pattern
ms.assetid: ac178408-1485-436f-8d3e-eee3bf80cb24
ms.openlocfilehash: 3d6d31fe0e041fbba147df14d290a775188755f2
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163515"
---
# <a name="implementing-the-ui-automation-tableitem-control-pattern"></a>Implementando o padrão de controle TableItem de interface de usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta diretrizes e convenções para implementação <xref:System.Windows.Automation.Provider.ITableItemProvider> , incluindo informações sobre eventos e propriedades. Links para referências adicionais são listados no final da visão geral.  
  
 O <xref:System.Windows.Automation.TableItemPattern> padrão de controle é usado para dar suporte a controles filho de contêineres que implementam o <xref:System.Windows.Automation.Provider.ITableProvider> . O acesso à funcionalidade de célula individual é fornecido pela implementação simultânea necessária do <xref:System.Windows.Automation.Provider.IGridItemProvider> . Esse padrão de controle é análogo à <xref:System.Windows.Automation.Provider.IGridItemProvider> distinção que qualquer controle que implementa <xref:System.Windows.Automation.Provider.ITableItemProvider> deve expor de forma programática a relação entre a célula individual e suas informações de linha e coluna. Para obter exemplos de controles que implementam esse padrão de controle, consulte [mapeamento de padrão de controle para clientes de automação da interface do usuário](control-pattern-mapping-for-ui-automation-clients.md).  
  
<a name="Implementation_Guidelines_and_Conventions"></a>
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
  
- Para obter a funcionalidade de item de grade relacionada, consulte [implementando o padrão de controle GridItem de automação da interface do usuário](implementing-the-ui-automation-griditem-control-pattern.md)  
  
<a name="Required_Members_for_ITableItemProvider"></a>
## <a name="required-members-for-itableitemprovider"></a>Membros necessários para ITableItemProvider  
  
|Membro necessário|Tipo de membro|Anotações|  
|---------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.ITableItemProvider.GetColumnHeaderItems%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.Provider.ITableItemProvider.GetRowHeaderItems%2A>|Método|Nenhum|  
  
 Este padrão de controle não tem propriedades ou eventos associados.  
  
<a name="Exceptions"></a>
## <a name="exceptions"></a>Exceções  
 Este padrão de controle não tem nenhuma exceção associada.  
  
## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Implementando o padrão de controle de tabela de automação de interface de usuário](implementing-the-ui-automation-table-control-pattern.md)
- [Implementando o padrão de controle GridItem de interface de usuário](implementing-the-ui-automation-griditem-control-pattern.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
