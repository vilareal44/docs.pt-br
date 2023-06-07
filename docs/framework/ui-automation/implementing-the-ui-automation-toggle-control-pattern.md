---
title: Implementando o padrão de controle Toggle de automação de interface de usuário
description: Examine as diretrizes e convenções para implementar o padrão de controle de alternância na automação da interface do usuário. Conheça os membros necessários para a interface IToggleProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- Toggle control pattern
- control patterns, Toggle
- UI Automation, Toggle control pattern
ms.assetid: 3cfe875f-b0c0-413d-9703-5f14e6a1a30e
ms.openlocfilehash: f9ae850a560101582b5f1a461de19f260ef59798
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168028"
---
# <a name="implementing-the-ui-automation-toggle-control-pattern"></a>Implementando o padrão de controle Toggle de automação de interface de usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta diretrizes e convenções para implementação <xref:System.Windows.Automation.Provider.IToggleProvider> , incluindo informações sobre métodos e propriedades. Links para referências adicionais são listados no final do tópico.  
  
 O <xref:System.Windows.Automation.TogglePattern> padrão de controle é usado para dar suporte a controles que podem percorrer um conjunto de Estados e manter um estado depois de definido. Para obter exemplos de controles que implementam esse padrão de controle, consulte [mapeamento de padrão de controle para clientes de automação da interface do usuário](control-pattern-mapping-for-ui-automation-clients.md).  
  
<a name="Implementation_Guidelines_and_Conventions"></a>
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
 Ao implementar o padrão de controle de alternância, observe as seguintes diretrizes e convenções:  
  
- Os controles que não mantêm o estado quando ativados, como botões, botões da barra de ferramentas e hiperlinks, devem implementar <xref:System.Windows.Automation.Provider.IInvokeProvider> em seu lugar.  
  
- Um controle deve percorrer seu <xref:System.Windows.Automation.ToggleState> na seguinte ordem: <xref:System.Windows.Automation.ToggleState.On> <xref:System.Windows.Automation.ToggleState.Off> e, se houver suporte, <xref:System.Windows.Automation.ToggleState.Indeterminate> .  
  
- <xref:System.Windows.Automation.TogglePattern>não fornece um método SetState (newState) devido a problemas em torno da configuração direta de uma caixa de seleção de três Estados sem percorrer a <xref:System.Windows.Automation.ToggleState> sequência apropriada.  
  
- O controle RadioButton não implementa <xref:System.Windows.Automation.Provider.IToggleProvider> , pois não é capaz de percorrer seus Estados válidos.  
  
<a name="Required_Members_for_IToggleProvider"></a>
## <a name="required-members-for-itoggleprovider"></a>Membros necessários para IToggleProvider  
 As propriedades e os métodos a seguir são necessários para implementar o <xref:System.Windows.Automation.Provider.IToggleProvider> .  
  
|Membro necessário|Tipo de membro|Anotações|  
|---------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.TogglePattern.Toggle%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.TogglePatternIdentifiers.ToggleStateProperty>|Propriedade|Nenhum|  
  
 Este padrão de controle não tem eventos associados.  
  
<a name="Exceptions"></a>
## <a name="exceptions"></a>Exceções  
 Este padrão de controle não tem nenhuma exceção associada.  
  
## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Obter o estado Toggle de uma caixa de seleção usando automação de interface do usuário](get-the-toggle-state-of-a-check-box-using-ui-automation.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
