---
title: Atualizando fontes de dados com DataAdapters
description: Saiba como o método de atualização de DataAdapter resolve as alterações de um conjunto de dados de volta para a origem dos aplicativos ADO.NET.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d1bd9a8c-0e29-40e3-bda8-d89176b72fb1
ms.openlocfilehash: e2348a3a89aa1c28856bfc21aaa25f2c8327aac7
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286178"
---
# <a name="updating-data-sources-with-dataadapters"></a>Atualizando fontes de dados com DataAdapters

O método `Update` do <xref:System.Data.Common.DataAdapter> é chamado para resolver alterações de um <xref:System.Data.DataSet> de volta para a fonte de dados. O método `Update`, como o método de `Fill`, utiliza como argumentos uma instância do `DataSet` e um objeto <xref:System.Data.DataTable> opcional ou um nome de `DataTable`. A instância do `DataSet` é o `DataSet` que contém as alterações que foram feitas, e o `DataTable` identifica a tabela da qual recuperar as alterações. Se nenhum `DataTable` for especificado, o primeiro `DataTable` no `DataSet` será usado.

Quando você chama o método `Update`, o `DataAdapter` analisa as alterações que foram feitas e executa o comando apropriado (INSERT, UPDATE ou DELETE). Quando o `DataAdapter` encontra uma alteração em um <xref:System.Data.DataRow>, ele usa <xref:System.Data.Common.DbDataAdapter.InsertCommand%2A>, <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> ou <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> para processar a alteração. Isso permite que você maximize o desempenho de seu aplicativo ADO.NET especificando a sintaxe de comando em tempo de design e, quando possível, usando procedimentos armazenados. Você deve definir explicitamente os comandos antes de chamar `Update`. Se `Update` for chamado e o comando apropriado não existir para uma atualização específica (por exemplo, nenhum `DeleteCommand` para linhas excluídas), será gerada uma exceção.

> [!NOTE]
> Se você estiver usando procedimentos armazenados do SQL Server para editar ou excluir dados usando um `DataAdapter`, não use SET NOCOUNT ON na definição do procedimento armazenado. Isso faz com que a contagem retornada de linhas afetadas seja zero, o que o `DataAdapter` interpreta como um conflito de simultaneidade. Nesse caso, será gerada uma <xref:System.Data.DBConcurrencyException>.

Os parâmetros do comando podem ser usados para especificar valores de entrada e saída para uma instrução SQL ou procedimento armazenado para cada linha modificada em um `DataSet`. Para obter mais informações, consulte [DataAdapter Parameters](dataadapter-parameters.md).

> [!NOTE]
> É importante compreender a diferença entre excluir uma linha em um <xref:System.Data.DataTable> e remover a linha. Quando você chama o método `Remove` ou `RemoveAt`, a linha é removida imediatamente. Nenhuma linha correspondente na fonte de dados de back-end será afetada se você passar o `DataTable` ou o `DataSet` para um `DataAdapter` e chamar `Update`. Quando você usa o método `Delete`, a linha permanece no `DataTable` e é marcada para exclusão. Se, em seguida, você passar o `DataTable` ou o `DataSet` para um `DataAdapter` e chamar `Update`, a linha correspondente na fonte de dados de back-end será excluída.

Se seu `DataTable` mapear para ou for gerado a partir de uma única tabela do banco de dados, você poderá aproveitar o objeto <xref:System.Data.Common.DbCommandBuilder> para gerar automaticamente os objetos `DeleteCommand`, `InsertCommand` e `UpdateCommand` para o `DataAdapter`. Para obter mais informações, consulte [gerando comandos com CommandBuilders](generating-commands-with-commandbuilders.md).

## <a name="using-updatedrowsource-to-map-values-to-a-dataset"></a>Usando UpdatedRowSource para mapear valores para um DataSet

Você pode controlar como os valores retornados da fonte de dados são mapeados de volta para o `DataTable` depois de uma chamada para o método Update de um `DataAdapter` usando a propriedade <xref:System.Data.Common.DbCommand.UpdatedRowSource%2A> de um objeto <xref:System.Data.Common.DbCommand>. Definindo a propriedade `UpdatedRowSource` para um dos valores de enumeração de <xref:System.Data.UpdateRowSource>, você pode controlar se os parâmetros de saída retornados pelos comandos de `DataAdapter` serão ignorados ou aplicados à linha alterada no `DataSet`. Você também pode especificar se a primeira linha retornada (se existir) é aplicada à linha alterada no `DataTable`.

A tabela a seguir descreve os diferentes valores da enumeração de `UpdateRowSource` e como eles afetam o comportamento de um comando usado com o `DataAdapter`.

|Enumeração de UpdatedRowSource|Description|
|----------------------------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|Os parâmetros de saída e a primeira linha de um conjunto de resultados retornado podem ser mapeados para a linha alterada no `DataSet`.|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|Somente os dados da primeira linha de um conjunto de resultados retornado podem ser mapeados para a linha alterada no `DataSet`.|
|<xref:System.Data.UpdateRowSource.None>|Todos os parâmetros de saída ou linhas de um conjunto de resultados retornado são ignorados.|
|<xref:System.Data.UpdateRowSource.OutputParameters>|Somente os parâmetros de saída podem ser mapeados para a linha alterada no `DataSet`.|

O método `Update` resolve as alterações de volta para a fonte de dados. No entanto, outros clientes podem ter modificado dados na fonte de dados desde a última vez que você preencheu o `DataSet`. Para atualizar seu `DataSet` com dados atuais, use o método `DataAdapter` e o método `Fill`. As novas linhas serão adicionadas à tabela, e informações atualizadas serão incorporadas às linhas existentes. O método `Fill` determina se uma nova linha será adicionada ou se uma linha existente será atualizada examinando os valores de chave primária das linhas no `DataSet` e as linhas retornadas pelo `SelectCommand`. Se o método `Fill` encontrar um valor de chave primária para uma linha no `DataSet` que corresponda a um valor de chave primária de uma linha nos resultados retornados pelo `SelectCommand`, ele atualizará a linha existente com as informações da linha retornadas pelo `SelectCommand` e definirá o <xref:System.Data.DataRow.RowState%2A> da linha existente como `Unchanged`. Se uma linha retornada pelo `SelectCommand` tiver um valor de chave primária que não corresponda a alguns dos valores de chave primária das linhas do `DataSet`, o método `Fill` adicionará uma nova linha com um `RowState` de `Unchanged`.

> [!NOTE]
> Se o `SelectCommand` retornar os resultados de um OUTER JOIN, o `DataAdapter` não definirá um valor de `PrimaryKey` para o`DataTable` resultante. Você deve definir o `PrimaryKey` você mesmo para garantir que as linhas duplicadas sejam resolvidas corretamente. Para obter mais informações, consulte [definindo chaves primárias](./dataset-datatable-dataview/defining-primary-keys.md).

Para lidar com exceções que podem ocorrer ao chamar o `Update` método, você pode usar o `RowUpdated` evento para responder a erros de atualização de linha à medida que eles ocorrem (consulte [manipulando eventos de DataAdapter](handling-dataadapter-events.md)), ou você pode definir `DataAdapter.ContinueUpdateOnError` como `true` antes `Update` de chamar e responder às informações de erro armazenadas na `RowError` propriedade de uma linha específica quando a atualização for concluída (consulte [informações de erro de linha](./dataset-datatable-dataview/row-error-information.md)).

> [!NOTE]
> Chamar `AcceptChanges` em `DataSet` , `DataTable` ou fará com `DataRow` que todos os `Original` valores de um sejam `DataRow` substituídos pelos `Current` valores de `DataRow` . Se os valores dos campos que identificam a linha como exclusiva foram modificados, depois de chamar `AcceptChanges` os valores `Original` não corresponderão mais aos valores na fonte de dados. `AcceptChanges` é chamado automaticamente para cada linha durante uma chamada para o método Update de um `DataAdapter`. Você pode preservar os valores originais durante uma chamada para o método Update definindo primeiro a propriedade `AcceptChangesDuringUpdate` do `DataAdapter` como false, ou criando um manipulador de eventos para o evento `RowUpdated` e definindo o <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> como <xref:System.Data.UpdateStatus.SkipCurrentRow>. Para obter mais informações, consulte [mesclando conteúdo do conjunto](./dataset-datatable-dataview/merging-dataset-contents.md) de dados e [manipulando eventos DataAdapter](handling-dataadapter-events.md).

## <a name="example"></a>Exemplo

Os exemplos a seguir demonstram como executar atualizações para linhas modificadas definindo explicitamente o `UpdateCommand` de a `DataAdapter` e chamando seu `Update` método. Observe que o parâmetro especificado na cláusula WHERE da instrução UPDATE está definido para usar o valor `Original` de `SourceColumn`. Isso é importante, porque o valor `Current` pode ter sido modificado e não corresponder ao valor na fonte de dados. O valor `Original` é o valor que foi usado para popular o `DataTable` da fonte de dados.

[!code-csharp[DataWorks SqlClient.DataAdapterUpdate#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.DataAdapterUpdate/CS/source.cs#1)]
[!code-vb[DataWorks SqlClient.DataAdapterUpdate#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.DataAdapterUpdate/VB/source.vb#1)]

## <a name="autoincrement-columns"></a>Colunas AutoIncrement

Se as tabelas de sua fonte de dados possuírem colunas de incremento automático, você poderá preencher as colunas em seu `DataSet` retornando o valor de incremento automático como um parâmetro de saída de um procedimento armazenado e mapeando esse valor para uma coluna em uma tabela, retornando o valor de incremento automático na primeira linha de um conjunto de resultados retornado por um procedimento armazenado ou instrução SQL ou usando o evento `RowUpdated` do `DataAdapter` para executar uma instrução SELECT adicional. Para obter mais informações e um exemplo, consulte [recuperando identidade ou valores de numeração automática](retrieving-identity-or-autonumber-values.md).

## <a name="ordering-of-inserts-updates-and-deletes"></a>Ordenação de atualizações, inserções e exclusões

Em muitas circunstâncias, a ordem em que as alterações feitas no `DataSet` são enviadas para a fonte de dados é importante. Por exemplo, se um valor de chave primária de uma linha existente for atualizado, e uma nova linha for adicionada com o novo valor de chave primária como uma chave estrangeira, é importante processar a atualização antes da inserção.

Você pode usar o método `Select` do `DataTable` para retornar uma matriz de `DataRow` que referencie somente linhas com um `RowState` específico. Você pode passar a matriz de `DataRow` retornada para o método `Update` do `DataAdapter` para processar as linhas modificadas. Especificando um subconjunto de linhas a serem atualizadas, você pode controlar a ordem na qual as inserções, atualizações e exclusões são processadas.

## <a name="example"></a>Exemplo

Por exemplo, o código a seguir garante que as linhas excluídas da tabela sejam processadas primeiro, em seguida as linhas atualizadas e depois as linhas inseridas.

```vb
Dim table As DataTable = dataSet.Tables("Customers")

' First process deletes.
dataSet.Update(table.Select(Nothing, Nothing, _
  DataViewRowState.Deleted))

' Next process updates.
adapter.Update(table.Select(Nothing, Nothing, _
  DataViewRowState.ModifiedCurrent))

' Finally, process inserts.
adapter.Update(table.Select(Nothing, Nothing, _
  DataViewRowState.Added))
```

```csharp
DataTable table = dataSet.Tables["Customers"];

// First process deletes.
adapter.Update(table.Select(null, null, DataViewRowState.Deleted));

// Next process updates.
adapter.Update(table.Select(null, null,
  DataViewRowState.ModifiedCurrent));

// Finally, process inserts.
adapter.Update(table.Select(null, null, DataViewRowState.Added));
```

## <a name="use-a-dataadapter-to-retrieve-and-update-data"></a>Use um DataAdapter para recuperar e atualizar dados

Você pode usar um DataAdapter para recuperar e atualizar os dados.

- O exemplo usa DataAdapter.AcceptChangesDuringFill para clonar os dados no banco de dados. Se a propriedade é definida como false, AcceptChanges não é chamado ao preencher a tabela e as linhas adicionadas recentemente são tratadas como linhas inseridas. Portanto, o exemplo usa essas linhas para inserir novas linhas no banco de dados.

- Os exemplos usam DataAdapter.TableMappings para definir o mapeamento entre a tabela de origem e o DataTable.

- O exemplo usa DataAdapter.FillLoadOption para determinar como o adaptador preenche o DataTable do DbDataReader. Quando você cria um DataTable, só pode gravar os dados do banco de dados na versão atual ou na versão original definindo a propriedade como LoadOption.Upsert ou LoadOption.PreserveChanges.

- O exemplo também atualizará a tabela usando DbDataAdapter.UpdateBatchSize para executar operações em lote.

Antes de compilar e executar o exemplo, você precisará criar o banco de dados de exemplo:

```sql
USE [master]
GO

CREATE DATABASE [MySchool]

GO

USE [MySchool]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Course]([CourseID] [nvarchar](10) NOT NULL,
[Year] [smallint] NOT NULL,
[Title] [nvarchar](100) NOT NULL,
[Credits] [int] NOT NULL,
[DepartmentID] [int] NOT NULL,
 CONSTRAINT [PK_Course] PRIMARY KEY CLUSTERED
(
[CourseID] ASC,
[Year] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Department]([DepartmentID] [int] IDENTITY(1,1) NOT NULL,
[Name] [nvarchar](50) NOT NULL,
[Budget] [money] NOT NULL,
[StartDate] [datetime] NOT NULL,
[Administrator] [int] NULL,
 CONSTRAINT [PK_Department] PRIMARY KEY CLUSTERED
(
[DepartmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1045', 2012, N'Calculus', 4, 7)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1061', 2012, N'Physics', 4, 1)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2021', 2012, N'Composition', 3, 2)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2042', 2012, N'Literature', 4, 2)

SET IDENTITY_INSERT [dbo].[Department] ON

INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (1, N'Engineering', 350000.0000, CAST(0x0000999C00000000 AS DateTime), 2)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (2, N'English', 120000.0000, CAST(0x0000999C00000000 AS DateTime), 6)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (4, N'Economics', 200000.0000, CAST(0x0000999C00000000 AS DateTime), 4)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (7, N'Mathematics', 250024.0000, CAST(0x0000999C00000000 AS DateTime), 3)
SET IDENTITY_INSERT [dbo].[Department] OFF

ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
```

Projetos em C# e Visual Basic com este exemplo de código podem ser encontrados em [exemplos de código do desenvolvedor](https://code.msdn.microsoft.com/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=How%20to%20use%20DataAdapter%20to%20retrieve%20and%20update%20data&f%5B1%5D).

```csharp
using System;
using System.Data;
using System.Data.Common;
using System.Data.SqlClient;
using System.Linq;
using CSDataAdapterOperations.Properties;

namespace CSDataAdapterOperations.Properties {
   internal sealed partial class Settings : global::System.Configuration.ApplicationSettingsBase {

      private static Settings defaultInstance = ((Settings)(global::System.Configuration.ApplicationSettingsBase.Synchronized(new Settings())));

      public static Settings Default {
         get {
            return defaultInstance;
         }
      }

      [global::System.Configuration.ApplicationScopedSettingAttribute()]
      [global::System.Configuration.DefaultSettingValueAttribute("Data Source=(local);Initial Catalog=MySchool;Integrated Security=True")]
      public string MySchoolConnectionString {
         get {
            return ((string)(this["MySchoolConnectionString"]));
         }
      }
   }
}

class Program {
   static void Main(string[] args) {
      Settings settings = new Settings();

      // Copy the data from the database.  Get the table Department and Course from the database.
      String selectString = @"SELECT [DepartmentID],[Name],[Budget],[StartDate],[Administrator]
                                     FROM [MySchool].[dbo].[Department];

                                   SELECT [CourseID],@Year as [Year],Max([Title]) as [Title],
                                   Max([Credits]) as [Credits],Max([DepartmentID]) as [DepartmentID]
                                   FROM [MySchool].[dbo].[Course]
                                   Group by [CourseID]";

      DataSet mySchool = new DataSet();

      SqlCommand selectCommand = new SqlCommand(selectString);
      SqlParameter parameter = selectCommand.Parameters.Add("@Year", SqlDbType.SmallInt, 2);
      parameter.Value = new Random(DateTime.Now.Millisecond).Next(9999);

      // Use DataTableMapping to map the source tables and the destination tables.
      DataTableMapping[] tableMappings = {new DataTableMapping("Table", "Department"), new DataTableMapping("Table1", "Course")};
      CopyData(mySchool, settings.MySchoolConnectionString, selectCommand, tableMappings);

      Console.WriteLine("The following tables are from the database.");
      foreach (DataTable table in mySchool.Tables) {
         Console.WriteLine(table.TableName);
         ShowDataTable(table);
      }

      // Roll back the changes
      DataTable department = mySchool.Tables["Department"];
      DataTable course = mySchool.Tables["Course"];

      department.Rows[0]["Name"] = "New" + department.Rows[0][1];
      course.Rows[0]["Title"] = "New" + course.Rows[0]["Title"];
      course.Rows[0]["Credits"] = 10;

      Console.WriteLine("After we changed the tables:");
      foreach (DataTable table in mySchool.Tables) {
         Console.WriteLine(table.TableName);
         ShowDataTable(table);
      }

      department.RejectChanges();
      Console.WriteLine("After use the RejectChanges method in Department table to roll back the changes:");
      ShowDataTable(department);

      DataColumn[] primaryColumns = { course.Columns["CourseID"] };
      DataColumn[] resetColumns = { course.Columns["Title"] };
      ResetCourse(course, settings.MySchoolConnectionString, primaryColumns, resetColumns);
      Console.WriteLine("After use the ResetCourse method in Course table to roll back the changes:");
      ShowDataTable(course);

      // Batch update the table.
      String insertString = @"Insert into [MySchool].[dbo].[Course]([CourseID],[Year],[Title],
                                   [Credits],[DepartmentID])
             values (@CourseID,@Year,@Title,@Credits,@DepartmentID)";
      SqlCommand insertCommand = new SqlCommand(insertString);
      insertCommand.Parameters.Add("@CourseID", SqlDbType.NVarChar, 10, "CourseID");
      insertCommand.Parameters.Add("@Year", SqlDbType.SmallInt, 2, "Year");
      insertCommand.Parameters.Add("@Title", SqlDbType.NVarChar, 100, "Title");
      insertCommand.Parameters.Add("@Credits", SqlDbType.Int, 4, "Credits");
      insertCommand.Parameters.Add("@DepartmentID", SqlDbType.Int, 4, "DepartmentID");

      const Int32 batchSize = 10;
      BatchInsertUpdate(course, settings.MySchoolConnectionString, insertCommand, batchSize);
   }

   private static void CopyData(DataSet dataSet, String connectionString, SqlCommand selectCommand, DataTableMapping[] tableMappings) {
      using (SqlConnection connection = new SqlConnection(connectionString)) {
         selectCommand.Connection = connection;

         connection.Open();

         using (SqlDataAdapter adapter = new SqlDataAdapter(selectCommand)) {adapter.TableMappings.AddRange(tableMappings);
            // If set the AcceptChangesDuringFill as the false, AcceptChanges will not be called on a
            // DataRow after it is added to the DataTable during any of the Fill operations.
            adapter.AcceptChangesDuringFill = false;

            adapter.Fill(dataSet);
         }
      }
   }

   // Roll back only one column or several columns data of the Course table by call ResetDataTable method.
   private static void ResetCourse(DataTable table, String connectionString,
       DataColumn[] primaryColumns, DataColumn[] resetColumns) {
      table.PrimaryKey = primaryColumns;

      // Build the query string
      String primaryCols = String.Join(",", primaryColumns.Select(col => col.ColumnName));
      String resetCols = String.Join(",", resetColumns.Select(col => $"Max({col.ColumnName}) as {col.ColumnName}"));

      String selectString = $"Select {primaryCols},{resetCols} from Course Group by {primaryCols}");

      SqlCommand selectCommand = new SqlCommand(selectString);

      ResetDataTable(table, connectionString, selectCommand);
   }

   // RejectChanges will roll back all changes made to the table since it was loaded, or the last time AcceptChanges
   // was called. When you copy from the database, you can lose all the data after calling RejectChanges
   // The ResetDataTable method rolls back one or more columns of data.
   private static void ResetDataTable(DataTable table, String connectionString,
       SqlCommand selectCommand) {
      using (SqlConnection connection = new SqlConnection(connectionString)) {
         selectCommand.Connection = connection;

         connection.Open();

         using (SqlDataAdapter adapter = new SqlDataAdapter(selectCommand)) {
            // The incoming values for this row will be written to the current version of each
            // column. The original version of each column's data will not be changed.
            adapter.FillLoadOption = LoadOption.Upsert;

            adapter.Fill(table);
         }
      }
   }

   private static void BatchInsertUpdate(DataTable table, String connectionString,
       SqlCommand insertCommand, Int32 batchSize) {
      using (SqlConnection connection = new SqlConnection(connectionString)) {
         insertCommand.Connection = connection;
         // When setting UpdateBatchSize to a value other than 1, all the commands
         // associated with the SqlDataAdapter have to have their UpdatedRowSource
         // property set to None or OutputParameters. An exception is thrown otherwise.
         insertCommand.UpdatedRowSource = UpdateRowSource.None;

         connection.Open();

         using (SqlDataAdapter adapter = new SqlDataAdapter()) {
            adapter.InsertCommand = insertCommand;
            // Gets or sets the number of rows that are processed in each round-trip to the server.
            // Setting it to 1 disables batch updates, as rows are sent one at a time.
            adapter.UpdateBatchSize = batchSize;

            adapter.Update(table);

            Console.WriteLine("Successfully to update the table.");
         }
      }
   }

   private static void ShowDataTable(DataTable table) {
      foreach (DataColumn col in table.Columns) {
         Console.Write("{0,-14}", col.ColumnName);
      }
      Console.WriteLine("{0,-14}", "RowState");

      foreach (DataRow row in table.Rows) {
         foreach (DataColumn col in table.Columns) {
            if (col.DataType.Equals(typeof(DateTime)))
               Console.Write("{0,-14:d}", row[col]);
            else if (col.DataType.Equals(typeof(Decimal)))
               Console.Write("{0,-14:C}", row[col]);
            else
               Console.Write("{0,-14}", row[col]);
         }
         Console.WriteLine("{0,-14}", row.RowState);
      }
   }
}
```

## <a name="see-also"></a>Veja também

- [DataAdapters e DataReaders](dataadapters-and-datareaders.md)
- [Estados de linha e versões de linha](./dataset-datatable-dataview/row-states-and-row-versions.md)
- [AcceptChanges e RejectChanges](./dataset-datatable-dataview/acceptchanges-and-rejectchanges.md)
- [Merging DataSet Contents](./dataset-datatable-dataview/merging-dataset-contents.md) (Mesclando o conteúdo do DataSet)
- [Recuperando identidade ou valores de Autonumber](retrieving-identity-or-autonumber-values.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
