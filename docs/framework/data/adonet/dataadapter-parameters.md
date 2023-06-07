---
title: Parâmetros DataAdapter
description: Saiba mais sobre as propriedades de DbDataAdapter que retornam dados de uma fonte de dados e gerenciam alterações na fonte de dados.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: f21e6aba-b76d-46ad-a83e-2ad8e0af1e12
ms.openlocfilehash: 74b6787162b48f83a48127257dc8e23e31a859b7
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286980"
---
# <a name="dataadapter-parameters"></a>Parâmetros DataAdapter
O <xref:System.Data.Common.DbDataAdapter> tem quatro propriedades que são usadas para recuperar dados da fonte de dados e atualizá-los nela: a propriedade <xref:System.Data.Common.DbDataAdapter.SelectCommand%2A> retorna dados da fonte de dados; e as propriedades <xref:System.Data.Common.DbDataAdapter.InsertCommand%2A>, <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> e <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> são usadas para gerenciar alterações na fonte de dados. A propriedade `SelectCommand` deve ser definida antes que você chame o método `Fill` do `DataAdapter`. A propriedade `InsertCommand`, `UpdateCommand` ou `DeleteCommand` deve ser definida antes que o método `Update` do `DataAdapter` seja chamado, dependendo de quais alterações foram feitas nos dados da <xref:System.Data.DataTable>. Por exemplo, se as linhas tiverem sido adicionadas, o `InsertCommand` deve ser definido antes de chamar `Update`. Quando `Update` estiver processando uma linha inserida, atualizada ou excluída, o `DataAdapter` usará a respectiva propriedade `Command` para processar a ação. As informações atuais sobre a linha modificada são passadas para o objeto `Command` através da coleção `Parameters`.  
  
 Ao atualizar uma linha na fonte de dados, você chama a instrução UPDATE, que usa um identificador exclusivo para identificar a linha na tabela a ser atualizada. O identificador exclusivo é geralmente o valor de um campo de chave primária. A instrução UPDATE usa os parâmetros que contêm o identificador exclusivo e as colunas e os valores a serem atualizados, conforme mostrado na declaração Transact-SQL a seguir.  
  
```sql
UPDATE Customers SET CompanyName = @CompanyName
  WHERE CustomerID = @CustomerID  
```  
  
> [!NOTE]
> A sintaxe para espaços reservados de parâmetro depende da fonte de dados. Este exemplo mostra os espaços reservados de uma fonte de dados do SQL Server. Use espaços reservados de ponto de interrogação (?) para os parâmetros <xref:System.Data.OleDb> e <xref:System.Data.Odbc>.  
  
 Neste Visual Basic exemplo, o `CompanyName` campo é atualizado com o valor do `@CompanyName` parâmetro para a linha em que `CustomerID` é igual ao valor do `@CustomerID` parâmetro. Os parâmetros recuperam informações da linha modificada usando a <xref:System.Data.SqlClient.SqlParameter.SourceColumn%2A> Propriedade do <xref:System.Data.SqlClient.SqlParameter> objeto. Estes são os parâmetros da instrução UPDATE de exemplo anterior. O código assume que a variável `adapter` representa um objeto <xref:System.Data.SqlClient.SqlDataAdapter> válido.  
  
```vb
adapter.Parameters.Add( _  
  "@CompanyName", SqlDbType.NChar, 15, "CompanyName")  
Dim parameter As SqlParameter = _  
  adapter.UpdateCommand.Parameters.Add("@CustomerID", _  
  SqlDbType.NChar, 5, "CustomerID")  
parameter.SourceVersion = DataRowVersion.Original  
```  
  
 O método `Add` da coleção `Parameters` adota o nome do parâmetro, o tipo de dados, o tamanho (se aplicável ao tipo) e o nome da <xref:System.Data.Common.DbParameter.SourceColumn%2A> da `DataTable`. Observe que <xref:System.Data.Common.DbParameter.SourceVersion%2A> do parâmetro `@CustomerID` é definido como `Original`. Isso garante que a linha existente na fonte de dados será atualizada se o valor da(s) coluna(s) de identificação tiverem sido alteradas na <xref:System.Data.DataRow> modificada. Nesse caso, o valor de linha `Original` corresponderia ao valor atual na fonte de dados, e o valor de linha `Current` conteria o valor atualizado. A `SourceVersion` do parâmetro `@CompanyName` não está definida e usa o padrão, o valor de linha `Current`.  
  
> [!NOTE]
> Para ambas as `Fill` operações do `DataAdapter` e os `Get` métodos do `DataReader` , o tipo de .NET Framework é inferido do tipo retornado do provedor de dados de .NET Framework. Os tipos de .NET Framework inferidos e os métodos de acessador para tipos de dados Microsoft SQL Server, OLE DB e ODBC são descritos em [mapeamentos de tipo de dados em ADO.net](data-type-mappings-in-ado-net.md).  
  
## <a name="parametersourcecolumn-parametersourceversion"></a>Parameter.SourceColumn, Parameter.SourceVersion  
 A `SourceColumn` e a `SourceVersion` podem ser passadas como argumentos para o construtor `Parameter` ou definidas como propriedades de um `Parameter` existente. A `SourceColumn` é o nome da <xref:System.Data.DataColumn> da <xref:System.Data.DataRow>, em que o valor do `Parameter` será recuperado. A `SourceVersion` especifica a versão da `DataRow` que o `DataAdapter` usa para recuperar o valor.  
  
 A tabela a seguir mostra os valores de enumeração <xref:System.Data.DataRowVersion> disponíveis para uso com a `SourceVersion`.  
  
|Enumeração DataRowVersion|Description|  
|--------------------------------|-----------------|  
|`Current`|O parâmetro usa o valor atual da coluna. Este é o padrão.|  
|`Default`|O parâmetro usa o `DefaultValue` da coluna.|  
|`Original`|O parâmetro usa o valor original da coluna.|  
|`Proposed`|O parâmetro usa um valor proposto.|  
  
 O exemplo de código `SqlClient` na seção a seguir define um parâmetro para um <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> em que a coluna `CustomerID` é usada como `SourceColumn` de dois parâmetros: `@CustomerID` (`SET CustomerID = @CustomerID`) e `@OldCustomerID` (`WHERE CustomerID = @OldCustomerID`). O `@CustomerID` parâmetro é usado para atualizar a coluna **CustomerID** para o valor atual no `DataRow` . Como resultado, o `CustomerID` `SourceColumn` com um `SourceVersion` de `Current` é usado. O `@OldCustomerID` parâmetro é usado para identificar a linha atual na fonte de dados. Como o valor de coluna correspondente é encontrado na versão `Original` da linha, a mesma `SourceColumn` (`CustomerID`) com uma `SourceVersion` de `Original` é usada.  
  
## <a name="working-with-sqlclient-parameters"></a>Trabalhando com parâmetros SqlClient  
 O exemplo a seguir demonstra como criar um <xref:System.Data.SqlClient.SqlDataAdapter> e definir <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> para <xref:System.Data.MissingSchemaAction.AddWithKey> a fim de recuperar informações adicionais do esquema no banco de dados. O conjunto de propriedades <xref:System.Data.SqlClient.SqlDataAdapter.SelectCommand%2A>, <xref:System.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, <xref:System.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> e <xref:System.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> e os objetos <xref:System.Data.SqlClient.SqlParameter> correspondentes adicionados à coleção <xref:System.Data.SqlClient.SqlCommand.Parameters%2A>. O método retorna um objeto `SqlDataAdapter`.  
  
 [!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter Example#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/Classic WebData SqlDataAdapter.SqlDataAdapter Example/CS/source.cs#1)]
 [!code-vb[Classic WebData SqlDataAdapter.SqlDataAdapter Example#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/Classic WebData SqlDataAdapter.SqlDataAdapter Example/VB/source.vb#1)]  
  
## <a name="oledb-parameter-placeholders"></a>Espaços reservados do parâmetro OleDb  
 Para os objetos <xref:System.Data.OleDb.OleDbDataAdapter> e <xref:System.Data.Odbc.OdbcDataAdapter>, você deve usar os espaços reservados de ponto de interrogação (?) para identificar os parâmetros.  
  
```vb  
Dim selectSQL As String = _  
  "SELECT CustomerID, CompanyName FROM Customers " & _  
  "WHERE CountryRegion = ? AND City = ?"  
Dim insertSQL AS String = _  
  "INSERT INTO Customers (CustomerID, CompanyName) VALUES (?, ?)"  
Dim updateSQL AS String = _  
  "UPDATE Customers SET CustomerID = ?, CompanyName = ? " & _  
  WHERE CustomerID = ?"  
Dim deleteSQL As String = "DELETE FROM Customers WHERE CustomerID = ?"  
```  
  
```csharp  
string selectSQL =
  "SELECT CustomerID, CompanyName FROM Customers " +  
  "WHERE CountryRegion = ? AND City = ?";  
string insertSQL =
  "INSERT INTO Customers (CustomerID, CompanyName) " +  
  "VALUES (?, ?)";  
string updateSQL =
  "UPDATE Customers SET CustomerID = ?, CompanyName = ? " +  
  "WHERE CustomerID = ? ";  
string deleteSQL = "DELETE FROM Customers WHERE CustomerID = ?";  
```  
  
 As instruções de consulta parametrizadas definem quais parâmetros de entrada e de saída devem ser criados. Para criar um parâmetro, use o método `Parameters.Add` ou o construtor `Parameter` para especificar o nome da coluna, o tipo de dados e o tamanho. Para tipos de dados intrínsecos, como `Integer`, você não precisa incluir o tamanho ou pode especificar o tamanho padrão.  
  
 O exemplo de código a seguir cria os parâmetros para uma instrução SQL e preenche um `DataSet`.  
  
## <a name="oledb-example"></a>Exemplo de OleDb  
  
```vb  
' Assumes that connection is a valid OleDbConnection object.  
Dim adapter As OleDbDataAdapter = New OleDbDataAdapter
  
Dim selectCMD AS OleDbCommand = New OleDbCommand(selectSQL, connection)  
adapter.SelectCommand = selectCMD  
  
' Add parameters and set values.  
selectCMD.Parameters.Add( _  
  "@CountryRegion", OleDbType.VarChar, 15).Value = "UK"  
selectCMD.Parameters.Add( _  
  "@City", OleDbType.VarChar, 15).Value = "London"  
  
Dim customers As DataSet = New DataSet  
adapter.Fill(customers, "Customers")  
```  
  
```csharp  
// Assumes that connection is a valid OleDbConnection object.  
OleDbDataAdapter adapter = new OleDbDataAdapter();  
  
OleDbCommand selectCMD = new OleDbCommand(selectSQL, connection);  
adapter.SelectCommand = selectCMD;  
  
// Add parameters and set values.  
selectCMD.Parameters.Add(  
  "@CountryRegion", OleDbType.VarChar, 15).Value = "UK";  
selectCMD.Parameters.Add(  
  "@City", OleDbType.VarChar, 15).Value = "London";  
  
DataSet customers = new DataSet();  
adapter.Fill(customers, "Customers");  
```  
  
## <a name="odbc-parameters"></a>Parâmetros Odbc  
  
```vb  
' Assumes that connection is a valid OdbcConnection object.  
Dim adapter As OdbcDataAdapter = New OdbcDataAdapter  
  
Dim selectCMD AS OdbcCommand = New OdbcCommand(selectSQL, connection)  
adapter.SelectCommand = selectCMD  
  
' Add Parameters and set values.  
selectCMD.Parameters.Add("@CountryRegion", OdbcType.VarChar, 15).Value = "UK"  
selectCMD.Parameters.Add("@City", OdbcType.VarChar, 15).Value = "London"  
  
Dim customers As DataSet = New DataSet  
adapter.Fill(customers, "Customers")  
```  
  
```csharp  
// Assumes that connection is a valid OdbcConnection object.  
OdbcDataAdapter adapter = new OdbcDataAdapter();  
  
OdbcCommand selectCMD = new OdbcCommand(selectSQL, connection);  
adapter.SelectCommand = selectCMD;  
  
//Add Parameters and set values.  
selectCMD.Parameters.Add("@CountryRegion", OdbcType.VarChar, 15).Value = "UK";  
selectCMD.Parameters.Add("@City", OdbcType.VarChar, 15).Value = "London";  
  
DataSet customers = new DataSet();  
adapter.Fill(customers, "Customers");  
```  
  
> [!NOTE]
> Se um nome de parâmetro não for fornecido para um parâmetro, o parâmetro receberá um nome padrão incremental do parâmetro*N* *,* começando com "parâmetro1". Recomendamos que você evite o parâmetro*N* Convenção de nomenclatura ao fornecer um nome de parâmetro, pois o nome que você fornece pode entrar em conflito com um nome de parâmetro padrão existente no `ParameterCollection` . Se o nome fornecido já existir, será gerada uma exceção.  
  
## <a name="see-also"></a>Veja também

- [DataAdapters e DataReaders](dataadapters-and-datareaders.md)
- [Comandos e parâmetros](commands-and-parameters.md)
- [Atualizando fontes de dados com DataAdapters](updating-data-sources-with-dataadapters.md)
- [Modificando dados com procedimentos armazenados](modifying-data-with-stored-procedures.md)
- [Data Type Mappings in ADO.NET](data-type-mappings-in-ado-net.md) (Mapeamentos de tipo de dados no ADO.NET)
- [Visão geral do ADO.NET](ado-net-overview.md)
