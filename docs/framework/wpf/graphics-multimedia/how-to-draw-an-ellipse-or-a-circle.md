---
title: Como desenhar uma elipse ou um círculo
description: Saiba como desenhar uma elipse ou um círculo com opções de espessura da estrutura de tópicos e cor interior no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
helpviewer_keywords:
- ellipses [WPF], drawing
- circles [WPF], drawing
- drawing circles [WPF]
- drawing ellipses [WPF]
- graphics [WPF], drawing circles
- graphics [WPF], drawing ellipses
ms.assetid: 99763b8c-bfc8-44be-8231-8a70daf5481a
ms.openlocfilehash: afa0e7d2af42aaee39dca164f23b1a1cacf430ca
ms.sourcegitcommit: b6a1869f97a37f11a68c90afde1a520a6887dcbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853564"
---
# <a name="how-to-draw-an-ellipse-or-a-circle"></a>Como desenhar uma elipse ou um círculo
Este exemplo mostra como desenhar reticências e círculos usando o <xref:System.Windows.Shapes.Ellipse> elemento. Para desenhar uma elipse, crie um <xref:System.Windows.Shapes.Ellipse> elemento e especifique seu <xref:System.Windows.FrameworkElement.Width%2A> e <xref:System.Windows.FrameworkElement.Height%2A> . Use sua <xref:System.Windows.Shapes.Shape.Fill%2A> propriedade para especificar o <xref:System.Windows.Media.Brush> que é usado para pintar o interior da elipse. Use sua <xref:System.Windows.Shapes.Shape.Stroke%2A> propriedade para especificar o <xref:System.Windows.Media.Brush> que é usado para pintar o contorno da elipse. A <xref:System.Windows.Shapes.Shape.StrokeThickness%2A> propriedade especifica a espessura da estrutura de tópicos da elipse.  
  
 Para desenhar um círculo, torne o <xref:System.Windows.FrameworkElement.Width%2A> e <xref:System.Windows.FrameworkElement.Height%2A> do <xref:System.Windows.Shapes.Ellipse> elemento igual um do outro.  
  
 O exemplo a seguir desenha quatro <xref:System.Windows.Shapes.Ellipse> elementos em um <xref:System.Windows.Controls.Canvas> .  
  
## <a name="example"></a>Exemplo  
 [!code-xaml[drawingwithshapeelements#EllipseExample1](~/samples/snippets/csharp/VS_Snippets_Wpf/DrawingWithShapeElements/CS/ellipseexample.xaml#ellipseexample1)]  
  
 Embora este exemplo use um <xref:System.Windows.Controls.Canvas> para conter as reticências, você pode usar elementos Ellipse (e todos os outros elementos Shape) com Any <xref:System.Windows.Controls.Panel> ou <xref:System.Windows.Controls.Control> que ofereça suporte a conteúdo que não seja de texto.  
  
 Este exemplo faz parte de um exemplo maior; para ver o exemplo completo, consulte o [Exemplo de elementos de forma](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/ShapeElements).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Shapes.Ellipse>
- <xref:System.Windows.Shapes.Shape>
- [Exemplo de elementos de forma](https://github.com/Microsoft/WPF-Samples/tree/master/Graphics/ShapeElements)
