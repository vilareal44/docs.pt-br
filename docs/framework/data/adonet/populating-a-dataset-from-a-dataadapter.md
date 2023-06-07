---
title: Populando um DataSet a partir de um DataAdapter
description: Saiba como popular um conjunto de dados de um DataAdapter no ADO.NET, que fornece um modelo de programação relacional consistente independente da fonte de dados.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 3fa0ac7d-e266-4954-bfac-3fbe2f913153
ms.openlocfilehash: 3d4da840e1d51ec6f309915787caa8891db3eb59
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286657"
---
# <a name="populating-a-dataset-from-a-dataadapter"></a>Populando um DataSet a partir de um DataAdapter
O ADO.NET <xref:System.Data.DataSet> é uma representação de dados residente na memória que fornece um modelo de programação relacional consistente independente da fonte de dados. O `DataSet` representa um conjunto completo de dados, que inclui tabelas, restrições e relações entre as tabelas. Como `DataSet` é independente da fonte de dados, um `DataSet` pode incluir o local de dados para o aplicativo, e os dados de várias fontes de dados. A interação com fontes de dados existente é controlada com o `DataAdapter`.  
  
 A propriedade `SelectCommand` do `DataAdapter` é um objeto `Command` que recupera dados da fonte de dados. As propriedades `InsertCommand`, `UpdateCommand`, e `DeleteCommand` do `DataAdapter` são objetos `Command` que gerenciam as atualizações aos dados na fonte de dados de acordo com as alterações feitas aos dados no `DataSet`. Essas propriedades são abordadas em mais detalhes na [atualização de fontes de dados com DataAdapters](updating-data-sources-with-dataadapters.md).  
  
 O método `Fill` do `DataAdapter` é usado para preencher um `DataSet` com os resultados do `SelectCommand` do `DataAdapter`. `Fill` utiliza como seus argumentos um `DataSet` a ser preenchido, e um objeto `DataTable`, ou nome do `DataTable` a ser preenchido com as linhas retornadas do `SelectCommand`.  
  
> [!NOTE]
> Usar o `DataAdapter` para recuperar todas as tabelas leva tempo, especialmente se há várias linhas na tabela. Isso ocorre porque acessar o banco de dados, localizar e processar os dados e, em seguida, transferir os dados para o cliente é demorado. Receber todas as tabelas para o cliente também bloqueia todas as linhas no servidor. Para melhorar o desempenho, você pode usar a cláusula `WHERE` para reduzir bastante o número de linhas retornadas para o cliente. Você também pode reduzir a quantidade de dados retornados para o cliente somente listando explicitamente as colunas necessárias na instrução `SELECT`. Outra boa solução alternativa é recuperar as linhas em lotes (como várias centenas de linhas de cada vez) e recuperar somente o próximo lote quando o cliente tiver terminado o lote atual.  
  
 O método `Fill` usa o objeto `DataReader` implicitamente para retornar os nomes de coluna e os tipos que são usados para criar as tabelas no `DataSet`, e os dados para preencher as linhas das tabelas no `DataSet`. As tabelas e as colunas são criadas somente se já não existirem; caso contrário, `Fill` usará o esquema existente do `DataSet`. Os tipos de coluna são criados como tipos de .NET Framework de acordo com as tabelas em [mapeamentos de tipo de dados no ADO.net](data-type-mappings-in-ado-net.md). As chaves primárias não são criadas, a menos que elas existam na fonte de dados e `DataAdapter` **.**`MissingSchemaAction` está definido como `MissingSchemaAction` **.** `AddWithKey` . Se `Fill` descobrir que uma chave primária existe para uma tabela, ele substituirá os dados no `DataSet` pelos dados da fonte de dados para as linhas onde os valores de coluna de chave primária coincidirem com os da linha retornada da fonte de dados. Se nenhuma chave primária tiver sido encontrada, os dados serão adicionados às tabelas no `DataSet`. `Fill`usa os mapeamentos que podem existir quando você preenche o `DataSet` (consulte [DataAdapter DataTable e os mapeamentos de DataColumn](dataadapter-datatable-and-datacolumn-mappings.md)).  
  
> [!NOTE]
> Se o `SelectCommand` retornar os resultados de um OUTER JOIN, o `DataAdapter` não definirá um valor de `PrimaryKey` para o`DataTable` resultante. Você deve definir o `PrimaryKey` você mesmo para garantir que as linhas duplicadas sejam resolvidas corretamente. Para obter mais informações, consulte [definindo chaves primárias](./dataset-datatable-dataview/defining-primary-keys.md).  
  
 O exemplo de código a seguir cria uma instância de um <xref:System.Data.SqlClient.SqlDataAdapter> que usa um <xref:System.Data.SqlClient.SqlConnection> para o banco de dados `Northwind` do Microsoft SQL Server e preenche uma <xref:System.Data.DataTable> em um `DataSet` com a lista de clientes. A instrução SQL e os argumentos <xref:System.Data.SqlClient.SqlConnection> passados para o construtor de <xref:System.Data.SqlClient.SqlDataAdapter> são usados para criar a propriedade de <xref:System.Data.SqlClient.SqlDataAdapter.SelectCommand%2A> de <xref:System.Data.SqlClient.SqlDataAdapter>.  
  
## <a name="example"></a>Exemplo  
  
```vb  
' Assumes that connection is a valid SqlConnection object.  
Dim queryString As String = _  
  "SELECT CustomerID, CompanyName FROM dbo.Customers"  
Dim adapter As SqlDataAdapter = New SqlDataAdapter( _  
  queryString, connection)  
  
Dim customers As DataSet = New DataSet  
adapter.Fill(customers, "Customers")  
```  
  
```csharp  
// Assumes that connection is a valid SqlConnection object.  
string queryString =
  "SELECT CustomerID, CompanyName FROM dbo.Customers";  
SqlDataAdapter adapter = new SqlDataAdapter(queryString, connection);  
  
DataSet customers = new DataSet();  
adapter.Fill(customers, "Customers");  
```  
  
> [!NOTE]
> O código mostrado neste exemplo não abre e não fecha explicitamente a `Connection`. O método `Fill` abre implicitamente a `Connection` que o `DataAdapter` estiver usando se descobrir que a conexão ainda não está aberta. Se `Fill` tiver aberto a conexão, ele também fechará a conexão quando `Fill` terminar. Isso pode simplificar o seu código quando você manipula uma única operação como `Fill` ou `Update`. No entanto, se você estiver executando várias operações que exigem uma conexão aberta, poderá melhorar o desempenho do seu aplicativo explicitamente chamando o método `Open` da `Connection`, executando operações na fonte de dados e, em seguida, chamando o método `Close` da `Connection`. Você deve tentar manter abertas as conexões para a fonte de dados o mais rapidamente possível para liberar recursos para o uso por outros aplicativos cliente.  
  
## <a name="multiple-result-sets"></a>Vários conjuntos de resultados  
 Se o `DataAdapter` encontrar vários conjuntos de resultados, criará várias tabelas no `DataSet`. As tabelas recebem um nome padrão incremental da tabela*N*, começando com "Table" para Table0. Se um nome de tabela for passado como um argumento para o `Fill` método, as tabelas receberão um nome padrão incremental de TableName*N*, começando com "TableName" para TableName0.  
  
## <a name="populating-a-dataset-from-multiple-dataadapters"></a>Populando um DataSet a partir de vários DataAdapters  
 Qualquer número de `DataAdapter` objetos pode ser usado com um `DataSet` . Cada `DataAdapter` pode ser usado para preencher um ou mais objetos `DataTable` e resolver atualizações de volta para a fonte de dados relevante. Os objetos `DataRelation` e `Constraint` podem ser adicionados ao `DataSet` localmente, o que permite relacionar dados de fontes de dados não semelhantes. Por exemplo, um `DataSet` pode conter dados de um banco de dados Microsoft SQL Server, um banco de dados IBM DB2 exposto por meio do OLE DB e uma fonte de dados que transmite XML. Um ou mais objetos `DataAdapter` podem administrar a comunicação para cada fonte de dados.  
  
### <a name="example"></a>Exemplo  
 O exemplo de código a seguir preenche uma lista de clientes do banco de dados `Northwind` no Microsoft SQL Server, e uma lista de pedidos do banco de dados `Northwind` armazenado no Microsoft Access 2000. As tabelas preenchidas estão relacionadas com um `DataRelation`, e a lista de clientes é exibida com os pedidos para aquele cliente. Para obter mais informações sobre `DataRelation` objetos, consulte [adicionando DataRelations](./dataset-datatable-dataview/adding-datarelations.md) e [navegando DataRelations](./dataset-datatable-dataview/navigating-datarelations.md).  
  
```vb  
' Assumes that customerConnection is a valid SqlConnection object.  
' Assumes that orderConnection is a valid OleDbConnection object.  
Dim custAdapter As SqlDataAdapter = New SqlDataAdapter( _  
  "SELECT * FROM dbo.Customers", customerConnection)  
  
Dim ordAdapter As OleDbDataAdapter = New OleDbDataAdapter( _  
  "SELECT * FROM Orders", orderConnection)  
  
Dim customerOrders As DataSet = New DataSet()  
custAdapter.Fill(customerOrders, "Customers")  
ordAdapter.Fill(customerOrders, "Orders")  
  
Dim relation As DataRelation = _  
  customerOrders.Relations.Add("CustOrders", _  
  customerOrders.Tables("Customers").Columns("CustomerID"), _
  customerOrders.Tables("Orders").Columns("CustomerID"))  
  
Dim pRow, cRow As DataRow  
For Each pRow In customerOrders.Tables("Customers").Rows  
  Console.WriteLine(pRow("CustomerID").ToString())  
  
  For Each cRow In pRow.GetChildRows(relation)  
    Console.WriteLine(vbTab & cRow("OrderID").ToString())  
  Next  
Next  
```  
  
```csharp  
// Assumes that customerConnection is a valid SqlConnection object.  
// Assumes that orderConnection is a valid OleDbConnection object.  
SqlDataAdapter custAdapter = new SqlDataAdapter(  
  "SELECT * FROM dbo.Customers", customerConnection);  
OleDbDataAdapter ordAdapter = new OleDbDataAdapter(  
  "SELECT * FROM Orders", orderConnection);  
  
DataSet customerOrders = new DataSet();  
  
custAdapter.Fill(customerOrders, "Customers");  
ordAdapter.Fill(customerOrders, "Orders");  
  
DataRelation relation = customerOrders.Relations.Add("CustOrders",  
  customerOrders.Tables["Customers"].Columns["CustomerID"],  
  customerOrders.Tables["Orders"].Columns["CustomerID"]);  
  
foreach (DataRow pRow in customerOrders.Tables["Customers"].Rows)  
{  
  Console.WriteLine(pRow["CustomerID"]);  
   foreach (DataRow cRow in pRow.GetChildRows(relation))  
    Console.WriteLine("\t" + cRow["OrderID"]);  
}  
```  
  
## <a name="sql-server-decimal-type"></a>Tipo decimal do SQL Server  
 Por padrão, o `DataSet` armazena dados usando .NET Framework tipos de dados. Para a maioria dos aplicativos, eles fornecem uma representação conveniente de informações da fonte de dados. No entanto, essa representação pode causar um problema quando o tipo de dados na fonte de dados é um decimal ou um tipo de dados numérico do SQL Server. O `decimal` tipo de dados .NET Framework permite um máximo de 28 dígitos significativos, enquanto o `decimal` tipo de dados SQL Server permite dígitos significativos de 38. Se o `SqlDataAdapter` determina durante uma operação de `Fill` que a precisão de um campo do SQL Server `decimal` é maior que 28 caracteres, a linha atual não será adicionada ao `DataTable`. Em vez disso, o evento de `FillError` ocorre, o que permite determinar se uma perda de precisão ocorrerá e responder corretamente. Para obter mais informações sobre o `FillError` evento, consulte [manipulando eventos de DataAdapter](handling-dataadapter-events.md). Para obter o valor `decimal` do SQL Server, você também poderá usar um objeto <xref:System.Data.SqlClient.SqlDataReader> e chamar o método <xref:System.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A>.  
  
 O ADO.NET 2,0 introduziu suporte aprimorado para <xref:System.Data.SqlTypes> o no `DataSet` . Para obter mais informações, consulte [SqlTypes e o DataSet](./sql/sqltypes-and-the-dataset.md).  
  
## <a name="ole-db-chapters"></a>Capítulos do OLE DB  
 Os conjuntos de linhas hierárquicos ou capítulos (tipo `DBTYPE_HCHAPTER` do OLE DB, tipo `adChapter` do ADO) podem ser usados para preencher o conteúdo de um `DataSet`. Quando o <xref:System.Data.OleDb.OleDbDataAdapter> encontra uma coluna de capítulos durante uma operação de `Fill`, um `DataTable` é criado para a coluna de capítulos e a tabela é preenchida com as colunas e linhas do capítulo. A tabela criada para a coluna com capítulo é nomeada usando o nome da tabela pai e o nome da coluna com capítulo no formato "*ParentTableNameChapteredColumnName*". Se uma tabela já existir no `DataSet` que corresponde ao nome da coluna de capítulos, a tabela atual será preenchida com os dados do capítulo. Se não houver nenhuma coluna em uma tabela existente que corresponde a uma coluna encontrada no capítulo, uma nova coluna será adicionada.  
  
 Antes que as tabelas no `DataSet` sejam preenchidas com os dados nas colunas de capítulos, um relacionamento é criado entre as tabelas pai e filho do conjunto de linhas hierárquico adicionando uma coluna de inteiros à tabela pai e filho, definindo a coluna pai para incrementar automaticamente, e criando uma `DataRelation` usando as colunas adicionadas das duas tabelas. A relação adicionada é nomeada por meio da tabela pai e dos nomes de coluna de capítulo no formato "*ParentTableNameChapterColumnName*".  
  
 Observe que a coluna relacionada somente existe no `DataSet`. Os preenchimentos subsequentes da fonte de dados podem fazer novas linhas serem adicionadas às tabelas em vez de as alterações serem mescladas em linhas existentes.  
  
 Observe também que, se você usar a sobrecarga de `DataAdapter.Fill` que utiliza uma `DataTable`, somente essa tabela será preenchida. Uma coluna de inteiros de incremento automático ainda será adicionada à tabela, mas nenhuma tabela filho será criada ou preenchida, e nenhuma relação será criada.  
  
 O exemplo a seguir usa o provedor de MSDataShape para gerar uma coluna de capítulo de pedidos para cada cliente em uma lista de clientes. Um `DataSet` é então preenchido com os dados.  
  
```vb  
Using connection As OleDbConnection = New OleDbConnection( _  
  "Provider=MSDataShape;Data Provider=SQLOLEDB;" & _  
  "Data Source=(local);Integrated " & _  
  "Security=SSPI;Initial Catalog=northwind")  
  
Dim adapter As OleDbDataAdapter = New OleDbDataAdapter( _  
  "SHAPE {SELECT CustomerID, CompanyName FROM Customers} " & _  
  "APPEND ({SELECT CustomerID, OrderID FROM Orders} AS Orders " & _  
  "RELATE CustomerID TO CustomerID)", connection)  
  
Dim customers As DataSet = New DataSet()  
  
adapter.Fill(customers, "Customers")  
End Using  
```  
  
```csharp  
using (OleDbConnection connection = new OleDbConnection("Provider=MSDataShape;Data Provider=SQLOLEDB;" +  
  "Data Source=(local);Integrated Security=SSPI;Initial Catalog=northwind"))  
{  
OleDbDataAdapter adapter = new OleDbDataAdapter("SHAPE {SELECT CustomerID, CompanyName FROM Customers} " +  
  "APPEND ({SELECT CustomerID, OrderID FROM Orders} AS Orders " +  
  "RELATE CustomerID TO CustomerID)", connection);  
  
DataSet customers = new DataSet();  
adapter.Fill(customers, "Customers");  
}  
```  
  
 Quando a operação de `Fill` estiver concluída, o `DataSet` conterá duas tabelas: `Customers` e `CustomersOrders`, onde `CustomersOrders` representa a coluna de capítulos. Uma coluna adicional chamada `Orders` é adicionada à tabela `Customers` e uma coluna extra chamada `CustomersOrders` é adicionada à tabela `CustomersOrders`. A coluna `Orders` na tabela `Customers` é definida para incrementar automaticamente. Um `DataRelation`, `CustomersOrders`, é criado usando as colunas que foram adicionadas às tabelas com `Customers` como a tabela pai. As tabelas a seguir mostram alguns resultados de exemplo.  
  
### <a name="tablename-customers"></a>TableName: Customers  
  
|CustomerID|CompanyName|Orders|  
|----------------|-----------------|------------|  
|ALFKI|Alfreds Futterkiste|0|  
|ANATR|Ana Trujillo Emparedados y helados|1|  
  
### <a name="tablename-customersorders"></a>TableName: CustomersOrders  
  
|CustomerID|OrderID|CustomersOrders|  
|----------------|-------------|---------------------|  
|ALFKI|10643|0|  
|ALFKI|10692|0|  
|ANATR|10308|1|  
|ANATR|10625|1|  
  
## <a name="see-also"></a>Veja também

- [DataAdapters e DataReaders](dataadapters-and-datareaders.md)
- [Data Type Mappings in ADO.NET](data-type-mappings-in-ado-net.md) (Mapeamentos de tipo de dados no ADO.NET)
- [Modificando dados com um DbDataAdapter](modifying-data-with-a-dbdataadapter.md)
- [MARS (Vários Conjuntos de Resultados Ativos)](./sql/multiple-active-result-sets-mars.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
