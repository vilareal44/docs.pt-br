---
title: Como posicionar o cursor no início ou no término do texto em um controle TextBox
description: Saiba como posicionar o cursor no início ou no final do conteúdo de texto de um controle de caixa de texto Windows Presentation Foundation.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- positioning cursor [WPF]
- TextBox control [WPF], positioning cursor
- cursor [WPF], positioning
ms.assetid: c771a0b8-c6b4-4240-aecd-a21d0ba51a2e
ms.openlocfilehash: afe8b11d032b5d61fa91cbf324f02cd8415d2ea8
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167516"
---
# <a name="how-to-position-the-cursor-at-the-beginning-or-end-of-text-in-a-textbox-control"></a>Como posicionar o cursor no início ou no término do texto em um controle TextBox
Este exemplo mostra como posicionar o cursor no início ou no final do conteúdo de texto de um <xref:System.Windows.Controls.TextBox> controle.  
  
## <a name="example"></a>Exemplo  
 O código a seguir [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] descreve um <xref:System.Windows.Controls.TextBox> controle e atribui um nome a ele.  
  
 [!code-xaml[TextBox_MiscCode#_MoveCursorXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml#_movecursorxaml)]  
  
## <a name="example"></a>Exemplo  
 Para posicionar o cursor no início do conteúdo de um <xref:System.Windows.Controls.TextBox> controle, chame o <xref:System.Windows.Controls.TextBox.Select%2A> método e especifique a posição inicial da seleção de 0 e um comprimento de seleção de 0.  
  
 [!code-csharp[TextBox_MiscCode#_CursorToStart](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml.cs#_cursortostart)]
 [!code-vb[TextBox_MiscCode#_CursorToStart](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TextBox_MiscCode/VisualBasic/Window1.xaml.vb#_cursortostart)]  
  
## <a name="example"></a>Exemplo  
 Para posicionar o cursor no final do conteúdo de um <xref:System.Windows.Controls.TextBox> controle, chame o <xref:System.Windows.Controls.TextBox.Select%2A> método e especifique a posição inicial da seleção igual ao comprimento do conteúdo do texto e um comprimento de seleção de 0.  
  
 [!code-csharp[TextBox_MiscCode#_CursorToEnd](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml.cs#_cursortoend)]
 [!code-vb[TextBox_MiscCode#_CursorToEnd](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TextBox_MiscCode/VisualBasic/Window1.xaml.vb#_cursortoend)]  
  
## <a name="see-also"></a>Confira também

- [Visão geral de TextBox](textbox-overview.md)
- [Visão geral de RichTextBox](richtextbox-overview.md)
