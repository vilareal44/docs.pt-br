---
title: Consumir um conjunto de um DataSet de um serviço Web XML
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
ms.assetid: 9edd6b71-0fa5-4649-ae1d-ac1c12541019
ms.openlocfilehash: e6dc32274cc3b0d7ec9d66a837a422c87fb2468b
ms.sourcegitcommit: 3492dafceb5d4183b6b0d2f3bdf4a1abc4d5ed8c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/16/2020
ms.locfileid: "86416210"
---
# <a name="consume-a-dataset-from-an-xml-web-service"></a>Consumir um conjunto de um DataSet de um serviço Web XML

O <xref:System.Data.DataSet> foi arquitetado com um design desconectado, em parte, para facilitar o transporte conveniente de dados pela Internet. O **conjunto** de dados é "serializável", pois pode ser especificado como uma entrada ou saída de Web Services XML sem qualquer codificação adicional necessária para transmitir o conteúdo do **conjunto** de dados de um serviço Web XML para um cliente e vice-versa. O **conjunto** de linha de os é implicitamente convertido em um fluxo XML usando o formato DiffGram, enviado pela rede e, em seguida, reconstruído a partir do fluxo XML como um **conjunto** de uma base de recebimentos. Isso fornece um método simples e flexível para transmissão e retorno de dados relacionais usando Web Services XML. Para obter mais informações sobre o formato DiffGram, consulte [DiffGrams](diffgrams.md).  
  
 O exemplo a seguir mostra como criar um serviço Web XML e um cliente que usam o **conjunto** de dados para transportar (incluindo dados modificados) e resolver quaisquer atualizações de volta para a fonte de dados original.  
  
> [!NOTE]
> A transmissão `DataSet` ou `DataTable` instâncias como parte das chamadas de serviço Web XML não serão seguras se a entrada não for confiável. Para obter mais informações, consulte [diretrizes de segurança do conjunto de dados e DataTable](security-guidance.md).
> Também recomendamos que você sempre considere as implicações de segurança ao criar um serviço Web XML. Para obter informações sobre como proteger um serviço Web XML, consulte [protegendo Web Services XML criados usando ASP.net](/previous-versions/dotnet/netframework-4.0/w67h0dw7(v=vs.100)).  
  
## <a name="create-an-xml-web-service"></a>Criar um serviço Web XML
  
1. Crie o serviço Web XML.  
  
     No exemplo, é criado um serviço Web XML que retorna dados, neste caso, uma lista de clientes do banco de dados **Northwind** e recebe um conjunto de um **DataSet** com atualizações para os dados, que o serviço Web XML resolve de volta para a fonte de dado original.  
  
     O Web Service XML expõe dois métodos: **GetCustomers**, para retornar a lista de Customers e **UpdateCustomers**, para resolver atualizações de volta para a fonte de dados. O serviço Web XML é armazenado em um arquivo no servidor Web chamado DataSetSample. asmx. O código a seguir descreve o conteúdo de DataSetSample. asmx.  
  
    ```vb  
    <% @ WebService Language = "vb" Class = "Sample" %>  
    Imports System  
    Imports System.Data  
    Imports System.Data.SqlClient  
    Imports System.Web.Services  
  
    <WebService(Namespace:="http://microsoft.com/webservices/")> _  
    Public Class Sample  
  
    Public connection As SqlConnection = New SqlConnection("Data Source=(local);Integrated Security=SSPI;Initial Catalog=Northwind")  
  
      <WebMethod( Description := "Returns Northwind Customers", EnableSession := False )> _  
      Public Function GetCustomers() As DataSet  
        Dim adapter As SqlDataAdapter = New SqlDataAdapter( _  
          "SELECT CustomerID, CompanyName FROM Customers", connection)  
  
        Dim custDS As DataSet = New DataSet()  
        adapter.MissingSchemaAction = MissingSchemaAction.AddWithKey  
        adapter.Fill(custDS, "Customers")  
  
        Return custDS  
      End Function  
  
      <WebMethod( Description := "Updates Northwind Customers", EnableSession := False )> _  
      Public Function UpdateCustomers(custDS As DataSet) As DataSet  
        Dim adapter As SqlDataAdapter = New SqlDataAdapter()  
  
        adapter.InsertCommand = New SqlCommand( _  
          "INSERT INTO Customers (CustomerID, CompanyName) " & _  
          "Values(@CustomerID, @CompanyName)", connection)  
        adapter.InsertCommand.Parameters.Add( _  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID")  
        adapter.InsertCommand.Parameters.Add( _  
          "@CompanyName", SqlDbType.NChar, 15, "CompanyName")  
  
        adapter.UpdateCommand = New SqlCommand( _  
          "UPDATE Customers Set CustomerID = @CustomerID, " & _  
          "CompanyName = @CompanyName WHERE CustomerID = " & _  
          @OldCustomerID", connection)  
        adapter.UpdateCommand.Parameters.Add( _  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID")  
        adapter.UpdateCommand.Parameters.Add( _  
          "@CompanyName", SqlDbType.NChar, 15, "CompanyName")  
  
        Dim parameter As SqlParameter = _  
          adapter.UpdateCommand.Parameters.Add( _  
          "@OldCustomerID", SqlDbType.NChar, 5, "CustomerID")  
        parameter.SourceVersion = DataRowVersion.Original  
  
        adapter.DeleteCommand = New SqlCommand( _  
          "DELETE FROM Customers WHERE CustomerID = @CustomerID", _  
          connection)  
        parameter = adapter.DeleteCommand.Parameters.Add( _  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID")  
        parameter.SourceVersion = DataRowVersion.Original  
  
        adapter.Update(custDS, "Customers")  
  
        Return custDS  
      End Function  
    End Class  
    ```  
  
    ```csharp  
    <% @ WebService Language = "C#" Class = "Sample" %>  
    using System;  
    using System.Data;  
    using System.Data.SqlClient;  
    using System.Web.Services;  
  
    [WebService(Namespace="http://microsoft.com/webservices/")]  
    public class Sample  
    {  
      public SqlConnection connection = new SqlConnection("Data Source=(local);Integrated Security=SSPI;Initial Catalog=Northwind");  
  
      [WebMethod( Description = "Returns Northwind Customers", EnableSession = false )]  
      public DataSet GetCustomers()  
      {  
        SqlDataAdapter adapter = new SqlDataAdapter(  
          "SELECT CustomerID, CompanyName FROM Customers", connection);  
  
        DataSet custDS = new DataSet();  
        adapter.MissingSchemaAction = MissingSchemaAction.AddWithKey;  
        adapter.Fill(custDS, "Customers");  
  
        return custDS;  
      }  
  
      [WebMethod( Description = "Updates Northwind Customers",  
        EnableSession = false )]  
      public DataSet UpdateCustomers(DataSet custDS)  
      {  
        SqlDataAdapter adapter = new SqlDataAdapter();  
  
        adapter.InsertCommand = new SqlCommand(  
          "INSERT INTO Customers (CustomerID, CompanyName) " +  
          "Values(@CustomerID, @CompanyName)", connection);  
        adapter.InsertCommand.Parameters.Add(  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID");  
        adapter.InsertCommand.Parameters.Add(  
          "@CompanyName", SqlDbType.NChar, 15, "CompanyName");  
  
        adapter.UpdateCommand = new SqlCommand(  
          "UPDATE Customers Set CustomerID = @CustomerID, " +  
          "CompanyName = @CompanyName WHERE CustomerID = " +  
          "@OldCustomerID", connection);  
        adapter.UpdateCommand.Parameters.Add(  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID");  
        adapter.UpdateCommand.Parameters.Add(  
          "@CompanyName", SqlDbType.NChar, 15, "CompanyName");  
        SqlParameter parameter = adapter.UpdateCommand.Parameters.Add(  
          "@OldCustomerID", SqlDbType.NChar, 5, "CustomerID");  
        parameter.SourceVersion = DataRowVersion.Original;  
  
        adapter.DeleteCommand = new SqlCommand(  
        "DELETE FROM Customers WHERE CustomerID = @CustomerID",  
         connection);  
        parameter = adapter.DeleteCommand.Parameters.Add(  
          "@CustomerID", SqlDbType.NChar, 5, "CustomerID");  
        parameter.SourceVersion = DataRowVersion.Original;  
  
        adapter.Update(custDS, "Customers");  
  
        return custDS;  
      }  
    }  
    ```  
  
     Em um cenário típico, o método **UpdateCustomers** seria escrito para capturar violações de simultaneidade otimistas. Para simplificar, o exemplo não inclui isso. Para obter mais informações sobre simultaneidade otimista, consulte [simultaneidade otimista](../optimistic-concurrency.md).  
  
2. Crie um proxy de serviço Web XML.  
  
     Os clientes do serviço Web XML exigem um proxy SOAP para consumir os métodos expostos. Você pode fazer com que o Visual Studio gere esse proxy para você. Ao definir uma referência Web para um serviço Web existente de dentro do Visual Studio, todo o comportamento descrito nesta etapa ocorre de forma transparente. Se você quiser criar a classe proxy por conta própria, continue com esta discussão. No entanto, na maioria das circunstâncias, o uso do Visual Studio para criar a classe de proxy para o aplicativo cliente é suficiente.  
  
     Um proxy pode ser criado usando a ferramenta de linguagem de descrição de serviços Web. Por exemplo, se o serviço Web XML for exposto na URL `http://myserver/data/DataSetSample.asmx` , emita um comando como o seguinte para criar um Visual Basic proxy .NET com um namespace de **WebData. DSSample** e armazená-lo no arquivo Sample. vb.  
  
    ```console
    wsdl /l:VB -out:sample.vb http://myserver/data/DataSetSample.asmx /n:WebData.DSSample  
    ```  
  
     Para criar um proxy C# no arquivo sample.cs, emita o comando a seguir.  
  
    ```console
    wsdl -l:CS -out:sample.cs http://myserver/data/DataSetSample.asmx -n:WebData.DSSample  
    ```  
  
     O proxy pode então ser compilado como uma biblioteca e importado para o cliente do serviço Web XML. Para compilar o Visual Basic código de proxy .NET armazenado em Sample. vb como sample.dll, emita o comando a seguir.  
  
    ```console  
    vbc -t:library -out:sample.dll sample.vb -r:System.dll -r:System.Web.Services.dll -r:System.Data.dll -r:System.Xml.dll  
    ```  
  
     Para compilar o código de proxy C# armazenado no sample.cs como sample.dll, emita o comando a seguir.  
  
    ```console
    csc -t:library -out:sample.dll sample.cs -r:System.dll -r:System.Web.Services.dll -r:System.Data.dll -r:System.Xml.dll  
    ```  
  
3. Criar um cliente de serviço Web XML.  
  
     Se você quiser que o Visual Studio gere a classe proxy do serviço Web para você, simplesmente crie o projeto cliente e, na janela Gerenciador de soluções, clique com o botão direito do mouse no projeto e selecione **Adicionar**  >  **referência de serviço**. Na caixa de diálogo **Adicionar referência de serviço** , selecione **avançado**e, em seguida, selecione **Adicionar referência Web**. Selecione o serviço Web na lista de serviços Web disponíveis (isso pode exigir o fornecimento do endereço do ponto de extremidade do serviço Web se o serviço Web não estiver disponível na solução atual ou no computador atual). Se você mesmo criar o proxy do Web Service XML (conforme descrito na etapa anterior), você poderá importá-lo para o código do cliente e consumir os métodos do Web Service XML.

     O código de exemplo a seguir importa a biblioteca de proxy, chama **GetCustomers** para obter uma lista de clientes, adiciona um novo Customer e, em seguida, retorna um **DataSet** com as atualizações para **UpdateCustomers**.  
  
     O exemplo passa o **conjunto** de registros retornado por **DataSet. GetChanges** para **UpdateCustomers** porque apenas as linhas modificadas precisam ser passadas para **UpdateCustomers**. **UpdateCustomers** retorna o **conjunto**de dados resolvido, que você pode **mesclar** no **conjunto** de dados existente para incorporar as alterações resolvidas e as informações de erro de linha da atualização. O código a seguir pressupõe que você usou o Visual Studio para criar a referência Web e que você renomeou a referência Web para DsSample na caixa de diálogo **Adicionar referência Web** .  
  
    ```vb  
    Imports System  
    Imports System.Data  
  
    Public Class Client  
  
      Public Shared Sub Main()  
        Dim proxySample As New DsSample.Sample ()  ' Proxy object.  
        Dim customersDataSet As DataSet = proxySample.GetCustomers()  
        Dim customersTable As DataTable = _  
          customersDataSet.Tables("Customers")  
  
        Dim rowAs DataRow = customersTable.NewRow()  
        row("CustomerID") = "ABCDE"  
        row("CompanyName") = "New Company Name"  
        customersTable.Rows.Add(row)  
  
        Dim updateDataSet As DataSet = _  
          proxySample.UpdateCustomers(customersDataSet.GetChanges())  
  
        customersDataSet.Merge(updateDataSet)  
        customersDataSet.AcceptChanges()  
      End Sub  
    End Class  
    ```  
  
    ```csharp  
    using System;  
    using System.Data;  
  
    public class Client  
    {  
      public static void Main()  
      {  
        Sample proxySample = new DsSample.Sample();  // Proxy object.  
        DataSet customersDataSet = proxySample.GetCustomers();  
        DataTable customersTable = customersDataSet.Tables["Customers"];  
  
        DataRow row = customersTable.NewRow();  
        row["CustomerID"] = "ABCDE";  
        row["CompanyName"] = "New Company Name";  
        customersTable.Rows.Add(row);  
  
        DataSet updateDataSet = new DataSet();  
  
        updateDataSet =
          proxySample.UpdateCustomers(customersDataSet.GetChanges());  
  
        customersDataSet.Merge(updateDataSet);  
        customersDataSet.AcceptChanges();  
      }  
    }  
    ```  
  
     Se você decidir criar a classe proxy por conta própria, deverá executar as etapas adicionais a seguir. Para compilar o exemplo, forneça a biblioteca de proxy que foi criada (sample.dll) e as bibliotecas do .NET relacionadas. Para compilar a versão Visual Basic .NET do exemplo, armazenado no arquivo client. vb, emita o comando a seguir.  
  
    ```console
    vbc client.vb -r:sample.dll -r:System.dll -r:System.Data.dll -r:System.Xml.dll -r:System.Web.Services.dll  
    ```  
  
     Para compilar a versão em C# do exemplo, armazenado no arquivo client.cs, emita o comando a seguir.  
  
    ```console
    csc client.cs -r:sample.dll -r:System.dll -r:System.Data.dll -r:System.Xml.dll -r:System.Web.Services.dll  
    ```  
  
## <a name="see-also"></a>Confira também

- [ADO.NET](../index.md)
- [DataSets, DataTables e DataViews](index.md)
- [DataTables](datatables.md)
- [Populando um DataSet a partir de um DataAdapter](../populating-a-dataset-from-a-dataadapter.md)
- [Atualizando fontes de dados com DataAdapters](../updating-data-sources-with-dataadapters.md)
- [Parâmetros DataAdapter](../dataadapter-parameters.md)
- [Ferramenta de linguagem de descrição de serviços Web (Wsdl.exe)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/7h3ystb6(v=vs.100))
- [Visão geral do ADO.NET](../ado-net-overview.md)
