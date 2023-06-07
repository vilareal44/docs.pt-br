---
title: Construtores de cadeia de conexão
description: Saiba mais sobre as classes do construtor de cadeias de conexão usadas para provedores diferentes no ADO.NET, todas herdadas de DbConnectionStringBuilder.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8434b608-c4d3-43d3-8ae3-6d8c6b726759
ms.openlocfilehash: e493140b4cf5a939e8ae8f42b617fb739ed09dec
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287058"
---
# <a name="connection-string-builders"></a>Construtores de cadeia de conexão
Em versões anteriores do ADO.NET, a verificação de tempo de compilação de cadeias de conexão com valores de cadeia de caracteres concatenados não ocorreu, de modo que, em tempo de execução, uma palavra-chave incorreta gerou um <xref:System.ArgumentException> . Cada um dos provedores de dados .NET Framework com suporte para uma sintaxe diferente para palavras-chave da cadeia de conexão, o que tornaria a construção de cadeias de conexão válidas é difícil se feito manualmente. Para resolver esse problema, o ADO.NET 2,0 introduziu novos construtores de cadeia de conexão para cada provedor de dados de .NET Framework. Cada provedor de dados inclui uma classe de construtor de cadeia de conexão fortemente tipada que herda de <xref:System.Data.Common.DbConnectionStringBuilder>. A tabela a seguir lista os provedores de dados .NET Framework e suas classes de construtor de cadeia de conexão associadas.  
  
|Provedor|Classe ConnectionStringBuilder|  
|--------------|-----------------------------------|  
|<xref:System.Data.SqlClient>|<xref:System.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=nameWithType>|  
|<xref:System.Data.OleDb>|<xref:System.Data.OleDb.OleDbConnectionStringBuilder?displayProperty=nameWithType>|  
|<xref:System.Data.Odbc>|<xref:System.Data.Odbc.OdbcConnectionStringBuilder?displayProperty=nameWithType>|  
|<xref:System.Data.OracleClient>|<xref:System.Data.OracleClient.OracleConnectionStringBuilder?displayProperty=nameWithType>|  
  
## <a name="connection-string-injection-attacks"></a>Ataques de injeção de cadeias de conexão  
 Um ataque de injeção de cadeia de conexão pode ocorrer quando a concatenação de cadeias dinâmicas é usada para criar cadeias de conexão com base na entrada do usuário. Se a cadeia de caracteres não for validada e o texto mal-intencionado ou os caracteres não forem escapados, um invasor poderá potencialmente acessar dados confidenciais ou outros recursos no servidor. Por exemplo, um invasor pode montar um ataque fornecendo um ponto e vírgula e acrescentando outro valor. A cadeia de conexão é analisada usando o algoritmo "o último vence", e a entrada hostil é substituída por um valor legítimo.  
  
 As classes de construtores de cadeias de conexão são criadas para eliminar hipóteses e proteger contra erros de sintaxe e vulnerabilidades à segurança. Elas fornecem métodos e propriedades que correspondem a pares chave-valor conhecidos e permitidos por cada provedor de dados. Cada classe mantém uma coleção fixa de sinônimos e pode converter um sinônimo no nome da chave conhecida correspondente. As verificações são executadas para pares chave-valor válidos e um par inválido gera uma exceção. Além disso, os valores injetados são tratados de maneira segura.  
  
 O exemplo a seguir demonstra como <xref:System.Data.SqlClient.SqlConnectionStringBuilder> trata um valor adicional inserido para a configuração `Initial Catalog`.  
  
```vb  
Dim builder As New System.Data.SqlClient.SqlConnectionStringBuilder  
builder("Data Source") = "(local)"  
builder("Integrated Security") = True  
builder("Initial Catalog") = "AdventureWorks;NewValue=Bad"  
Console.WriteLine(builder.ConnectionString)  
```  
  
```csharp  
System.Data.SqlClient.SqlConnectionStringBuilder builder =  
  new System.Data.SqlClient.SqlConnectionStringBuilder();  
builder["Data Source"] = "(local)";  
builder["integrated Security"] = true;  
builder["Initial Catalog"] = "AdventureWorks;NewValue=Bad";  
Console.WriteLine(builder.ConnectionString);  
```  
  
 A saída mostra que <xref:System.Data.SqlClient.SqlConnectionStringBuilder> tratou esse valor corretamente colocando em uma sequência de escape, entre aspas duplas, o valor adicional, em vez de acrescentá-lo à cadeia de conexão como um novo par chave-valor.  
  
```output  
data source=(local);Integrated Security=True;  
initial catalog="AdventureWorks;NewValue=Bad"  
```  
  
## <a name="building-connection-strings-from-configuration-files"></a>Construindo cadeias de conexão a partir de arquivos de configuração  
 Se determinados elementos de uma cadeia de conexão forem conhecidos antecipadamente, eles poderão ser armazenados em um arquivo de configuração e recuperados em tempo de execução para construir uma cadeia de conexão completa. Por exemplo, o nome do banco de dados pode ser conhecido com antecedência, mas não o nome do servidor. Ou, talvez, você deseje que um usuário forneça um nome e uma senha em tempo de execução, sem que possa injetar outros valores na cadeia de conexão.  
  
 Um dos construtores sobrecarregados de um construtor de cadeias de conexão obtém um <xref:System.String> como argumento, o que permite a você fornecer uma cadeia de conexão parcial que depois poderá ser concluída pela entrada do usuário. A cadeia de conexão parcial pode ser armazenada em um arquivo de configuração e recuperada em tempo de execução.  
  
> [!NOTE]
> O namespace <xref:System.Configuration> permite acesso programático aos arquivos de configuração que usam <xref:System.Web.Configuration.WebConfigurationManager> para aplicativos Web e <xref:System.Configuration.ConfigurationManager> para aplicativos do Windows. Para obter mais informações sobre como trabalhar com cadeias de conexão e arquivos de configuração, consulte [cadeias de conexão e arquivos de configuração](connection-strings-and-configuration-files.md).  
  
### <a name="example"></a>Exemplo  
 Este exemplo demonstra como recuperar uma cadeia de conexão parcial de um arquivo de configuração e concluí-la definindo as propriedades <xref:System.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A>, <xref:System.Data.SqlClient.SqlConnectionStringBuilder.UserID%2A> e <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> do <xref:System.Data.SqlClient.SqlConnectionStringBuilder>. O arquivo de configuração é definido como a seguir.  
  
```xml  
<connectionStrings>  
  <clear/>  
  <add name="partialConnectString"
    connectionString="Initial Catalog=Northwind;"  
    providerName="System.Data.SqlClient" />  
</connectionStrings>  
```  
  
> [!NOTE]
> Você deve definir uma referência à `System.Configuration.dll` em seu projeto para que o código seja executado.  
  
 [!code-csharp[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlConnectionStringBuilder.UserNamePwd/CS/source.cs#1)]
 [!code-vb[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlConnectionStringBuilder.UserNamePwd/VB/source.vb#1)]  
  
## <a name="see-also"></a>Veja também

- [Cadeias de conexão](connection-strings.md)
- [Privacidade e segurança de dados](privacy-and-data-security.md)
- [Visão geral do ADO.NET](ado-net-overview.md)
