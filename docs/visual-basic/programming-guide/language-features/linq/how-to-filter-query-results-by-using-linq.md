---
title: Como filtrar resultados de consulta usando LINQ
ms.date: 07/20/2015
helpviewer_keywords:
- filtering [Visual Basic]
- filtering data [LINQ in Visual Basic]
- filtering [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], filtering results
- querying databases [LINQ]
- queries [LINQ in Visual Basic], how-to topics
- query samples [Visual Basic]
- filtering data [Visual Basic]
ms.assetid: ef103092-9bed-4134-97f4-2db696e83c12
ms.openlocfilehash: 4d91783429f24abfe4149217542f8f7a6073bfef
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404985"
---
# <a name="how-to-filter-query-results-by-using-linq-visual-basic"></a>Como filtrar resultados de consulta usando LINQ (Visual Basic)

A consulta integrada à linguagem (LINQ) facilita o acesso a informações do banco de dados e a execução de consultas.

O exemplo a seguir mostra como criar um novo aplicativo que executa consultas em um banco de dados SQL Server e filtra os resultados por um valor específico usando a `Where` cláusula. Para obter mais informações, consulte a [cláusula WHERE](../../../language-reference/queries/where-clause.md).

Os exemplos neste tópico usam o banco de dados de exemplo Northwind. Caso não tenha esse banco de dados no seu computador de desenvolvimento, você poderá baixá-lo no Centro de Download da Microsoft. Para obter instruções, consulte [baixar bancos de dados de exemplo](../../../../framework/data/adonet/sql/linq/downloading-sample-databases.md).

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-create-a-connection-to-a-database"></a>Para criar uma conexão com um banco de dados

1. No Visual Studio, abra **Gerenciador de servidores** / **Gerenciador de banco de dados** clicando em **Gerenciador de servidores** / **Gerenciador de banco de dados** no menu **Exibir** .

2. Clique com o botão direito do mouse em **conexões de dados** em **Gerenciador de servidores** / **Gerenciador de banco de dados** e clique em **Adicionar conexão**.

3. Especifique uma conexão válida para o banco de dados de exemplo Northwind.

## <a name="to-add-a-project-that-contains-a-linq-to-sql-file"></a>Para adicionar um projeto que contém um arquivo de LINQ to SQL

1. No Visual Studio, no menu **arquivo** , aponte para **novo** e clique em **projeto**. Selecione Visual Basic **aplicativo Windows Forms** como o tipo de projeto.

2. No menu **Projeto** , clique em **Adicionar Novo Item**. Selecione o modelo de item **classes de LINQ to SQL** .

3. Atribua um nome ao arquivo `northwind.dbml`. Clique em **Adicionar**. O Object Relational Designer (O/R Designer) é aberto para o arquivo Northwind. dbml.

## <a name="to-add-tables-to-query-to-the-or-designer"></a>Para adicionar tabelas para consulta ao o/R Designer

1. Em **Gerenciador de servidores** / **Gerenciador de banco de dados**, expanda a conexão com o banco de dados Northwind. Expanda a pasta **Tabelas** .

     Se você fechou o o/R Designer, você pode reabri-lo clicando duas vezes no arquivo Northwind. dbml que você adicionou anteriormente.

2. Clique na tabela clientes e arraste-a para o painel esquerdo do designer. Clique na tabela Orders e arraste-a para o painel esquerdo do designer.

     O designer cria novos `Customer` `Order` objetos e para o seu projeto. Observe que o designer detecta automaticamente as relações entre as tabelas e cria propriedades filhas para objetos relacionados. Por exemplo, o IntelliSense mostrará que o `Customer` objeto tem uma `Orders` propriedade para todos os pedidos relacionados a esse cliente.

3. Salve as alterações e feche o designer.

4. Salve seu projeto.

## <a name="to-add-code-to-query-the-database-and-display-the-results"></a>Para adicionar código para consultar o banco de dados e exibir os resultados

1. Na **caixa de ferramentas**, arraste um <xref:System.Windows.Forms.DataGridView> controle para o formulário padrão do Windows para seu projeto, Form1.

2. Clique duas vezes em Form1 para adicionar código ao `Load` evento do formulário.

3. Quando você adicionou tabelas ao o/R Designer, o designer adicionou um <xref:System.Data.Linq.DataContext> objeto ao seu projeto. Esse objeto contém o código que você deve ter para acessar essas tabelas, além de objetos individuais e coleções para cada tabela. O <xref:System.Data.Linq.DataContext> objeto para seu projeto é nomeado com base no nome do seu arquivo. dbml. Para este projeto, o <xref:System.Data.Linq.DataContext> objeto é nomeado `northwindDataContext` .

    Você pode criar uma instância do <xref:System.Data.Linq.DataContext> no seu código e consultar as tabelas especificadas pelo o/R Designer.

    Adicione o seguinte código ao `Load` evento para consultar as tabelas que são expostas como propriedades do seu contexto de dados. A consulta filtra os resultados e retorna somente os clientes que estão localizados no `London` .

    [!code-vb[VbLINQToSQLHowTos#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form5.vb#11)]

4. Pressione F5 para executar o projeto e exibir os resultados.

5. A seguir estão alguns outros filtros que você pode tentar.

    [!code-vb[VbLINQToSQLHowTos#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form5.vb#12)]

## <a name="see-also"></a>Confira também

- [LINQ](index.md)
- [Consultas](../../../language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [Métodos DataContext (O/R Designer)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
