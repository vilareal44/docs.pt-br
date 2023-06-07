---
title: Como alinhar horizontal e verticalmente o conteúdo em um StackPanel
description: Saiba como ajustar a orientação do conteúdo em um Windows Presentation Foundation StackPanel e o HorizontalAlignment e o VerticalAlignment de conteúdo filho.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- StackPanel control [WPF], content alignment [WPF]
- content alignment [WPF]
- aligning [WPF], content
ms.assetid: c1e8f962-72c8-4e7a-8670-7a2d7e021791
ms.openlocfilehash: d3c7215d8193d1d8a72597c44cf265f5bd400ea1
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167626"
---
# <a name="how-to-horizontally-or-vertically-align-content-in-a-stackpanel"></a>Como alinhar horizontal e verticalmente o conteúdo em um StackPanel
Este exemplo mostra como ajustar o <xref:System.Windows.Controls.StackPanel.Orientation%2A> conteúdo dentro de um <xref:System.Windows.Controls.StackPanel> elemento e também como ajustar o <xref:System.Windows.FrameworkElement.HorizontalAlignment%2A> e o <xref:System.Windows.FrameworkElement.VerticalAlignment%2A> conteúdo filho.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria três <xref:System.Windows.Controls.ListBox> elementos no [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] . Cada <xref:System.Windows.Controls.ListBox> um representa os valores possíveis das <xref:System.Windows.Controls.StackPanel.Orientation%2A> <xref:System.Windows.FrameworkElement.HorizontalAlignment%2A> Propriedades, e <xref:System.Windows.FrameworkElement.VerticalAlignment%2A> de a <xref:System.Windows.Controls.StackPanel> . Quando um usuário seleciona um valor em qualquer um dos <xref:System.Windows.Controls.ListBox> elementos, a propriedade associada do <xref:System.Windows.Controls.StackPanel> e seus elementos filho são <xref:System.Windows.Controls.Button> alterados.  
  
 [!code-xaml[StackPanelIntroSamp#1](~/samples/snippets/csharp/VS_Snippets_Wpf/StackPanelIntroSamp/CSharp/Window1.xaml#1)]  
  
 O arquivo code-behind a seguir define as alterações nos eventos associados às <xref:System.Windows.Controls.ListBox> alterações de seleção. <xref:System.Windows.Controls.StackPanel>é identificado pelo <xref:System.Windows.FrameworkElement.Name%2A> `sp1` .  
  
 [!code-csharp[StackPanelIntroSamp#2](~/samples/snippets/csharp/VS_Snippets_Wpf/StackPanelIntroSamp/CSharp/Window1.xaml.cs#2)]
 [!code-vb[StackPanelIntroSamp#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/StackPanelIntroSamp/VisualBasic/Window1.xaml.vb#2)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.StackPanel>
- <xref:System.Windows.Controls.ListBox>
- <xref:System.Windows.FrameworkElement.HorizontalAlignment%2A>
- <xref:System.Windows.FrameworkElement.VerticalAlignment%2A>
- [Visão geral de painéis](panels-overview.md)
