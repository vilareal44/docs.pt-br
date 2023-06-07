---
title: Implementar provedores de automação de interface do usuário em um aplicativo cliente
description: Consulte um exemplo de como implementar um provedor de automação de interface do usuário no lado do cliente em um aplicativo. Observe que esse é um cenário incomum.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- client-side UI Automation provider, implementation within applications
- UI Automation, implementing client-side provider within application
ms.assetid: f325f0d8-1715-41ea-85ca-45b82ffea8bc
ms.openlocfilehash: c604b68021886abdf06360bfb8afefe3640c12fe
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164117"
---
# <a name="implement-ui-automation-providers-in-a-client-application"></a>Implementar provedores de automação de interface do usuário em um aplicativo cliente
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico contém um código de exemplo que mostra como implementar um provedor de automação de interface do usuário no lado do cliente em um aplicativo.  
  
 Esse é um cenário incomum. Geralmente, um aplicativo cliente de automação de interface do usuário usa provedores do lado do servidor ou provedores do lado do cliente que residem em uma DLL.  
  
## <a name="example"></a>Exemplo  
 O código de exemplo a seguir implementa um provedor simples para uma janela de console. O código não tem nenhuma funcionalidade útil, mas destina-se a demonstrar as etapas básicas na configuração de um provedor dentro do código do cliente e seu registro usando o <xref:System.Windows.Automation.ClientSettings.RegisterClientSideProviders%2A> .  
  
 [!code-csharp[UIAClientSideProvider_snip#201](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClientSideProvider_snip/CSharp/ClientImplementationProgram.cs#201)]
 [!code-vb[UIAClientSideProvider_snip#201](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClientSideProvider_snip/visualbasic/clientimplementationprogram.vb#201)]  
  
## <a name="see-also"></a>Confira também

- [Visão Geral dos Provedores de Automação de Interface do Usuário](ui-automation-providers-overview.md)
- [Registrar um Módulo Provedor do Lado do Cliente](register-a-client-side-provider-assembly.md)
- [Criar um Provedor de Automação de Interface de Usuário do Lado do Cliente](create-a-client-side-ui-automation-provider.md)
- [Implementação de Provedor de Automação de Interface de Usuário do Lado do Cliente](client-side-ui-automation-provider-implementation.md)
