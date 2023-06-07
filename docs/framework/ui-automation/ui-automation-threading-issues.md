---
title: Problemas de Threading na Automação da Interface do Usuário
description: Leia sobre problemas com Threading de automação da interface do usuário. Por exemplo, os conflitos podem ocorrer se um aplicativo cliente tentar interagir com sua própria interface do usuário no thread da interface.
ms.date: 03/30/2017
helpviewer_keywords:
- UI Automation, threading issues
- threading issues with UI Automation
ms.assetid: 0ab8d42c-5b8b-481b-b788-2caecc2f0191
ms.openlocfilehash: 290c26981d5eb993e2a9ab387f8d106aafa54efb
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86924533"
---
# <a name="ui-automation-threading-issues"></a>Problemas de Threading na Automação da Interface do Usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Devido à maneira como o [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] usa mensagens do Windows, os conflitos podem ocorrer quando um aplicativo cliente tenta interagir com [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] o seu próprio no [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] thread. Esses conflitos podem levar a um desempenho muito lento ou até mesmo fazer com que o aplicativo pare de responder.  
  
 Se o seu aplicativo cliente se destina a interagir com todos os elementos na área de trabalho, incluindo seu próprio [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] , você deve fazer todas as [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] chamadas em um thread separado. Isso inclui a localização de elementos (por exemplo, usando <xref:System.Windows.Automation.TreeWalker> o ou o <xref:System.Windows.Automation.AutomationElement.FindAll%2A> método) e o uso de padrões de controle.  
  
 É seguro fazer [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] chamadas dentro de um [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] manipulador de eventos, pois o manipulador de eventos é sempre chamado em um não [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] thread. No entanto, ao assinar eventos que podem se originar de seus aplicativos cliente [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] , você deve fazer a chamada para <xref:System.Windows.Automation.Automation.AddAutomationEventHandler%2A> , ou um método relacionado, em um não [!INCLUDE[TLA2#tla_ui](../../../includes/tla2sharptla-ui-md.md)] thread. Remova os manipuladores de eventos no mesmo thread.
