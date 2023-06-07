---
title: Como definir foco em um controle TextBox
description: Saiba como usar o método Focus para definir o foco em um controle TextBox Windows Presentation Foundation.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- focus [WPF], setting
- TextBox control [WPF], setting focus
ms.assetid: 24b61b45-dc2d-425e-9839-b017af7ab86f
ms.openlocfilehash: e107c22a3c5b701e02cbc8506d10fa35ee15c79d
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168062"
---
# <a name="how-to-set-focus-in-a-textbox-control"></a>Como definir foco em um controle TextBox
Este exemplo mostra como usar o <xref:System.Windows.UIElement.Focus%2A> método para definir o foco em um <xref:System.Windows.Controls.TextBox> controle.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] descreve um <xref:System.Windows.Controls.TextBox> controle simples chamado *tbFocusMe*  
  
 [!code-xaml[TextBox_MiscCode#_TextBoxFocusXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml#_textboxfocusxaml)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir chama o <xref:System.Windows.UIElement.Focus%2A> método para definir o foco no <xref:System.Windows.Controls.TextBox> controle com o nome *tbFocusMe*.  
  
 [!code-csharp[TextBox_MiscCode#_FocusTextBox](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml.cs#_focustextbox)]
 [!code-vb[TextBox_MiscCode#_FocusTextBox](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TextBox_MiscCode/VisualBasic/Window1.xaml.vb#_focustextbox)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.UIElement.Focusable%2A>
- <xref:System.Windows.UIElement.IsFocused%2A>
- [Visão geral de TextBox](textbox-overview.md)
- [Visão geral de RichTextBox](richtextbox-overview.md)
