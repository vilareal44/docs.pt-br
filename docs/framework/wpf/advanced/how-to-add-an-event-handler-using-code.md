---
title: Como adicionar um manipulador de eventos usando código
description: Use este exemplo para aprender a adicionar um manipulador de eventos a um elemento no Windows Presentation Foundation usando código, em vez de declará-lo usando XAML.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- event handlers [WPF], adding
- XAML [WPF], adding event handlers
ms.assetid: 269c61e0-6bd9-4291-9bed-1c5ee66da486
ms.openlocfilehash: b36969f7a65ccbf6c9d03b22767d9eacdc177ad1
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166369"
---
# <a name="how-to-add-an-event-handler-using-code"></a>Como adicionar um manipulador de eventos usando código
Esse exemplo mostra como adicionar um manipulador de eventos a um elemento usando código.  
  
 Se você desejar adicionar um manipulador de eventos a um elemento [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] e a página de marcação que contém o elemento já foi carregada, será necessário adicionar o manipulador usando código. Como alternativa, se estiver compilando a árvore de elementos para um aplicativo usando inteiramente código e não declarando nenhum elemento usando [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)], você poderá chamar métodos específicos para adicionar manipuladores de eventos à árvore de elementos construídos.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir adiciona um novo <xref:System.Windows.Controls.Button> a uma página existente que é inicialmente definida em [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] . Um arquivo code-behind implementa um método manipulador de eventos e, em seguida, adiciona esse método como um novo manipulador de eventos no <xref:System.Windows.Controls.Button> .  
  
 O exemplo de C# usa o `+=` operador para atribuir um manipulador a um evento. Esse é o mesmo operador usado para atribuir um manipulador no modelo de manipulação de eventos Common Language Runtime (CLR). O Microsoft Visual Basic não oferece suporte a esse operador como um meio de adicionar manipuladores de eventos. Em vez disso, ele requer uma de duas técnicas:  
  
- Use o <xref:System.Windows.UIElement.AddHandler%2A> método, junto com um `AddressOf` operador, para fazer referência à implementação do manipulador de eventos.  
  
- Use a palavra-chave `Handles` como parte da definição do manipulador de eventos. Essa técnica não é mostrada aqui. Consulte [Manipulação de eventos do Visual Basic e WPF](visual-basic-and-wpf-event-handling.md).  
  
 [!code-xaml[RoutedEventAddRemoveHandler#XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/RoutedEventAddRemoveHandler/CSharp/default.xaml#xaml)]  
  
 [!code-csharp[RoutedEventAddRemoveHandler#Handler](~/samples/snippets/csharp/VS_Snippets_Wpf/RoutedEventAddRemoveHandler/CSharp/default.xaml.cs#handler)]
 [!code-vb[RoutedEventAddRemoveHandler#Handler](~/samples/snippets/visualbasic/VS_Snippets_Wpf/RoutedEventAddRemoveHandler/VisualBasic/default.xaml.vb#handler)]  
  
> [!NOTE]
> É muito mais simples adicionar um manipulador de eventos na página [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] analisada inicialmente. Dentro do elemento de objetos ao qual você deseja adicionar o manipulador de eventos, adicione um atributo que corresponde ao nome do evento que você deseja manipular. Em seguida, especifique o valor desse atributo como o nome do método do manipulador de eventos que você definiu no arquivo code-behind da página [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)]. Para obter mais informações, consulte [Visão geral de XAML (WPF)](../../../desktop-wpf/fundamentals/xaml.md) ou [Visão geral de eventos roteados](routed-events-overview.md).  
  
## <a name="see-also"></a>Confira também

- [Visão geral de eventos roteados](routed-events-overview.md)
- [Tópicos explicativos](events-how-to-topics.md)
