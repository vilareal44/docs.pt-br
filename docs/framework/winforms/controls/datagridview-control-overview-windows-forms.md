---
title: Visão geral do controle DataGridView
description: Saiba como usar o controle Windows Forms DataGridView para exibir e editar dados tabulares de vários tipos diferentes de fontes de dados.
ms.date: 03/30/2017
f1_keywords:
- DataGridView
helpviewer_keywords:
- DataGridView control [Windows Forms], about DataGridView control
- grid controls [Windows Forms]
- tables [Windows Forms], displaying in DataGridView control
- tables [Windows Forms], binding to DataGridView control
- columns [Windows Forms], DataGridView control
- bound controls [Windows Forms], dataGridView control
- datasets [Windows Forms], binding to DataGridView control
- data grids [Windows Forms], about data grids
- data [Windows Forms], resorting
- data [Windows Forms], navigating
- grids [Windows Forms]
- data binding [Windows Forms], DataGridView control
- data sources [Windows Forms], binding to DataGridView control
- DataGridView control [Windows Forms], data binding
ms.assetid: 0a45c661-89dc-4390-9cc6-c47eee501488
ms.openlocfilehash: 3e68f536853081453f6ba746d342bc016bc8ca17
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174608"
---
# <a name="datagridview-control-overview-windows-forms"></a>Visão geral do controle DataGridView (Windows Forms)
> [!NOTE]
> O controle <xref:System.Windows.Forms.DataGridView> substitui e adiciona funcionalidade ao controle <xref:System.Windows.Forms.DataGrid>, no entanto, o controle <xref:System.Windows.Forms.DataGrid> é mantido para compatibilidade com versões anteriores e para uso futuro, se desejado. Para obter mais informações, consulte [diferenças entre os controles Windows Forms DataGridView e DataGrid](differences-between-the-windows-forms-datagridview-and-datagrid-controls.md).  
  
 Com o <xref:System.Windows.Forms.DataGridView> controle, você pode exibir e editar dados tabulares de vários tipos diferentes de fontes de dados.  
  
 A vinculação de dados ao <xref:System.Windows.Forms.DataGridView> controle é direta e intuitiva e, em muitos casos, é tão simples quanto definir a <xref:System.Windows.Forms.DataGridView.DataSource%2A> propriedade. Ao associar a uma fonte de dados que contém várias listas ou tabelas, defina a <xref:System.Windows.Forms.DataGridView.DataMember%2A> propriedade como uma cadeia de caracteres que especifica a lista ou tabela à qual associar.  
  
 O <xref:System.Windows.Forms.DataGridView> controle dá suporte ao modelo de associação de dados padrão Windows Forms, portanto, ele se associará a instâncias de classes descritas na lista a seguir:  
  
- Qualquer classe que implemente a <xref:System.Collections.IList> interface, incluindo matrizes unidimensionais.  
  
- Qualquer classe que implemente a <xref:System.ComponentModel.IListSource> interface, como as <xref:System.Data.DataTable> <xref:System.Data.DataSet> classes e.  
  
- Qualquer classe que implemente a <xref:System.ComponentModel.IBindingList> interface, como a <xref:System.ComponentModel.BindingList%601> classe.  
  
- Qualquer classe que implemente a <xref:System.ComponentModel.IBindingListView> interface, como a <xref:System.Windows.Forms.BindingSource> classe.  
  
 O <xref:System.Windows.Forms.DataGridView> controle oferece suporte à ligação de dados com as propriedades públicas dos objetos retornados por essas interfaces ou à coleção Properties retornada por uma <xref:System.ComponentModel.ICustomTypeDescriptor> interface, se implementada nos objetos retornados.  
  
 Normalmente, você se associará a um <xref:System.Windows.Forms.BindingSource> componente e associará o <xref:System.Windows.Forms.BindingSource> componente a outra fonte de dados ou o preencherá com objetos comerciais. O <xref:System.Windows.Forms.BindingSource> componente é a fonte de dados preferida porque pode ser associado a uma ampla variedade de fontes de dados e pode resolver vários problemas de ligação de dados automaticamente. Para obter mais informações, consulte [Componente BindingSource](bindingsource-component.md).  
  
 O <xref:System.Windows.Forms.DataGridView> controle também pode ser usado no modo não *associado* , sem nenhum armazenamento de dados subjacente. Para obter um exemplo de código que usa um <xref:System.Windows.Forms.DataGridView> controle não associado, consulte [Walkthrough: Criando um controle Windows Forms DataGridView não associado](walkthrough-creating-an-unbound-windows-forms-datagridview-control.md).  
  
 O <xref:System.Windows.Forms.DataGridView> controle é altamente configurável e extensível e fornece muitas propriedades, métodos e eventos para personalizar sua aparência e comportamento. Quando você quiser que seu aplicativo Windows Forms exiba dados tabulares, considere usar o <xref:System.Windows.Forms.DataGridView> controle antes de outros (por exemplo, <xref:System.Windows.Forms.DataGrid> ). Se você estiver exibindo uma pequena grade de valores somente leitura ou se estiver permitindo que um usuário edite uma tabela com milhões de registros, o <xref:System.Windows.Forms.DataGridView> controle fornecerá uma solução prontamente programável e com economia de memória.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Resumo de tecnologia do controle DataGridView](datagridview-control-technology-summary-windows-forms.md)  
 Resume <xref:System.Windows.Forms.DataGridView> os conceitos de controle e o uso de classes relacionadas.  
  
 [Arquitetura de controle DataGridView](datagridview-control-architecture-windows-forms.md)  
 Descreve a arquitetura do <xref:System.Windows.Forms.DataGridView> controle, explicando sua hierarquia de tipos e estrutura de herança.  
  
 [Cenários do controle DataGridView](datagridview-control-scenarios-windows-forms.md)  
 Descreve os cenários mais comuns em que os <xref:System.Windows.Forms.DataGridView> controles são usados.  
  
 [Diretório de código do controle DataGridView](datagridview-control-code-directory-windows-forms.md)  
 Fornece links para exemplos de código na documentação para várias <xref:System.Windows.Forms.DataGridView> tarefas. Esses exemplos são categorizados por tipo de tarefa.  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Tipos de coluna no controle DataGridView dos Windows Forms](column-types-in-the-windows-forms-datagridview-control.md)  
 Discute os tipos de coluna no controle de Windows Forms <xref:System.Windows.Forms.DataGridView> usado para exibir informações e permitir que os usuários modifiquem ou adicionem informações.  
  
 [Exibindo dados no controle DataGridView dos Windows Forms](displaying-data-in-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem como preencher o controle com os dados manualmente ou de uma fonte de dados externa.  
  
 [Personalizando o controle DataGridView dos Windows Forms](customizing-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem a pintura personalizada de células e linhas <xref:System.Windows.Forms.DataGridView>, bem como a criação de tipos de célula, coluna e linha derivados.  
  
 [Ajuste de desempenho no controle DataGridView dos Windows Forms](performance-tuning-in-the-windows-forms-datagridview-control.md)  
 Fornece tópicos que descrevem como usar o controle com eficiência para evitar problemas de desempenho ao trabalhar com grandes volumes de dados.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.BindingSource>
- [Controle DataGridView](datagridview-control-windows-forms.md)
- [Funcionalidade padrão no controle DataGridView dos Windows Forms](default-functionality-in-the-windows-forms-datagridview-control.md)
- [Manipulação de teclado e mouse padrão no controle DataGridView dos Windows Forms](default-keyboard-and-mouse-handling-in-the-windows-forms-datagridview-control.md)
