---
title: Percorrer texto usando automação de interface do usuário
description: Veja um exemplo de como percorrer o conteúdo de texto de um documento usando a automação da interface do usuário da Microsoft, em incrementos de TextUnit.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- UI Automation, traversing text
- text, traversing
- traversing text
ms.assetid: 3ddb3b7b-1d6b-4dba-8678-5a68e868aadb
ms.openlocfilehash: 0b4269d043fd6cd0cc5da9825714aab4ead701f9
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168083"
---
# <a name="traverse-text-using-ui-automation"></a>Percorrer texto usando automação de interface do usuário
> [!NOTE]
> Esta documentação destina-se a desenvolvedores do .NET Framework que querem usar as classes da [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] gerenciadas definidas no namespace <xref:System.Windows.Automation>. Para obter as informações mais recentes sobre a [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consulte [Windows Automation API: UI Automation](/windows/win32/winauto/entry-uiauto-win32) (API de Automação do Windows: Automação da Interface do Usuário).  
  
 Este tópico mostra como usar [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] o para percorrer o conteúdo textual de um documento por <xref:System.Windows.Automation.Text.TextUnit> incrementos.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como atravessar o conteúdo de um provedor de texto de automação de interface do usuário. O <xref:System.Windows.Automation.Text.TextPatternRange.Move%2A> método move os <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.Start> <xref:System.Windows.Automation.Text.TextPatternRangeEndpoint.End> pontos de extremidade e de um <xref:System.Windows.Automation.Text.TextPatternRange> . Normalmente, esse intervalo de texto é um intervalo de degeneramento que representa o ponto de inserção de texto.  
  
> [!NOTE]
> Como apenas objetos inseridos com base em texto são considerados parte do fluxo de texto, objetos incorporados, como imagens, não afetam `Move` ou seu valor de retorno.  
  
[!code-csharp[FindText#StartApp](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#startapp)]
[!code-vb[FindText#StartApp](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#startapp)]  
[!code-csharp[FindText#FindTextProvider](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#findtextprovider)]
[!code-vb[FindText#FindTextProvider](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#findtextprovider)]  
[!code-csharp[FindText#Navigate](../../../samples/snippets/csharp/VS_Snippets_Wpf/FindText/CSharp/SearchWindow.cs#navigate)]
[!code-vb[FindText#Navigate](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/FindText/VisualBasic/SearchWindow.vb#navigate)]  
  
 Qualquer método <xref:System.Windows.Automation.Text.TextUnit> que use será adiado para o próximo maior <xref:System.Windows.Automation.Text.TextUnit> suporte se o dado <xref:System.Windows.Automation.Text.TextUnit> não for suportado pelo controle.  
  
## <a name="see-also"></a>Confira também

- [Visão geral de TextPattern de automação da interface do usuário](ui-automation-textpattern-overview.md)
- [Acrescentar Conteúdo a um Text Box Utilizando Automação de IU](add-content-to-a-text-box-using-ui-automation.md)
- [Encontre e destaque texto usando automação de interface de usuário](find-and-highlight-text-using-ui-automation.md)
- [Visão Geral de Padrões de Controle de Automação de Interface de Usuário](ui-automation-control-patterns-overview.md)
- [Padrões de Controle para Clientes de Automação de IU](ui-automation-control-patterns-for-clients.md)
