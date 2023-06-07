---
title: Obter Padrões de Controle de Automação de IU Suportados
description: Leia um exemplo que mostra como recuperar objetos de padrão de controle com suporte de elementos de automação da interface do usuário.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- control patterns, getting
- UI Automation, getting control patterns
- getting, control patterns
ms.assetid: 006c54c9-50bf-48d9-a855-9d62eb95603a
ms.openlocfilehash: f2905b81a1af2f86c78b082f0241e2181c384d25
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164162"
---
# <a name="get-supported-ui-automation-control-patterns"></a>Obter Padrões de Controle de Automação de IU Suportados
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico mostra como recuperar objetos de padrão de controle de [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] elementos.  
  
### <a name="obtain-all-control-patterns"></a>Obter todos os padrões de controle  
  
1. Obtenha os <xref:System.Windows.Automation.AutomationElement> padrões de controle cujos quais você está interessado.  
  
2. Chame <xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A> para obter todos os padrões de controle do elemento.  
  
> [!CAUTION]
> É altamente recomendável que um cliente não use <xref:System.Windows.Automation.AutomationElement.GetSupportedPatterns%2A> . O desempenho pode ser seriamente afetado, pois esse método chama <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> internamente para cada padrão de controle existente. Se possível, um cliente deve chamar <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> para os principais padrões de interesse.  
  
### <a name="obtain-a-specific-control-pattern"></a>Obter um padrão de controle específico  
  
1. Obtenha os <xref:System.Windows.Automation.AutomationElement> padrões de controle cujos quais você está interessado.  
  
2. Chame <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> ou <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A> para consultar um padrão específico. Esses métodos são semelhantes, mas se o padrão não for encontrado, o <xref:System.Windows.Automation.AutomationElement.GetCurrentPattern%2A> gerará uma exceção e <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A> retornará `false` .  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir recupera um <xref:System.Windows.Automation.AutomationElement> para um item de lista e Obtém um <xref:System.Windows.Automation.SelectionItemPattern> desse elemento.  
  
 [!code-csharp[UIAClient_snip#103](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#103)]
 [!code-vb[UIAClient_snip#103](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#103)]  
  
## <a name="see-also"></a>Confira também

- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
