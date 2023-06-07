---
title: Padrões de Controle para Clientes de Automação de IU
description: Leia uma visão geral sobre padrões de controle para clientes de automação da interface do usuário. Use padrões de controle para acessar informações sobre a interface do usuário.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, control patterns for clients
- control patterns, UI Automation clients
ms.assetid: 571561d8-5f49-43a9-a054-87735194e013
ms.openlocfilehash: f2def328228a30ace6d0edc0661d6e79f237d6f4
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163866"
---
# <a name="ui-automation-control-patterns-for-clients"></a>Padrões de Controle para Clientes de Automação de IU
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Esta visão geral apresenta padrões de controle para clientes de automação da interface do usuário. Ele inclui informações sobre como um cliente de automação de interface do usuário pode usar padrões de controle para acessar informações sobre o [!INCLUDE[TLA#tla_ui](../../../includes/tlasharptla-ui-md.md)] .  
  
 Os padrões de controle permitem categorizar e expor a funcionalidade de um controle independente do tipo ou da aparência do controle. Os clientes de automação da interface do usuário podem examinar um <xref:System.Windows.Automation.AutomationElement> para determinar quais padrões de controle têm suporte e estar garantidos do comportamento do controle.  
  
 Para obter uma lista completa de padrões de controle, consulte [visão geral dos padrões de controle de automação da interface do usuário](ui-automation-control-patterns-overview.md).  
  
<a name="uiautomation_getting_control_patterns"></a>
## <a name="getting-control-patterns"></a>Obtendo padrões de controle  
 Os clientes recuperam um padrão de controle de um <xref:System.Windows.Automation.AutomationElement> chamando o <xref:System.Windows.Automation.AutomationElement.GetCachedPattern%2A?displayProperty=nameWithType> ou o <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A?displayProperty=nameWithType> .  
  
 Os clientes podem usar o <xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A> método ou uma `IsPatternAvailable` propriedade individual (por exemplo, <xref:System.Windows.Automation.AutomationElement.IsTextPatternAvailableProperty> ) para determinar se um padrão ou grupo de padrões tem suporte no <xref:System.Windows.Automation.AutomationElement> . No entanto, é mais eficiente tentar obter o padrão de controle e testar uma `null` referência do que verificar as propriedades com suporte e recuperar o padrão de controle, pois isso resulta em menos chamadas de processo cruzado.  
  
 O exemplo a seguir demonstra como obter um <xref:System.Windows.Automation.TextPattern> padrão de controle de um <xref:System.Windows.Automation.AutomationElement> .  
  
 [!code-csharp[UIATextPattern_snip#1037](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIATextPattern_snip/CSharp/SearchWindow.cs#1037)]  
  
<a name="uiautomation_properties_on_control_patterns"></a>
## <a name="retrieving-properties-on-control-patterns"></a>Recuperando propriedades em padrões de controle  
 Os clientes podem recuperar os valores de propriedade nos padrões de controle chamando <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A?displayProperty=nameWithType> ou <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A?displayProperty=nameWithType> e convertendo o objeto retornado para um tipo apropriado. Para obter mais informações sobre [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades, consulte [Propriedades de automação da interface do usuário para clientes](ui-automation-properties-for-clients.md).  
  
 Além dos `GetPropertyValue` métodos, os valores de propriedade podem ser recuperados por meio dos acessadores Common Language Runtime (CLR) para acessar as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] Propriedades em um padrão.  
  
<a name="uiautomation_with_variable_patterns"></a>
## <a name="controls-with-variable-patterns"></a>Controles com padrões de variáveis  
 Alguns tipos de controle dão suporte a padrões diferentes, dependendo do Estado ou da maneira como o controle está sendo usado. Exemplos de controles que podem ter padrões de variáveis são exibições de lista (miniaturas, blocos, ícones, lista, detalhes), gráficos do Microsoft Excel (pizza, linha, barra, valor de célula com uma fórmula), área de documento do Microsoft Word (modelos normais, layout da Web, estrutura de tópicos, layout de impressão, visualização de impressão) e Microsoft Windows Media Player.  
  
 Controles que implementam tipos de controle personalizados podem ter qualquer conjunto de padrões de controle que são necessários para representar sua funcionalidade.  
  
## <a name="see-also"></a>Confira também

- [Padrões de controle de automação da interface do usuário](ui-automation-control-patterns.md)
- [Padrão de texto de automação da interface do usuário](ui-automation-text-pattern.md)
- [Invocando um Controle Utilizando Automação de IU](invoke-a-control-using-ui-automation.md)
- [Obter o estado Toggle de uma caixa de seleção usando automação de interface do usuário](get-the-toggle-state-of-a-check-box-using-ui-automation.md)
- [Mapeamento de Padrão de Controles para Clientes de Automação de IU](control-pattern-mapping-for-ui-automation-clients.md)
- [Amostra de texto de inserção de TextPattern](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InsertText)
- [Exemplo de pesquisa e seleção de TextPattern](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/FindText)
- [Exemplo de InvokePattern, ExpandCollapsePattern e TogglePattern](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InvokePattern)
