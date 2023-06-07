---
title: Implementando o Padrão Controle de Window de Automação de Interface de Usuário
description: Examine as diretrizes e convenções para implementar o padrão de controle de janela na automação da interface do usuário. Conheça os membros necessários para a interface IWindowProvider.
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, Window
- UI Automation, Window control pattern
- Window control pattern
ms.assetid: a28cb286-296e-4a62-b4cb-55ad636ebccc
ms.openlocfilehash: e1d7429f86896947a10b73965caa7d771f54490b
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168188"
---
# <a name="implementing-the-ui-automation-window-control-pattern"></a>Implementando o Padrão Controle de Window de Automação de Interface de Usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico apresenta as diretrizes e convenções para implementar o <xref:System.Windows.Automation.Provider.IWindowProvider> , incluindo informações sobre <xref:System.Windows.Automation.WindowPattern> Propriedades, métodos e eventos. Links para referências adicionais são listados no final do tópico.  
  
 O <xref:System.Windows.Automation.WindowPattern> padrão de controle é usado para dar suporte a controles que fornecem funcionalidade básica baseada em janela dentro de uma GUI (interface gráfica do usuário) tradicional. Exemplos de controles que devem implementar esse padrão de controle incluem janelas de aplicativo de nível superior, janelas filhas de MDI (interface de vários documentos), controles de painel dividido redimensionável, caixas de diálogo modais e janelas de ajuda de balão.  
  
<a name="Implementation_Guidelines_and_Conventions"></a>
## <a name="implementation-guidelines-and-conventions"></a>Diretrizes e convenções de implementação  
 Ao implementar o padrão de controle Window, observe as seguintes diretrizes e convenções:  
  
- Para dar suporte à capacidade de modificar o tamanho da janela e a posição da tela usando a automação da interface do usuário, um controle deve implementar além <xref:System.Windows.Automation.Provider.ITransformProvider> do <xref:System.Windows.Automation.Provider.IWindowProvider> .  
  
- Controles que contêm barras de título e elementos de barra de título que permitem que o controle seja movido, redimensionado, maximizado, minimizado ou fechado normalmente são necessários para implementar <xref:System.Windows.Automation.Provider.IWindowProvider> .  
  
- Controles como pop-ups de dica de ferramenta e caixas suspensas de caixa de combinação ou de menu normalmente não implementam <xref:System.Windows.Automation.Provider.IWindowProvider> .  
  
- As janelas de ajuda de balão são diferenciadas de pop-ups de dica de ferramenta básica pelo provisionamento de um botão de fechamento de janela.  
  
- O modo de tela inteira não tem suporte do IWindowProvider, pois ele é específico ao recurso para um aplicativo e não é um comportamento de janela típico.  
  
<a name="Required_Members_for_IWindowProvider"></a>
## <a name="required-members-for-iwindowprovider"></a>Membros necessários para IWindowProvider  
 As propriedades, os métodos e os eventos a seguir são necessários para a interface IWindowProvider.  
  
|Membro necessário|Tipo de membro|Anotações|  
|---------------------|-----------------|-----------|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.InteractionState%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.IsModal%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.IsTopmost%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.Maximizable%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.Minimizable%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.VisualState%2A>|Propriedade|Nenhum|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.Close%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.SetVisualState%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.WaitForInputIdle%2A>|Método|Nenhum|  
|<xref:System.Windows.Automation.WindowPattern.WindowClosedEvent>|Evento|Nenhum|  
|<xref:System.Windows.Automation.WindowPattern.WindowOpenedEvent>|Evento|Nenhum|  
|<xref:System.Windows.Automation.WindowInteractionState>|Evento|Não é garantido que seja<xref:System.Windows.Automation.WindowInteractionState.ReadyForUserInteraction>|  
  
<a name="Exceptions"></a>
## <a name="exceptions"></a>Exceções  
 Os provedores devem lançar as seguintes exceções.  
  
|Tipo de exceção|Condição|  
|--------------------|---------------|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.Provider.IWindowProvider.SetVisualState%2A><br /><br /> -Quando um controle não dá suporte a um comportamento solicitado.|  
|<xref:System.ArgumentOutOfRangeException>|<xref:System.Windows.Automation.Provider.IWindowProvider.WaitForInputIdle%2A><br /><br /> -Quando o parâmetro não é um número válido.|  
  
## <a name="see-also"></a>Confira também

- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de suporte em um provedor de automação da interface do usuário](support-control-patterns-in-a-ui-automation-provider.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Visão geral da árvore de automação de interface do usuário](ui-automation-tree-overview.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
