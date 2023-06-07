---
title: Obter detalhes de atributos de texto misto usando automação de interface do usuário
description: Obtenha detalhes de atributo de texto misto usando as classes de automação da interface do usuário gerenciada no namespace System. Windows. Automation da API do .NET.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d0e4c005-abd1-42bb-92a4-5faf87097311
ms.openlocfilehash: 111d110be9365c4a58f2bd2b033c1ff4e3a6a95d
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84903851"
---
# <a name="obtain-mixed-text-attribute-details-using-ui-automation"></a>Obter detalhes de atributos de texto misto usando automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico mostra como usar o [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] para obter detalhes de atributo de texto de um intervalo de texto que abrange vários valores de atributo. Um intervalo de texto pode corresponder ao local atual do cursor (ou degenerar a seleção) dentro de um documento, uma seleção de texto contígua, uma coleção de seleções de texto não contíguos ou todo o conteúdo textual de um documento.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como obter o <xref:System.Windows.Automation.TextPattern.FontNameAttribute> de um intervalo de texto em que <xref:System.Windows.Automation.Text.TextPatternRange.GetAttributeValue%2A> retorna um <xref:System.Windows.Automation.TextPattern.MixedAttributeValue> objeto.  
  
[!code-csharp[FindText#RetrieveMixedAttributes](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#retrievemixedattributes)]
[!code-vb[FindText#RetrieveMixedAttributes](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#retrievemixedattributes)]  
  
 O <xref:System.Windows.Automation.TextPattern> padrão de controle, em conjunto com a <xref:System.Windows.Automation.Text.TextPatternRange> classe, dá suporte a atributos, propriedades e métodos de texto básico. Para funcionalidade específica de controle que não é suportada pelo <xref:System.Windows.Automation.TextPattern> ou <xref:System.Windows.Automation.Text.TextPatternRange> , a <xref:System.Windows.Automation.AutomationElement> classe fornece métodos para um cliente de automação da interface do usuário acessar o modelo de objeto nativo correspondente.  
  
## <a name="see-also"></a>Veja também

- [Visão geral de TextPattern de automação da interface do usuário](ui-automation-textpattern-overview.md)
- [Acrescentar Conteúdo a um Text Box Utilizando Automação de IU](add-content-to-a-text-box-using-ui-automation.md)
- [Encontre e destaque texto usando automação de interface de usuário](find-and-highlight-text-using-ui-automation.md)
- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de controle de automação de interface do usuário para clientes](ui-automation-control-patterns-for-clients.md)
- [Obter atributos de texto usando automação de interface do usuário](obtain-text-attributes-using-ui-automation.md)
