---
title: Recuperando dados usando um DataReader
description: Saiba como recuperar dados usando um DataReader no ADO.NET com este código de exemplo. O DataReader fornece um fluxo de dados sem buffer.
ms.date: 10/29/2018
dev_langs:
- csharp
- vb
ms.assetid: 97afc121-fb8b-465b-bab3-6d844420badb
ms.openlocfilehash: 6e5161cc325bf0379bb9241b99c473c539ad1081
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286592"
---
# <a name="retrieve-data-using-a-datareader"></a>Recuperar dados usando um DataReader
Para recuperar dados usando um **DataReader**, crie uma instância do objeto **Command** e, em seguida, crie um **DataReader** chamando **Command. ExecuteReader** para recuperar linhas de uma fonte de dados. O **DataReader** fornece um fluxo de dados sem buffer que permite que a lógica de procedimento processe com eficiência os resultados de uma fonte de dados em sequência. O **DataReader** é uma boa opção quando você está recuperando grandes quantidades de dados porque os dados não são armazenados em cache na memória.

O exemplo a seguir ilustra o uso de um **DataReader**, em que `reader` representa um DataReader válido e `command` representa um objeto de comando válido.  

```csharp
reader = command.ExecuteReader();  
```

```vb
reader = command.ExecuteReader()
```  

Use o método **DataReader. Read** para obter uma linha dos resultados da consulta. Você pode acessar cada coluna da linha retornada passando o nome ou o número ordinal da coluna para o **DataReader**. No entanto, para obter um melhor desempenho, o **DataReader** fornece uma série de métodos que permitem que você acesse valores de coluna em seus tipos de dados nativos (**GetDateTime**, **GetDouble**, **GetGUID**, **GetInt32**e assim por diante). Para obter uma lista de métodos acessadores tipados para **Datalêdores**específicos do provedor de dados, consulte <xref:System.Data.OleDb.OleDbDataReader> e <xref:System.Data.SqlClient.SqlDataReader> . Usar os métodos acessadores tipados quando você sabe que o tipo de dados subjacente reduz a quantidade de conversão de tipo necessária ao recuperar o valor da coluna.  
  
 O exemplo a seguir itera por meio de um objeto **DataReader** e retorna duas colunas de cada linha.  
  
 [!code-csharp[DataWorks SqlClient.HasRows#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.HasRows/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.HasRows#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.HasRows/VB/source.vb#1)]  
  
## <a name="closing-the-datareader"></a>Fechando o DataReader  
 Sempre chame o método **Close** quando terminar de usar o objeto **DataReader** .  
  
 Se o **comando** contiver parâmetros de saída ou valores de retorno, esses valores não estarão disponíveis até que o **DataReader** seja fechado.  
  
 Enquanto um **DataReader** está aberto, a **conexão** é usada exclusivamente por esse **DataReader**. Você não pode executar nenhum comando para a **conexão**, incluindo a criação de outro **DataReader**, até que o **DataReader** original seja fechado.  
  
> [!NOTE]
> Não chame **fechar** ou **descartar** em **uma conexão**, um **DataReader**ou qualquer outro objeto gerenciado no método **Finalize** da sua classe. Em um finalizador, libere somente recursos não gerenciados que sua classe possui diretamente. Se sua classe não tiver nenhum recurso não gerenciado, não inclua um método **Finalize** em sua definição de classe. Para obter mais informações, consulte [coleta de lixo](../../../standard/garbage-collection/index.md).  
  
## <a name="retrieving-multiple-result-sets-using-nextresult"></a>Recuperando vários conjuntos de resultados usando NextResult  
 Se o **DataReader** retornar vários conjuntos de resultados, chame o método **NextResult** para iterar nos conjuntos de resultados sequencialmente. O exemplo a seguir mostra <xref:System.Data.SqlClient.SqlDataReader> processando os resultados de duas instruções SELECT usando o método <xref:System.Data.SqlClient.SqlCommand.ExecuteReader%2A>.  
  
 [!code-csharp[DataWorks SqlClient.NextResult#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.NextResult/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.NextResult#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.NextResult/VB/source.vb#1)]  
  
## <a name="getting-schema-information-from-the-datareader"></a>Obtendo informações de esquema do DataReader  
 Embora um **DataReader** esteja aberto, você pode recuperar informações de esquema sobre o conjunto de resultados atual usando o método **GetSchemaTable** . **GetSchemaTable** retorna um <xref:System.Data.DataTable> objeto populado com linhas e colunas que contêm as informações de esquema para o conjunto de resultados atual. A **DataTable** contém uma linha para cada coluna do conjunto de resultados. Cada coluna da tabela de esquema é mapeada para uma propriedade das colunas retornadas nas linhas do conjunto de resultados, em que **ColumnName** é o nome da propriedade e o valor da coluna é o valor da propriedade. O exemplo a seguir grava as informações de esquema para **DataReader**.  
  
 [!code-csharp[DataWorks SqlClient.GetSchemaTable#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.GetSchemaTable/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.GetSchemaTable#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.GetSchemaTable/VB/source.vb#1)]  
  
## <a name="working-with-ole-db-chapters"></a>Trabalhando com capítulos de OLE DB  
 Os conjuntos de linhas hierárquicos ou os capítulos (OLE DB tipo **DBTYPE_HCHAPTER**, tipo de ADO **adChapter**), podem ser recuperados usando o <xref:System.Data.OleDb.OleDbDataReader> . Quando uma consulta que inclui um capítulo é retornada como um **DataReader**, o capítulo é retornado como uma coluna nesse **DataReader** e é exposto como um objeto **DataReader** .  
  
 O **conjunto** de ADO.net também pode ser usado para representar conjuntos de linhas hierárquicos usando relações pai-filho entre tabelas. Para obter mais informações, consulte [conjuntos de dados, tabelas e](./dataset-datatable-dataview/index.md)dados.  
  
 O exemplo de código a seguir usa o provedor de MSDataShape para gerar uma coluna de capítulo de pedidos para cada cliente em uma lista de clientes.  
  
```vb  
Using connection As OleDbConnection = New OleDbConnection(
    "Provider=MSDataShape;Data Provider=SQLOLEDB;" &
    "Data Source=localhost;Integrated Security=SSPI;Initial Catalog=northwind")

    Using custCMD As OleDbCommand = New OleDbCommand(
        "SHAPE {SELECT CustomerID, CompanyName FROM Customers} " &
        "APPEND ({SELECT CustomerID, OrderID FROM Orders} AS CustomerOrders " &
        "RELATE CustomerID TO CustomerID)", connection)

        connection.Open()

        Using custReader As OleDbDataReader = custCMD.ExecuteReader()

            Do While custReader.Read()
                Console.WriteLine("Orders for " & custReader.GetString(1))
                ' custReader.GetString(1) = CompanyName  

                Using orderReader As OleDbDataReader = custReader.GetValue(2)
                    ' custReader.GetValue(2) = Orders chapter as DataReader  

                    Do While orderReader.Read()
                        Console.WriteLine(vbTab & orderReader.GetInt32(1))
                        ' orderReader.GetInt32(1) = OrderID  
                    Loop
                    orderReader.Close()
                End Using
            Loop
            ' Make sure to always close readers and connections.  
            custReader.Close()
        End Using
    End Using
End Using
```  
  
```csharp  
using (OleDbConnection connection = new OleDbConnection(
    "Provider=MSDataShape;Data Provider=SQLOLEDB;" +
    "Data Source=localhost;Integrated Security=SSPI;Initial Catalog=northwind"))
{
    using (OleDbCommand custCMD = new OleDbCommand(
        "SHAPE {SELECT CustomerID, CompanyName FROM Customers} " +
        "APPEND ({SELECT CustomerID, OrderID FROM Orders} AS CustomerOrders " +
        "RELATE CustomerID TO CustomerID)", connection))
    {
        connection.Open();

        using (OleDbDataReader custReader = custCMD.ExecuteReader())
        {

            while (custReader.Read())
            {
                Console.WriteLine("Orders for " + custReader.GetString(1));
                // custReader.GetString(1) = CompanyName  

                using (OleDbDataReader orderReader = (OleDbDataReader)custReader.GetValue(2))
                {
                    // custReader.GetValue(2) = Orders chapter as DataReader  

                    while (orderReader.Read())
                        Console.WriteLine("\t" + orderReader.GetInt32(1));
                    // orderReader.GetInt32(1) = OrderID  
                    orderReader.Close();
                }
            }
            // Make sure to always close readers and connections.  
            custReader.Close();
        }
    }
}
```  
  
## <a name="returning-results-with-oracle-ref-cursors"></a>Retornando resultados com CURSOres de referência do Oracle  
 O Provedor de Dados .NET Framework para Oracle oferece suporte ao uso de REF CURSORs Oracle para retornar um resultado de consulta. Um REF CURSOR Oracle é retornado como um <xref:System.Data.OracleClient.OracleDataReader>.  
  
 Você pode recuperar um <xref:System.Data.OracleClient.OracleDataReader> objeto que representa um cursor de referência do Oracle usando o <xref:System.Data.OracleClient.OracleCommand.ExecuteReader%2A> método. Você também pode especificar um <xref:System.Data.OracleClient.OracleCommand> que retorne um ou mais cursores de referência do Oracle como o **SelectCommand** para um <xref:System.Data.OracleClient.OracleDataAdapter> usado para preencher um <xref:System.Data.DataSet> .  
  
 Para acessar um CURSOR de referência retornado de uma fonte de dados Oracle, crie um <xref:System.Data.OracleClient.OracleCommand> para sua consulta e adicione um parâmetro de saída que faça referência ao cursor de referência para a <xref:System.Data.OracleClient.OracleCommand.Parameters> coleção de seu <xref:System.Data.OracleClient.OracleCommand> . O nome do parâmetro deve corresponder ao nome do parâmetro REF CURSOR em sua consulta. Defina o tipo do parâmetro como <xref:System.Data.OracleClient.OracleType.Cursor?displayProperty=nameWithType> . O <xref:System.Data.OracleClient.OracleCommand.ExecuteReader?displayProperty=nameWithType> método de <xref:System.Data.OracleClient.OracleCommand> retorna um <xref:System.Data.OracleClient.OracleDataReader> para o cursor ref.  
  
 Se o <xref:System.Data.OracleClient.OracleCommand> retornar vários cursores de referência, adicione vários parâmetros de saída. Você pode acessar os diferentes CURSOres de referência chamando o <xref:System.Data.OracleClient.OracleCommand.ExecuteReader?displayProperty=nameWithType> método. A chamada para <xref:System.Data.OracleClient.OracleCommand.ExecuteReader> retorna um <xref:System.Data.OracleClient.OracleDataReader> referenciando o primeiro cursor ref. Em seguida, você pode chamar o <xref:System.Data.OracleClient.OracleDataReader.NextResult?displayProperty=nameWithType> método para acessar os próximos cursores de referência. Embora os parâmetros em sua <xref:System.Data.OracleClient.OracleCommand.Parameters?displayProperty=nameWithType> coleção correspondam aos parâmetros de saída do cursor de referência por nome, o os <xref:System.Data.OracleClient.OracleDataReader> acessa na ordem em que foram adicionados à <xref:System.Data.OracleClient.OracleCommand.Parameters> coleção.  
  
 Por exemplo, considere o seguinte pacote Oracle e o corpo do pacote.  
  
```sql
CREATE OR REPLACE PACKAGE CURSPKG AS
  TYPE T_CURSOR IS REF CURSOR;
  PROCEDURE OPEN_TWO_CURSORS (EMPCURSOR OUT T_CURSOR,
    DEPTCURSOR OUT T_CURSOR);
END CURSPKG;  
  
CREATE OR REPLACE PACKAGE BODY CURSPKG AS
  PROCEDURE OPEN_TWO_CURSORS (EMPCURSOR OUT T_CURSOR,
    DEPTCURSOR OUT T_CURSOR)
  IS
  BEGIN
    OPEN EMPCURSOR FOR SELECT * FROM DEMO.EMPLOYEE;
    OPEN DEPTCURSOR FOR SELECT * FROM DEMO.DEPARTMENT;
  END OPEN_TWO_CURSORS;
END CURSPKG;
```  
  
 O código a seguir cria um <xref:System.Data.OracleClient.OracleCommand> que retorna os cursores de referência do pacote Oracle anterior adicionando dois parâmetros do tipo <xref:System.Data.OracleClient.OracleType.Cursor?displayProperty=nameWithType> à <xref:System.Data.OracleClient.OracleCommand.Parameters?displayProperty=nameWithType> coleção.  
  
```vb  
Dim cursCmd As OracleCommand = New OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn)  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
```  
  
```csharp  
OracleCommand cursCmd = new OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn);  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
```  
  
 O código a seguir retorna os resultados do comando anterior usando os <xref:System.Data.OracleClient.OracleDataReader.Read> <xref:System.Data.OracleClient.OracleDataReader.NextResult> métodos e do <xref:System.Data.OracleClient.OracleDataReader> . Os parâmetros REF CURSOR são retornados em ordem.  
  
```vb  
oraConn.Open()  
  
Dim cursCmd As OracleCommand = New OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn)  
cursCmd.CommandType = CommandType.StoredProcedure  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output  
  
Dim reader As OracleDataReader = cursCmd.ExecuteReader()  
  
Console.WriteLine(vbCrLf & "Emp ID" & vbTab & "Name")  
  
Do While reader.Read()  
  Console.WriteLine("{0}" & vbTab & "{1}, {2}", reader.GetOracleNumber(0), reader.GetString(1), reader.GetString(2))  
Loop  
  
reader.NextResult()  
  
Console.WriteLine(vbCrLf & "Dept ID" & vbTab & "Name")  
  
Do While reader.Read()  
  Console.WriteLine("{0}" & vbTab & "{1}", reader.GetOracleNumber(0), reader.GetString(1))  
Loop  
' Make sure to always close readers and connections.  
reader.Close()  
oraConn.Close()  
```  
  
```csharp  
oraConn.Open();  
  
OracleCommand cursCmd = new OracleCommand("CURSPKG.OPEN_TWO_CURSORS", oraConn);  
cursCmd.CommandType = CommandType.StoredProcedure;  
cursCmd.Parameters.Add("EMPCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
cursCmd.Parameters.Add("DEPTCURSOR", OracleType.Cursor).Direction = ParameterDirection.Output;  
  
OracleDataReader reader = cursCmd.ExecuteReader();  
  
Console.WriteLine("\nEmp ID\tName");  
  
while (reader.Read())  
  Console.WriteLine("{0}\t{1}, {2}", reader.GetOracleNumber(0), reader.GetString(1), reader.GetString(2));  
  
reader.NextResult();  
  
Console.WriteLine("\nDept ID\tName");  
  
while (reader.Read())  
  Console.WriteLine("{0}\t{1}", reader.GetOracleNumber(0), reader.GetString(1));  
// Make sure to always close readers and connections.  
reader.Close();  
oraConn.Close();  
```  
  
 O exemplo a seguir usa o comando anterior para popular um <xref:System.Data.DataSet> com os resultados do pacote Oracle.  
  
```vb  
Dim ds As DataSet = New DataSet()  
  
Dim adapter As OracleDataAdapter = New OracleDataAdapter(cursCmd)  
adapter.TableMappings.Add("Table", "Employees")  
adapter.TableMappings.Add("Table1", "Departments")  
  
adapter.Fill(ds)  
```  
  
```csharp  
DataSet ds = new DataSet();  
  
OracleDataAdapter adapter = new OracleDataAdapter(cursCmd);  
adapter.TableMappings.Add("Table", "Employees");  
adapter.TableMappings.Add("Table1", "Departments");  
  
adapter.Fill(ds);  
```

> [!NOTE]
> Para evitar uma **estourexception**, recomendamos que você também trate qualquer conversão do tipo de número Oracle para um tipo de .NET Framework válido antes de armazenar o valor em um <xref:System.Data.DataRow> . Você pode usar o <xref:System.Data.Common.DataAdapter.FillError> evento para determinar se houve uma **estourexception** . Para obter mais informações sobre o <xref:System.Data.Common.DataAdapter.FillError> evento, consulte [manipulando eventos DataAdapter](handling-dataadapter-events.md).  
  
## <a name="see-also"></a>Veja também

- [DataAdapters e DataReaders](dataadapters-and-datareaders.md)
- [Comandos e parâmetros](commands-and-parameters.md)
- [Recuperando informações de esquema de banco de dados](retrieving-database-schema-information.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
