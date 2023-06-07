---
title: Sintaxe da cadeia de conexão
description: Saiba mais sobre a sintaxe de cadeias de conexão em ADO.NET. A sintaxe de cada provedor é documentada em sua propriedade ConnectionString.
ms.date: 05/22/2018
ms.assetid: 0977aeee-04d1-4cce-bbed-750c77fce06e
ms.openlocfilehash: bb29365a4729e731ddeffc7cfa61e379c3144a46
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287045"
---
# <a name="connection-string-syntax"></a>Sintaxe da cadeia de conexão
Cada provedor de dados .NET Framework tem um objeto de `Connection` que herda de <xref:System.Data.Common.DbConnection> bem como de uma propriedade <xref:System.Data.Common.DbConnection.ConnectionString%2A> específica do provedor. A sintaxe específica da cadeia de conexão para cada provedor está documentada em sua propriedade `ConnectionString`. A tabela a seguir lista os quatro provedores de dados que estão incluídos no .NET Framework.  
  
|Provedor de dados .NET Framework|Description|  
|----------------------------------|-----------------|  
|<xref:System.Data.SqlClient>|Fornece acesso a dados para o Microsoft SQL Server. Para obter mais informações sobre a sintaxe da cadeia de conexão, consulte <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>.|  
|<xref:System.Data.OleDb>|Fornece acesso a dados para as fontes de dados expostas usando OLE DB. Para obter mais informações sobre a sintaxe da cadeia de conexão, consulte <xref:System.Data.OleDb.OleDbConnection.ConnectionString%2A>.|  
|<xref:System.Data.Odbc>|Fornece acesso a dados para as fontes de dados expostas usando ODBC. Para obter mais informações sobre a sintaxe da cadeia de conexão, consulte <xref:System.Data.Odbc.OdbcConnection.ConnectionString%2A>.|  
|<xref:System.Data.OracleClient>|Fornece acesso a dados para Oracle versão 8.1.7 ou posterior. Para obter mais informações sobre a sintaxe da cadeia de conexão, consulte <xref:System.Data.OracleClient.OracleConnection.ConnectionString%2A>.|  
  
## <a name="connection-string-builders"></a>Construtores de cadeia de conexão  
 O ADO.NET 2.0 introduziu os seguintes construtores de cadeia de conexão para provedores de dados .NET Framework.  
  
- <xref:System.Data.SqlClient.SqlConnectionStringBuilder>  
  
- <xref:System.Data.OleDb.OleDbConnectionStringBuilder>  
  
- <xref:System.Data.Odbc.OdbcConnectionStringBuilder>  
  
- <xref:System.Data.OracleClient.OracleConnectionStringBuilder>  
  
 Os construtores de cadeia de conexão permitem que você construa cadeias de conexão sintaticamente válidas em tempo de execução, para que você não tenha que manualmente concatenar os valores de cadeia de conexão no seu código. Para obter mais informações, confira [Construtores de cadeias de conexão](connection-string-builders.md).  

## <a name="windows-authentication"></a>Autenticação do Windows  
 É recomendável usar a autenticação do Windows (às vezes conhecida como *segurança integrada*) para se conectar a fontes de dados que dão suporte a ela. A sintaxe empregada na cadeia de conexão varia de acordo com o provedor. A tabela a seguir mostra a sintaxe de Autenticação do Windows usada com os provedores de dados .NET Framework.  
  
|Provedor|Sintaxe|  
|--------------|------------|  
|`SqlClient`|`Integrated Security=true;`<br /><br /> `-- or --`<br /><br /> `Integrated Security=SSPI;`|  
|`OleDb`|`Integrated Security=SSPI;`|  
|`Odbc`|`Trusted_Connection=yes;`|  
|`OracleClient`|`Integrated Security=yes;`|  
  
> [!NOTE]
> O `Integrated Security=true` gera uma exceção quando usado com o provedor `OleDb`.  
  
## <a name="sqlclient-connection-strings"></a>Cadeias de conexão do SqlClient  
A sintaxe para uma cadeia de conexão <xref:System.Data.SqlClient.SqlConnection> está documentada na propriedade <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>. Você pode usar a propriedade <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A> para obter ou definir uma cadeia de conexão para um banco de dados do SQL Server. Se você precisar de conexão a uma versão anterior do SQL Server, você deve usar o provedor de dados .NET Framework para OleDb (<xref:System.Data.OleDb>). A maioria das palavras-chave de cadeia de conexão também mapeiam para as propriedades no <xref:System.Data.SqlClient.SqlConnectionStringBuilder>.  

> [!IMPORTANT]
> A configuração padrão para a `Persist Security Info` palavra-chave é `false` . Configurá-lo como `true` ou `yes` permite informações confidenciais de segurança, incluindo a identificação de usuário e a senha, para serem obtidas da conexão depois que ela tiver sido aberta. Mantenha `Persist Security Info` definido como `false` para garantir que uma fonte não confiável não tenha acesso às informações de cadeia de conexão confidenciais.  

### <a name="windows-authentication-with-sqlclient"></a>Autenticação do Windows com SqlClient
 Cada uma das formas de sintaxe a seguir usa a autenticação do Windows para se conectar ao banco de dados **AdventureWorks** em um servidor local.  
  
```csharp  
"Persist Security Info=False;Integrated Security=true;  
    Initial Catalog=AdventureWorks;Server=MSSQL1"  
"Persist Security Info=False;Integrated Security=SSPI;  
    database=AdventureWorks;server=(local)"  
"Persist Security Info=False;Trusted_Connection=True;  
    database=AdventureWorks;server=(local)"  
```  
  
### <a name="sql-server-authentication-with-sqlclient"></a>Autenticação de SQL Server com SqlClient
 A Autenticação do Windows é preferencial para se conectar ao SQL Server. No entanto, se a Autenticação do SQL Server for necessária, use a seguinte sintaxe para especificar um nome de usuário e uma senha. Nesse exemplo, os asteriscos são usados para representar um nome de usuário e uma senha válidos.  
  
```csharp  
"Persist Security Info=False;User ID=*****;Password=*****;Initial Catalog=AdventureWorks;Server=MySqlServer"  
```  

Quando você se conectar ao banco de dados SQL do Azure ou ao SQL Data Warehouse do Azure e fornecer um logon no formato `user@servername` , verifique se o `servername` valor no logon corresponde ao valor fornecido para `Server=` .

> [!NOTE]
> A autenticação do Windows tem precedência sobre logons do SQL Server. Se você especificar Integrated Security=true assim como um nome de usuário e uma senha, o nome de usuário e a senha serão ignorados e a autenticação do Windows será usada.  

### <a name="connect-to-a-named-instance-of-sql-server"></a>Conectar-se a uma instância nomeada do SQL Server
Para se conectar a uma instância nomeada do SQL Server, use a sintaxe de *nome \ instância do servidor* .  
  
```csharp  
"Data Source=MySqlServer\MSSQL1;"  
```  

Você também pode definir a propriedade <xref:System.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A> de `SqlConnectionStringBuilder` para o nome da instância ao criar uma cadeia de conexão. A propriedade <xref:System.Data.SqlClient.SqlConnection.DataSource%2A> de um objeto <xref:System.Data.SqlClient.SqlConnection> é somente leitura.  
  
### <a name="type-system-version-changes"></a>Alterações de versão do sistema de tipos  
 A `Type System Version` palavra-chave em um <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> especifica a representação do lado do cliente de tipos de SQL Server. Consulte <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> para obter mais informações sobre a palavra-chave `Type System Version`.  
  
## <a name="connecting-and-attaching-to-sql-server-express-user-instances"></a>Conectando e anexando a instâncias de usuário do SQL Server Express  
 As instâncias de usuário são um recurso no SQL Server Express. Elas permitem que um usuário que esteja executando uma conta local do Windows com menos privilégios anexe e execute um banco de dados do SQL Server sem exigir privilégios administrativos. Uma instância de usuário é executada com as credenciais do Windows do usuário, não como um serviço.  
  
 Para obter mais informações sobre como trabalhar com instâncias de usuário, consulte [SQL Server Express instâncias de usuário](./sql/sql-server-express-user-instances.md).  
  
## <a name="using-trustservercertificate"></a>Usando TrustServerCertificate  
 A `TrustServerCertificate` palavra-chave é válida somente ao se conectar a uma instância do SQL Server com um certificado válido. Quando `TrustServerCertificate` estiver definido como `true`, a camada de transporte usará SSL para criptografar o canal e não precisar passar pela verificação da cadeia do certificado para validar a confiança.  
  
```csharp  
"TrustServerCertificate=true;"
```  
  
> [!NOTE]
> Se `TrustServerCertificate` estiver definido como `true` e a criptografia estiver ativada, o nível de criptografia especificado no servidor será usado mesmo que `Encrypt` esteja definido como `false` na cadeia de conexão. A conexão falhará se isso for feito de outra maneira.  
  
### <a name="enabling-encryption"></a>Habilitando a criptografia  
 Para habilitar a criptografia quando um certificado não foi provisionado no servidor, as opções **forçar criptografia de protocolo** e certificado de **servidor confiável** devem ser definidas em SQL Server Configuration Manager. Neste caso, a criptografia usará um certificado do servidor autoassinado sem validação, se nenhum certificado verificável tiver sido provisionado no servidor.  
  
 As configurações do aplicativo não podem reduzir o nível de segurança configurado no SQL Server, mas podem opcionalmente reforçá-lo. Um aplicativo pode solicitar criptografia definindo as `TrustServerCertificate` `Encrypt` palavras-chave e como `true` , garantindo que a criptografia ocorra mesmo quando um certificado de servidor não tiver sido provisionado e forçar a **criptografia de protocolo** não ter sido configurada para o cliente. No entanto, se `TrustServerCertificate` não estiver ativado na configuração do cliente, um certificado do servidor provisionado ainda será necessário.  
  
 A tabela a seguir descreve todos os casos.  
  
|Configuração do cliente Forçar Criptografia de Protocolo|Configuração do cliente Confiar em Certificado do Servidor|Criptografar/Usar criptografia para a cadeia de conexão/atributo de dados|Cadeia de conexão/atributo do Certificado do Servidor de Confiança|Resultado|  
|----------------------------------------------|---------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------|------------|  
|Não|N/D|Não (padrão)|Ignored|Não ocorre criptografia.|  
|Não|N/D|Sim|Não (padrão)|A criptografia só ocorrerá se houver um certificado de servidor verificável; caso contrário, a tentativa de conexão falhará.|  
|Não|N/D|Sim|Sim|A criptografia sempre ocorre, mas pode usar um certificado do servidor autoassinado.|  
|Sim|Não|Ignored|Ignored|A criptografia ocorre somente se houver um certificado de servidor verificável; caso contrário, a tentativa de conexão falhará.|  
|Sim|Sim|Não (padrão)|Ignored|A criptografia sempre ocorre, mas pode usar um certificado do servidor autoassinado.|  
|Sim|Sim|Sim|Não (padrão)|A criptografia ocorre somente se houver um certificado de servidor verificável; caso contrário, a tentativa de conexão falhará.|  
|Sim|Sim|Sim|Sim|A criptografia sempre ocorre, mas pode usar um certificado do servidor autoassinado.|  
  
 Para obter mais informações, confira [Usando criptografia sem validação](/sql/relational-databases/native-client/features/using-encryption-without-validation).
  
## <a name="oledb-connection-strings"></a>Cadeias de conexão do OleDb  
 A propriedade <xref:System.Data.OleDb.OleDbConnection.ConnectionString%2A> de um <xref:System.Data.OleDb.OleDbConnection> permite que você obtenha ou defina uma cadeia de conexão para uma fonte de dados do OLE DB, como o Microsoft Access. Você também pode criar uma cadeia de conexão `OleDb` em tempo de execução usando a classe <xref:System.Data.OleDb.OleDbConnectionStringBuilder>.  
  
### <a name="oledb-connection-string-syntax"></a>Sintaxe da cadeia de conexão OleDb  
 Você deve especificar um nome de provedor para uma cadeia de conexão <xref:System.Data.OleDb.OleDbConnection>. A seguinte cadeia de conexão conecta-se a um banco de dados do Microsoft Access usando o provedor Jet. Observe que as palavras-chave `User ID` e `Password` serão opcionais se o banco de dados for inseguro (o padrão).  
  
```csharp
Provider=Microsoft.Jet.OLEDB.4.0; Data Source=d:\Northwind.mdb;User ID=Admin;Password=;
```  
  
 Se o banco de dados Jet estiver protegido usando a segurança de nível de usuário, você deverá fornecer o local do arquivo de informações do grupo de trabalho (.mdw). O arquivo de informações do grupo de trabalho é usado para validar as credenciais apresentadas na cadeia de conexão.  
  
```csharp
Provider=Microsoft.Jet.OLEDB.4.0;Data Source=d:\Northwind.mdb;Jet OLEDB:System Database=d:\NorthwindSystem.mdw;User ID=*****;Password=*****;  
```  
  
> [!IMPORTANT]
> É possível fornecer informações de conexão para uma **OleDbConnection** em um arquivo de universal data link (udl); no entanto, você deve evitar fazer isso. Os arquivos UDL não são criptografados e expõem as informações da cadeia de conexão em texto não criptografado. Como um arquivo UDL é um recurso externo com base em arquivo para o seu aplicativo, ele não poderá ser protegido usando o .NET Framework. Não há suporte para arquivos UDL para **SqlClient**.  
  
### <a name="using-datadirectory-to-connect-to-accessjet"></a>Usando DataDirectory para conectar-se ao Access/Jet  
 `DataDirectory` não é exclusivo para `SqlClient`. Ele também pode ser usado com os provedores de dados <xref:System.Data.OleDb> e <xref:System.Data.Odbc> do .NET. A cadeia de caracteres de exemplo <xref:System.Data.OleDb.OleDbConnection> a seguir demonstra a sintaxe necessária para se conectar ao Northwind.mdb localizado na pasta app_data do aplicativo. O banco de dados do sistema (System.mdw) também é armazenado nesse local.  
  
```csharp  
"Provider=Microsoft.Jet.OLEDB.4.0;  
Data Source=|DataDirectory|\Northwind.mdb;  
Jet OLEDB:System Database=|DataDirectory|\System.mdw;"  
```  
  
> [!IMPORTANT]
> Especificar o local do banco de dados do sistema na cadeia de conexão não será necessário se o banco de dados Access/Jet for inseguro. A segurança é desativada por padrão, com todos os usuários que se conectam como o usuário Admin interno com uma senha em branco. Mesmo quando a segurança em nível de usuário estiver implementada corretamente, um banco de dados Jet permanecerá vulnerável ao ataque. Portanto, armazenar informações confidenciais em um banco de dados Access/Jet não é recomendado devido à fraqueza inerente de seu esquema de segurança baseado em arquivo.  
  
### <a name="connecting-to-excel"></a>Conectando ao Excel  
 O provedor Microsoft Jet é usado para conectar-se a uma pasta de trabalho do Excel. Na cadeia de conexão a seguir, a palavra-chave de `Extended Properties` define as propriedades que são específicas do Excel. "HDR = Yes;" indica que a primeira linha contém nomes de coluna, não dados e "IMEX = 1;" instrui o driver a sempre ler colunas de dados "intermistas" como texto.  
  
```csharp
Provider=Microsoft.Jet.OLEDB.4.0;Data Source=D:\MyExcel.xls;Extended Properties=""Excel 8.0;HDR=Yes;IMEX=1""  
```  
  
 Observe que o caractere de aspas duplas necessário para as `Extended Properties` também deve estar entre aspas duplas.  
  
### <a name="data-shape-provider-connection-string-syntax"></a>Sintaxe da cadeia de conexão do provedor de forma de dados  
 Use as palavras-chave `Provider` e `Data Provider` ao usar o provedor Microsoft Data Shape. O exemplo a seguir usa o provedor Shape para se conectar a uma instância local do SQL Server.  
  
```csharp  
"Provider=MSDataShape;Data Provider=SQLOLEDB;Data Source=(local);Initial Catalog=pubs;Integrated Security=SSPI;"
```  
  
## <a name="odbc-connection-strings"></a>Cadeias de conexão do Odbc  
 A propriedade <xref:System.Data.Odbc.OdbcConnection.ConnectionString%2A> de um <xref:System.Data.Odbc.OdbcConnection> permite obter ou definir uma cadeia de conexão para uma fonte de dados do OLE DB. As cadeias de conexão de Odbc também têm suporte pelo <xref:System.Data.Odbc.OdbcConnectionStringBuilder>.  
  
 A cadeia de conexão a seguir usa o Microsoft Text Driver.  
  
```csharp  
Driver={Microsoft Text Driver (*.txt; *.csv)};DBQ=d:\bin  
```  
  
### <a name="using-datadirectory-to-connect-to-visual-foxpro"></a>Usando DataDirectory para conectar-se ao Visual FoxPro  
 O exemplo a seguir da cadeia de conexão <xref:System.Data.Odbc.OdbcConnection> demonstra o uso do `DataDirectory` para se conectar a um arquivo do Microsoft Visual FoxPro.  
  
```csharp  
"Driver={Microsoft Visual FoxPro Driver};  
SourceDB=|DataDirectory|\MyData.DBC;SourceType=DBC;"  
```  
  
## <a name="oracle-connection-strings"></a>Cadeias de conexão do Oracle  
 A propriedade <xref:System.Data.OracleClient.OracleConnection.ConnectionString%2A> de um <xref:System.Data.OracleClient.OracleConnection> permite obter ou definir uma cadeia de conexão para uma fonte de dados do OLE DB. As cadeias de conexão de Oracle também têm suporte pelo <xref:System.Data.OracleClient.OracleConnectionStringBuilder>.  
  
```csharp
Data Source=Oracle9i;User ID=*****;Password=*****;  
```  
  
 Para obter mais informações sobre a sintaxe da cadeia de conexão ODBC, consulte <xref:System.Data.OracleClient.OracleConnection.ConnectionString%2A>.  
  
## <a name="see-also"></a>Veja também

- [Cadeias de conexão](connection-strings.md)
- [Conectando a uma Fonte de Dados](connecting-to-a-data-source.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
