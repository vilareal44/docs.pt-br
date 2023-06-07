---
title: Obter atributos de texto usando automação de interface do usuário
description: Saiba como obter atributos de texto usando a automação da interface do usuário. Consulte um exemplo de código que obtém atributos de texto de um intervalo de texto.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- getting, text attributes
- UI Automation, getting text attributes
- text attributes, getting
ms.assetid: fdefc6c3-b836-4cfe-8dec-1484bfdc5551
ms.openlocfilehash: b48f773e27088ba4b33ad01b299d77a0992a9159
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168010"
---
# <a name="obtain-text-attributes-using-ui-automation"></a>Obter atributos de texto usando automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico mostra como usar o [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] para obter atributos de texto de um intervalo de texto. Um intervalo de texto pode corresponder ao local atual do cursor (ou degenerar a seleção) dentro de um documento, uma seleção de texto contígua, uma coleção de seleções de texto não contíguos ou todo o conteúdo textual de um documento.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como obter o <xref:System.Windows.Automation.TextPattern.FontNameAttribute> de um intervalo de texto.  
  
 [!code-csharp[UIATextPattern_snip#StartTarget](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIATextPattern_snip/CSharp/SearchWindow.cs#starttarget)]
 [!code-vb[UIATextPattern_snip#StartTarget](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIATextPattern_snip/VisualBasic/SearchWindow.vb#starttarget)]  
[!code-csharp[UIATextPattern_snip#GetTextElement](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIATextPattern_snip/CSharp/SearchWindow.cs#gettextelement)]
[!code-vb[UIATextPattern_snip#GetTextElement](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIATextPattern_snip/VisualBasic/SearchWindow.vb#gettextelement)]  
[!code-csharp[UIATextPattern_snip#FontName](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIATextPattern_snip/CSharp/SearchWindow.cs#fontname)]
[!code-vb[UIATextPattern_snip#FontName](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIATextPattern_snip/VisualBasic/SearchWindow.vb#fontname)]  
  
 O <xref:System.Windows.Automation.TextPattern> padrão de controle, em conjunto com a <xref:System.Windows.Automation.Text.TextPatternRange> classe, dá suporte a atributos, propriedades e métodos de texto básico. Para funcionalidade específica de controle que não tem suporte no <xref:System.Windows.Automation.TextPattern> ou <xref:System.Windows.Automation.Text.TextPatternRange> na <xref:System.Windows.Automation.AutomationElement> , a classe fornece métodos para um cliente de automação de interface do usuário acessar o modelo de objeto nativo correspondente.  
  
## <a name="see-also"></a>Confira também

- [Visão geral de TextPattern de automação da interface do usuário](ui-automation-textpattern-overview.md)
- [Acrescentar Conteúdo a um Text Box Utilizando Automação de IU](add-content-to-a-text-box-using-ui-automation.md)
- [Encontre e destaque texto usando automação de interface de usuário](find-and-highlight-text-using-ui-automation.md)
- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
- [Obter detalhes de atributos de texto misto usando automação de interface do usuário](obtain-mixed-text-attribute-details-using-ui-automation.md)
