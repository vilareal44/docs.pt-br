---
title: Como modificar dados em um banco de dados usando LINQ
ms.date: 07/20/2015
helpviewer_keywords:
- inserting rows [LINQ to SQL]
- deleting rows [LINQ to SQL]
- updating rows [LINQ to SQL]
- inserting data [Visual Basic]
- deleting data
- data [Visual Basic], updating
- updating data [LINQ]
- queries [LINQ in Visual Basic], data changes in database
- queries [LINQ in Visual Basic], how-to topics
ms.assetid: cf52635f-0c1b-46c3-aff1-bdf181cf19b1
ms.openlocfilehash: eb076d9156fa66858f2e560422eef0dc61ba22b5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403479"
---
# <a name="how-to-modify-data-in-a-database-by-using-linq-visual-basic"></a>Como modificar dados em um banco de dados usando LINQ (Visual Basic)

As consultas de LINQ (consulta integrada à linguagem) facilitam o acesso a informações do banco de dados e a modificação de valores no banco de dados.

O exemplo a seguir mostra como criar um novo aplicativo que recupera e atualiza informações em um banco de dados SQL Server.

Os exemplos neste tópico usam o banco de dados de exemplo Northwind. Caso não tenha esse banco de dados no seu computador de desenvolvimento, você poderá baixá-lo no Centro de Download da Microsoft. Para obter instruções, consulte [baixar bancos de dados de exemplo](../../../../framework/data/adonet/sql/linq/downloading-sample-databases.md).

### <a name="to-create-a-connection-to-a-database"></a>Para criar uma conexão com um banco de dados

1. No Visual Studio, abra **Gerenciador de servidores** / **Gerenciador de banco de dados** clicando no menu **Exibir** e, em seguida, selecione **Gerenciador de servidores** / **Gerenciador de banco de dados**.

2. Clique com o botão direito do mouse em **conexões de dados** no **Gerenciador de servidores** / **Gerenciador de banco de dados**e clique em **Adicionar conexão**.

3. Especifique uma conexão válida para o banco de dados de exemplo Northwind.

### <a name="to-add-a-project-with-a-linq-to-sql-file"></a>Para adicionar um projeto com um arquivo LINQ to SQL

1. No Visual Studio, no menu **arquivo** , aponte para **novo** e clique em **projeto**. Selecione Visual Basic **aplicativo Windows Forms** como o tipo de projeto.

2. No menu **Projeto** , clique em **Adicionar Novo Item**. Selecione o modelo de item **classes de LINQ to SQL** .

3. Atribua um nome ao arquivo `northwind.dbml`. Clique em **Adicionar**. O Object Relational Designer (O/R Designer) é aberto para o `northwind.dbml` arquivo.

### <a name="to-add-tables-to-query-and-modify-to-the-designer"></a>Para adicionar tabelas à consulta e modificar para o designer

1. Em **Gerenciador de servidores** / **Gerenciador de banco de dados**, expanda a conexão com o banco de dados Northwind. Expanda a pasta **Tabelas** .

     Se você fechou o o/R Designer, você pode reabri-lo clicando duas vezes no `northwind.dbml` arquivo que você adicionou anteriormente.

2. Clique na tabela clientes e arraste-a para o painel esquerdo do designer.

     O designer cria um novo objeto de cliente para seu projeto.

3. Salve as alterações e feche o designer.

4. Salve seu projeto.

### <a name="to-add-code-to-modify-the-database-and-display-the-results"></a>Para adicionar código para modificar o banco de dados e exibir os resultados

1. Na **caixa de ferramentas**, arraste um <xref:System.Windows.Forms.DataGridView> controle para o formulário padrão do Windows para seu projeto, Form1.

2. Quando você adicionou tabelas ao o/R Designer, o designer adicionou um <xref:System.Data.Linq.DataContext> objeto ao seu projeto. Este objeto contém código que você pode usar para acessar a tabela Customers. Ele também contém um código que define um objeto Customer local e uma coleção Customers para a tabela. O <xref:System.Data.Linq.DataContext> objeto para seu projeto é nomeado com base no nome do seu arquivo. dbml. Para este projeto, o <xref:System.Data.Linq.DataContext> objeto é nomeado `northwindDataContext` .

     Você pode criar uma instância do <xref:System.Data.Linq.DataContext> objeto em seu código e consultar e modificar a coleção Customers especificada pelo o/R Designer. As alterações feitas na coleção clientes não são refletidas no banco de dados até que você as envie chamando o <xref:System.Data.Linq.DataContext.SubmitChanges%2A> método do <xref:System.Data.Linq.DataContext> objeto.

     Clique duas vezes no Windows Form, Form1, para adicionar código ao <xref:System.Windows.Forms.Form.Load> evento para consultar a tabela Customers que é exposta como uma propriedade do seu <xref:System.Data.Linq.DataContext> . Adicione os códigos a seguir:

    ```vb
    Private db As northwindDataContext

    Private Sub Form1_Load(ByVal sender As System.Object,
                           ByVal e As System.EventArgs
                          ) Handles MyBase.Load
      db = New northwindDataContext()

      RefreshData()
    End Sub

    Private Sub RefreshData()
      Dim customers = From cust In db.Customers
                      Where cust.City(0) = "W"
                      Select cust

      DataGridView1.DataSource = customers
    End Sub
    ```

3. Na **caixa de ferramentas**, arraste três <xref:System.Windows.Forms.Button> controles para o formulário. Selecione o primeiro `Button` controle. Na janela **Propriedades** , defina o `Name` do `Button` controle como `AddButton` e o `Text` como `Add` . Selecione o segundo botão e defina a `Name` propriedade como `UpdateButton` e a `Text` propriedade como `Update` . Selecione o terceiro botão e defina a `Name` propriedade como `DeleteButton` e a `Text` propriedade como `Delete` .

4. Clique duas vezes no botão **Adicionar** para adicionar código ao seu `Click` evento. Adicione os códigos a seguir:

    ```vb
    Private Sub AddButton_Click(ByVal sender As System.Object,
                                ByVal e As System.EventArgs
                               ) Handles AddButton.Click
      Dim cust As New Customer With {
        .City = "Wellington",
        .CompanyName = "Blue Yonder Airlines",
        .ContactName = "Jill Frank",
        .Country = "New Zealand",
        .CustomerID = "JILLF"}

      db.Customers.InsertOnSubmit(cust)

      Try
        db.SubmitChanges()
      Catch
        ' Handle exception.
      End Try

      RefreshData()
    End Sub
    ```

5. Clique duas vezes no botão **Atualizar** para adicionar código ao seu `Click` evento. Adicione os códigos a seguir:

    ```vb
    Private Sub UpdateButton_Click(ByVal sender As System.Object, _
                                   ByVal e As System.EventArgs
                                  ) Handles UpdateButton.Click
      Dim updateCust = (From cust In db.Customers
                        Where cust.CustomerID = "JILLF").ToList()(0)

      updateCust.ContactName = "Jill Shrader"
      updateCust.Country = "Wales"
      updateCust.CompanyName = "Red Yonder Airlines"
      updateCust.City = "Cardiff"

      Try
        db.SubmitChanges()
      Catch
        ' Handle exception.
      End Try

      RefreshData()
    End Sub
    ```

6. Clique duas vezes no botão **excluir** para adicionar código ao seu `Click` evento. Adicione os códigos a seguir:

    ```vb
    Private Sub DeleteButton_Click(ByVal sender As System.Object, _
                                   ByVal e As System.EventArgs
                                  ) Handles DeleteButton.Click
      Dim deleteCust = (From cust In db.Customers
                        Where cust.CustomerID = "JILLF").ToList()(0)

      db.Customers.DeleteOnSubmit(deleteCust)

      Try
        db.SubmitChanges()
      Catch
        ' Handle exception.
      End Try

      RefreshData()
    End Sub
    ```

7. Pressione F5 para executar o projeto. Clique em **Adicionar** para adicionar um novo registro. Clique em **Atualizar** para modificar o novo registro. Clique em **excluir** para excluir o novo registro.

## <a name="see-also"></a>Confira também

- [LINQ](index.md)
- [Consultas](../../../language-reference/queries/index.md)
- [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)
- [Métodos DataContext (O/R Designer)](/visualstudio/data-tools/datacontext-methods-o-r-designer)
- [Como atribuir procedimentos armazenados para executar atualizações, inserções e exclusões (Designer Relacional de Objetos)](/visualstudio/data-tools/how-to-assign-stored-procedures-to-perform-updates-inserts-and-deletes-o-r-designer)
