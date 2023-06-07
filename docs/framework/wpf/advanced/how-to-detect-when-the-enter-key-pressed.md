---
title: Como detectar quando a tecla Enter é pressionada
description: Detectar quando a tecla Enter é selecionada no teclado em Windows Presentation Foundation. Este exemplo consiste em XAML e um arquivo code-behind.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Enter key [WPF], detecting
- keys [WPF], Enter
ms.assetid: a66f39d2-ef4a-43a5-b454-a4ea0fe88655
ms.openlocfilehash: a955f52ec7bf93b32c70259b27fb51747664ac2e
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87163180"
---
# <a name="how-to-detect-when-the-enter-key-pressed"></a>Como detectar quando a tecla Enter é pressionada
Este exemplo mostra como detectar quando a <xref:System.Windows.Input.Key.Enter> tecla é pressionada no teclado.  
  
 Este exemplo consiste em um [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] arquivo e um arquivo code-behind.  
  
## <a name="example"></a>Exemplo  
 Quando o usuário pressiona a <xref:System.Windows.Input.Key.Enter> tecla no <xref:System.Windows.Controls.TextBox> , a entrada na caixa de texto é exibida em outra área do [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] .  
  
 O XAML a seguir cria a interface do usuário, que consiste em um <xref:System.Windows.Controls.StackPanel> , um <xref:System.Windows.Controls.TextBlock> e um <xref:System.Windows.Controls.TextBox> .  
  
 [!code-xaml[keydown#KeyDownUI](~/samples/snippets/csharp/VS_Snippets_Wpf/KeyDown/CSharp/Window1.xaml#keydownui)]  
  
 O code-behind a seguir cria o <xref:System.Windows.UIElement.KeyDown> manipulador de eventos.  Se a chave que é pressionada for a <xref:System.Windows.Input.Key.Enter> chave, uma mensagem será exibida no <xref:System.Windows.Controls.TextBlock> .  
  
 [!code-csharp[keydown#KeyDownSample](~/samples/snippets/csharp/VS_Snippets_Wpf/KeyDown/CSharp/Window1.xaml.cs#keydownsample)]
 [!code-vb[keydown#KeyDownSample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/KeyDown/VisualBasic/Window1.xaml.vb#keydownsample)]  
  
## <a name="see-also"></a>Confira também

- [Visão geral da entrada](input-overview.md)
- [Visão geral de eventos roteados](routed-events-overview.md)
