---
title: DataGrid
description: Saiba como o controle DataGrid permite exibir e editar dados de diferentes fontes, como um banco de dados, consulta LINQ ou qualquer outra fonte de dado ligável.
ms.date: 03/30/2017
helpviewer_keywords:
- DataGrid column types [WPF]
- DataGrid scenarios [WPF]
- DataGrid control [WPF]
- controls [WPF], DataGrid
- DataGrid [WPF], common tasks for
- DataGrid [WPF], customizing the appearance of
- DataGrid columns [WPF], using
ms.assetid: bf89ea63-79b6-422b-bc9f-0485ad803216
ms.openlocfilehash: 3db49c12fcb363ef7f99e5c69beae09ab05addcf
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87168321"
---
# <a name="datagrid"></a>DataGrid
O <xref:System.Windows.Controls.DataGrid> controle permite que você exiba e edite dados de várias fontes diferentes, como de um banco de dados SQL, consulta LINQ ou qualquer outra fonte de dado ligável. Para obter mais informações, consulte [Visão geral de origens da associação](../data/binding-sources-overview.md).  
  
 As colunas podem exibir texto, controles, como um <xref:System.Windows.Controls.ComboBox> ou qualquer outro conteúdo do WPF, como imagens, botões ou qualquer conteúdo contido em um modelo. Você pode usar um <xref:System.Windows.Controls.DataGridTemplateColumn> para exibir dados definidos em um modelo. A tabela a seguir lista os tipos de coluna fornecidos por padrão.  
  
|Tipo de coluna gerada|Tipo de Dados|  
|---------------------------|---------------|  
|<xref:System.Windows.Controls.DataGridTextColumn>|<xref:System.String>|  
|<xref:System.Windows.Controls.DataGridCheckBoxColumn>|<xref:System.Boolean>|  
|<xref:System.Windows.Controls.DataGridComboBoxColumn>|<xref:System.Enum>|  
|<xref:System.Windows.Controls.DataGridHyperlinkColumn>|<xref:System.Uri>|  
  
 <xref:System.Windows.Controls.DataGrid>pode ser personalizado na aparência, como fonte, cor e tamanho da célula. <xref:System.Windows.Controls.DataGrid>dá suporte a todos os estilos e funcionalidade de modelagem de outros controles WPF. <xref:System.Windows.Controls.DataGrid>também inclui comportamentos padrão e personalizáveis para edição, classificação e validação.  
  
 A tabela a seguir lista algumas das tarefas comuns para o <xref:System.Windows.Controls.DataGrid> e como realizá-las. Ao exibir a API relacionada, você pode encontrar mais informações e o código de exemplo.  
  
|Cenário|Abordagem|  
|--------------|--------------|  
|Alternando as cores da tela de fundo|Defina a <xref:System.Windows.Controls.ItemsControl.AlternationIndex%2A> propriedade como 2 ou mais e, em seguida, atribua uma <xref:System.Windows.Media.Brush> às <xref:System.Windows.Controls.DataGrid.RowBackground%2A> <xref:System.Windows.Controls.DataGrid.AlternatingRowBackground%2A> Propriedades e.|  
|Definir o comportamento da seleção de linha e de célula|Definir as propriedades <xref:System.Windows.Controls.DataGrid.SelectionMode%2A> e <xref:System.Windows.Controls.DataGrid.SelectionUnit%2A>.|  
|Personalizar a aparência de cabeçalhos, células e linhas|Aplique um novo <xref:System.Windows.Style> às <xref:System.Windows.Controls.DataGrid.ColumnHeaderStyle%2A> Propriedades, <xref:System.Windows.Controls.DataGrid.RowHeaderStyle%2A> , <xref:System.Windows.Controls.DataGrid.CellStyle%2A> ou <xref:System.Windows.Controls.DataGrid.RowStyle%2A> .|  
|Definir opções de dimensionamento|Defina as <xref:System.Windows.FrameworkElement.Height%2A> <xref:System.Windows.FrameworkElement.MaxHeight%2A> Propriedades,, <xref:System.Windows.FrameworkElement.MinHeight%2A> ,, <xref:System.Windows.FrameworkElement.Width%2A> <xref:System.Windows.FrameworkElement.MaxWidth%2A> ou <xref:System.Windows.FrameworkElement.MinWidth%2A> . Para obter mais informações, consulte [Opções de dimensionamento no controle DataGrid](sizing-options-in-the-datagrid-control.md).|  
|Acessar itens selecionados|Verifique a <xref:System.Windows.Controls.DataGrid.SelectedCells%2A> propriedade para obter as células selecionadas e a <xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems%2A> propriedade para obter as linhas selecionadas. Para obter mais informações, consulte <xref:System.Windows.Controls.DataGrid.SelectedCells%2A>.|  
|Personalizar as interações do usuário final|Defina as <xref:System.Windows.Controls.DataGrid.CanUserAddRows%2A> <xref:System.Windows.Controls.DataGrid.CanUserDeleteRows%2A> Propriedades,, <xref:System.Windows.Controls.DataGrid.CanUserReorderColumns%2A> ,, <xref:System.Windows.Controls.DataGrid.CanUserResizeColumns%2A> <xref:System.Windows.Controls.DataGrid.CanUserResizeRows%2A> e <xref:System.Windows.Controls.DataGrid.CanUserSortColumns%2A> .|  
|Cancelar ou alterar colunas geradas automaticamente|Manipule o <xref:System.Windows.Controls.DataGrid.AutoGeneratingColumn> evento.|  
|Congelar uma coluna|Defina a <xref:System.Windows.Controls.DataGrid.FrozenColumnCount%2A> propriedade como 1 e mova a coluna para a posição mais à esquerda definindo a <xref:System.Windows.Controls.DataGridColumn.DisplayIndex%2A> propriedade como 0.|  
|Usar dados XML como a fonte de dados|Associe o <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> no <xref:System.Windows.Controls.DataGrid> à consulta XPath que representa a coleção de itens. Crie cada coluna no <xref:System.Windows.Controls.DataGrid> . Associe cada coluna configurando o XPath na associação à consulta que obtém a propriedade na origem do item. Para ver um exemplo, consulte <xref:System.Windows.Controls.DataGridTextColumn>.|  
  
## <a name="related-topics"></a>Tópicos Relacionados  
  
|Título|Descrição|  
|-----------|-----------------|  
|[Passo a passo: exibir dados de um banco de dados do SQL Server em um controle DataGrid](walkthrough-display-data-from-a-sql-server-database-in-a-datagrid-control.md)|Descreve como configurar um novo projeto do WPF, adicionar um elemento Entity Framework, definir a origem e exibir os dados em um <xref:System.Windows.Controls.DataGrid> .|  
|[Como: adicionar detalhes de linha a um controle DataGrid](how-to-add-row-details-to-a-datagrid-control.md)|Descreve como criar detalhes de linha para um <xref:System.Windows.Controls.DataGrid> .|  
|[Como: implementar validação com o controle DataGrid](how-to-implement-validation-with-the-datagrid-control.md)|Descreve como validar valores em <xref:System.Windows.Controls.DataGrid> células e linhas e exibir comentários de validação.|  
|[Teclado padrão e comportamento do mouse no controle DataGrid](default-keyboard-and-mouse-behavior-in-the-datagrid-control.md)|Descreve como interagir com o <xref:System.Windows.Controls.DataGrid> controle usando o teclado e o mouse.|  
|[Como: agrupar, classificar e filtrar dados no controle DataGrid](how-to-group-sort-and-filter-data-in-the-datagrid-control.md)|Descreve como exibir dados de <xref:System.Windows.Controls.DataGrid> maneira diferente, agrupando, classificando e filtrando os dados.|  
|[Opções de dimensionamento no controle DataGrid](sizing-options-in-the-datagrid-control.md)|Descreve como controlar o dimensionamento absoluto e automático no <xref:System.Windows.Controls.DataGrid> .|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.DataGrid>
- [Estilo e modelagem](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [Visão geral da ligação de dados](../../../desktop-wpf/data/data-binding-overview.md)
- [Visão geral de modelagem dos dados](../data/data-templating-overview.md)
- [Controles](index.md)
- [Modelo de conteúdo do WPF](wpf-content-model.md)
