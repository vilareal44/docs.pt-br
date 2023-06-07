---
title: Associar dados ao controle DataGridView
ms.date: 02/08/2019
description: Saiba como o controle DataGridView dá suporte ao modelo de associação de dados padrão Windows Forms para que ele possa ser associado a uma variedade de fontes de dados.
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding [Windows Forms], grids
- data binding [Windows Forms], DataGridView control
- DataGridView control [Windows Forms], data binding
ms.assetid: 1660f69c-5711-45d2-abc1-e25bc6779124
ms.openlocfilehash: 3c3502804d1bc4a5eeffc22b4ec5f2773411ef69
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904410"
---
# <a name="how-to-bind-data-to-the-windows-forms-datagridview-control"></a>Como associar dados ao controle Windows Forms DataGridView

O <xref:System.Windows.Forms.DataGridView> controle dá suporte ao modelo de associação de dados do Windows Forms padrão, portanto, ele pode ser associado a uma variedade de fontes de dados. Normalmente, você se associa a um <xref:System.Windows.Forms.BindingSource> que gerencia a interação com a fonte de dados. O <xref:System.Windows.Forms.BindingSource> pode ser qualquer Windows Forms fonte de dados, que oferece grande flexibilidade ao escolher ou modificar o local de seus dados. Para obter mais informações sobre fontes de dados <xref:System.Windows.Forms.DataGridView> às quais o controle dá suporte, consulte a [visão geral do controle DataGridView](datagridview-control-overview-windows-forms.md).  

O Visual Studio tem amplo suporte para vinculação de dados ao controle DataGridView. Para obter mais informações, consulte [como associar dados ao controle DataGridView Windows Forms usando o designer](bind-data-to-the-datagrid-using-the-designer.md).  

Para conectar um controle DataGridView aos dados:

1. Implemente um método para lidar com os detalhes da recuperação dos dados. O exemplo de código a seguir implementa um `GetData` método que inicializa um <xref:System.Data.SqlClient.SqlDataAdapter> e o usa para preencher um <xref:System.Data.DataTable> . Em seguida, ele associa o <xref:System.Data.DataTable> ao <xref:System.Windows.Forms.BindingSource> .

2. No manipulador de eventos do formulário <xref:System.Windows.Forms.Form.Load> , associe o <xref:System.Windows.Forms.DataGridView> controle ao <xref:System.Windows.Forms.BindingSource> e chame o `GetData` método para recuperar os dados.  

## <a name="example"></a>Exemplo

Este exemplo de código completo recupera dados de um banco de dado para preencher um controle DataGridView em um Windows Form. O formulário também tem botões para recarregar os dados e enviar as alterações para ele.  

Este exemplo requer:

- Acesso a um banco de dados Northwind SQL Server exemplo. Para obter mais informações sobre como instalar o banco de dados de exemplo Northwind, consulte [obter os exemplos de exemplo de ADO.net de código](../../data/adonet/sql/linq/downloading-sample-databases.md).

- Referências ao sistema, System. Windows. Forms, System. Data e System.Xml assemblies.  

Para compilar e executar este exemplo, Cole o código no arquivo de código *Form1* em um novo projeto Windows Forms. Para obter informações sobre como criar a partir da linha de comando do C# ou Visual Basic, consulte [criação de linha de comando com csc.exe](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md) ou [Build na linha de comando](../../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md).  
  
Preencha a `connectionString` variável no exemplo com os valores de sua conexão de banco de dados Northwind SQL Server exemplo. A autenticação do Windows, também chamada de segurança integrada, é uma maneira mais segura de se conectar ao banco de dados do que armazenar uma senha na cadeia de conexão. Para obter mais informações sobre segurança de conexão, consulte [proteger informações de conexão](../../data/adonet/protecting-connection-information.md).  

[!code-csharp[System.Windows.Forms.DataGridViewBoundEditable](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewBoundEditable/CS/datagridviewboundeditable.cs)]
[!code-vb[System.Windows.Forms.DataGridViewBoundEditable](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewBoundEditable/VB/datagridviewboundeditable.vb)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.DataSource%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.BindingSource>
- [Exibir dados no controle Windows Forms DataGridView](displaying-data-in-the-windows-forms-datagridview-control.md)
- [Proteger informações de conexão](../../data/adonet/protecting-connection-information.md)
