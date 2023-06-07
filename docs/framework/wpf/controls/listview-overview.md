---
title: Visão geral de ListView
description: Saiba mais sobre o controle ListView Windows Presentation Foundation, que fornece a infraestrutura para exibir itens de dados em diferentes layouts ou exibições.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [WPF], ListView
- ListView controls [WPF], about ListView control
ms.assetid: 989e12b0-260e-4570-95c6-489284003ce2
ms.openlocfilehash: 419c6216f0af696ec71e7607c79c2db637caa785
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164563"
---
# <a name="listview-overview"></a>Visão geral de ListView
O <xref:System.Windows.Controls.ListView> controle fornece a infraestrutura para exibir um conjunto de itens de dados em diferentes layouts ou exibições. Por exemplo, talvez um usuário queira exibir itens de dados em uma tabela e classificar suas colunas.  

<a name="WhatisaListView"></a>
## <a name="what-is-a-listview"></a>O que é uma ListView?  
 O <xref:System.Windows.Controls.ListView> controle é um <xref:System.Windows.Controls.ItemsControl> derivado de <xref:System.Windows.Controls.ListBox> . Normalmente, seus itens são membros de uma coleção de dados e são representados como <xref:System.Windows.Controls.ListViewItem> objetos. Um <xref:System.Windows.Controls.ListViewItem> é um <xref:System.Windows.Controls.ContentControl> e pode conter apenas um único elemento filho. No entanto, esse elemento filho pode ser qualquer elemento visual.  
  
<a name="DefiningaListViewView"></a>
## <a name="defining-a-view-mode-for-a-listview"></a>Definindo um modo de exibição para um ListView  
 Para especificar um modo de exibição para o conteúdo de um <xref:System.Windows.Controls.ListView> controle, defina a <xref:System.Windows.Controls.ListView.View%2A> propriedade. Um modo de exibição que [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] o fornece é <xref:System.Windows.Controls.GridView> , que exibe uma coleção de itens de dados em uma tabela que tem colunas personalizáveis.  
  
 O exemplo a seguir mostra como definir um <xref:System.Windows.Controls.GridView> para um <xref:System.Windows.Controls.ListView> controle que exibe informações de funcionários.  
  
 [!code-xaml[ListViewCode#ListViewEmployee](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewCode/CSharp/Window1.xaml#listviewemployee)]  
  
 A ilustração a seguir mostra como os dados são exibidos no exemplo anterior.  
  
 ![Captura de tela que mostra um ListView com saída GridView.](./media/gridview-overview/listview-gridview-output.jpg)  
  
 Você pode criar um modo de exibição personalizado definindo uma classe que herda da <xref:System.Windows.Controls.ViewBase> classe. A <xref:System.Windows.Controls.ViewBase> classe fornece a infraestrutura de que você precisa para criar uma exibição personalizada. Para obter mais informações sobre como criar uma exibição personalizada, consulte [Criar um modo de exibição personalizado para um ListView](how-to-create-a-custom-view-mode-for-a-listview.md).  
  
<a name="BindingDatatoaListView"></a>
## <a name="binding-data-to-a-listview"></a>Associando dados a um ListView  
 Use as <xref:System.Windows.Controls.ItemsControl.Items%2A> <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> Propriedades e para especificar itens para um <xref:System.Windows.Controls.ListView> controle. O exemplo a seguir define a <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> propriedade como uma coleção de dados que é chamada `EmployeeInfoDataSource` .  
  
 [!code-xaml[ListViewCode#ItemsSource](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewCode/CSharp/Window1.xaml#itemssource)]  
  
 Em um <xref:System.Windows.Controls.GridView> , <xref:System.Windows.Controls.GridViewColumn> objetos são associados a campos de dados especificados. O exemplo a seguir associa um <xref:System.Windows.Controls.GridViewColumn> objeto a um campo de dados especificando um <xref:System.Windows.Data.Binding> para a <xref:System.Windows.Controls.GridViewColumn.DisplayMemberBinding%2A> propriedade.  
  
 [!code-csharp[ListViewCode#GridViewColumnProperties](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewCode/CSharp/Window1.xaml.cs#gridviewcolumnproperties)]
 [!code-vb[ListViewCode#GridViewColumnProperties](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ListViewCode/visualbasic/window1.xaml.vb#gridviewcolumnproperties)]
 [!code-xaml[ListViewCode#GridViewColumnProperties](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewCode/CSharp/Window1.xaml#gridviewcolumnproperties)]  
  
 Você também pode especificar um <xref:System.Windows.Data.Binding> como parte de uma <xref:System.Windows.DataTemplate> definição que você usa para estilizar as células em uma coluna. No exemplo a seguir, o <xref:System.Windows.DataTemplate> que é identificado com um <xref:System.Windows.ResourceKey> define o <xref:System.Windows.Data.Binding> para um <xref:System.Windows.Controls.GridViewColumn> . Observe que esse exemplo não define o <xref:System.Windows.Controls.GridViewColumn.DisplayMemberBinding%2A> porque fazer isso substitui a associação especificada por <xref:System.Windows.DataTemplate> .  
  
 [!code-xaml[ListViewTemplate#GridViewCellTemplate](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewTemplate/CS/window1.xaml#gridviewcelltemplate)]  
  
 [!code-xaml[ListViewTemplate#CellTemplateProperty](~/samples/snippets/csharp/VS_Snippets_Wpf/ListViewTemplate/CS/window1.xaml#celltemplateproperty)]  
  
<a name="StylingaListView"></a>
## <a name="styling-a-listview-that-implements-a-gridview"></a>Estilizando uma ListView que implemente um GridView  
 O <xref:System.Windows.Controls.ListView> controle contém <xref:System.Windows.Controls.ListViewItem> objetos, que representam os itens de dados que são exibidos. Você pode usar as propriedades a seguir para definir o conteúdo e estilo de itens de dados:  
  
- No <xref:System.Windows.Controls.ListView> controle, use as <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> Propriedades, <xref:System.Windows.Controls.ItemsControl.ItemTemplateSelector%2A> e <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> .  
  
- No <xref:System.Windows.Controls.ListViewItem> controle, use as <xref:System.Windows.Controls.ContentControl.ContentTemplate%2A> Propriedades e <xref:System.Windows.Controls.ContentControl.ContentTemplateSelector%2A> .  
  
 Para evitar problemas de alinhamento entre células em um <xref:System.Windows.Controls.GridView> , não use o <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> para definir propriedades ou adicionar conteúdo que afete a largura de um item em um <xref:System.Windows.Controls.ListView> . Por exemplo, um problema de alinhamento pode ocorrer quando você define a <xref:System.Windows.FrameworkElement.Margin%2A> propriedade no <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> . Para especificar propriedades ou definir o conteúdo que afeta a largura dos itens em um <xref:System.Windows.Controls.GridView> , use as propriedades da <xref:System.Windows.Controls.GridView> classe e suas classes relacionadas, como <xref:System.Windows.Controls.GridViewColumn> .  
  
 Para obter mais informações sobre como usar <xref:System.Windows.Controls.GridView> o e suas classes de suporte, consulte [visão geral de GridView](gridview-overview.md).  
  
 Se você definir um <xref:System.Windows.Controls.ItemsControl.ItemContainerStyle%2A> para um <xref:System.Windows.Controls.ListView> controle e também definir um <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> , deverá incluir um <xref:System.Windows.Controls.ContentPresenter> no estilo para <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> que o funcione corretamente.  
  
 Não use as <xref:System.Windows.Controls.Control.HorizontalContentAlignment%2A> Propriedades e <xref:System.Windows.Controls.Control.VerticalContentAlignment%2A> para o <xref:System.Windows.Controls.ListView> conteúdo que é exibido usando um <xref:System.Windows.Controls.GridView> . Para especificar o alinhamento do conteúdo em uma coluna de a <xref:System.Windows.Controls.GridView> , defina um <xref:System.Windows.Controls.GridViewColumn.CellTemplate%2A> .  
  
<a name="UsingtheSameViewMoreThanOnce"></a>
## <a name="sharing-the-same-view-mode"></a>Compartilhando o mesmo modo de exibição  
 Dois <xref:System.Windows.Controls.ListView> controles não podem compartilhar o mesmo modo de exibição ao mesmo tempo. Se você tentar usar o mesmo modo de exibição com mais de um <xref:System.Windows.Controls.ListView> controle, ocorrerá uma exceção.  
  
 Para especificar um modo de exibição que possa ser usado simultaneamente por mais de um <xref:System.Windows.Controls.ListView> , use modelos ou estilos.
  
<a name="CreatingaCustomView"></a>
## <a name="creating-a-custom-view-mode"></a>Criando um modo de exibição personalizada  
 Modos de exibição personalizados como <xref:System.Windows.Controls.GridView> são derivados da <xref:System.Windows.Controls.ViewBase> classe abstrata, que fornece as ferramentas para exibir itens de dados que são representados como <xref:System.Windows.Controls.ListViewItem> objetos.
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.GridView>
- <xref:System.Windows.Controls.ListView>
- <xref:System.Windows.Controls.ListViewItem>
- <xref:System.Windows.Data.Binding>
- [Visão geral de GridView](gridview-overview.md)
- [Tópicos explicativos](listview-how-to-topics.md)
- [Controles](../advanced/optimizing-performance-controls.md)
