---
title: Como usar SelectedValue, SelectedValuePath e SelectedItem
description: Saiba como usar as propriedades SelectedValue e SelectedValuePath para especificar um valor para o SelectedItem de um Windows Presentation Foundation TreeView.
ms.date: 03/30/2017
helpviewer_keywords:
- TreeView control [WPF], SelectedValue properties
- Control class [WPF], SelectedItem properties
- Control class [WPF], TreeView properties
- Control class [WPF], SelectedValue properties
- TreeView control [WPF], SelectedItem properties
- SelectedValue [WPF], SelectedValuePath properties
- TreeView control [WPF], SelectedValuePath properties
- Control class [WPF], SelectedValuePath properties
- SelectedValue [WPF], SelectedItem properties
ms.assetid: 2fc92ad4-f02c-4f89-bbe9-d4978a7af0db
ms.openlocfilehash: ddac2455dee0bf69d25307340eddd5364e43e823
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87166274"
---
# <a name="how-to-use-selectedvalue-selectedvaluepath-and-selecteditem"></a>Como usar SelectedValue, SelectedValuePath e SelectedItem
Este exemplo mostra como usar as <xref:System.Windows.Controls.TreeView.SelectedValue%2A> Propriedades e <xref:System.Windows.Controls.TreeView.SelectedValuePath%2A> para especificar um valor para o <xref:System.Windows.Controls.TreeView.SelectedItem%2A> de um <xref:System.Windows.Controls.TreeView> .  
  
## <a name="example"></a>Exemplo  
 A <xref:System.Windows.Controls.TreeView.SelectedValuePath%2A> propriedade fornece uma maneira de especificar um <xref:System.Windows.Controls.TreeView.SelectedValue%2A> para o <xref:System.Windows.Controls.TreeView.SelectedItem%2A> em um <xref:System.Windows.Controls.TreeView> . O <xref:System.Windows.Controls.TreeView.SelectedItem%2A> representa um objeto na <xref:System.Windows.Controls.ItemsControl.Items%2A> coleção e <xref:System.Windows.Controls.TreeView> exibe o valor de uma única propriedade do item selecionado. A <xref:System.Windows.Controls.TreeView.SelectedValuePath%2A> propriedade especifica o caminho para a propriedade que é usada para determinar o valor da <xref:System.Windows.Controls.TreeView.SelectedValue%2A> propriedade. Os exemplos neste tópico ilustram esse conceito.  
  
 O exemplo a seguir mostra um <xref:System.Windows.Data.XmlDataProvider> que contém informações de funcionários.  
  
 [!code-xaml[TreeViewSelectedValue#XMLDataProvider](~/samples/snippets/csharp/VS_Snippets_Wpf/TreeViewSelectedValue/CS/Window1.xaml#xmldataprovider)]  
  
 O exemplo a seguir define um <xref:System.Windows.HierarchicalDataTemplate> que exibe o `EmployeeName` e `EmployeeWorkDay` do `Employee` . Observe que o <xref:System.Windows.HierarchicalDataTemplate> não especifica o `EmployeeNumber` como parte do modelo.  
  
 [!code-xaml[TreeViewSelectedValue#HierarchicalDataTemplate](~/samples/snippets/csharp/VS_Snippets_Wpf/TreeViewSelectedValue/CS/Window1.xaml#hierarchicaldatatemplate)]  
  
 O exemplo a seguir mostra um <xref:System.Windows.Controls.TreeView> que usa o definido anteriormente <xref:System.Windows.HierarchicalDataTemplate> e que define a <xref:System.Windows.Controls.TreeView.SelectedValue%2A> propriedade para o `EmployeeNumber` . Quando você seleciona um `EmployeeName` no <xref:System.Windows.Controls.TreeView> , a <xref:System.Windows.Controls.TreeView.SelectedItem%2A> propriedade retorna o `EmployeeInfo` item de dados que corresponde ao selecionado `EmployeeName` . No entanto, como o <xref:System.Windows.Controls.TreeView.SelectedValuePath%2A> de <xref:System.Windows.Controls.TreeView> é definido como `EmployeeNumber` , o <xref:System.Windows.Controls.TreeView.SelectedValue%2A> é definido como `EmployeeNumber` .  
  
 [!code-xaml[TreeViewSelectedValue#SelectedValuePath](~/samples/snippets/csharp/VS_Snippets_Wpf/TreeViewSelectedValue/CS/Window1.xaml#selectedvaluepath)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.TreeView>
- <xref:System.Windows.Controls.TreeViewItem>
- [Visão geral de TreeView](treeview-overview.md)
- [Tópicos explicativos](treeview-how-to-topics.md)
