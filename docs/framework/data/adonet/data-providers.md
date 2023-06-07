---
title: Provedores de dados .NET Framework
description: Saiba como um provedor de dados de .NET Framework é usado para se conectar a um banco de dado, executar comandos e recuperar resultados em ADO.NET.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 03a9fc62-2d24-491a-9fe6-d6bdb6dcb131
ms.openlocfilehash: 2d4c513b7a4b0e111f2b7e7384c6ee4970d5665f
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286993"
---
# <a name="net-framework-data-providers"></a>Provedores de dados .NET Framework
Um provedor de dados .NET Framework é usado para se conectar a um banco de dado, executar comandos e recuperar resultados. Os resultados são processados diretamente, colocados no <xref:System.Data.DataSet> para serem expostos ao usuário conforme o necessário, combinados com os dados de várias fontes ou colocados remotamente entre camadas. .NET Framework provedores de dados são leves, criando uma camada mínima entre a fonte de dados e o código, aumentando o desempenho sem sacrificar a funcionalidade.  
  
 A tabela a seguir lista os provedores de dados que estão incluídos no .NET Framework.  
  
|Provedor de dados .NET Framework|Description|  
|-------------------------------------------------------------------------------|-----------------|  
|Provedor de dados do .NET Framework para SQL Server|Fornece acesso a dados para o Microsoft SQL Server. Usa o namespace <xref:System.Data.SqlClient>.|  
|Provedor de dados .NET Framework para OLE DB|Para fontes de dados expostas usando o OLE DB. Usa o namespace <xref:System.Data.OleDb>.|  
|Provedor de dados .NET Framework para ODBC|Para fontes de dados expostas usando ODBC. Usa o namespace <xref:System.Data.Odbc>.|  
|Provedor de Dados do .NET Framework para Oracle|Para fontes de dados Oracle. O .NET Framework Provedor de Dados para Oracle dá suporte ao software cliente Oracle versão 8.1.7 e posterior e usa o <xref:System.Data.OracleClient> namespace.|  
|Provedor EntityClient|Fornece acesso a dados para aplicativos de Modelo de Dados de Entidade (EDM). Usa o namespace <xref:System.Data.EntityClient>.|  
|.NET Framework Provedor de Dados para SQL Server Compact 4,0.|Fornece acesso a dados para o Microsoft SQL Server Compact 4,0. Usa o namespace [System. Data. SqlServerCe](https://docs.microsoft.com/previous-versions/sql/compact/sql-server-compact-4.0/ec4st0e3(v=vs.100)) .|  
  
## <a name="core-objects-of-net-framework-data-providers"></a>Os objetos principais de provedores de dados .NET Framework  
 A tabela a seguir descreve os quatro objetos principais que compõem um provedor de dados .NET Framework.  
  
|Objeto|Descrição|  
|------------|-----------------|  
|`Connection`|Estabelece uma conexão com uma fonte de dados específica. A classe base para todos os objetos de `Connection` é a classe <xref:System.Data.Common.DbConnection>.|  
|`Command`|Executa um comando em uma fonte de dados. Expõe `Parameters` e pode ser executado no escopo de uma `Transaction` de `Connection`. A classe base para todos os objetos de `Command` é a classe <xref:System.Data.Common.DbCommand>.|  
|`DataReader`|Ler um fluxo de dados apenas de encaminhamento e somente leitura de uma fonte de dados. A classe base para todos os objetos de `DataReader` é a classe <xref:System.Data.Common.DbDataReader>.|  
|`DataAdapter`|Preenche um `DataSet` e resolve atualizações com a fonte de dados. A classe base para todos os objetos de `DataAdapter` é a classe <xref:System.Data.Common.DbDataAdapter>.|  
  
 Além das classes principais listadas na tabela anterior neste documento, um provedor de dados .NET Framework também contém as classes listadas na tabela a seguir.  
  
|Objeto|Descrição|  
|------------|-----------------|  
|`Transaction`|Insere comandos nas transações na fonte de dados. A classe base para todos os objetos de `Transaction` é a classe <xref:System.Data.Common.DbTransaction>. O ADO.NET também fornece suporte para transações usando classes no namespace <xref:System.Transactions>.|  
|`CommandBuilder`|Um objeto auxiliar que gera automaticamente propriedades de comando de um `DataAdapter` ou deriva informações de parâmetro de um procedimento armazenado e preenche a coleção `Parameters` de um objeto `Command`. A classe base para todos os objetos de `CommandBuilder` é a classe <xref:System.Data.Common.DbCommandBuilder>.|  
|`ConnectionStringBuilder`|Um objeto auxiliar que fornece uma maneira simples de criar e gerenciar o conteúdo de cadeias de conexão usadas por objetos de `Connection`. A classe base para todos os objetos de `ConnectionStringBuilder` é a classe <xref:System.Data.Common.DbConnectionStringBuilder>.|  
|`Parameter`|Define os parâmetros de valores de entrada, saída e retorno para comandos e procedimentos armazenados. A classe base para todos os objetos de `Parameter` é a classe <xref:System.Data.Common.DbParameter>.|  
|`Exception`|Retornado quando um erro é encontrado na fonte de dados. Para um erro encontrado no cliente, .NET Framework provedores de dados geram uma exceção .NET Framework. A classe base para todos os objetos de `Exception` é a classe <xref:System.Data.Common.DbException>.|  
|`Error`|Expõe as informações de um aviso ou erro retornado por uma fonte de dados.|  
|`ClientPermission`|Fornecido para .NET Framework atributos de segurança de acesso ao código do provedor de dados. A classe base para todos os objetos de `ClientPermission` é a classe <xref:System.Data.Common.DBDataPermission>.|  
  
## <a name="net-framework-data-provider-for-sql-server-sqlclient"></a>.NET Framework Provedor de Dados para SQL Server (SqlClient)  
 O .NET Framework Provedor de Dados para SQL Server (SqlClient) usa seu próprio protocolo para se comunicar com SQL Server. É leve e funciona bem porque é otimizado para acessar um SQL Server diretamente sem adicionar uma camada de OLE DB ou ODBC (conectividade de banco de dados aberta). A ilustração a seguir contrasta o .NET Framework Provedor de Dados para SQL Server com a Provedor de Dados .NET Framework para OLE DB. O Provedor de Dados de .NET Framework para OLE DB se comunica com uma fonte de dados de OLE DB por meio do componente de serviço OLE DB, que fornece os serviços de pooling de conexão e de transação e o provedor de OLE DB para a fonte de dados.  
  
> [!NOTE]
> O Provedor de Dados .NET Framework para ODBC tem uma arquitetura semelhante à Provedor de Dados de .NET Framework para OLE DB; por exemplo, ele chama um componente de serviço ODBC.  
  
 ![Provedores de dados](./media/netdataproviders-bpuedev11.gif "NETDataProviders_bpuedev11")  
Comparação do Provedor de dados .NET Framework para SQL Server e o Provedor de dados .NET Framework para OLE DB  
  
 O Provedor de Dados .NET Framework para classes de SQL Server estão localizados no <xref:System.Data.SqlClient> namespace.  
  
 O Provedor de Dados .NET Framework para SQL Server dá suporte a transações locais e distribuídas. Para transações distribuídas, o .NET Framework Provedor de Dados para SQL Server, por padrão, é automaticamente inscrito em uma transação e obtém os detalhes da transação dos serviços de componentes do Windows ou <xref:System.Transactions> . Para obter mais informações, consulte [Transações e simultaneidade](transactions-and-concurrency.md).  
  
 O exemplo de código a seguir mostra como incluir o namespace `System.Data.SqlClient` em seus aplicativos.  
  
```vb  
Imports System.Data.SqlClient  
```  
  
```csharp  
using System.Data.SqlClient;  
```  
  
## <a name="net-framework-data-provider-for-ole-db"></a>Provedor de dados .NET Framework para OLE DB  
 O .NET Framework Provedor de Dados para OLE DB (OleDb) usa o OLE DB nativo por meio de interoperabilidade COM para habilitar o acesso a dados. O Provedor de Dados .NET Framework para OLE DB dá suporte a transações locais e distribuídas. Para transações distribuídas, o .NET Framework Provedor de Dados para OLE DB, por padrão, é automaticamente inscrito em uma transação e obtém os detalhes da transação dos serviços de componentes do Windows. Para obter mais informações, consulte [Transações e simultaneidade](transactions-and-concurrency.md).  
  
 A tabela a seguir mostra os provedores que foram testados com o ADO.NET.  
  
|Driver|Provedor|  
|------------|--------------|  
|SQLOLEDB|Provedor de OLE DB da Microsoft para SQL Server|  
|MSDAORA|Provedor do Microsoft OLE DB para Oracle|  
|Microsoft.Jet.OLEDB.4.0|Provedor do OLE DB para Microsoft Jet|  
  
> [!NOTE]
> Não é recomendável usar um banco de dados do Access (Jet) como uma fonte para aplicativos multithread, como aplicativos ASP.NET. Se você precisar usar o Jet como uma fonte de dados para um aplicativo ASP.NET, perceba que os aplicativos ASP.NET que se conectam a um banco de dado do Access podem encontrar problemas de conexão.  
  
 O Provedor de Dados .NET Framework para OLE DB não oferece suporte às interfaces OLE DB versão 2,5. OLE DB provedores que exigem suporte para interfaces OLE DB 2,5 não funcionarão corretamente com o Provedor de Dados de .NET Framework para OLE DB. Isso inclui o provedor do Microsoft OLE DB para Exchange e o provedor do Microsoft OLE DB para publicação na Internet.  
  
 O Provedor de Dados .NET Framework para OLE DB não funciona com o provedor de OLE DB para ODBC (MSDASQL). Para acessar uma fonte de dados ODBC usando o ADO.NET, use o Provedor de Dados .NET Framework para ODBC.  
  
 .NET Framework Provedor de Dados para as classes de OLE DB estão localizadas no <xref:System.Data.OleDb> namespace. O exemplo de código a seguir mostra como incluir o namespace `System.Data.OleDb` em seus aplicativos.  
  
```vb  
Imports System.Data.OleDb  
```  
  
```csharp  
using System.Data.OleDb;  
```  
  
## <a name="net-framework-data-provider-for-odbc"></a>Provedor de dados .NET Framework para ODBC  
 O .NET Framework Provedor de Dados para ODBC (ODBC) usa o DM (Gerenciador de driver ODBC) nativo para habilitar o acesso a dados. O provedor de dados do ODBC oferece suporte a transações locais e distribuídas. Para transações distribuídas, o Provedor de dados do ODBC, por padrão, automaticamente inscreve-se em uma transação e obtém detalhes de transação do Windows Component Services. Para obter mais informações, consulte [Transações e simultaneidade](transactions-and-concurrency.md).  
  
 A tabela a seguir mostra os drivers ODBC testados com ADO.NET.  
  
|Driver|  
|------------|  
|SQL Server|  
|Microsoft ODBC para Oracle|  
|Driver do Microsoft Access (*.mdb)|  
  
 .NET Framework Provedor de Dados para classes ODBC estão localizadas no <xref:System.Data.Odbc> namespace.  
  
 O exemplo de código a seguir mostra como incluir o namespace `System.Data.Odbc` em seus aplicativos.  
  
```vb  
Imports System.Data.Odbc  
```  
  
```csharp  
using System.Data.Odbc;  
```  
  
> [!NOTE]
> O .NET Framework Provedor de Dados para ODBC requer o MDAC 2,6 ou uma versão posterior, e o MDAC 2,8 SP1 é recomendado. Você pode baixar o MDAC 2,8 SP1 no [centro de download da Microsoft](https://www.microsoft.com/download/details.aspx?id=5793).
  
## <a name="net-framework-data-provider-for-oracle"></a>Provedor de Dados do .NET Framework para Oracle  
 O .NET Framework Provedor de Dados para Oracle (OracleClient) permite o acesso a dados para fontes de dados Oracle por meio do software de conectividade de cliente Oracle. O provedor de dados oferece suporte a um software cliente do Oracle versão 8.1.7 ou posterior. O provedor de dados oferece suporte a transações locais e distribuídas. Para obter mais informações, consulte [Transações e simultaneidade](transactions-and-concurrency.md).  
  
 O .NET Framework Provedor de Dados para Oracle requer o software cliente Oracle (versão 8.1.7 ou uma versão posterior) no sistema para que você possa se conectar a uma fonte de dados Oracle.  
  
 .NET Framework Provedor de Dados para classes Oracle estão localizadas no <xref:System.Data.OracleClient> namespace e estão contidas no `System.Data.OracleClient.dll` assembly. Você deve fazer referência a `System.Data.dll` e a `System.Data.OracleClient.dll` ao criar um aplicativo que usa o provedor de dados.  
  
 O exemplo de código a seguir mostra como incluir o namespace `System.Data.OracleClient` em seus aplicativos.  
  
```vb  
Imports System.Data  
Imports System.Data.OracleClient  
```  
  
```csharp  
using System.Data;  
using System.Data.OracleClient;  
```  
  
## <a name="choosing-a-net-framework-data-provider"></a>Escolhendo um Provedor de dados .NET Framework  
 Dependendo do design e da fonte de dados do seu aplicativo, sua escolha de .NET Framework provedor de dados pode melhorar o desempenho, a capacidade e a integridade do seu aplicativo. A tabela a seguir discute as vantagens e limitações de cada provedor de dados de .NET Framework.  
  
|Provedor|Observações|  
|--------------|-----------|  
|Provedor de dados do .NET Framework para SQL Server|Recomendado para aplicativos de camada intermediária que usam Microsoft SQL Server.<br /><br /> Recomendado para aplicativos de camada única que usam o Microsoft Mecanismo de Banco de Dados (MSDE) ou SQL Server.<br /><br /> Recomendado para uso do provedor de OLE DB para SQL Server (SQLOLEDB) com o Provedor de Dados de .NET Framework para OLE DB.|  
|Provedor de dados .NET Framework para OLE DB|Por SQL Server, o .NET Framework Provedor de Dados para SQL Server é recomendado em vez desse provedor.<br /><br /> Recomendado para aplicativos de camada única que usam os bancos de dados Microsoft Access. O uso de um banco de dados Access para um aplicativo de camada intermediária não é recomendado.|  
|Provedor de dados .NET Framework para ODBC|Recomendado para aplicativos de camada única e intermediária que usam fontes de dados ODBC.|  
|Provedor de Dados do .NET Framework para Oracle|Recomendado para aplicativos de camada única e intermediária que usam fontes de dados Oracle.|  
  
## <a name="entityclient-provider"></a>Provedor EntityClient  
 O provedor EntityClient é usado para acessar dados com base no EDM (Modelo de Dados de Entidade). Diferentemente de outros provedores de dados .NET Framework, ele não interage diretamente com uma fonte de dados. Em vez disso, ele usa o Entity SQL para se comunicar com o provedor de dados subjacente. Para obter mais informações, consulte [EntityClient Provider para o Entity Framework](./ef/entityclient-provider-for-the-entity-framework.md).  
  
## <a name="see-also"></a>Veja também

- [Visão geral do ADO.NET](ado-net-overview.md)
- [Retrieving and Modifying Data in ADO.NET](retrieving-and-modifying-data.md) (Recuperando e modificando dados no ADO.NET)
