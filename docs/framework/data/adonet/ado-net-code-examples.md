---
title: Exemplos de código
description: Esses exemplos mostram aos programadores de .NET Framework como recuperar dados de um banco de dados usando ADO.NET data providers e ADO.NET Entity Framework.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c119657a-9ce6-4940-91e4-ac1d5f0d9584
ms.openlocfilehash: 54df0e253716c970cf23446434d96b104b8e9b03
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287162"
---
# <a name="adonet-code-examples"></a>Exemplos de código ADO.NET

As listagens de código nesta página demonstram como recuperar dados de um banco de dado usando as seguintes tecnologias ADO.NET:

- Provedores de dados do ADO.NET:

  - [SqlClient](#sqlclient) ( `System.Data.SqlClient` )

  - [OLEDB](#oledb) ( `System.Data.OleDb` )

  - [ODBC](#odbc) ( `System.Data.Odbc` )

  - [OracleClient](#oracleclient) ( `System.Data.OracleClient` )

- ADO.NET Entity Framework:

  - [LINQ to Entities](#linq-to-entities)

  - [ObjectQuery tipado](#typed-objectquery)

  - [EntityClient](#entityclient) ( `System.Data.EntityClient` )

- [LINQ to SQL](#linq-to-sql)

## <a name="adonet-data-provider-examples"></a>Exemplos do provedor de dados ADO.NET
As listagens de código a seguir demonstram como recuperar dados de um banco de dados usando provedores de dados do ADO.NET. Os dados são retornados em um `DataReader`. Para obter mais informações, consulte [recuperando dados usando um DataReader](retrieving-data-using-a-datareader.md).

### <a name="sqlclient"></a>SqlClient
O código neste exemplo pressupõe que você pode se conectar ao `Northwind` banco de dados de exemplo no Microsoft SQL Server. O código cria um <xref:System.Data.SqlClient.SqlCommand> para selecionar linhas da tabela Products, adicionando um <xref:System.Data.SqlClient.SqlParameter> para restringir os resultados às linhas com um UnitPrice maior que o valor do parâmetro especificado, neste caso, 5. O <xref:System.Data.SqlClient.SqlConnection> é aberto dentro de um `using` bloco, o que garante que os recursos sejam fechados e descartados quando o código sair. O código executa o comando usando um <xref:System.Data.SqlClient.SqlDataReader> e exibe os resultados na janela do console.

 [!code-csharp[DataWorks SampleApp.SqlClient#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.SqlClient/CS/source.cs#1)]
 [!code-vb[DataWorks SampleApp.SqlClient#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.SqlClient/VB/source.vb#1)]

### <a name="oledb"></a>OleDb
O código neste exemplo pressupõe que você pode se conectar ao banco de dados Northwind do Microsoft Access. O código cria um <xref:System.Data.OleDb.OleDbCommand> para selecionar linhas da tabela Products, adicionando um <xref:System.Data.OleDb.OleDbParameter> para restringir os resultados às linhas com um UnitPrice maior que o valor do parâmetro especificado, neste caso, 5. O <xref:System.Data.OleDb.OleDbConnection> é aberto dentro de um bloco `using`, o que garante que os recursos sejam fechados e descartados quando o código for fechado. O código executa o comando usando um <xref:System.Data.OleDb.OleDbDataReader> e exibe os resultados na janela do console.

 [!code-csharp[DataWorks SampleApp.OleDb#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.OleDb/CS/source.cs#1)]
 [!code-vb[DataWorks SampleApp.OleDb#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.OleDb/VB/source.vb#1)]

### <a name="odbc"></a>Odbc
O código neste exemplo pressupõe que você pode se conectar ao banco de dados Northwind do Microsoft Access. O código cria um <xref:System.Data.Odbc.OdbcCommand> para selecionar linhas da tabela Products, adicionando um <xref:System.Data.Odbc.OdbcParameter> para restringir os resultados às linhas com um UnitPrice maior que o valor do parâmetro especificado, neste caso, 5. O <xref:System.Data.Odbc.OdbcConnection> é aberto dentro de um `using` bloco, o que garante que os recursos sejam fechados e descartados quando o código sair. O código executa o comando usando um <xref:System.Data.Odbc.OdbcDataReader> e exibe os resultados na janela do console.

[!code-csharp[DataWorks SampleApp.Odbc#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.Odbc/CS/source.cs#1)]
[!code-vb[DataWorks SampleApp.Odbc#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.Odbc/VB/source.vb#1)]

### <a name="oracleclient"></a>OracleClient
O código neste exemplo pressupõe uma conexão a DEMO.CUSTOMER em um servidor Oracle. Você também deve adicionar uma referência ao System.Data.OracleClient.dll. O código retorna os dados em um <xref:System.Data.OracleClient.OracleDataReader>.

 [!code-csharp[DataWorks SampleApp.Oracle#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.Oracle/CS/source.cs#1)]
 [!code-vb[DataWorks SampleApp.Oracle#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.Oracle/VB/source.vb#1)]

## <a name="entity-framework-examples"></a>Exemplos de Entity Framework
As seguintes listagens de código demonstram como recuperar dados de uma fonte de dados consultando entidades em um EDM (Modelo de Dados de Entidade). Esses exemplos usam um modelo baseado no banco de dados de exemplo Northwind. Para obter mais informações sobre Entity Framework, consulte [Entity Framework visão geral](./ef/overview.md).

### <a name="linq-to-entities"></a>LINQ to Entities
O código neste exemplo usa uma consulta LINQ para retornar dados como objetos Categories, que são projetados como um tipo anônimo que contém somente as propriedades CategoryID e CategoryName. Para obter mais informações, consulte [LINQ to Entities visão geral](./ef/language-reference/linq-to-entities.md).

```csharp
using System;
using System.Linq;
using System.Data.Objects;
using NorthwindModel;

class LinqSample
{
    public static void ExecuteQuery()
    {
        using (NorthwindEntities context = new NorthwindEntities())
        {
            try
            {
                var query = from category in context.Categories
                            select new
                            {
                                categoryID = category.CategoryID,
                                categoryName = category.CategoryName
                            };

                foreach (var categoryInfo in query)
                {
                    Console.WriteLine("\t{0}\t{1}",
                        categoryInfo.categoryID, categoryInfo.categoryName);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    }
}
```

```vb
Option Explicit On
Option Strict On

Imports System.Linq
Imports System.Data.Objects
Imports NorthwindModel

Class LinqSample
    Public Shared Sub ExecuteQuery()
        Using context As NorthwindEntities = New NorthwindEntities()
            Try
                Dim query = From category In context.Categories _
                            Select New With _
                            { _
                                .categoryID = category.CategoryID, _
                                .categoryName = category.CategoryName _
                            }

                For Each categoryInfo In query
                    Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                        categoryInfo.categoryID, categoryInfo.categoryName)
                Next
            Catch ex As Exception
                Console.WriteLine(ex.Message)
            End Try
        End Using
    End Sub
End Class
```

### <a name="typed-objectquery"></a>ObjectQuery tipado
O código neste exemplo usa um <xref:System.Data.Objects.ObjectQuery%601> para retornar dados como objetos Categories. Para obter mais informações, consulte [consultas de objeto](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb896241(v=vs.100)).

```csharp
using System;
using System.Data.Objects;
using NorthwindModel;

class ObjectQuerySample
{
    public static void ExecuteQuery()
    {
        using (NorthwindEntities context = new NorthwindEntities())
        {
            ObjectQuery<Categories> categoryQuery = context.Categories;

            foreach (Categories category in
                categoryQuery.Execute(MergeOption.AppendOnly))
            {
                Console.WriteLine("\t{0}\t{1}",
                    category.CategoryID, category.CategoryName);
            }
        }
    }
}
```

```vb
Option Explicit On
Option Strict On

Imports System.Data.Objects
Imports NorthwindModel

Class ObjectQuerySample
    Public Shared Sub ExecuteQuery()
        Using context As NorthwindEntities = New NorthwindEntities()
            Dim categoryQuery As ObjectQuery(Of Categories) = context.Categories

            For Each category As Categories In _
                categoryQuery.Execute(MergeOption.AppendOnly)
                Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                    category.CategoryID, category.CategoryName)
            Next
        End Using
    End Sub
End Class
```

### <a name="entityclient"></a>EntityClient
O código neste exemplo usa um <xref:System.Data.EntityClient.EntityCommand> para executar uma consulta Entity SQL. Esta consulta retorna uma lista de registros que representam instâncias do tipo de entidade Categories. Um <xref:System.Data.EntityClient.EntityDataReader> é usado para acessar os registros de dados no conjunto de resultados. Para obter mais informações, consulte [EntityClient Provider para o Entity Framework](./ef/entityclient-provider-for-the-entity-framework.md).

```csharp
using System;
using System.Data;
using System.Data.Common;
using System.Data.EntityClient;
using NorthwindModel;

class EntityClientSample
{
    public static void ExecuteQuery()
    {
        string queryString =
            @"SELECT c.CategoryID, c.CategoryName
                FROM NorthwindEntities.Categories AS c";

        using (EntityConnection conn =
            new EntityConnection("name=NorthwindEntities"))
        {
            try
            {
                conn.Open();
                using (EntityCommand query = new EntityCommand(queryString, conn))
                {
                    using (DbDataReader rdr =
                        query.ExecuteReader(CommandBehavior.SequentialAccess))
                    {
                        while (rdr.Read())
                        {
                            Console.WriteLine("\t{0}\t{1}", rdr[0], rdr[1]);
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    }
}
```

```vb
Option Explicit On
Option Strict On

Imports System.Data
Imports System.Data.Common
Imports System.Data.EntityClient
Imports NorthwindModel

Class EntityClientSample
    Public Shared Sub ExecuteQuery()
        Dim queryString As String = _
            "SELECT c.CategoryID, c.CategoryName " & _
                "FROM NorthwindEntities.Categories AS c"

        Using conn As EntityConnection = _
            New EntityConnection("name=NorthwindEntities")

            Try
                conn.Open()
                Using query As EntityCommand = _
                New EntityCommand(queryString, conn)
                    Using rdr As DbDataReader = _
                        query.ExecuteReader(CommandBehavior.SequentialAccess)
                        While rdr.Read()
                            Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                                              rdr(0), rdr(1))
                        End While
                    End Using
                End Using
            Catch ex As Exception
                Console.WriteLine(ex.Message)
            End Try
        End Using
    End Sub
End Class
```

## <a name="linq-to-sql"></a>LINQ to SQL
O código neste exemplo usa uma consulta LINQ para retornar dados como objetos Categories, que são projetados como um tipo anônimo que contém somente as propriedades CategoryID e CategoryName. Este exemplo é baseado no contexto de dados do Northwind. Para obter mais informações, consulte [introdução](./sql/linq/getting-started.md).

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Northwind;

    class LinqSqlSample
    {
        public static void ExecuteQuery()
        {
            using (NorthwindDataContext db = new NorthwindDataContext())
            {
                try
                {
                    var query = from category in db.Categories
                                select new
                                {
                                    categoryID = category.CategoryID,
                                    categoryName = category.CategoryName
                                };

                    foreach (var categoryInfo in query)
                    {
                        Console.WriteLine("vbTab {0} vbTab {1}",
                            categoryInfo.categoryID, categoryInfo.categoryName);
                    }
                }
                catch (Exception ex)
                {
                    Console.WriteLine(ex.Message);
                }
            }
        }
    }
```

```vb
Option Explicit On
Option Strict On

Imports System.Collections.Generic
Imports System.Linq
Imports System.Text
Imports Northwind

Class LinqSqlSample
    Public Shared Sub ExecuteQuery()
        Using db As NorthwindDataContext = New NorthwindDataContext()
            Try
                Dim query = From category In db.Categories _
                                Select New With _
                                { _
                                    .categoryID = category.CategoryID, _
                                    .categoryName = category.CategoryName _
                                }

                For Each categoryInfo In query
                    Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                            categoryInfo.categoryID, categoryInfo.categoryName)
                Next
            Catch ex As Exception
                Console.WriteLine(ex.Message)
            End Try
            End Using
    End Sub
End Class
```

## <a name="see-also"></a>Veja também

- [Visão geral do ADO.NET](ado-net-overview.md)
- [Retrieving and Modifying Data in ADO.NET](retrieving-and-modifying-data.md) (Recuperando e modificando dados no ADO.NET)
- [Criando aplicativos de dados](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/h0y4a0f6(v=vs.120))
- [Consultando um Modelo de Dados de Entidade (Entity Framework tarefas)](https://docs.microsoft.com/previous-versions/bb738455(v=vs.90))
- [Como executar uma consulta que retorna objetos de tipo anônimo](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb738512(v=vs.100))
