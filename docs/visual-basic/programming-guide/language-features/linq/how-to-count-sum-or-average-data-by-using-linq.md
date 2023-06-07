---
title: Como contar, somar ou fazer média de dados usando LINQ
ms.date: 07/20/2015
helpviewer_keywords:
- average operator [LINQ in Visual Basic]
- aggregate operator [LINQ in Visual Basic]
- aggregate queries
- queries [LINQ in Visual Basic], sum results
- Aggregate clause [Visual Basic]
- sum operator [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], aggregate queries
- queries [LINQ in Visual Basic], counting results
- querying databases [LINQ]
- queries [LINQ in Visual Basic], how-to topics
- query samples [Visual Basic]
- count operator [LINQ in Visual Basic]
ms.assetid: 51ca1f59-7770-4884-8b76-113002e54fc0
ms.openlocfilehash: 8be585c3e11bc3637b2dd1cfaf3437620aa2ba09
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84405011"
---
# <a name="how-to-count-sum-or-average-data-by-using-linq-visual-basic"></a>Como contar, somar ou fazer média de dados usando LINQ (Visual Basic)
A consulta integrada à linguagem (LINQ) facilita o acesso a informações do banco de dados e a execução de consultas.  
  
 O exemplo a seguir mostra como criar um novo aplicativo que executa consultas em um banco de dados SQL Server. O exemplo conta, soma e calcula a média dos resultados usando as `Aggregate` `Group By` cláusulas e. Para obter mais informações, consulte [cláusula Aggregate](../../../language-reference/queries/aggregate-clause.md) e [cláusula Group by](../../../language-reference/queries/group-by-clause.md).  
  
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
  
2. Clique na tabela clientes e arraste-a para o painel esquerdo do designer. Clique na tabela Orders e arraste-a para o painel esquerdo do designer.  
  
     O designer cria novos `Customer` `Order` objetos e para o seu projeto. Observe que o designer detecta automaticamente as relações entre as tabelas e cria propriedades filhas para objetos relacionados. Por exemplo, o IntelliSense mostrará que o `Customer` objeto tem uma `Orders` propriedade para todos os pedidos relacionados a esse cliente.  
  
3. Salve as alterações e feche o designer.  
  
4. Salve seu projeto.  
  
### <a name="to-add-code-to-query-the-database-and-display-the-results"></a>Para adicionar código para consultar o banco de dados e exibir os resultados  
  
1. Na **caixa de ferramentas**, arraste um <xref:System.Windows.Forms.DataGridView> controle para o formulário padrão do Windows para seu projeto, Form1.  
  
2. Clique duas vezes em Form1 para adicionar código ao `Load` evento do formulário.  
  
3. Quando você adicionou tabelas ao o/R Designer, o designer adicionou um <xref:System.Data.Linq.DataContext> objeto ao seu projeto. Esse objeto contém o código que você deve ter para acessar essas tabelas e para acessar objetos individuais e coleções para cada tabela. O <xref:System.Data.Linq.DataContext> objeto para seu projeto é nomeado com base no nome do seu arquivo. dbml. Para este projeto, o <xref:System.Data.Linq.DataContext> objeto é nomeado `northwindDataContext` .  
  
     Você pode criar uma instância do <xref:System.Data.Linq.DataContext> no seu código e consultar as tabelas especificadas pelo o/R Designer.  
  
     Adicione o código a seguir ao `Load` evento para consultar as tabelas que são expostas como propriedades de seu <xref:System.Data.Linq.DataContext> e contagem, soma e média dos resultados. O exemplo usa a `Aggregate` cláusula para consultar um único resultado e a `Group By` cláusula para mostrar uma média para resultados agrupados.  
  
     [!code-vb[VbLINQToSQLHowTos#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQtoSQLHowTos/VB/Form6.vb#13)]  
  
4. Pressione F5 para executar o projeto e exibir os resultados.  
  
## <a name="see-also"></a>Confira também

- [LINQ](index.md)
- [Consultas](../../../language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [Métodos DataContext (O/R Designer)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
- [Cláusula Aggregate](../../../language-reference/queries/aggregate-clause.md)
- [Cláusula Group By](../../../language-reference/queries/group-by-clause.md)
