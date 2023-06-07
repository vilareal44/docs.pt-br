---
title: Encontrar um Elemento de Automação de Interface de Usuário para um Item de Lista
description: Veja um exemplo que mostra como localizar um elemento de automação da interface do usuário para um item de lista quando o índice do item é conhecido.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- list items, finding elements for
- elements, finding for list items
- UI Automation, finding elements for List items
ms.assetid: c326ad2b-2144-4f64-ae4c-d850c74f95c5
ms.openlocfilehash: ec6464bc0ec504fd34ed113c9bed1a54a7d4eaec
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168414"
---
# <a name="find-a-ui-automation-element-for-a-list-item"></a>Encontrar um Elemento de Automação de Interface de Usuário para um Item de Lista
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico mostra como recuperar um <xref:System.Windows.Automation.AutomationElement> para um item dentro de uma lista quando o índice do item é conhecido.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra duas maneiras de recuperar um item especificado de uma lista, um usando <xref:System.Windows.Automation.TreeWalker> o e o outro usando <xref:System.Windows.Automation.AutomationElement.FindAll%2A> .  
  
 A primeira técnica tende a ser mais rápida para controles do Win32, mas a segunda é mais rápida para controles de Windows Presentation Foundation (WPF).  
  
 [!code-csharp[UIAClient_snip#184](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#184)]
 [!code-vb[UIAClient_snip#184](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#184)]  
  
## <a name="see-also"></a>Confira também

- [Obtendo elementos da automação interface do usuário](obtaining-ui-automation-elements.md)
