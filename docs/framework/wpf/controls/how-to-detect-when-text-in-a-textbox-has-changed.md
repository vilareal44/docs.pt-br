---
title: Como detectar quando o texto em um TextBox foi alterado
description: Saiba como usar o evento TextChanged para executar um método sempre que o texto em um controle TextBox for alterado em um aplicativo Windows Presentation Foundation.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- TextBox control [WPF], detecting text change
- text change [WPF], detecting
- detecting text change [WPF]
ms.assetid: 1c39ee14-e37f-49fb-a0d1-a9824ca13584
ms.openlocfilehash: 1e054380a8c77d32e6bb4adbbcb032e531bbefd0
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166225"
---
# <a name="how-to-detect-when-text-in-a-textbox-has-changed"></a>Como detectar quando o texto em um TextBox foi alterado

Este exemplo mostra uma maneira de usar o <xref:System.Windows.Controls.Primitives.TextBoxBase.TextChanged> evento para executar um método sempre que o texto em um <xref:System.Windows.Controls.TextBox> controle for alterado.

Na classe code-behind para o [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] que contém o <xref:System.Windows.Controls.TextBox> controle que você deseja monitorar para as alterações, insira um método a ser chamado sempre que o evento for disparado <xref:System.Windows.Controls.Primitives.TextBoxBase.TextChanged> .  Esse método deve ter uma assinatura que corresponda ao que é esperado pelo <xref:System.Windows.Controls.TextChangedEventHandler> delegado.

O manipulador de eventos é chamado sempre que o conteúdo do <xref:System.Windows.Controls.TextBox> controle é alterado, seja por um usuário ou programaticamente.

> [!NOTE]
> Esse evento é acionado quando o <xref:System.Windows.Controls.TextBox> controle é criado e preenchido inicialmente com texto.

## <a name="example"></a>Exemplo

No [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] que define seu <xref:System.Windows.Controls.TextBox> controle, especifique o <xref:System.Windows.Controls.Primitives.TextBoxBase.TextChanged> atributo com um valor que corresponda ao nome do método do manipulador de eventos.

[!code-xaml[TextBox_MiscCode#_TextChangedXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml#_textchangedxaml)]

## <a name="example"></a>Exemplo

Na classe code-behind para o [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] que contém o <xref:System.Windows.Controls.TextBox> controle que você deseja monitorar para as alterações, insira um método a ser chamado sempre que o evento for disparado <xref:System.Windows.Controls.Primitives.TextBoxBase.TextChanged> .  Esse método deve ter uma assinatura que corresponda ao que é esperado pelo <xref:System.Windows.Controls.TextChangedEventHandler> delegado.

[!code-csharp[TextBox_MiscCode#_TextChangedEventHandler](~/samples/snippets/csharp/VS_Snippets_Wpf/TextBox_MiscCode/CSharp/Window1.xaml.cs#_textchangedeventhandler)]
[!code-vb[TextBox_MiscCode#_TextChangedEventHandler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/TextBox_MiscCode/VisualBasic/Window1.xaml.vb#_textchangedeventhandler)]

O manipulador de eventos é chamado sempre que o conteúdo do <xref:System.Windows.Controls.TextBox> controle é alterado, seja por um usuário ou programaticamente.

> [!NOTE]
> Esse evento é acionado quando o <xref:System.Windows.Controls.TextBox> controle é criado e preenchido inicialmente com texto.

Comentários

## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.TextChangedEventArgs>
- [Visão geral de TextBox](textbox-overview.md)
- [Visão geral de RichTextBox](richtextbox-overview.md)
