---
title: Como criar uma associação em código
description: Saiba como criar uma associação no código em um aplicativo Windows Presentation Foundation chamando o método SetBinding diretamente.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- binding data [WPF], creating
- data binding [WPF], creating
ms.assetid: 1a606db9-cf5f-42ed-a1c5-9e4722ec77a0
ms.openlocfilehash: aa2a29f4c1262d8ac54641b856c297b284b23a38
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165804"
---
# <a name="how-to-create-a-binding-in-code"></a>Como criar uma associação em código
Este exemplo mostra como criar e definir um <xref:System.Windows.Data.Binding> no código.  
  
## <a name="example"></a>Exemplo  
 A <xref:System.Windows.FrameworkElement> classe e a <xref:System.Windows.FrameworkContentElement> classe expõem um `SetBinding` método. Se você estiver ligando um elemento que herde qualquer uma dessas classes, você pode chamar o <xref:System.Windows.FrameworkElement.SetBinding%2A> método diretamente.  
  
 O exemplo a seguir cria uma classe chamada `MyData`, que contém uma propriedade chamada `MyDataProperty`.  
  
 [!code-csharp[CodeOnlyBinding#DataObject](~/samples/snippets/csharp/VS_Snippets_Wpf/CodeOnlyBinding/CSharp/MyData.cs#dataobject)]
 [!code-vb[CodeOnlyBinding#DataObject](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CodeOnlyBinding/VisualBasic/MyData.vb#dataobject)]  
  
 O exemplo a seguir mostra como criar um objeto de associação para definir a origem da associação.  O exemplo usa <xref:System.Windows.FrameworkElement.SetBinding%2A> para associar a <xref:System.Windows.Controls.TextBlock.Text%2A> propriedade de `myText` , que é um <xref:System.Windows.Controls.TextBlock> controle, ao `MyDataProperty` .  
  
 [!code-csharp[CodeOnlyBinding#1](~/samples/snippets/csharp/VS_Snippets_Wpf/CodeOnlyBinding/CSharp/binding.cs#1)]
 [!code-vb[CodeOnlyBinding#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CodeOnlyBinding/VisualBasic/App.vb#1)]  
  
 Para ver o exemplo de código completo, confira [Exemplo de associação somente de código](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms771500(v=vs.90)).  
  
 Em vez de chamar <xref:System.Windows.FrameworkElement.SetBinding%2A> , você pode usar o <xref:System.Windows.Data.BindingOperations.SetBinding%2A> método estático da <xref:System.Windows.Data.BindingOperations> classe. O exemplo a seguir chama <xref:System.Windows.Data.BindingOperations.SetBinding%2A?displayProperty=nameWithType> em vez de <xref:System.Windows.FrameworkElement.SetBinding%2A?displayProperty=nameWithType> associar `myText` a `myDataProperty` .  
  
 [!code-csharp[CodeOnlyBinding#BOSetBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/CodeOnlyBinding/CSharp/binding.cs#bosetbinding)]
 [!code-vb[CodeOnlyBinding#BOSetBinding](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CodeOnlyBinding/VisualBasic/App.vb#bosetbinding)]  
  
## <a name="see-also"></a>Confira também

- [Visão geral da ligação de dados](../../../desktop-wpf/data/data-binding-overview.md)
- [Tópicos explicativos](data-binding-how-to-topics.md)
