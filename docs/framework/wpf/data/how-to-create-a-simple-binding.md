---
title: Como criar uma associação simples
description: Crie uma associação simples para seus aplicativos por meio deste exemplo de instruções no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
helpviewer_keywords:
- simple binding [WPF], creating
- data binding [WPF], creating simple bindings
- binding data [WPF], creating
ms.assetid: 69b80f72-6259-44cb-8294-5bdcebca1e08
ms.openlocfilehash: 63dc44b442bb4658382bf12faf57b51c8e0698bb
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618695"
---
# <a name="how-to-create-a-simple-binding"></a>Como criar uma associação simples
Este exemplo mostra como criar um simples <xref:System.Windows.Data.Binding> .  
  
## <a name="example"></a>Exemplo  
 Neste exemplo, você tem um `Person` objeto com uma propriedade de cadeia de caracteres chamada `PersonName` . O objeto `Person` é definido no namespace chamado `SDKSample`.  
  
 A linha realçada que contém o `<src>` elemento no exemplo a seguir instancia o `Person` objeto com um `PersonName` valor de propriedade de `Joe` . Isso é feito na `Resources` seção e atribuído a `x:Key` .  
  
 [!code-xaml[SimpleBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleBinding/CSharp/Page1.xaml?highlight=9,37)]  
  
 A linha realçada que contém o `<TextBlock>` elemento, em seguida, associa o <xref:System.Windows.Controls.TextBlock> controle à `PersonName` propriedade. Como resultado, o <xref:System.Windows.Controls.TextBlock> aparece com o valor "Joe".  
  
## <a name="see-also"></a>Consulte também

- [Visão geral da ligação de dados](../../../desktop-wpf/data/data-binding-overview.md)
- [Tópicos explicativos](data-binding-how-to-topics.md)
