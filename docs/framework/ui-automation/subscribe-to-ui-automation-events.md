---
title: Assinar eventos de automação de interface do usuário
description: Consulte como assinar eventos gerados por provedores de automação da interface do usuário. O código de exemplo registra um manipulador de eventos para o evento gerado quando um controle é invocado.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- UI Automation, subscribing to events
- subscribing to UI Automation events
- events, subscribing to
- listening for events
ms.assetid: b688effa-b3e8-4b05-944d-05ed89a245aa
ms.openlocfilehash: 8f456702657c70837c6137e3e60335110361bcd9
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163536"
---
# <a name="subscribe-to-ui-automation-events"></a>Assinar eventos de automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico mostra como assinar eventos gerados por provedores de automação da interface do usuário.  
  
## <a name="example"></a>Exemplo  
 O código de exemplo a seguir registra um manipulador de eventos para o evento que é gerado quando um controle como um botão é invocado e o Remove quando o formulário do aplicativo é fechado. O evento é identificado por um <xref:System.Windows.Automation.AutomationEvent> passado como parâmetro para <xref:System.Windows.Automation.Automation.AddAutomationEventHandler%2A> .  
  
 [!code-csharp[UIAClient_snip#101](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#101)]
 [!code-vb[UIAClient_snip#101](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#101)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] o para assinar um evento que é gerado quando o foco é alterado. O manipulador de eventos tem o registro cancelado em um método que pode ser chamado no desligamento do aplicativo ou quando a notificação de eventos da interface do usuário não é mais necessária.  
  
 [!code-csharp[UIAClient_snip#102](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#102)]
 [!code-vb[UIAClient_snip#102](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#102)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Automation.Automation.AddAutomationEventHandler%2A>
- <xref:System.Windows.Automation.Automation.RemoveAllEventHandlers%2A>
- <xref:System.Windows.Automation.Automation.RemoveAutomationEventHandler%2A>
- [Visão geral sobre eventos de automação de interface do usuário](ui-automation-events-overview.md)
