---
title: Como disponibilizar dados para associação em XAML
description: Descubra as várias maneiras como você pode disponibilizar os dados de acordo com as necessidades do seu aplicativo no Windows Presentation Foundation (WPF).
ms.date: 01/29/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding [WPF], making data available for binding
- binding data [WPF], making data available for
ms.assetid: 7103c2e8-0e31-4a13-bf12-ca382221a8d5
ms.openlocfilehash: 16618ce8b19f5dd5854c4d126e7fc455f0428a28
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85620788"
---
# <a name="how-to-make-data-available-for-binding-in-xaml"></a>Como disponibilizar dados para associação em XAML
Este tópico discute várias maneiras de disponibilizar dados para associação no [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] , dependendo das necessidades do seu aplicativo.  
  
## <a name="example"></a>Exemplo  
 Se você tiver um objeto Common Language Runtime (CLR) ao qual gostaria de associar [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] , uma maneira de tornar o objeto disponível para associação é defini-lo como um recurso e fornecê-lo `x:Key` . No exemplo a seguir, temos um objeto `Person` com uma propriedade de cadeia de caracteres denominada `PersonName`. O `Person` objeto (na linha mostrada realçada que contém o `<src>` elemento) é definido no namespace chamado `SDKSample` .  
  
 [!code-xaml[SimpleBinding#Instantiation](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleBinding/CSharp/Page1.xaml?highlight=9,37)]  
  
 Em seguida, você pode associar o <xref:System.Windows.Controls.TextBlock> controle ao objeto no [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] , como a linha realçada que contém o `<TextBlock>` elemento mostra.
  
 Como alternativa, você pode usar a <xref:System.Windows.Data.ObjectDataProvider> classe, como no exemplo a seguir:  
  
 [!code-xaml[ObjectDataProvider}](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleBinding/VisualBasic/Page1.xaml?highlight=10-14,42)]  
  
 Você define a associação da mesma maneira, pois a linha realçada que contém o `<TextBlock>` elemento mostra.  
  
 Neste exemplo específico, o resultado é o mesmo: você tem um <xref:System.Windows.Controls.TextBlock> com o conteúdo de texto `Joe` . No entanto, a <xref:System.Windows.Data.ObjectDataProvider> classe fornece funcionalidade como a capacidade de associar ao resultado de um método. Você pode optar por usar a <xref:System.Windows.Data.ObjectDataProvider> classe se precisar da funcionalidade que ela fornece.  
  
 No entanto, se estiver associando a um objeto que já foi criado, será necessário definir o `DataContext` no código, como no exemplo a seguir.  
  
 [!code-csharp[ADODataSet#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ADODataSet/CSharp/Window1.xaml.cs#1)]
 [!code-vb[ADODataSet#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ADODataSet/VisualBasic/Window1.xaml.vb#1)]  
  
 Para acessar dados XML para associação usando a <xref:System.Windows.Data.XmlDataProvider> classe, consulte [associar dados XML usando um XmlDataProvider e consultas XPath](how-to-bind-to-xml-data-using-an-xmldataprovider-and-xpath-queries.md). Para acessar dados XML para associação usando a <xref:System.Windows.Data.ObjectDataProvider> classe, confira [associar a resultados da consulta de vincular a XDocument, XELEMENT ou LINQ for XML](how-to-bind-to-xdocument-xelement-or-linq-for-xml-query-results.md).  
  
 Para obter informações sobre várias maneiras que você pode especificar os dados aos quais você está ligando, consulte [especificar a origem da Associação](how-to-specify-the-binding-source.md). Para obter informações sobre quais tipos de dados você pode associar ou como implementar seus próprios objetos Common Language Runtime (CLR) para associação, consulte [visão geral de fontes de associação](binding-sources-overview.md).  
  
## <a name="see-also"></a>Consulte também

- [Visão geral da ligação de dados](../../../desktop-wpf/data/data-binding-overview.md)
- [Tópicos explicativos](data-binding-how-to-topics.md)
