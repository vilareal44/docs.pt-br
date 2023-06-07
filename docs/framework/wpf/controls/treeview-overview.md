---
title: Visão geral de TreeView
description: Saiba como o Windows Presentation Foundation controle TreeView exibe informações em uma estrutura hierárquica usando nós, incluindo exemplos simples.
ms.date: 03/30/2017
helpviewer_keywords:
- expanding node [WPF]
- TreeView control [WPF], about TreeView control
- Control class [WPF], TreeView
ms.assetid: 62212512-5a5c-4864-949e-b6a6a3a52c02
ms.openlocfilehash: 6ef77febdd3facb7c7e1af566841c2a1aeaff810
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87164532"
---
# <a name="treeview-overview"></a>Visão geral de TreeView
O <xref:System.Windows.Controls.TreeView> controle fornece uma maneira de exibir informações em uma estrutura hierárquica usando nós recolhíveis. Este tópico apresenta os <xref:System.Windows.Controls.TreeView> controles e e <xref:System.Windows.Controls.TreeViewItem> fornece exemplos simples de uso.  

<a name="Simple_TreeView_Control"></a>
## <a name="what-is-a-treeview"></a>O que é um TreeView?  
 <xref:System.Windows.Controls.TreeView>é um <xref:System.Windows.Controls.ItemsControl> que Aninha os itens usando <xref:System.Windows.Controls.TreeViewItem> controles. O exemplo a seguir cria um <xref:System.Windows.Controls.TreeView> .  
  
 [!code-xaml[TreeViewSnips#EmbeddedTVIs](~/samples/snippets/csharp/VS_Snippets_Wpf/TreeViewSnips/CSharp/Window1.xaml#embeddedtvis)]  
  
<a name="Creating_a_TreeView"></a>
## <a name="creating-a-treeview"></a>Criando um TreeView  
 O <xref:System.Windows.Controls.TreeView> controle contém uma hierarquia de <xref:System.Windows.Controls.TreeViewItem> controles. Um <xref:System.Windows.Controls.TreeViewItem> controle é um <xref:System.Windows.Controls.HeaderedItemsControl> que tem um <xref:System.Windows.Controls.HeaderedItemsControl.Header%2A> e uma <xref:System.Windows.Controls.ItemsControl.Items%2A> coleção.  
  
 Se você estiver definindo um <xref:System.Windows.Controls.TreeView> usando [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] o, poderá definir explicitamente o <xref:System.Windows.Controls.HeaderedItemsControl.Header%2A> conteúdo de um <xref:System.Windows.Controls.TreeViewItem> controle e os itens que compõem sua coleção. A ilustração anterior demonstra esse método.  
  
 Você também pode especificar um <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> como uma fonte de dados e, em seguida, especificar um <xref:System.Windows.Controls.HeaderedItemsControl.HeaderTemplate%2A> e <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> para definir o <xref:System.Windows.Controls.TreeViewItem> conteúdo.  
  
 Para definir o layout de um <xref:System.Windows.Controls.TreeViewItem> controle, você também pode usar <xref:System.Windows.HierarchicalDataTemplate> objetos. Para obter mais informações e um exemplo, consulte [Usar SelectedValue, SelectedValuePath e SelectedItem](how-to-use-selectedvalue-selectedvaluepath-and-selecteditem.md).  
  
 Se um item não for um <xref:System.Windows.Controls.TreeViewItem> controle, ele será automaticamente incluído por um <xref:System.Windows.Controls.TreeViewItem> controle quando o <xref:System.Windows.Controls.TreeView> controle for exibido.  
  
<a name="Expanding_and_Collapsing_a_TreeViewItem"></a>
## <a name="expanding-and-collapsing-a-treeviewitem"></a>Expandindo e Recolhendo um TreeViewItem  
 Se o usuário expandir um <xref:System.Windows.Controls.TreeViewItem> , a <xref:System.Windows.Controls.TreeViewItem.IsExpanded%2A> propriedade será definida como `true` . Você também pode expandir ou recolher um <xref:System.Windows.Controls.TreeViewItem> sem qualquer ação direta do usuário, definindo a <xref:System.Windows.Controls.TreeViewItem.IsExpanded%2A> propriedade como `true` (expandir) ou `false` (recolher). Quando essa propriedade é alterada, <xref:System.Windows.Controls.TreeViewItem.Expanded> <xref:System.Windows.Controls.TreeViewItem.Collapsed> ocorre um evento ou.  
  
 Quando o <xref:System.Windows.FrameworkElement.BringIntoView%2A> método é chamado em um <xref:System.Windows.Controls.TreeViewItem> controle, o <xref:System.Windows.Controls.TreeViewItem> e seus controles pai se <xref:System.Windows.Controls.TreeViewItem> expandem. Se um <xref:System.Windows.Controls.TreeViewItem> não estiver visível ou parcialmente visível, o <xref:System.Windows.Controls.TreeView> rolará para torná-lo visível.  
  
<a name="TreeViewItem_Selection"></a>
## <a name="treeviewitem-selection"></a>Seleção de TreeViewItem  
 Quando um usuário clica em um <xref:System.Windows.Controls.TreeViewItem> controle para selecioná-lo, o <xref:System.Windows.Controls.TreeViewItem.Selected> evento ocorre e sua <xref:System.Windows.Controls.TreeViewItem.IsSelected%2A> propriedade é definida como `true` . O <xref:System.Windows.Controls.TreeViewItem> também se torna o <xref:System.Windows.Controls.TreeView.SelectedItem%2A> do <xref:System.Windows.Controls.TreeView> controle. Por outro lado, quando a seleção é alterada de um <xref:System.Windows.Controls.TreeViewItem> controle, seu <xref:System.Windows.Controls.TreeViewItem.Unselected> evento ocorre e sua <xref:System.Windows.Controls.TreeViewItem.IsSelected%2A> propriedade é definida como `false` .  
  
 A <xref:System.Windows.Controls.TreeView.SelectedItem%2A> propriedade no <xref:System.Windows.Controls.TreeView> controle é uma propriedade somente leitura; portanto, você não pode defini-la explicitamente. A <xref:System.Windows.Controls.TreeView.SelectedItem%2A> propriedade será definida se o usuário clicar em um <xref:System.Windows.Controls.TreeViewItem> controle ou quando a <xref:System.Windows.Controls.TreeViewItem.IsSelected%2A> propriedade estiver definida como `true` no <xref:System.Windows.Controls.TreeViewItem> controle.  
  
 Use a <xref:System.Windows.Controls.TreeView.SelectedValuePath%2A> propriedade para especificar um <xref:System.Windows.Controls.TreeView.SelectedValue%2A> de um <xref:System.Windows.Controls.TreeView.SelectedItem%2A> . Para obter mais informações, consulte [Usar SelectedValue, SelectedValuePath e SelectedItem](how-to-use-selectedvalue-selectedvaluepath-and-selecteditem.md).  
  
 Você pode registrar um manipulador de eventos no <xref:System.Windows.Controls.TreeView.SelectedItemChanged> evento para determinar quando uma <xref:System.Windows.Controls.TreeViewItem> alteração selecionada. O <xref:System.Windows.RoutedPropertyChangedEventArgs%601> que é fornecido ao manipulador de eventos especifica o <xref:System.Windows.RoutedPropertyChangedEventArgs%601.OldValue%2A> , que é a seleção anterior, e o <xref:System.Windows.RoutedPropertyChangedEventArgs%601.NewValue%2A> , que é a seleção atual. O valor poderá ser `null` se o aplicativo ou o usuário não tiver feito uma seleção anterior ou atual.  
  
<a name="TreeView_Style"></a>
## <a name="treeview-style"></a>Estilo TreeView  
 O estilo padrão para um <xref:System.Windows.Controls.TreeView> controle o coloca dentro de um <xref:System.Windows.Controls.StackPanel> objeto que contém um <xref:System.Windows.Controls.ScrollViewer> controle. Quando você define as <xref:System.Windows.FrameworkElement.Width%2A> <xref:System.Windows.FrameworkElement.Height%2A> Propriedades e para um <xref:System.Windows.Controls.TreeView> , esses valores são usados para dimensionar o <xref:System.Windows.Controls.StackPanel> objeto que exibe o <xref:System.Windows.Controls.TreeView> . Se o conteúdo a ser exibido for maior que a área de exibição, um será <xref:System.Windows.Controls.ScrollViewer> exibido automaticamente para que o usuário possa percorrer o <xref:System.Windows.Controls.TreeView> conteúdo.  
  
 Para personalizar a aparência de um <xref:System.Windows.Controls.TreeViewItem> controle, defina a <xref:System.Windows.FrameworkElement.Style%2A> propriedade como um personalizado <xref:System.Windows.Style> .  
  
 O exemplo a seguir mostra como definir os <xref:System.Windows.Controls.Control.Foreground%2A> <xref:System.Windows.Controls.Control.FontSize%2A> valores de propriedade e para um <xref:System.Windows.Controls.TreeViewItem> controle usando um <xref:System.Windows.FrameworkElement.Style%2A> .  
  
 [!code-xaml[TreeViewSimple#8](~/samples/snippets/csharp/VS_Snippets_Wpf/TreeViewSimple/CS/Window1.xaml#8)]  
  
<a name="Adding_Images_and_oOther_Content_to_TreeView_Items"></a>
## <a name="adding-images-and-other-content-to-treeview-items"></a>Adicionando imagens e outros tipos de conteúdo aos itens do TreeView  
 Você pode incluir mais de um objeto no <xref:System.Windows.Controls.HeaderedItemsControl.Header%2A> conteúdo de um <xref:System.Windows.Controls.TreeViewItem> . Para incluir vários objetos no <xref:System.Windows.Controls.HeaderedItemsControl.Header%2A> conteúdo, coloque os objetos dentro de um controle de layout, como um <xref:System.Windows.Controls.Panel> ou <xref:System.Windows.Controls.StackPanel> .  
  
 O exemplo a seguir mostra como definir o <xref:System.Windows.Controls.HeaderedItemsControl.Header%2A> de a <xref:System.Windows.Controls.TreeViewItem> como um <xref:System.Windows.Controls.CheckBox> e <xref:System.Windows.Controls.TextBlock> que estão ambos incluídos em um <xref:System.Windows.Controls.DockPanel> controle.  
  
 [!code-xaml[TreeViewSnips#TVIHeader](~/samples/snippets/csharp/VS_Snippets_Wpf/TreeViewSnips/CSharp/Window1.xaml#tviheader)]  
  
 O exemplo a seguir mostra como definir um <xref:System.Windows.DataTemplate> que contém um <xref:System.Windows.Controls.Image> e um <xref:System.Windows.Controls.TextBlock> que são colocados em um <xref:System.Windows.Controls.DockPanel> controle. Você pode usar um <xref:System.Windows.DataTemplate> para definir o <xref:System.Windows.Controls.HeaderedItemsControl.HeaderTemplate%2A> ou <xref:System.Windows.Controls.ItemsControl.ItemTemplate%2A> para um <xref:System.Windows.Controls.TreeViewItem> .  
  
 [!code-xaml[TreeViewDataBinding#6](~/samples/snippets/csharp/VS_Snippets_Wpf/TreeViewDataBinding/CSharp/Window1.xaml#6)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.TreeView>
- <xref:System.Windows.Controls.TreeViewItem>
- [Tópicos explicativos](treeview-how-to-topics.md)
- [Modelo de conteúdo do WPF](wpf-content-model.md)
