---
title: Como associar a uma coleção e exibir informações com base na seleção
description: Siga este exemplo para descobrir como associar a uma coleção e exibir informações com base na seleção no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data collections [WPF], selecting data for views
- data binding [WPF], creating views of data collections
- data binding [WPF], selecting data for views
- data binding [WPF], binding to collections
ms.assetid: 952a7d76-dd29-49e5-86f5-32c4530e70eb
ms.openlocfilehash: fb8757ee6818a2812308a0a132fa9d06951ad52e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619605"
---
# <a name="how-to-bind-to-a-collection-and-display-information-based-on-selection"></a>Como associar a uma coleção e exibir informações com base na seleção
Em um cenário de detalhes mestre simples, você tem uma associação de dados, como <xref:System.Windows.Controls.ItemsControl> um <xref:System.Windows.Controls.ListBox> . Com base na seleção do usuário, se exibe mais informações sobre o item selecionado. Este exemplo mostra como implementar este cenário.  
  
## <a name="example"></a>Exemplo  
 Neste exemplo, `People` é uma <xref:System.Collections.ObjectModel.ObservableCollection%601> das `Person` classes. Esta classe `Person` contém três propriedades: `FirstName`, `LastName` e `HomeTown`, todas de tipo `string`.  
  
 [!code-xaml[CollectionBinding#Source](~/samples/snippets/csharp/VS_Snippets_Wpf/CollectionBinding/CSharp/Window1.xaml#source)]  
[!code-xaml[CollectionBinding#UI](~/samples/snippets/csharp/VS_Snippets_Wpf/CollectionBinding/CSharp/Window1.xaml#ui)]  
  
 O <xref:System.Windows.Controls.ContentControl> usa o seguinte <xref:System.Windows.DataTemplate> que define como as informações de um `Person` são apresentadas:  
  
 [!code-xaml[CollectionBinding#DetailTemplate](~/samples/snippets/csharp/VS_Snippets_Wpf/CollectionBinding/CSharp/Window1.xaml#detailtemplate)]  
  
 A seguir temos uma imagem do que o exemplo produz. O <xref:System.Windows.Controls.ContentControl> mostra as outras propriedades da pessoa selecionada.  
  
 ![Associação a uma coleção](./media/databinding-collectionbindingsample.png "DataBinding_CollectionBindingSample")  
  
 Os dois aspectos a observar neste exemplo são:  
  
1. O <xref:System.Windows.Controls.ListBox> e o <xref:System.Windows.Controls.ContentControl> associado à mesma fonte. As <xref:System.Windows.Data.Binding.Path%2A> Propriedades de ambas as associações não são especificadas porque ambos os controles estão sendo vinculados a todo o objeto da coleção.  
  
2. Você deve definir a <xref:System.Windows.Controls.Primitives.Selector.IsSynchronizedWithCurrentItem%2A> propriedade como para `true` que isso funcione. Definir essa propriedade garante que o item selecionado sempre seja definido como o <xref:System.Windows.Controls.ItemCollection.CurrentItem%2A> . Como alternativa, se o <xref:System.Windows.Controls.ListBox> obter dados de um <xref:System.Windows.Data.CollectionViewSource> , ele sincronizará a seleção e a moeda automaticamente.  
  
 Observe que a classe `Person` substitui o método `ToString` da seguinte forma. Por padrão, o <xref:System.Windows.Controls.ListBox> chama `ToString` e exibe uma representação de cadeia de caracteres de cada objeto na coleção associada. É por isso que cada `Person` um aparece como um primeiro nome no <xref:System.Windows.Controls.ListBox> .  
  
 [!code-csharp[CollectionBinding#ToString](~/samples/snippets/csharp/VS_Snippets_Wpf/CollectionBinding/CSharp/Data.cs#tostring)]
 [!code-vb[CollectionBinding#ToString](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CollectionBinding/VisualBasic/Person.vb#tostring)]  
  
## <a name="see-also"></a>Consulte também

- [Usar o padrão de detalhes mestre com dados hierárquicos](how-to-use-the-master-detail-pattern-with-hierarchical-data.md)
- [Usar o padrão de detalhes mestre com dados XML hierárquicos](how-to-use-the-master-detail-pattern-with-hierarchical-xml-data.md)
- [Visão geral da ligação de dados](../../../desktop-wpf/data/data-binding-overview.md)
- [Visão geral de modelagem dos dados](data-templating-overview.md)
- [Tópicos explicativos](data-binding-how-to-topics.md)
