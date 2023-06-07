---
title: Como definir o conteúdo de texto de um controle TextBox
description: Saiba como usar a propriedade Text para definir o conteúdo do texto inicial de um Windows Presentation Foundation controle TextBox.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- text content [WPF], setting
- TextBox control [WPF], setting text content
ms.assetid: bcd25fc7-a52f-4453-b802-2c8d2b335ab8
ms.openlocfilehash: 41efb69e297b3c6fdb1203c358dcc72d7a9f806f
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168047"
---
# <a name="how-to-set-the-text-content-of-a-textbox-control"></a>Como definir o conteúdo de texto de um controle TextBox

Este exemplo mostra como usar a <xref:System.Windows.Controls.TextBox.Text%2A> propriedade para definir o conteúdo de texto inicial de um <xref:System.Windows.Controls.TextBox> controle.

> [!NOTE]
> Embora a [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] versão do exemplo possa usar as `<TextBox.Text>` marcas em volta do texto do conteúdo de cada botão <xref:System.Windows.Controls.TextBox> , isso não é necessário porque o <xref:System.Windows.Controls.TextBox> aplica o <xref:System.Windows.Markup.ContentPropertyAttribute> atributo à <xref:System.Windows.Controls.TextBox.Text%2A> propriedade. Para obter mais informações, consulte [visão geral de XAML (WPF)](../../../desktop-wpf/fundamentals/xaml.md).

## <a name="example"></a>Exemplo

[!code-xaml[TextBox_MiscCode#_TextBoxSetTextXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml#_textboxsettextxaml)]

## <a name="example"></a>Exemplo

[!code-csharp[TextBox_MiscCode#_TextBoxSetText](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml.cs#_textboxsettext)]
[!code-vb[TextBox_MiscCode#_TextBoxSetText](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TextBox_MiscCode/VisualBasic/Window1.xaml.vb#_textboxsettext)]

## <a name="see-also"></a>Confira também

- [Visão geral de TextBox](textbox-overview.md)
- [Visão geral de RichTextBox](richtextbox-overview.md)
