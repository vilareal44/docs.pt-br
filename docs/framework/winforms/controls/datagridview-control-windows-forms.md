---
title: Controle DataGridView
description: Saiba como usar o `DataGridView` controle para mostrar exibições somente leitura de uma pequena quantidade de dados ou dimensioná-la para mostrar exibições editáveis de conjuntos de dados muito grandes.
ms.date: 03/30/2017
helpviewer_keywords:
- tables [Windows Forms]
- data grids [Windows Forms
- data [Windows Forms], displaying in tabular format
- grid controls [Windows Forms]
- datasets [Windows Forms], user interface
- Windows Forms, displaying data
- data presentation
- tabular data [Windows Forms], displaying on Windows Forms
- datasets [Windows Forms], displaying in DataGridView control
- DataGridView control [Windows Forms]
ms.assetid: dbee73f2-bba6-4874-9389-cd21d44309be
ms.openlocfilehash: f9a45e8be7975697ca5c0d019c6bc4119f562aea
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325893"
---
# <a name="datagridview-control-windows-forms"></a>Controle DataGridView (Windows Forms)
O controle de `DataGridView` fornece uma maneira poderosa e flexível para exibir dados em um formato de tabela. Você pode usar o controle `DataGridView` para mostrar as exibições somente leitura de uma pequena quantidade de dados ou pode ajustar a escala para mostrar exibições editáveis de grandes conjuntos de dados.  
  
 Você pode estender o controle `DataGridView` de várias maneiras para compilar comportamentos personalizados em seus aplicativos. Por exemplo, você pode especificar seus próprios algoritmos de classificação com programação, podendo também criar seus próprios tipos de células. É possível personalizar facilmente a aparência do controle `DataGridView` escolhendo dentre várias propriedades. Muitos tipos de armazenamentos de dados podem ser usados como uma fonte de dados ou o controle `DataGridView` pode operar sem nenhuma fonte de dados associada a ele.  
  
 Os tópicos nesta seção descrevem os conceitos e técnicas que você pode usar para compilar recursos `DataGridView` em seus aplicativos.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Visão geral do controle DataGridView](datagridview-control-overview-windows-forms.md)  
 Fornece tópicos que descrevem as arquitetura e os principais conceitos do controle `DataGridView` do Windows Forms.  
  
 [Funcionalidade padrão no controle DataGridView dos Windows Forms](default-functionality-in-the-windows-forms-datagridview-control.md)  
 Descreve a aparência padrão e o comportamento do controle `DataGridView` do Windows Forms quando ele é associado a uma fonte de dados.  
  
 [Tipos de coluna no controle DataGridView dos Windows Forms](column-types-in-the-windows-forms-datagridview-control.md)  
 Descreve os tipos de coluna no controle `DataGridView` do Windows Forms usado para exibir dados e permitir que os usuários modifiquem ou adicionem dados.  
  
 [Recursos básicos de coluna, linha e célula no controle DataGridView dos Windows Forms](basic-column-row-and-cell-features-wf-datagridview-control.md)  
 Fornece tópicos que descrevem as propriedades da célula, linha e coluna mais usados.  
  
 [Formatação e estilos básicos no controle DataGridView do Windows Forms](basic-formatting-and-styling-in-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem como modificar a aparência básica do controle e a formatação de exibição de dados da célula.  
  
 [Exibindo dados no controle DataGridView dos Windows Forms](displaying-data-in-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem como preencher o controle com os dados manualmente ou de uma fonte de dados externa.  
  
 [Redimensionando colunas e linhas no controle DataGridView dos Windows Forms](resizing-columns-and-rows-in-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem como o tamanho de linhas e colunas pode ser ajustado automaticamente para caber no conteúdo da célula ou para ajustar à largura do controle disponível.  
  
 [Classificando dados no controle DataGridView do Windows Forms](sorting-data-in-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem os recursos de classificação no controle.  
  
 [Entrada de Dados no controle DataGridView dos Windows Forms](data-entry-in-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem como alterar os maneira como os usuários adicionam e modificam dados no controle.  
  
 [Seleção e uso da Área de Transferência com o controle DataGridView dos Windows Forms](selection-and-clipboard-use-with-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem os recursos de seleção de célula, linha e coluna no controle.  
  
 [Programando com células, linhas e colunas no controle DataGridView dos Windows Forms](programming-with-cells-rows-and-columns-in-the-datagrid.md)  
 Fornece tópicos que descrevem como programar com objetos de célula, linha e coluna.  
  
 [Personalizando o controle DataGridView dos Windows Forms](customizing-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem a pintura personalizada de células e linhas `DataGridView`, bem como a criação de tipos de célula, coluna e linha derivados.  
  
 [Ajuste de desempenho no controle DataGridView dos Windows Forms](performance-tuning-in-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem como usar o controle com eficiência para evitar problemas de desempenho ao trabalhar com grandes volumes de dados.  
  
 [Manipulação de teclado e mouse padrão no controle DataGridView dos Windows Forms](default-keyboard-and-mouse-handling-in-the-windows-forms-datagridview-control.md)  
 Descreve como os usuários podem interagir com o controle `DataGridView` por meio de um teclado e mouse.  
  
 [Diferenças entre os controles DataGridView e DataGrid dos Windows Forms ](differences-between-the-windows-forms-datagridview-and-datagrid-controls.md)  
 Descreve como o `DataGridView` controle melhora e substitui o <xref:System.Windows.Forms.DataGrid> controle.  
  
 Consulte também [Usando o designer com o controle DataGridView do Windows Forms](using-the-designer-with-the-windows-forms-datagridview-control.md).  
  
## <a name="reference"></a>Referência  
 <xref:System.Windows.Forms.DataGridView>  
 Fornece documentação de referência para o <xref:System.Windows.Forms.DataGridView> controle.  
  
 <xref:System.Windows.Forms.BindingSource>  
 Fornece documentação de referência para o <xref:System.Windows.Forms.BindingSource> componente. O <xref:System.Windows.Forms.DataGridView> controle e o <xref:System.Windows.Forms.BindingSource> componente são projetados para trabalhar juntos.  
  
## <a name="see-also"></a>Veja também

- [Controles a serem usados em Windows Forms](controls-to-use-on-windows-forms.md)
