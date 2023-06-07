---
title: Como desenhar um retângulo preenchido em um formulário do Windows Forms
description: Saiba como desenhar programaticamente um retângulo preenchido em um formulário do Windows. Além disso, saiba como compilar seu código.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
f1_keywords:
- Graphics.FillRectangle
helpviewer_keywords:
- drawing [Windows Forms], rectangles
- rectangles [Windows Forms], drawing
- drawing rectangles
ms.assetid: d656a93c-987d-4809-aafd-493fe17450f0
ms.openlocfilehash: 0ad8ec97000e29b2194a9eda713aa43d5557b44c
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621633"
---
# <a name="how-to-draw-a-filled-rectangle-on-a-windows-form"></a>Como desenhar um retângulo preenchido em um formulário do Windows Forms
Este exemplo desenha um retângulo preenchido em um formulário.  
  
## <a name="example"></a>Exemplo  
 [!code-cpp[System.Drawing.ConceptualHowTos#2](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Drawing.ConceptualHowTos/cpp/form1.cpp#2)]
 [!code-csharp[System.Drawing.ConceptualHowTos#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.ConceptualHowTos/CS/form1.cs#2)]
 [!code-vb[System.Drawing.ConceptualHowTos#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.ConceptualHowTos/VB/form1.vb#2)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Você não pode chamar esse método no <xref:System.Windows.Forms.Form.Load> manipulador de eventos. O conteúdo desenhado não será redesenhado se o formulário for redimensionado ou obscurecido por outro formulário. Para tornar seu conteúdo redesenhado automaticamente, você deve substituir o <xref:System.Windows.Forms.Control.OnPaint%2A> método.  
  
## <a name="robust-programming"></a>Programação robusta  
 Você sempre deve chamar <xref:System.IDisposable.Dispose%2A> todos os objetos que consomem recursos do sistema, como <xref:System.Drawing.Brush> <xref:System.Drawing.Graphics> objetos e.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Drawing.Graphics.FillRectangle%2A>
- <xref:System.Windows.Forms.Control.OnPaint%2A>
- [Introdução à Programação de Elementos Gráficos](getting-started-with-graphics-programming.md)
- [Elementos gráficos e desenho no Windows Forms](graphics-and-drawing-in-windows-forms.md)
- [Uso de uma caneta para desenhar linhas e formas](using-a-pen-to-draw-lines-and-shapes.md)
- [Pincéis e Formas Preenchidas no GDI+](brushes-and-filled-shapes-in-gdi.md)
