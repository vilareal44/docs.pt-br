---
title: Dados de data e hora
description: Saiba mais sobre os tipos de dados para manipular informações de data e hora no Provedor de Dados de .NET Framework para SQL Server.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.openlocfilehash: 9345e995dcb1179e7d0a86f62737f9fda5889f42
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286488"
---
# <a name="date-and-time-data"></a>Dados de data e hora
O SQL Server 2008 apresenta novos tipos de dados para o tratamento de informações de data e hora. Os novos tipos de dados incluem tipos separados para data e hora e tipos de dados expandidos com maior reconhecimento de intervalo, precisão e fuso horário. A partir do .NET Framework versão 3.5 Service Pack (SP) 1, o Provedor de Dados .NET Framework para SQL Server (<xref:System.Data.SqlClient>) oferece suporte completo para todos os novos recursos do Mecanismo de Banco de Dados do SQL Server 2008. Você deve instalar o .NET Framework 3.5 SP1 (ou posterior) para usar esses novos recursos com o SqlClient.  
  
 As versões do SQL Server anteriores ao SQL Server 2008 tinham apenas dois tipos de dados para trabalhar com valores de data e temporal: `datetime` e `smalldatetime`. Ambos os tipos de dados contêm o valor de data e um valor temporal, o que dificulta o trabalho apenas com valores de data ou temporal. Além disso, esses tipos de dados apenas são compatíveis com datas que ocorrem após a introdução do calendário gregoriano, na Inglaterra, em 1753. Outra limitação é que esses tipos de dados mais antigos não têm reconhecimento de fuso horário, o que dificulta o trabalho usando dados provenientes de vários fusos horários.  
  
 A documentação completa para tipos de dados do SQL Server está disponível nos Manuais Online do SQL Server. A tabela a seguir lista os tópicos básicos específicos de versão para dados de data e hora.  
  
 **Documentação do SQL Server**  
  
1. [Usando dados de data e hora](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/ms180878(v=sql.100))  
  
## <a name="datetime-data-types-introduced-in-sql-server-2008"></a>Tipos de dados de data/hora introduzidos no SQL Server 2008  
 A tabela a seguir descreve os novos tipos de dados de data e hora.  
  
|Tipo de dados do SQL Server|Description|  
|--------------------------|-----------------|  
|`date`|O tipo de dados `date` tem um intervalo de 1º de janeiro de 01 a 31 de dezembro de 9999 com precisão de um dia. O valor padrão é 1º de janeiro de 1900. O tamanho do armazenamento é 3 bytes.|  
|`time`|O tipo de dados `time` armazena somente valores temporais, com base em um relógio de 24 horas. O tipo de dados `time` tem um intervalo de 00:00:00.0000000 a 23:59:59,9999999 com precisão de 100 nanossegundos. O valor padrão é 00:00:00.0000000 (meia-noite). O tipo de dados `time` dá suporte à precisão de fração de segundo definida pelo usuário. O tamanho do armazenamento varia de 3 a 6 bytes, com base na precisão especificada.|  
|`datetime2`|O tipo de dados `datetime2` combina o intervalo e a precisão dos tipos de dados `date` e `time` em um tipo de dados.<br /><br /> Os valores padrão e os formatos literais de cadeia de caracteres são os mesmos que os definidos nos tipos de dados `date` e `time`.|  
|`datetimeoffset`|O tipo de dados `datetimeoffset` tem todos os recursos de `datetime2` com um deslocamento de fuso horário adicional. O deslocamento de fuso horário é representado como [+&#124;-] HH:MM. HH equivale a dois dígitos, variando de 00 a 14, que representam o número de horas no deslocamento de fuso horário. MM equivale a dois dígitos, variando de 00 a 59, que representam o número de minutos adicionais no deslocamento de fuso horário. Os formatos de hora são compatíveis com 100 nanossegundos. O sinal + ou - obrigatório indica se o deslocamento de fuso horário é adicionado ou subtraído do UTC (Tempo Universal Coordenado) ou do GMT (Hora de Greenwich) para obter a hora local.|  
  
> [!NOTE]
> Para obter mais informações sobre o uso da palavra-chave `Type System Version`, confira <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>.  
  
## <a name="date-format-and-date-order"></a>Formato de data e ordem de data  
 O modo como o SQL Server analisa os valores de data e temporal não depende apenas da versão do sistema do tipo e da versão do servidor, mas também das configurações de formato e idioma padrão do servidor. Uma cadeia de caracteres de data que funciona para os formatos de data de um idioma poderá ser irreconhecível se a consulta for executada por uma conexão que usa uma configuração diferente de idioma e formato de data.  
  
 A instrução SET LANGUAGE do Transact-SQL define implicitamente o DATEFORMAT que determina a ordem das partes da data. Você pode usar a instrução SET DATEFORMAT do Transact-SQL em uma conexão para eliminar a ambiguidade dos valores de data ordenando as partes da data na ordem MDA, DMA, AMD, ADM, MAD ou DAM.  
  
 Se você não especificar nenhum DATEFORMAT para a conexão, o SQL Server usará o idioma padrão associado à conexão. Por exemplo, uma cadeia de caracteres de data de '01/02/03' seria interpretada como MDA (2 de janeiro de 2003) em um servidor com uma configuração de idioma de inglês dos Estados Unidos e como DMA (1º de fevereiro de 2003) em um servidor com uma configuração de idioma de inglês britânico. O ano é determinado usando a regra de ano limiar do SQL Server, que define a data limiar para atribuir o valor do século. Para obter mais informações, consulte [opção de corte de ano de dois dígitos](/sql/database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option).  
  
> [!NOTE]
> Não há suporte para o formato de data ADM ao converter de um formato de cadeia de caracteres para `date`, `time`, `datetime2` ou `datetimeoffset`.  
  
 Para obter mais informações sobre como SQL Server interpreta dados de data e hora, consulte [usando dados de data e hora](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/ms180878(v=sql.100)).  
  
## <a name="datetime-data-types-and-parameters"></a>Parâmetros e tipos de dados de data/hora  
 As enumerações a seguir foram adicionadas ao <xref:System.Data.SqlDbType> para dar suporte aos novos tipos de dados de data e hora.  
  
- `SqlDbType.Date`  
  
- `SqlDbType.Time`  
  
- `SqlDbType.DateTime2`  
  
- `SqlDbType.DateTimeOffSet`  

Você pode especificar o tipo de dados de um <xref:System.Data.SqlClient.SqlParameter> usando uma das enumerações <xref:System.Data.SqlDbType> anteriores.

> [!NOTE]
> Não é possível definir a propriedade `DbType` de um `SqlParameter` como `SqlDbType.Date`.

 Você também pode especificar genericamente o tipo de um <xref:System.Data.SqlClient.SqlParameter> configurando a propriedade <xref:System.Data.SqlClient.SqlParameter.DbType%2A> de um objeto `SqlParameter` para um determinado valor de enumeração <xref:System.Data.DbType>. Os seguintes valores de enumeração foram adicionados ao <xref:System.Data.DbType> para dar suporte aos tipos de dados `datetime2` e `datetimeoffset`:  
  
- DbType.DateTime2  
  
- DbType.DateTimeOffset  
  
 Essas novas enumerações suplementam as enumerações `Date`, `Time` e `DateTime`, que existiam em versões anteriores do .NET Framework.  
  
 O tipo do provedor de dados do .NET Framework de um objeto de parâmetro é inferido do tipo do .NET Framework do valor do objeto de parâmetro ou do `DbType` do objeto de parâmetro. Nenhum tipo novo de dados <xref:System.Data.SqlTypes> foi introduzido para dar suporte aos novos tipos de dados de data e hora. A tabela a seguir descreve os mapeamentos entre os tipos de dados de data e hora do SQL Server 2008 e os tipos de dados do CLR.  
  
|Tipo de dados do SQL Server|Tipo de .NET Framework|System.Data.SqlDbType|System.Data.DbType|  
|--------------------------|-------------------------|---------------------------|------------------------|  
|date|System.DateTime|Data|Data|  
|time|System.TimeSpan|Hora|Hora|  
|datetime2|System.DateTime|DateTime2|DateTime2|  
|datetimeoffset|System.DateTimeOffset|DateTimeOffset|DateTimeOffset|  
|DATETIME|System.DateTime|Datetime|Datetime|  
|smalldatetime|System.DateTime|Datetime|Datetime|  
  
### <a name="sqlparameter-properties"></a>Propriedades do SqlParameter  
 A tabela a seguir descreve as propriedades `SqlParameter` que são relevantes para os tipos de dados de data e hora.  
  
|Propriedade|Descrição|  
|--------------|-----------------|  
|<xref:System.Data.SqlClient.SqlParameter.IsNullable%2A>|Obtém ou define se um valor é anulável. Quando você envia um valor de parâmetro nulo para o servidor, deve especificar <xref:System.DBNull>, não `null` (`Nothing` no Visual Basic). Para obter mais informações sobre nulos de banco de dados, consulte [lidando com valores nulos](handling-null-values.md).|  
|<xref:System.Data.SqlClient.SqlParameter.Precision%2A>|Obtém ou define o número máximo de dígitos usados para representar o valor. Essa configuração é ignorada para tipos de dados de data e hora.|  
|<xref:System.Data.SqlClient.SqlParameter.Scale%2A>|Obtém ou define o número de casas decimais para as quais a parte de hora do valor é resolvida para `Time`, `DateTime2` e `DateTimeOffset`. O valor padrão é 0, o que significa que a escala real é inferida do valor e enviada ao servidor.|  
|<xref:System.Data.SqlClient.SqlParameter.Size%2A>|Ignorado para tipos de dados de data e hora.|  
|<xref:System.Data.SqlClient.SqlParameter.Value%2A>|Obtém ou define o valor do parâmetro.|  
|<xref:System.Data.SqlClient.SqlParameter.SqlValue%2A>|Obtém ou define o valor do parâmetro.|  
  
> [!NOTE]
> Valores temporais menores que zero ou maiores ou iguais a 24 horas lançarão um <xref:System.ArgumentException>.  
  
### <a name="creating-parameters"></a>Criar parâmetros  
 Você pode criar um objeto <xref:System.Data.SqlClient.SqlParameter> usando o construtor ou adicionando-o a uma coleção de <xref:System.Data.SqlClient.SqlCommand><xref:System.Data.SqlClient.SqlCommand.Parameters%2A> chamando o método `Add` do <xref:System.Data.SqlClient.SqlParameterCollection>. O método `Add` usará como entrada argumentos do construtor ou um objeto de parâmetro existente.  
  
 As próximas seções neste tópico fornecem exemplos de como especificar parâmetros de data e hora. Para obter exemplos adicionais de como trabalhar com parâmetros, consulte [configurando parâmetros e tipos de dados de parâmetro](../configuring-parameters-and-parameter-data-types.md) e [parâmetros de DataAdapter](../dataadapter-parameters.md).  
  
### <a name="date-example"></a>Exemplo de data  
 O fragmento de código a seguir demonstra como especificar um parâmetro `date`.  
  
```csharp  
SqlParameter parameter = new SqlParameter();  
parameter.ParameterName = "@Date";  
parameter.SqlDbType = SqlDbType.Date;  
parameter.Value = "2007/12/1";  
```  
  
```vb  
Dim parameter As New SqlParameter()  
parameter.ParameterName = "@Date"  
parameter.SqlDbType = SqlDbType.Date  
parameter.Value = "2007/12/1"  
```  
  
### <a name="time-example"></a>Exemplo de hora  
 O fragmento de código a seguir demonstra como especificar um parâmetro `time`.  
  
```csharp  
SqlParameter parameter = new SqlParameter();  
parameter.ParameterName = "@time";  
parameter.SqlDbType = SqlDbType.Time;  
parameter.Value = DateTime.Parse("23:59:59").TimeOfDay;  
```  
  
```vb  
Dim parameter As New SqlParameter()  
parameter.ParameterName = "@Time"  
parameter.SqlDbType = SqlDbType.Time  
parameter.Value = DateTime.Parse("23:59:59").TimeOfDay;  
```  
  
### <a name="datetime2-example"></a>Exemplo de Datetime2  
 O fragmento de código a seguir demonstra como especificar um parâmetro `datetime2` com as partes de data e hora.  
  
```csharp  
SqlParameter parameter = new SqlParameter();  
parameter.ParameterName = "@Datetime2";  
parameter.SqlDbType = SqlDbType.DateTime2;  
parameter.Value = DateTime.Parse("1666-09-02 1:00:00");  
```  
  
```vb  
Dim parameter As New SqlParameter()  
parameter.ParameterName = "@Datetime2"  
parameter.SqlDbType = SqlDbType.DateTime2  
parameter.Value = DateTime.Parse("1666-09-02 1:00:00");  
```  
  
### <a name="datetimeoffset-example"></a>Exemplo de DateTimeOffSet  
 O fragmento de código a seguir demonstra como especificar um parâmetro `DateTimeOffSet` com uma data, uma hora e um deslocamento de fuso horário de 0.  
  
```csharp  
SqlParameter parameter = new SqlParameter();  
parameter.ParameterName = "@DateTimeOffSet";  
parameter.SqlDbType = SqlDbType.DateTimeOffSet;  
parameter.Value = DateTimeOffset.Parse("1666-09-02 1:00:00+0");  
```  
  
```vb  
Dim parameter As New SqlParameter()  
parameter.ParameterName = "@DateTimeOffSet"  
parameter.SqlDbType = SqlDbType.DateTimeOffSet  
parameter.Value = DateTimeOffset.Parse("1666-09-02 1:00:00+0");  
```  
  
### <a name="addwithvalue"></a>AddWithValue  
 Você também pode fornecer parâmetros usando o método `AddWithValue` de um <xref:System.Data.SqlClient.SqlCommand>, conforme mostrado no fragmento de código a seguir. No entanto, o método `AddWithValue` não permite que você especifique o <xref:System.Data.SqlClient.SqlParameter.DbType%2A> ou o <xref:System.Data.SqlClient.SqlParameter.SqlDbType%2A> para o parâmetro.  
  
```csharp  
command.Parameters.AddWithValue(
    "@date", DateTimeOffset.Parse("16660902"));  
```  
  
```vb  
command.Parameters.AddWithValue( _  
    "@date", DateTimeOffset.Parse("16660902"))  
```  
  
 O parâmetro `@date` pode mapear para um tipo de dados `date`, `datetime` ou `datetime2` no servidor. Ao trabalhar com os novos tipos de dados `datetime`, você deve definir explicitamente a propriedade <xref:System.Data.SqlDbType> do parâmetro como o tipo de dados da instância. Usar <xref:System.Data.SqlDbType.Variant> ou fornecer valores de parâmetro implicitamente pode causar problemas com a compatibilidade com versões anteriores com os tipos de dados `datetime` e `smalldatetime`.  
  
 A seguinte tabela mostra quais `SqlDbTypes` são inferidos de quais tipos CLR:  
  
|Tipo CLR|SqlDbType inferido|  
|--------------|------------------------|  
|Datetime|SqlDbType.DateTime|  
|TimeSpan|SqlDbType.Time|  
|DateTimeOffset|SqlDbType.DateTimeOffset|  
  
## <a name="retrieving-date-and-time-data"></a>Recuperando dados de data e hora  
 A tabela a seguir descreve os métodos que são usados para recuperar os valores de data e hora do SQL Server 2008.  
  
|Método SqlClient|Description|  
|----------------------|-----------------|  
|<xref:System.Data.SqlClient.SqlDataReader.GetDateTime%2A>|Recupera o valor da coluna especificada como uma estrutura <xref:System.DateTime>.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetDateTimeOffset%2A>|Recupera o valor da coluna especificada como uma estrutura <xref:System.DateTimeOffset>.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificFieldType%2A>|Retorna o tipo que é o tipo específico do provedor subjacente para o campo. Retorna os mesmos tipos que `GetFieldType` para novos tipos de data e hora.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValue%2A>|Recupera o valor da coluna especificada. Retorna os mesmos tipos que `GetValue` para novos tipos de data e hora.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValues%2A>|Recupera os valores na matriz especificada.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetSqlString%2A>|Recupera o valor da coluna como um <xref:System.Data.SqlTypes.SqlString>. Um <xref:System.InvalidCastException> ocorrerá se os dados não puderem ser expressos como um `SqlString`.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetSqlValue%2A>|Recupera os dados da coluna como seu `SqlDbType` padrão. Retorna os mesmos tipos que `GetValue` para novos tipos de data e hora.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetSqlValues%2A>|Recupera os valores na matriz especificada.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetString%2A>|Recupera o valor da coluna como uma cadeia de caracteres se a versão do sistema do tipo está definida como SQL Server 2005. Um <xref:System.InvalidCastException> ocorrerá se os dados não puderem ser expressos como uma cadeia de caracteres.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetTimeSpan%2A>|Recupera o valor da coluna especificada como uma estrutura <xref:System.TimeSpan>.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetValue%2A>|Recupera o valor da coluna especificada como seu tipo CLR subjacente.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetValues%2A>|Recupera valores de coluna em uma matriz.|  
|<xref:System.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>|Retorna um <xref:System.Data.DataTable> que descreve os metadados do conjunto de resultados.|  
  
> [!NOTE]
> Os novos `SqlDbTypes` de data e hora não são compatíveis com o código que está executando em processo no SQL Server. Uma exceção será gerada se um desses tipos for passado para o servidor.  
  
## <a name="specifying-date-and-time-values-as-literals"></a>Especificando valores de data e hora como literais  
 Você pode especificar tipos de dados de data e hora usando uma variedade de formatos de cadeia de caracteres literais diferentes, que, em seguida, o SQL Server avalia no tempo de execução, convertendo-os em estruturas de data/hora internas. O SQL Server reconhece dados de data e hora que estão entre aspas simples ('). Os seguintes exemplos demonstram alguns formatos:  
  
- Formatos de datas alfabéticos, como `'October 15, 2006'`.  
  
- Formatos de datas numéricos, como `'10/15/2006'`.  
  
- Formatos de cadeia de caracteres não separados, como `'20061015'`, que serão interpretados como 15 de outubro de 2006 se você estiver usando o formato de data padrão ISO.  
  
> [!NOTE]
> Você pode encontrar a documentação completa para todos os formatos de cadeia de caracteres literais e outros recursos dos tipos de dados de data e hora nos Manuais Online do SQL Server.  
  
 Valores temporais menores que zero ou maiores ou iguais a 24 horas lançarão um <xref:System.ArgumentException>.  
  
## <a name="resources-in-sql-server-books-online"></a>Recursos nos Manuais Online do SQL Server  
 Para obter mais informações sobre como trabalhar com valores de data e hora no SQL Server, consulte os recursos a seguir em Manuais Online do SQL Server.  
  
|Tópico|Descrição|  
|-----------|-----------------|  
|[tipos de dados e funções de data e hora (Transact-SQL)](/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql)|Fornece uma visão geral de todos os tipos de dados e as funções de data e hora do Transact-SQL.|  
|[Usando dados de data e hora](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/ms180878(v=sql.100))|Fornece informações sobre as funções e os tipos de dados de data e hora e exemplos de como usá-los.|  
|[Tipos de dados (Transact-SQL)](/sql/t-sql/data-types/data-types-transact-sql)|Descreve os tipos de dados do sistema no SQL Server.|  
  
## <a name="see-also"></a>Veja também

- [Mapeamentos de tipo de dados do SQL Server](../sql-server-data-type-mappings.md)
- [Configurando parâmetros e tipos de dados de parâmetro](../configuring-parameters-and-parameter-data-types.md)
- [Tipos de dados SQL Server e ADO.NET](sql-server-data-types.md)
- [Visão geral do ADO.NET](../ado-net-overview.md)
