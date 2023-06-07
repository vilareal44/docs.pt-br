---
title: 'Passo a passo: exibir dados de um banco de dados do SQL Server em um controle DataGrid'
description: Saiba como obter dados de um banco de SQL Server e exibi-los em um controle DataGrid Windows Presentation Foundation usando este passo a passos.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGrid [WPF], displaying data from SQL Server
- controls [WPF], DataGrid
ms.assetid: 6810b048-0a23-4f86-bfa5-97f92b3cfab4
ms.openlocfilehash: cc41979c869021c9c363f3f68ce590d4702e068c
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87167549"
---
# <a name="walkthrough-display-data-from-a-sql-server-database-in-a-datagrid-control"></a>Walkthrough: exibir dados de um SQL Server banco de dado em um controle DataGrid

Neste passo a passos, você recupera dados de um SQL Server um banco de dados e os exibe em um <xref:System.Windows.Controls.DataGrid> controle. O ADO.NET Entity Framework é usado para criar as classes de entidade que representam os dados e o LINQ é usado para gravar uma consulta que recupera os dados especificados de uma classe de entidade.

## <a name="prerequisites"></a>Pré-requisitos

Você precisará dos seguintes componentes para concluir este passo a passo:

- Visual Studio.

- Acesso a uma instância em execução do SQL Server ou SQL Server Express que tem o banco de dados de exemplo AdventureWorks anexado a ele. Você pode baixar o banco de dados AdventureWorks do [GitHub](https://github.com/Microsoft/sql-server-samples/releases).

## <a name="create-entity-classes"></a>Criar classes de entidade

1. Crie um novo projeto de Aplicativo do WPF no Visual Basic ou no C#, chamado `DataGridSQLExample`.

2. No Gerenciador de Soluções, clique com o botão direito do mouse no projeto, aponte para **Adicionar** e selecione **Novo Item**.

     A caixa de diálogo Adicionar Novo Item é exibida.

3. No painel modelos instalados, selecione **dados** e, na lista de modelos, selecione **ADO.NET modelo de dados de entidade**.

     ![ADO.NET modelo de item de Modelo de Dados de Entidade](../../wcf/feature-details/media/ado-net-entity-data-model-item-template.png)

4. Nomeie o arquivo `AdventureWorksModel.edmx` e, em seguida, clique em **Adicionar**.

     O Assistente do Modelo de Dados de Entidade é aberto.

5. Na tela escolher conteúdo do modelo, selecione **designer do EF no banco de dados** e clique em **Avançar**.

6. Na tela Escolher a Conexão de Dados, forneça a conexão ao banco de dados AdventureWorksLT2008. Para obter mais informações, consulte [Escolher uma Caixa de Diálogo de Conexão de Dados](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb399244(v=vs.100)).

    Verifique se o nome é `AdventureWorksLT2008Entities` e se a caixa de seleção **salvar configurações de conexão de entidade no App.Config como** está marcada e clique em **Avançar**.

7. Na tela Escolher Objetos de Banco de Dados, expanda o nó Tabelas e selecione as tabelas **Product** e **ProductCategory**.

     É possível gerar classes de entidade para todas as tabelas; no entanto, neste exemplo, somente os dados dessas duas tabelas serão recuperados.

     ![Selecionar produto e ProductCategory de tabelas](./media/datagrid-sql-ef-step4.png "DataGrid_SQL_EF_Step4")

8. Clique em **Concluir**.

     As entidades Product e ProductCategory serão exibidas no Entity Designer.

     ![Modelos de entidade produto e ProductCategory](./media/datagrid-sql-ef-step5.png "DataGrid_SQL_EF_Step5")

## <a name="retrieve-and-present-the-data"></a>Recuperar e apresentar os dados

1. Abra o arquivo MainWindow.xaml.

2. Defina a <xref:System.Windows.FrameworkElement.Width%2A> propriedade no <xref:System.Windows.Window> como 450.

3. No editor XAML, adicione a seguinte <xref:System.Windows.Controls.DataGrid> marca entre as `<Grid>` marcas e `</Grid>` para adicionar um <xref:System.Windows.Controls.DataGrid> nome `dataGrid1` .

     [!code-xaml[DataGrid_SQL_EF_Walkthrough#3](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_SQL_EF_Walkthrough/CS/MainWindow.xaml#3)]

     ![Janela com DataGrid](./media/datagrid-sql-ef-step6.png "DataGrid_SQL_EF_Step6")

4. Selecione o <xref:System.Windows.Window>.

5. Usando o editor de janela Propriedades ou XAML, crie um manipulador de eventos para o <xref:System.Windows.Window> nome do `Window_Loaded` <xref:System.Windows.FrameworkElement.Loaded> evento. Para obter mais informações, consulte [como: criar um manipulador de eventos simples](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/bb675300(v=vs.100)).

     O exemplo a seguir mostra o XAML de MainWindow.xaml.

    > [!NOTE]
    > Se você estiver usando o Visual Basic, na primeira linha de MainWindow.xaml, substitua `x:Class="DataGridSQLExample.MainWindow"` por `x:Class="MainWindow"`.

     [!code-xaml[DataGrid_SQL_EF_Walkthrough#1](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_SQL_EF_Walkthrough/CS/MainWindow.xaml#1)]

6. Abra o arquivo code-behind (MainWindow. XAML. vb ou MainWindow.xaml.cs) para o <xref:System.Windows.Window> .

7. Adicione o código a seguir para recuperar apenas valores específicos das tabelas Unidas e definir a <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> Propriedade do <xref:System.Windows.Controls.DataGrid> para os resultados da consulta.

     [!code-csharp[DataGrid_SQL_EF_Walkthrough#2](~/samples/snippets/csharp/VS_Snippets_Wpf/DataGrid_SQL_EF_Walkthrough/CS/MainWindow.xaml.cs#2)]
     [!code-vb[DataGrid_SQL_EF_Walkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DataGrid_SQL_EF_Walkthrough/VB/MainWindow.xaml.vb#2)]

8. Execute o exemplo.

     Você deve ver um <xref:System.Windows.Controls.DataGrid> que exibe dados.

     ![DataGrid com dados do banco de dados SQL](./media/datagrid-sql-ef-step7.png "DataGrid_SQL_EF_Step7")

## <a name="see-also"></a>Confira também

- <xref:System.Windows.Controls.DataGrid>
