---
title: Obter propriedades de elemento da automação de interface do usuário
description: Consulte as instruções e um exemplo que mostra como recuperar as propriedades atuais ou armazenadas em cache de um elemento de automação da interface do usuário.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- properties, retrieving
- UI Automation, retrieving properties of elements
ms.assetid: 09576b1a-291f-435c-980e-dee32d899ae1
ms.openlocfilehash: 277822c9d89046bfbad50df16bce83da7dd45b3b
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164100"
---
# <a name="get-ui-automation-element-properties"></a>Obter propriedades de elemento da automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico mostra como recuperar propriedades de um [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] elemento.  
  
### <a name="get-a-current-property-value"></a>Obter um valor da propriedade atual  
  
1. Obtenha o <xref:System.Windows.Automation.AutomationElement> cuja propriedade você deseja obter.  
  
2. Chame <xref:System.Windows.Automation.AutomationElement.GetCurrentPropertyValue%2A> ou recupere a <xref:System.Windows.Automation.AutomationElement.Current%2A> estrutura de propriedade e obtenha o valor de um de seus membros.  
  
### <a name="get-a-cached-property-value"></a>Obter um valor de propriedade em cache  
  
1. Obtenha o <xref:System.Windows.Automation.AutomationElement> cuja propriedade você deseja obter. A propriedade deve ter sido especificada no <xref:System.Windows.Automation.CacheRequest> .  
  
2. Chame <xref:System.Windows.Automation.AutomationElement.GetCachedPropertyValue%2A> ou recupere a <xref:System.Windows.Automation.AutomationElement.Cached%2A> estrutura de propriedade e obtenha o valor de um de seus membros.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra várias maneiras de recuperar as propriedades atuais de um <xref:System.Windows.Automation.AutomationElement> .  
  
 [!code-csharp[UIAClient_snip#170](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#170)]
 [!code-vb[UIAClient_snip#170](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#170)]  
  
## <a name="see-also"></a>Confira também

- [Automação de Propriedades de Interface de Usuário para Clientes.](ui-automation-properties-for-clients.md)
- [Usar armazenamento em cache em automação de interface do usuário](use-caching-in-ui-automation.md)
- [Armazenando em cache em clientes de automação de interface do usuário](caching-in-ui-automation-clients.md)
