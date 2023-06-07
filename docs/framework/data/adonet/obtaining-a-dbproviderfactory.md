---
title: Obtendo um DbProviderFactory
description: Saiba como obter um DbProviderFactory da classe DbProviderFactories para trabalhar com fontes de dados específicas no .NET Framework.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a16e4a4d-6a5b-45db-8635-19570e4572ae
ms.openlocfilehash: b790c87cc3ec293c18bf730567f92b490c7c6594
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286709"
---
# <a name="obtaining-a-dbproviderfactory"></a>Obtendo um DbProviderFactory
O processo de obter <xref:System.Data.Common.DbProviderFactory> envolve passar informações sobre um provedor de dados para a classe <xref:System.Data.Common.DbProviderFactories>. Com base nessas informações, o método <xref:System.Data.Common.DbProviderFactories.GetFactory%2A> cria uma fábrica de provedor fortemente tipada. Por exemplo, para criar <xref:System.Data.SqlClient.SqlClientFactory>, você pode passar para `GetFactory` uma cadeia de caracteres com o nome do provedor especificado como “System.Data.SqlClient”. Outra sobrecarga de `GetFactory` utiliza <xref:System.Data.DataRow>. Uma vez que você criar a fábrica de provedor, poderá usar seus métodos para criar objetos adicionais. Alguns dos métodos de `SqlClientFactory` incluem <xref:System.Data.SqlClient.SqlClientFactory.CreateConnection%2A>, <xref:System.Data.SqlClient.SqlClientFactory.CreateCommand%2A> e <xref:System.Data.SqlClient.SqlClientFactory.CreateDataAdapter%2A>.  
  
> [!NOTE]
> As classes <xref:System.Data.OracleClient.OracleClientFactory>, <xref:System.Data.Odbc.OdbcFactory> e <xref:System.Data.OleDb.OleDbFactory> do .NET Framework também oferecem funcionalidade semelhante.  
  
## <a name="registering-dbproviderfactories"></a>Registrando DbProviderFactories  
 Cada provedor de dados de .NET Framework que dá suporte a uma classe baseada em fábrica registra informações de configuração na seção **DbProviderFactories** do arquivo **Machine. config** no computador local. O fragmento do arquivo de configuração a seguir mostra a sintaxe e o formato para <xref:System.Data.SqlClient>.  
  
```xml  
<system.data>  
  <DbProviderFactories>  
    <add name="SqlClient Data Provider"  
     invariant="System.Data.SqlClient"
     description=".Net Framework Data Provider for SqlServer"
     type="System.Data.SqlClient.SqlClientFactory, System.Data,
     Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"  
    />  
  </DbProviderFactories>  
</system.data>  
```  
  
 O atributo **invariável** identifica o provedor de dados subjacente. Essa sintaxe de nomenclatura de três partes também é usada na criação de uma nova fábrica e para identificar o provedor em um arquivo de configuração de aplicativo, de forma que o nome do provedor, juntamente com a cadeia de conexão associada, possa ser recuperado em tempo de execução.  
  
## <a name="retrieving-provider-information"></a>Recuperando informações sobre provedor  
 Você pode recuperar informações sobre todos os provedores de dados instalados no computador local usando o método <xref:System.Data.Common.DbProviderFactories.GetFactoryClasses%2A>. Ele retorna um <xref:System.Data.DataTable> **DbProviderFactories** nomeado que contém as colunas descritas na tabela a seguir.  
  
|Ordinal de coluna|Nome da coluna|Saída de exemplo|Descrição|  
|--------------------|-----------------|--------------------|-----------------|  
|0|**Nome**|Provedor de Dados SqlClient|Nome legível do provedor de dados|  
|1|**Descrição**|Provedor de Dados .Net Framework para SqlServer|Descrição legível do provedor de dados|  
|2|**InvariantName**|System.Data.SqlClient|Nome que pode ser usado programaticamente para se referir ao provedor de dados|  
|3|**AssemblyQualifiedName**|System.Data.SqlClient.SqlClientFactory, System.Data, Version= 2.0.0.0, = neutral, PublicKeyToken=b77a5c561934e089|Nome totalmente qualificado da classe de fábrica, que contém informações suficientes para criar uma instância do objeto|  
  
 Essa `DataTable` pode ser usada para permitir que um usuário selecione uma <xref:System.Data.DataRow> em tempo de execução. A `DataRow` selecionada pode então ser passada para o método <xref:System.Data.Common.DbProviderFactories.GetFactory%2A> para criar um <xref:System.Data.Common.DbProviderFactory> fortemente tipado. Uma <xref:System.Data.DataRow> selecionada pode ser passada para o método `GetFactory` para criar o objeto `DbProviderFactory` desejado.  
  
## <a name="listing-the-installed-provider-factory-classes"></a>Listando as classes de fábrica de provedor instaladas  
 Este exemplo demonstra como usar o método <xref:System.Data.Common.DbProviderFactories.GetFactoryClasses%2A> para retornar uma <xref:System.Data.DataTable> que contém informações sobre os provedores instalados. O código itera por cada linha na `DataTable`, exibindo informações para cada provedor instalado na janela do console.  
  
 [!code-csharp[DataWorks DbProviderFactories#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DbProviderFactories/CS/source.cs#1)]
 [!code-vb[DataWorks DbProviderFactories#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DbProviderFactories/VB/source.vb#1)]  
  
## <a name="using-application-configuration-files-to-store-factory-information"></a>Usando arquivos de configuração de aplicativo para armazenar informações da fábrica  
 O padrão de design usado para trabalhar com fábricas envolve o armazenamento de informações de cadeia de conexão e provedor em um arquivo de configuração de aplicativo, como **app. config** para um aplicativo do Windows e **Web. config** para um aplicativo ASP.net.  
  
 O fragmento de arquivo de configuração a seguir demonstra como salvar duas cadeias de conexão nomeadas: “NorthwindSQL” para uma conexão com o banco de dados Northwind no SQL Server e “NorthwindAccess” para uma conexão com o banco de dados Northwind no Access/Jet. O nome **invariável** é usado para o atributo **ProviderName** .  
  
```xml  
<configuration>  
  <connectionStrings>  
    <clear/>  
    <add name="NorthwindSQL"
     providerName="System.Data.SqlClient"
     connectionString=  
     "Data Source=MSSQL1;Initial Catalog=Northwind;Integrated Security=true"  
    />  
  
    <add name="NorthwindAccess"
     providerName="System.Data.OleDb"
     connectionString=  
     "Provider=Microsoft.Jet.OLEDB.4.0;Data Source=C:\Data\Northwind.mdb;"  
    />  
  </connectionStrings>  
</configuration>  
```  
  
### <a name="retrieving-a-connection-string-by-provider-name"></a>Recuperando uma cadeia de conexão por nome de provedor  
 Para criar uma fábrica de provedor, você deve fornecer uma cadeia de conexão e o nome do provedor. Este exemplo demonstra como recuperar uma cadeia de conexão de um arquivo de configuração de aplicativo passando o nome do provedor no formato invariável "*System. Data. ProviderName*". O código itera por <xref:System.Configuration.ConnectionStringSettingsCollection>. Ele retorna <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A> em caso de êxito; caso contrário, `null` (`Nothing` no Visual Basic). Se houver várias entradas para um provedor, a primeira encontrada será retornada. Para obter mais informações e exemplos de como recuperar cadeias de conexão de arquivos de configuração, consulte [cadeias de conexão e arquivos de configuração](connection-strings-and-configuration-files.md).  
  
> [!NOTE]
> Uma referência a `System.Configuration.dll` é necessária para que o código seja executado.  
  
 [!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider/CS/source.cs#1)]
 [!code-vb[DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider/VB/source.vb#1)]  
  
## <a name="creating-the-dbproviderfactory-and-dbconnection"></a>Criando DbProviderFactory e DbConnection  
 Este exemplo demonstra como criar um <xref:System.Data.Common.DbProviderFactory> objeto e <xref:System.Data.Common.DbConnection> passando-o o nome do provedor no formato "*System. Data. ProviderName*" e uma cadeia de conexão. Um objeto `DbConnection` é retornado em caso de êxito; e `null` (`Nothing` no Visual Basic) é retornado em caso de erro.  
  
 O código obtém `DbProviderFactory` chamando <xref:System.Data.Common.DbProviderFactories.GetFactory%2A>. Em seguida, o método <xref:System.Data.Common.DbProviderFactory.CreateConnection%2A> cria o objeto <xref:System.Data.Common.DbConnection>, e a propriedade <xref:System.Data.Common.DbConnection.ConnectionString%2A> é definida como a cadeia de conexão.  
  
 [!code-csharp[DataWorks DbProviderFactories.GetFactory#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.GetFactory/CS/source.cs#1)]
 [!code-vb[DataWorks DbProviderFactories.GetFactory#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.GetFactory/VB/source.vb#1)]  
  
## <a name="see-also"></a>Veja também

- [DbProviderFactories](dbproviderfactories.md)
- [Cadeias de conexão](connection-strings.md)
- [Usando as classes de configuração](https://docs.microsoft.com/previous-versions/aspnet/ms228063(v=vs.100))
- [Visão geral do ADO.NET](ado-net-overview.md)
