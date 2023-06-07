---
title: Como alterar o tipo de cursor
description: Alterar o cursor do ponteiro do mouse para um elemento e para um aplicativo em Windows Presentation Foundation. Este exemplo consiste em XAML e um arquivo code-behind.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- mouse pointer [WPF], cursor type
- cursor (mouse pointer)
ms.assetid: 08c945a7-8ab0-4320-acf3-0b4955a344c2
ms.openlocfilehash: ce0bc290948a0e52e85f76ceb62a330b49fd87ea
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165966"
---
# <a name="how-to-change-the-cursor-type"></a>Como alterar o tipo de cursor
Este exemplo mostra como alterar o <xref:System.Windows.Input.Cursor> do ponteiro do mouse para um elemento específico e para o aplicativo.  
  
 Esse exemplo consiste em um arquivo [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] e um arquivo code-behind.  
  
## <a name="example"></a>Exemplo  
 A interface do usuário é criada, que consiste em um <xref:System.Windows.Controls.ComboBox> para selecionar o desejado <xref:System.Windows.Input.Cursor> , um par de <xref:System.Windows.Controls.RadioButton> objetos para determinar se a alteração do cursor se aplica a apenas um único elemento ou se aplica a todo o aplicativo, e um <xref:System.Windows.Controls.Border> que é o elemento ao qual o novo cursor é aplicado.  
  
 [!code-xaml[cursors#ChangeCursorsXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/cursors/CSharp/Window1.xaml#changecursorsxaml)]  
  
 O code-behind a seguir cria um <xref:System.Windows.Controls.Primitives.Selector.SelectionChanged> manipulador de eventos que é chamado quando o tipo de cursor é alterado no <xref:System.Windows.Controls.ComboBox> .  Uma instrução switch filtra o nome do cursor e define a <xref:System.Windows.FrameworkElement.Cursor%2A> propriedade no <xref:System.Windows.Controls.Border> que é chamado de *DisplayArea*.  
  
 Se a alteração do cursor estiver definida como "aplicativo inteiro", a <xref:System.Windows.Input.Mouse.OverrideCursor%2A> propriedade será definida como a <xref:System.Windows.FrameworkElement.Cursor%2A> Propriedade do <xref:System.Windows.Controls.Border> controle.  Isso força o cursor a mudar para o aplicativo inteiro.  
  
 [!code-csharp[cursors#ChangeCursorsSample](~/samples/snippets/csharp/VS_Snippets_Wpf/cursors/CSharp/Window1.xaml.cs#changecursorssample)]
 [!code-vb[cursors#ChangeCursorsSample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/cursors/VisualBasic/Window1.xaml.vb#changecursorssample)]  
  
## <a name="see-also"></a>Confira também

- [Visão geral da entrada](input-overview.md)
