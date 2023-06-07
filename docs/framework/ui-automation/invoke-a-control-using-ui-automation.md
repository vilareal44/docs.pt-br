---
title: Invocando um Controle Utilizando Automação de IU
description: Use a automação da interface do usuário para localizar um controle que corresponda a determinadas condições de propriedade, crie um AutomationElement, obtenha um InvokePattern e use Invoke no controle.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- invoking controls
- UI Automation, invoking controls
- controls, invoking
ms.assetid: 5ee2de3f-256c-43ec-b64c-62ace91f9983
ms.openlocfilehash: 2347a620aab848bf6bcc649a9780aa5a3a520822
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168172"
---
# <a name="invoke-a-control-using-ui-automation"></a>Invocando um Controle Utilizando Automação de IU
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico demonstra como executar as seguintes tarefas:  
  
- Localize um controle que corresponda a condições de propriedade específicas percorrendo a exibição de controle da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] árvore para o aplicativo de destino.  
  
- Crie um <xref:System.Windows.Automation.AutomationElement> para cada controle.  
  
- Obtenha um <xref:System.Windows.Automation.InvokePattern> objeto de qualquer [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] elemento encontrado que dê suporte ao <xref:System.Windows.Automation.InvokePattern> padrão de controle.  
  
- Use <xref:System.Windows.Automation.InvokePattern.Invoke%2A> para invocar o controle de um manipulador de eventos de cliente.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa o <xref:System.Windows.Automation.AutomationElement.TryGetCurrentPattern%2A> método da <xref:System.Windows.Automation.AutomationElement> classe para gerar um <xref:System.Windows.Automation.InvokePattern> objeto e invocar um controle usando o <xref:System.Windows.Automation.InvokePattern.Invoke%2A> método.  
  
 [!code-csharp[InvokePatternApp#1100](../../../samples/snippets/csharp/VS_Snippets_Wpf/InvokePatternApp/CSharp/InvokePatternApp.cs#1100)]
 [!code-vb[InvokePatternApp#1100](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/InvokePatternApp/VisualBasic/Client.vb#1100)]  
[!code-csharp[InvokePatternApp#1102](../../../samples/snippets/csharp/VS_Snippets_Wpf/InvokePatternApp/CSharp/InvokePatternApp.cs#1102)]
[!code-vb[InvokePatternApp#1102](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/InvokePatternApp/VisualBasic/Client.vb#1102)]  
  
## <a name="see-also"></a>Confira também

- [Exemplo de InvokePattern, ExpandCollapsePattern e TogglePattern](https://github.com/Microsoft/WPF-Samples/tree/master/Accessibility/InvokePattern)
