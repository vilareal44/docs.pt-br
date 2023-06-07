---
title: Como retornar um resultado de consulta LINQ como um tipo específico
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], specific type returned
- projection [LINQ in Visual Basic]
- anonymous types [Visual Basic]
- querying databases [LINQ]
- queries [LINQ in Visual Basic], how-to topics
- query samples [Visual Basic]
ms.assetid: 621bb10a-e5d7-44fb-a025-317964b19d92
ms.openlocfilehash: c8ed792bf3ffefd903d60522f621958e44546d32
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404946"
---
# <a name="how-to-return-a-linq-query-result-as-a-specific-type-visual-basic"></a>Como retornar um resultado de consulta LINQ como um tipo específico (Visual Basic)
A consulta integrada à linguagem (LINQ) facilita o acesso a informações do banco de dados e a execução de consultas. Por padrão, as consultas LINQ retornam uma lista de objetos como um tipo anônimo. Você também pode especificar que uma consulta retorne uma lista de um tipo específico usando a `Select` cláusula.  
  
 O exemplo a seguir mostra como criar um novo aplicativo que executa consultas em um banco de dados SQL Server e projeta os resultados como um tipo nomeado específico. Para obter mais informações, consulte [tipos anônimos](../objects-and-classes/anonymous-types.md) e [cláusula SELECT](../../../language-reference/queries/select-clause.md).  
  
 Os exemplos neste tópico usam o banco de dados de exemplo Northwind. Caso não tenha esse banco de dados no seu computador de desenvolvimento, você poderá baixá-lo no Centro de Download da Microsoft. Para obter instruções, consulte [baixar bancos de dados de exemplo](../../../../framework/data/adonet/sql/linq/downloading-sample-databases.md).  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-create-a-connection-to-a-database"></a>Para criar uma conexão com um banco de dados  
  
1. No Visual Studio, abra **Gerenciador de servidores** / **Gerenciador de banco de dados** clicando em **Gerenciador de servidores** / **Gerenciador de banco de dados** no menu **Exibir** .  
  
2. Clique com o botão direito do mouse em **conexões de dados** em **Gerenciador de servidores** / **Gerenciador de banco de dados** e clique em **Adicionar conexão**.  
  
3. Especifique uma conexão válida para o banco de dados de exemplo Northwind.  
  
### <a name="to-add-a-project-that-contains-a-linq-to-sql-file"></a>Para adicionar um projeto que contém um arquivo de LINQ to SQL  
  
1. No Visual Studio, no menu **arquivo** , aponte para **novo** e clique em **projeto**. Selecione Visual Basic **aplicativo Windows Forms** como o tipo de projeto.  
  
2. No menu **Projeto** , clique em **Adicionar Novo Item**. Selecione o modelo de item **classes de LINQ to SQL** .  
  
3. Atribua um nome ao arquivo `northwind.dbml`. Clique em **Adicionar**. O Object Relational Designer (O/R Designer) é aberto para o arquivo Northwind. dbml.  
  
### <a name="to-add-tables-to-query-to-the-or-designer"></a>Para adicionar tabelas para consulta ao o/R Designer  
  
1. Em **Gerenciador de servidores** / **Gerenciador de banco de dados**, expanda a conexão com o banco de dados Northwind. Expanda a pasta **Tabelas** .  
  
     Se você fechou o o/R Designer, você pode reabri-lo clicando duas vezes no arquivo Northwind. dbml que você adicionou anteriormente.  
  
2. Clique na tabela clientes e arraste-a para o painel esquerdo do designer.  
  
     O designer cria um novo `Customer` objeto para o seu projeto. Você pode projetar um resultado de consulta como o `Customer` tipo ou como um tipo que você cria. Este exemplo criará um novo tipo em um procedimento posterior e projetará um resultado de consulta como esse tipo.  
  
3. Salve as alterações e feche o designer.  
  
4. Salve seu projeto.  
  
### <a name="to-add-code-to-query-the-database-and-display-the-results"></a>Para adicionar código para consultar o banco de dados e exibir os resultados  
  
1. Na **caixa de ferramentas**, arraste um <xref:System.Windows.Forms.DataGridView> controle para o formulário padrão do Windows para seu projeto, Form1.  
  
2. Clique duas vezes em Form1 para modificar a classe Form1.  
  
3. Após a `End Class` instrução da classe Form1, adicione o código a seguir para criar um `CustomerInfo` tipo para manter os resultados da consulta para este exemplo.  
  
     [!code-vb[VbLINQToSQLHowTos#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form8.vb#16)]  
  
4. Quando você adicionou tabelas ao o/R Designer, o designer adicionou um <xref:System.Data.Linq.DataContext> objeto ao seu projeto. Esse objeto contém o código que você deve ter para acessar essas tabelas e para acessar objetos individuais e coleções para cada tabela. O <xref:System.Data.Linq.DataContext> objeto para seu projeto é nomeado com base no nome do seu arquivo. dbml. Para este projeto, o <xref:System.Data.Linq.DataContext> objeto é nomeado `northwindDataContext` .  
  
     Você pode criar uma instância do <xref:System.Data.Linq.DataContext> no seu código e consultar as tabelas especificadas pelo o/R Designer.  
  
     No `Load` caso da classe Form1, adicione o código a seguir para consultar as tabelas que são expostas como propriedades do seu contexto de dados. A `Select` cláusula da consulta criará um novo `CustomerInfo` tipo em vez de um tipo anônimo para cada item do resultado da consulta.  
  
     [!code-vb[VbLINQToSQLHowTos#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form8.vb#15)]  
  
5. Pressione F5 para executar o projeto e exibir os resultados.  
  
## <a name="see-also"></a>Confira também

- [LINQ](index.md)
- [Consultas](../../../language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [Métodos DataContext (O/R Designer)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
