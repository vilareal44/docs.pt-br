---
title: Como criar um controle TextBox multilinha
description: Saiba como usar o XAML para definir um controle TextBox que se expande para acomodar várias linhas de texto em um aplicativo Windows Presentation Foundation.
ms.date: 03/30/2017
helpviewer_keywords:
- TextBox control [WPF], multiple lines of text
ms.assetid: 05914a93-d0ea-4a9a-b693-09df7d4e2ac2
ms.openlocfilehash: 0a88d4d768884df135afddb491431650b9ba2d24
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166255"
---
# <a name="how-to-create-a-multiline-textbox-control"></a>Como criar um controle TextBox multilinha
Este exemplo mostra como usar o [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] para definir um <xref:System.Windows.Controls.TextBox> controle que será expandido automaticamente para acomodar várias linhas de texto.  
  
## <a name="example"></a>Exemplo  
 A definição do <xref:System.Windows.Controls.TextBox.TextWrapping%2A> atributo a ser **encapsulado** fará com que o texto inserido seja quebrado em uma nova linha quando a borda do <xref:System.Windows.Controls.TextBox> controle for atingida, expandindo automaticamente o <xref:System.Windows.Controls.TextBox> controle para incluir espaço para uma nova linha, se necessário.  
  
 A configuração do <xref:System.Windows.Controls.Primitives.TextBoxBase.AcceptsReturn%2A> atributo como **true** faz com que uma nova linha seja inserida quando a tecla de retorno é pressionada, mais uma vez automaticamente expandindo a sala de <xref:System.Windows.Controls.TextBox> inclusão para uma nova linha, se necessário.  
  
 O <xref:System.Windows.Controls.Primitives.TextBoxBase.VerticalScrollBarVisibility%2A> atributo adiciona uma barra de rolagem ao <xref:System.Windows.Controls.TextBox> , para que o conteúdo do <xref:System.Windows.Controls.TextBox> possa ser rolado por meio de se <xref:System.Windows.Controls.TextBox> expandir para além do tamanho do quadro ou da janela que o inclui.  
  
 [!code-xaml[TextBox_MiscCode#_MultilineTextBoxXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml#_multilinetextboxxaml)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.TextWrapping>
- [Visão geral de TextBox](textbox-overview.md)
- [Visão geral de RichTextBox](richtextbox-overview.md)
