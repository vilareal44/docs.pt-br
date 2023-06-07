---
title: Mapeamento de Padrão de Controles para Clientes de Automação de IU
description: Exiba uma tabela de mapeamento de padrão de controle para clientes de automação da interface do usuário. As ações para certos tipos de controle podem ter suporte, com suporte condicional ou sem suporte.
ms.date: 03/30/2017
helpviewer_keywords:
- control patterns, for UI Automation clients
- UI Automation, clients, control patterns for
ms.assetid: 8b81645b-8be3-4e26-9c98-4fb0fceca06b
ms.openlocfilehash: 7673ce4ac88cc36a7c35e2e946a31d23b2ce6eca
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164182"
---
# <a name="control-pattern-mapping-for-ui-automation-clients"></a>Mapeamento de Padrão de Controles para Clientes de Automação de IU
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico lista os tipos de controle e seus padrões de controle associados.  
  
 A tabela a seguir organiza os padrões de controle nas seguintes categorias:  
  
- Com suporte. O controle deve dar suporte a esse padrão de controle.  
  
- Suporte condicional. O controle pode dar suporte a esse padrão de controle dependendo do estado do controle.  
  
- Não há suporte. O controle não oferece suporte a este padrão de controle; os controles personalizados podem dar suporte a esse padrão de controle.  
  
> [!NOTE]
> Alguns controles têm suporte condicional para vários padrões de controle, dependendo da funcionalidade do controle. Por exemplo, o controle de item de menu tem suporte condicional para o <xref:System.Windows.Automation.InvokePattern> padrão de controle,, <xref:System.Windows.Automation.ExpandCollapsePattern> <xref:System.Windows.Automation.TogglePattern> ou <xref:System.Windows.Automation.SelectionItemPattern> , dependendo de sua função no controle de menu.  
  
<a name="control_mapping_clients"></a>
## <a name="ui-automation-control-patterns-for-clients"></a>Padrões de Controle para Clientes de Automação de IU  
  
|Tipo de controle|Com suporte|Suporte condicional|Sem suporte|  
|------------------|---------------|-------------------------|-------------------|  
|Botão|Nenhum|Invocar, ativar/desativar, expandir recolher|Nenhum|  
|Calendário|Grade, tabela|Seleção, rolar|Valor|  
|Caixa de seleção|Alternar|Nenhum|Nenhum|  
|Caixa de Combinação|Expandir recolher|Seleção, valor|Rolar|  
|Grade de dados|Grade|Rolar, selecionar, tabela|Nenhum|  
|Item de Dados|Item de Seleção|Expandir recolhimento, item de grade, item de rolagem, tabela, alternância, valor|Nenhum|  
|Document|Texto|Rolar, valor|Nenhum|  
|Editar|Nenhum|Texto, valor do intervalo, valor|Nenhum|  
|Agrupar|Nenhum|Expandir recolher|Nenhum|  
|Cabeçalho|Nenhum|Transformar|Nenhum|  
|Item de Cabeçalho|Nenhum|Transformar, invocar|Nenhum|  
|Hyperlink|Invoke|Valor|Nenhum|  
|Image|Nenhum|Item de grade, item de tabela|Invocar, item de seleção|  
|Lista|Nenhum|Grade, exibição múltipla, rolagem, seleção|Tabela|  
|Item de lista|Item de Seleção|Expandir recolhimento, item de grade, invocar, rolar item, alternar, valor|Nenhum|  
|Menu|Nenhum|Nenhum|Nenhum|  
|Barra de menu|Nenhum|Expandir recolher, encaixar, transformar|Nenhum|  
|Item de menu|Nenhum|Expandir recolher, invocar, selecionar item, alternar|Nenhum|  
|Painel|Nenhum|Ancorá. Rolar, transformar|Janela|  
|Barra de Andamento|Nenhum|Valor do intervalo, valor|Nenhum|  
|Botão de Opção|Item de Seleção|Nenhum|Alternar|  
|Barra de Rolagem|Nenhum|Valor de Intervalo|Rolar|  
|Separador|Nenhum|Nenhum|Nenhum|  
|Controle deslizante|Nenhum|Valor de intervalo, seleção, valor|Nenhum|  
|Controle giratório|Nenhum|Valor de intervalo, seleção, valor|Nenhum|  
|Botão de Divisão|Invocar, expandir recolher|Nenhum|Nenhum|  
|Barra de Status|Nenhum|Grade|Nenhum|  
|Tab|Seleção|Rolar|Nenhum|  
|Item da Guia|Item de Seleção|Nenhum|Invoke|  
|Tabela|Grade, item de grade, tabela, item de tabela|Nenhum|Nenhum|  
|Texto|Nenhum|Item de grade, item de tabela, texto|Valor|  
|Posição|Transformar|Nenhum|Nenhum|  
|Barra de Título|Nenhum|Nenhum|Nenhum|  
|Barra de ferramentas|Nenhum|Encaixar, expandir, recolher, transformar|Nenhum|  
|Dica de ferramenta|Nenhum|Texto, janela|Nenhum|  
|Árvore|Nenhum|Rolar, seleção|Nenhum|  
|Item de Árvore|Expandir recolher|Invocar, rolar item, item de seleção, alternar|Nenhum|  
|Janela|Transformação, janela|Dock|Nenhum|  
  
> [!NOTE]
> Se um tipo de controle não tiver padrões de controle com suporte listados, mas tiver um ou mais padrões de controle com suporte condicionalmente, um desses padrões de controle condicional terá suporte em todos os momentos.  
  
## <a name="see-also"></a>Confira também

- [Visão geral de automação da interface do usuário](ui-automation-overview.md)
