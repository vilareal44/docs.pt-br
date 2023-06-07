---
title: Como desenhar uma linha em um formulário do Windows Forms
description: Saiba como desenhar uma linha em um formulário manipulando o evento de pintura e, em seguida, executar o desenho usando a Propriedade Graphics de PaintEventArgs.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
f1_keywords:
- Graphics.DrawLine
helpviewer_keywords:
- examples [Windows Forms], drawing lines on forms
- drawing [Windows Forms], lines
- lines [Windows Forms], drawing
- drawing lines
ms.assetid: 55c1dbeb-75d0-430c-9814-a24b8971ad8c
ms.openlocfilehash: c8e92dc265c63413275561d0e2e3aa820eaec724
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621620"
---
# <a name="how-to-draw-a-line-on-a-windows-form"></a>Como desenhar uma linha em um formulário do Windows Forms
Este exemplo desenha uma linha em um formulário. Normalmente, quando você desenha em um formulário, manipula o evento do formulário <xref:System.Windows.Forms.Control.Paint> e executa o desenho usando a <xref:System.Windows.Forms.PaintEventArgs.Graphics%2A> Propriedade do <xref:System.Windows.Forms.PaintEventArgs> , conforme mostrado neste exemplo  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[System.Drawing.UsingAPen#11](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.UsingAPen/CS/Class1.cs#11)]
 [!code-vb[System.Drawing.UsingAPen#11](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.UsingAPen/VB/Class1.vb#11)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 O exemplo anterior foi projetado para uso com Windows Forms e requer <xref:System.Windows.Forms.PaintEventArgs> `e` , que é um parâmetro do <xref:System.Windows.Forms.Control.Paint> manipulador de eventos.  
  
## <a name="robust-programming"></a>Programação robusta  
 Você sempre deve chamar <xref:System.IDisposable.Dispose%2A> todos os objetos que consomem recursos do sistema, como <xref:System.Drawing.Pen> objetos.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Drawing.Graphics.DrawLine%2A>
- <xref:System.Windows.Forms.Control.OnPaint%2A>
- [Introdução à Programação de Elementos Gráficos](getting-started-with-graphics-programming.md)
- [Uso de uma caneta para desenhar linhas e formas](using-a-pen-to-draw-lines-and-shapes.md)
- [Elementos gráficos e desenho no Windows Forms](graphics-and-drawing-in-windows-forms.md)
