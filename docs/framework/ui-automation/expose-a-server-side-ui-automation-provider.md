---
title: Expor um provedor de automação de interface do usuário do lado do servidor
description: Exiba um exemplo que mostra como expor um provedor de automação de interface do usuário do lado do servidor que está hospedado em uma janela System. Windows. Forms. Control.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- expose a server-side UI Automation provider
- UI Automation, server-side provider, exposing
- server-side UI Automation provider, exposing
ms.assetid: 55d419c0-2201-4101-90c9-2888df4dbb47
ms.openlocfilehash: 66380c31da45b23d24b14154aea9770c6369aaf2
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168450"
---
# <a name="expose-a-server-side-ui-automation-provider"></a>Expor um provedor de automação de interface do usuário do lado do servidor
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico contém um código de exemplo que mostra como expor um provedor de automação de interface do usuário do lado do servidor que é hospedado em uma <xref:System.Windows.Forms.Control?displayProperty=nameWithType> janela.  
  
 O exemplo substitui o procedimento de janela para interceptar WM_GETOBJECT, que é a mensagem enviada pelo [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] serviço principal quando um aplicativo cliente solicita informações sobre a janela.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[UIAFragmentProvider_snip#116](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAFragmentProvider_snip/CSharp/ListFragment.cs#116)]
 [!code-vb[UIAFragmentProvider_snip#116](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAFragmentProvider_snip/VisualBasic/ListFragment.vb#116)]  
  
## <a name="see-also"></a>Confira também

- [Visão Geral dos Provedores de Automação de Interface do Usuário](ui-automation-providers-overview.md)
- [Implementação do provedor de automação de interface do usuário no lado do servidor](server-side-ui-automation-provider-implementation.md)
