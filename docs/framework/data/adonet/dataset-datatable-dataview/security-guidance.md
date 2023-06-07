---
title: Diretrizes de segurança do conjunto de tabela e DataTable
ms.date: 07/14/2020
dev_langs:
- csharp
ms.openlocfilehash: f0fa43c467cc7866e69115acb5f807e6487fda7a
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608522"
---
# <a name="dataset-and-datatable-security-guidance"></a>Diretrizes de segurança do conjunto de tabela e DataTable

Este artigo aplica-se a:

* .NET Framework (todas as versões)
* .NET Core e posterior
* .NET 5,0 e posterior

Os tipos [DataSet](/dotnet/api/system.data.dataset) e [DataTable](/dotnet/api/system.data.datatable) são componentes .net herdados que permitem a representação de conjuntos de dados como objetos gerenciados. Esses componentes foram introduzidos no .NET 1,0 como parte da infraestrutura original do [ADO.net](/dotnet/framework/data/adonet/dataset-datatable-dataview/). Sua meta era fornecer uma exibição gerenciada sobre um conjunto de dados relacionais, abstraindo se a fonte subjacente dos dados era XML, SQL ou outra tecnologia.

Para obter mais informações sobre ADO.NET, incluindo paradigmas de exibição de dados mais modernos, consulte [a documentação do ADO.net](/dotnet/framework/data/adonet/).

## <a name="default-restrictions-when-deserializing-a-dataset-or-datatable-from-xml"></a>Restrições padrão ao desserializar um DataSet ou DataTable de XML

Em todas as versões com suporte do .NET Framework, .NET Core e .NET, `DataSet` e `DataTable` Coloque as seguintes restrições sobre quais tipos de objetos podem estar presentes nos dados desserializados. Por padrão, essa lista é restrita a:

* Primitivas e equivalentes primitivos:,,,,,, `bool` `char` `sbyte` `byte` `short` `ushort` `int` , `uint` ,,,,,,,,,,,, `long` `ulong` `float` `double` `decimal` `DateTime` `DateTimeOffset` `TimeSpan` `string` `Guid` `SqlBinary` `SqlBoolean` , `SqlByte` , `SqlBytes` ,,,, `SqlChars` ,,,,,, `SqlDateTime` `SqlDecimal` `SqlDouble` `SqlGuid` `SqlInt16` `SqlInt32` `SqlInt64` `SqlMoney` `SqlSingle` e `SqlString` .
* Não primitivos comumente usados: `Type` , `Uri` e `BigInteger` .
* Tipos de _sistema. Drawing_ usados com frequência: `Color` ,,,, `Point` `PointF` `Rectangle` `RectangleF` , `Size` e `SizeF` .
* `Enum` digita.
* Matrizes e listas dos tipos acima.

Se os dados XML de entrada contiverem um objeto cujo tipo não está nesta lista:

* Uma exceção é gerada.
* A operação de desserialização falha.

Ao carregar XML em uma `DataSet` instância ou existente `DataTable` , as definições de coluna existentes também são levadas em conta. Se a tabela já contiver uma definição de coluna de um tipo personalizado, esse tipo será adicionado temporariamente à lista de permissões para a duração da operação de desserialização de XML.

```cs
XmlReader xmlReader = GetXmlReader();

// Assume the XML blob contains data for type MyCustomClass.
// The following call to ReadXml fails because MyCustomClass isn't in the allowed types list.

DataTable table = new DataTable("MyDataTable");
table.ReadXml(xmlReader);

// However, the following call to ReadXml succeeds, since the DataTable instance
// already defines a column of type MyCustomClass.

DataTable table = new DataTable("MyDataTable");
table.Columns.Add("MyColumn", typeof(MyCustomClass));
table.ReadXml(xmlReader); // this call will succeed
```

As restrições de tipo de objeto também se aplicam ao usar `XmlSerializer` para desserializar uma instância do `DataSet` ou `DataTable` . No entanto, eles não podem se aplicar ao usar `BinaryFormatter` o para desserializar uma instância do `DataSet` ou `DataTable` .

As restrições de tipo de objeto não se aplicam ao usar `DataAdapter.Fill` o, como quando uma `DataTable` instância é populada diretamente de um banco de dados sem usar as APIs de desserialização de XML.

### <a name="extend-the-list-of-allowed-types"></a>Estender a lista de tipos permitidos

Um aplicativo pode estender a lista de tipos permitidos para incluir tipos personalizados, além dos tipos internos listados acima. Se estiver estendendo a lista de tipos permitidos, a alteração afetará _todas as_ `DataSet` `DataTable` instâncias e no aplicativo. Os tipos não podem ser removidos da lista de tipos permitidos internos.

#### <a name="extend-through-configuration-net-framework-40---48"></a>Estender por meio da configuração (.NET Framework 4,0-4,8)

_App.config_ pode ser usado para estender a lista de tipos permitidos. Para estender a lista de tipos permitidos:

* Use o `<configSections>` elemento para adicionar uma referência à seção de configuração _System. Data_ .
* Use `<system.data.dataset.serialization>` / `<allowedTypes>` para especificar tipos adicionais.

Cada `<add>` elemento deve especificar apenas um tipo usando seu nome de tipo qualificado de assembly. Para adicionar tipos adicionais à lista de tipos permitidos, use vários `<add>` elementos.

O exemplo a seguir mostra a extensão da lista de tipos permitidos adicionando o tipo personalizado `Fabrikam.CustomType` .

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <sectionGroup name="system.data.dataset.serialization" type="System.Data.SerializationSettingsSectionGroup, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">
      <section name="allowedTypes" type="System.Data.AllowedTypesSectionHandler, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
    </sectionGroup>
  </configSections>
  <system.data.dataset.serialization>
    <allowedTypes>
      <!-- <add type="assembly qualified type name" /> -->
      <add type="Fabrikam.CustomType, Fabrikam, Version=1.0.0.0, Culture=neutral, PublicKeyToken=2b3831f2f2b744f7" />
      <!-- additional <add /> elements as needed -->
    </allowedTypes>
  </system.data.dataset.serialization>
</configuration>
```

Para recuperar o nome qualificado do assembly de um tipo, use a propriedade [Type. AssemblyQualifiedName](/dotnet/api/system.type.assemblyqualifiedname) , conforme demonstrado no código a seguir.

```cs
string assemblyQualifiedName = typeof(Fabrikam.CustomType).AssemblyQualifiedName;
```

<a name="etc"></a>

#### <a name="extend-through-configuration-net-framework-20---35"></a>Estender por meio da configuração (.NET Framework 2,0-3,5)

Se seu aplicativo tiver como destino .NET Framework 2,0 ou 3,5, você ainda poderá usar o mecanismo de _App.config_ acima para estender a lista de tipos permitidos. No entanto, seu `<configSections>` elemento parecerá um pouco diferente, conforme mostrado no código a seguir:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <!-- The below <sectionGroup> and <section> are specific to .NET Framework 2.0 and 3.5. -->
    <sectionGroup name="system.data.dataset.serialization" type="System.Data.SerializationSettingsSectionGroup, System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">
      <section name="allowedTypes" type="System.Data.AllowedTypesSectionHandler, System.Data, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
    </sectionGroup>
  </configSections>
  <system.data.dataset.serialization>
    <allowedTypes>
      <!-- <add /> elements, as demonstrated in the .NET Framework 4.0 - 4.8 sample code above. -->
    </allowedTypes>
  </system.data.dataset.serialization>
</configuration>
```

#### <a name="extend-programmatically-net-framework-net-core-net-50"></a>Estender programaticamente (.NET Framework, .NET Core, .NET 5.0 +)

A lista de tipos permitidos também pode ser estendida programaticamente usando [AppDomain. SetData](/dotnet/api/system.appdomain.setdata) com a chave conhecida _System. Data. DataSetDefaultAllowedTypes_, conforme mostrado no código a seguir.

```cs
Type[] extraAllowedTypes = new Type[]
{
    typeof(Fabrikam.CustomType),
    typeof(Contoso.AdditionalCustomType)
};

AppDomain.CurrentDomain.SetData("System.Data.DataSetDefaultAllowedTypes", extraAllowedTypes);
```

Se estiver usando o mecanismo de extensão, o valor associado à chave _System. Data. DataSetDefaultAllowedTypes_ deverá ser do tipo `Type[]` .

Em .NET Framework, a lista de tipos permitidos pode ser estendida com _App.config_ e `AppDomain.SetData` . Nesse caso, `DataSet` e `DataTable` permitirá que um objeto seja desserializado como parte dos dados se seu tipo estiver presente em qualquer uma das listas.

### <a name="run-an-app-in-audit-mode-net-framework"></a>Executar um aplicativo no modo de auditoria (.NET Framework)

No .NET Framework, `DataSet` e `DataTable` fornecem um recurso de modo de auditoria. Quando o modo de auditoria está habilitado `DataSet` e `DataTable` compara os tipos de objetos de entrada com a lista de tipos permitidos. No entanto, se um objeto cujo tipo não é permitido for visto, uma exceção **não** será lançada. Em vez disso, `DataSet` e `DataTable` notifique as instâncias anexadas `TraceListener` que um tipo suspeito está presente, permitindo que o `TraceListener` Registre essas informações. Nenhuma exceção é lançada e a operação de desserialização continua.

> [!WARNING]
> A execução de um aplicativo no "modo de auditoria" deve ser apenas uma medida temporária usada para teste. Quando o modo de auditoria está habilitado `DataSet` e `DataTable` não impõe restrições de tipo, que podem introduzir uma brecha de segurança dentro de seu aplicativo. Para obter mais informações, consulte as seções intituladas [removendo todas as restrições de tipo](#ratr) e [segurança em relação à entrada não confiável](#swr).

O modo de auditoria pode ser habilitado por meio do _App.config_:

* Consulte a seção [ampliando a configuração](#etc) deste documento para obter informações sobre o valor adequado a ser colocado para o `<configSections>` elemento.
* Use `<allowedTypes auditOnly="true">` para habilitar o modo de auditoria, conforme mostrado na marcação a seguir.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <!-- See the section of this document titled "Extending through configuration" for the appropriate
         <sectionGroup> and <section> elements to put here, depending on whether you're running .NET
         Framework 2.0 - 3.5 or 4.0 - 4.8. -->
  </configSections>
  <system.data.dataset.serialization>
    <allowedTypes auditOnly="true"> <!-- setting auditOnly="true" enables audit mode -->
      <!-- Optional <add /> elements as needed. -->
    </allowedTypes>
  </system.data.dataset.serialization>
</configuration>
```

Depois que o modo de auditoria estiver habilitado, você poderá usar _App.config_ para conectar seu preferido `TraceListener` ao `DataSet` `TraceSource.` nome da origem de rastreamento interna é _System. Data. DataSet_. O exemplo a seguir demonstra a gravação de eventos de rastreamento no console _e_ em um arquivo de log no disco.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.diagnostics>
    <sources>
      <source name="System.Data.DataSet"
              switchType="System.Diagnostics.SourceSwitch"
              switchValue="Warning">
        <listeners>
          <!-- write to the console -->
          <add name="console"
               type="System.Diagnostics.ConsoleTraceListener" />
          <!-- *and* write to a log file on disk -->
          <add name="filelog"
               type="System.Diagnostics.TextWriterTraceListener"
               initializeData="c:\logs\mylog.txt" />
        </listeners>
      </source>
    </sources>
  </system.diagnostics>
</configuration>
```

Para obter mais informações `TraceSource` sobre `TraceListener` o e o, consulte o documento [como: usar rastreamento e filtros com ouvintes de rastreamento](../../../debug-trace-profile/how-to-use-tracesource-and-filters-with-trace-listeners.md).

> [!NOTE]
> A execução de um aplicativo no modo de auditoria não está disponível no .NET Core ou no .NET 5,0 e posterior.

<a name="ratr"></a>

### <a name="remove-all-type-restrictions"></a>Remover todas as restrições de tipo

Se um aplicativo deve remover todas as restrições de limitação de tipo de `DataSet` e `DataTable` :

* Há várias opções para suprimir as restrições de limitação de tipo.
* As opções disponíveis dependem da estrutura de destino do aplicativo.

> [!WARNING]
> A remoção de todas as restrições de tipo pode introduzir uma brecha de segurança dentro do aplicativo. Ao usar esse mecanismo, verifique se o aplicativo **não** usa `DataSet` ou `DataTable` para ler a entrada não confiável. Para obter mais informações, consulte [CVE-2020-1147](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2020-1147) e a seção a seguir intitulada [segurança em relação à entrada não confiável](#swr).

#### <a name="through-appcontext-configuration-net-framework-46---48-net-core-21-and-later-net-50-and-later"></a>Por meio da configuração do AppContext (.NET Framework 4,6-4,8, .NET Core 2,1 e posterior, .NET 5,0 e posterior)

A `AppContext` opção, `Switch.System.Data.AllowArbitraryDataSetTypeInstantiation` , quando definida como `true` Remove todas as restrições de limitação de tipo de `DataSet` e `DataTable` .

No .NET Framework, essa opção pode ser habilitada via _App.config_, conforme mostrado na seguinte configuração:

```xml
<configuration>
   <runtime>
      <!-- Warning: setting the following switch can introduce a security problem. -->
      <AppContextSwitchOverrides value="Switch.System.Data.AllowArbitraryDataSetTypeInstantiation=true" />
   </runtime>
</configuration>
```

No ASP.NET, o `<AppContextSwitchOverrides>` elemento não está disponível. Em vez disso, a opção pode ser habilitada por meio de _Web.config_, conforme mostrado na seguinte configuração:

```xml
<configuration>
    <appSettings>
        <!-- Warning: setting the following switch can introduce a security problem. -->
        <add key="AppContext.SetSwitch:Switch.System.Data.AllowArbitraryDataSetTypeInstantiation" value="true" />
    </appSettings>
</configuration>
```

Para obter mais informações, consulte o [\<AppContextSwitchOverrides>](../../../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) elemento.

No .NET Core, no .NET 5 e no ASP.NET Core, essa configuração é controlada por _runtimeconfig.jsem_, conforme mostrado no JSON a seguir:

```json
{
  "runtimeOptions": {
    "configProperties": {
      "Switch.System.Data.AllowArbitraryDataSetTypeInstantiation": true
    }
  }
}
```

Para obter mais informações, consulte ["definições de configuração de tempo de execução do .NET Core"](/dotnet/core/run-time-config/).

`AllowArbitraryDataSetTypeInstantiation` também pode ser definido programaticamente via [AppContext. setswitch](/dotnet/api/system.appcontext.setswitch) em vez de usar um arquivo de configuração, conforme mostrado no código a seguir:

```cs
// Warning: setting the following switch can introduce a security problem.
AppContext.SetSwitch("Switch.System.Data.AllowArbitraryDataSetTypeInstantiation", true);
```

 Se você escolher a abordagem programática anterior, a chamada para `AppContext.SetSwitch` deverá ocorrer no início da inicialização dos aplicativos.

#### <a name="through-the-machine-wide-registry-net-framework-20---48"></a>Por meio do registro em todo o computador (.NET Framework 2,0-4,8)

Se `AppContext` não estiver disponível, as verificações de limitação de tipo poderão ser desabilitadas com o registro do Windows:

* Um administrador deve configurar o registro.
* O uso do registro é uma alteração em todo o computador e afetará _todos os_ aplicativos em execução no computador.

| Type  |  Valor |
|---|---|
| **Chave do Registro** | `HKLM\SOFTWARE\Microsoft\.NETFramework\AppContext` |
| **Nome do valor** | `Switch.System.Data.AllowArbitraryDataSetTypeInstantiation` |
| **Tipo de valor** | `REG_SZ` |
| **Dados do valor** | `true` |

Em um sistema operacional de 64 bits, esse valor deve ser adicionado para a chave de 64 bits (mostrada acima) e a chave 32 bits. A chave de 32 bits está localizada em `HKLM\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\AppContext` .

Para obter mais informações sobre como usar o registro para configurar o `AppContext` , consulte ["AppContext for Library consumidores"](/dotnet/api/system.appcontext#appcontext-for-library-consumers).

<a name="swr"></a>

## <a name="safety-with-regard-to-untrusted-input"></a>Segurança em relação à entrada não confiável

Embora `DataSet` e `DataTable` imponham as limitações padrão nos tipos que podem estar presentes durante a desserialização de cargas XML __ `DataSet` e, `DataTable` em geral, não são seguras quando preenchidas com entrada não confiável.__ Veja a seguir uma lista de maneiras não exaustivas de que `DataSet` uma `DataTable` instância ou pode ler uma entrada não confiável.

* Um `DataAdapter` faz referência a um banco de dados e o `DataAdapter.Fill` método é usado para popular um `DataSet` com o conteúdo de uma consulta de banco de dados.
* O `DataSet.ReadXml` `DataTable.ReadXml` método ou é usado para ler um arquivo XML contendo informações de coluna e linha.
* Uma `DataSet` `DataTable` instância ou é serializada como parte de um ASP.net (SOAP) serviços Web ou um ponto de extremidade WCF.
* Um serializador, como `XmlSerializer` é usado para desserializar `DataSet` uma `DataTable` instância ou de um fluxo XML.
* Um serializador, como `JsonConvert` é usado para desserializar `DataSet` uma `DataTable` instância ou de um fluxo JSON. `JsonConvert` é um método no conhecidoNewtonsoft.Jsde terceiros [ na](https://www.newtonsoft.com/json) biblioteca.
* Um serializador, como `BinaryFormatter` é usado para desserializar `DataSet` uma `DataTable` instância ou de um fluxo de bytes brutos.

Este documento discute considerações de segurança para os cenários anteriores.

## <a name="use-dataadapterfill-to-populate-a-dataset-from-an-untrusted-data-source"></a>Usar `DataAdapter.Fill` para popular um `DataSet` de uma fonte de dados não confiável

Uma `DataSet` instância pode ser populada a partir de um usando `DataAdapter` [o `DataAdapter.Fill` método](/dotnet/api/system.data.common.dataadapter.fill), conforme mostrado no exemplo a seguir.

```cs
// Assumes that connection is a valid SqlConnection object.  
string queryString =
  "SELECT CustomerID, CompanyName FROM dbo.Customers";  
SqlDataAdapter adapter = new SqlDataAdapter(queryString, connection);  
  
DataSet customers = new DataSet();  
adapter.Fill(customers, "Customers");
```

(O exemplo de código acima faz parte de um exemplo maior encontrado em [Populando um conjunto de uma base de um DataAdapter](../populating-a-dataset-from-a-dataadapter.md).)

> A maioria dos aplicativos pode simplificar e assumir que sua camada de banco de dados é confiável. No entanto, se você estiver no hábito de [modelar](https://www.microsoft.com/securityengineering/sdl/threatmodeling) seus aplicativos, seu modelo de ameaça poderá considerar que há um limite de confiança entre o aplicativo (cliente) e a camada de banco de dados (servidor). O uso da [autenticação mútua](/sql/relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections) ou da [autenticação do AAD](/azure/azure-sql/database/authentication-aad-overview) entre o cliente e o servidor é uma maneira de ajudar a resolver os riscos associados a isso. O restante desta seção aborda o resultado possível de um cliente que se conecta a um servidor não confiável.

As consequências de apontar um `DataAdapter` em uma fonte de dados não confiável dependem da implementação da `DataAdapter` si mesma.

### <a name="sqldataadapter"></a>SqlDataAdapter

Para o tipo interno [SqlDataAdapter](/dotnet/api/microsoft.data.sqlclient.sqldataadapter), a referência a uma fonte de dados não confiável pode resultar em um ataque de negação de serviço (dos). O ataque DoS pode fazer com que o aplicativo fique sem resposta ou falhe. Se um invasor puder realizar a fábrica de uma DLL junto com o aplicativo, ele também poderá conseguir a execução de código local.

### <a name="other-dataadapter-types"></a>Outros tipos de DataAdapter

`DataAdapter`Implementações de terceiros devem fazer suas próprias avaliações sobre quais garantias de segurança eles fornecem diante de entradas não confiáveis. O .NET não pode garantir nenhuma garantia de segurança referente a essas implementações.

<a name="dsrdtr"></a>

## <a name="datasetreadxml-and-datatablereadxml"></a>DataSet. ReadXml e DataTable. ReadXml

Os `DataSet.ReadXml` `DataTable.ReadXml` métodos e não são seguros quando usados com entrada não confiável. É altamente recomendável que os consumidores considerem o uso de uma das alternativas descritas posteriormente neste documento.

As implementações do `DataSet.ReadXml` e `DataTable.ReadXml` foram criadas originalmente antes das vulnerabilidades de serialização eram uma categoria de ameaça bem compreendida. Como resultado, o código não segue as práticas recomendadas de segurança atuais. Essas APIs podem ser usadas como vetores para que os invasores executem ataques de DoS aos aplicativos Web. Esses ataques podem tornar o serviço Web sem resposta ou resultar em um encerramento de processo inesperado. A estrutura não fornece mitigações para essas categorias de ataque e o .NET considera esse comportamento "por design".

O .NET lançou atualizações de segurança para atenuar alguns problemas, como divulgação de informações ou execução remota de código no `DataSet.ReadXml` e no `DataTable.ReadXml` . As atualizações de segurança do .NET podem não fornecer proteção completa contra essas categorias de ameaças. Os consumidores devem avaliar seus cenários individuais e considerar sua exposição potencial a esses riscos.

Os consumidores devem estar cientes de que as atualizações de segurança para essas APIs podem afetar a compatibilidade de aplicativos em algumas situações. Além disso, existe a possibilidade de que uma vulnerabilidade de romance nessas APIs seja descoberta para a qual o .NET não possa publicar de forma praticamente uma atualização de segurança.

Recomendamos que os consumidores dessas APIs sejam:

* Considere usar uma das alternativas descritas posteriormente neste documento.
* Execute avaliações de risco individuais em seus aplicativos.

É a única responsabilidade do consumidor determinar se deve utilizar essas APIs. Os consumidores devem avaliar quaisquer riscos de segurança, técnicos e legais, incluindo requisitos regulatórios, que possam acompanhar o uso dessas APIs.

## <a name="dataset-and-datatable-via-aspnet-web-services-or-wcf"></a>DataSet e DataTable por meio de serviços Web ASP.NET ou WCF

É possível aceitar uma `DataSet` `DataTable` instância do ou em um serviço Web ASP.net (SOAP), conforme demonstrado no código a seguir:

```cs
using System.Data;
using System.Web.Services;

[WebService(Namespace = "http://contoso.com/")]
public class MyService : WebService
{
    [WebMethod]
    public string MyWebMethod(DataTable dataTable)
    {
        /* Web method implementation. */
    }
}
```

Uma variação sobre isso não é aceitar `DataSet` ou `DataTable` diretamente como um parâmetro, mas sim aceitá-lo como parte do gráfico de objeto SERIALIZADO em SOAP geral, como mostrado no código a seguir:

```cs
using System.Data;
using System.Web.Services;

[WebService(Namespace = "http://contoso.com/")]
public class MyService : WebService
{
    [WebMethod]
    public string MyWebMethod(MyClass data)
    {
        /* Web method implementation. */
    }
}

public class MyClass
{
    // Property of type DataTable, automatically serialized and
    // deserialized as part of the overall MyClass payload.
    public DataTable MyDataTable { get; set; }
}
```

Ou, usando o WCF em vez de serviços Web do ASP.NET:

```cs
using System.Data;
using System.ServiceModel;

[ServiceContract(Namespace = "http://contoso.com/")]
public interface IMyContract
{
    [OperationContract]
    string MyMethod(DataTable dataTable);
    [OperationContract]
    string MyOtherMethod(MyClass data);
}

public class MyClass
{
    // Property of type DataTable, automatically serialized and
    // deserialized as part of the overall MyClass payload.
    public DataTable MyDataTable { get; set; }
}
```

Em todos esses casos, o modelo de ameaça e as garantias de segurança são os mesmos da [seção DataSet. ReadXml e DataTable. ReadXml](#dsrdtr).

## <a name="deserialize-a-dataset-or-datatable-via-xmlserializer"></a>Desserializar um DataSet ou DataTable via XmlSerializer

Os desenvolvedores podem usar `XmlSerializer` para desserializar `DataSet` e `DataTable` instâncias, conforme mostrado no código a seguir:

```cs
using System.Data;
using System.IO;
using System.Xml.Serialization;

public DataSet PerformDeserialization1(Stream stream) {
    XmlSerializer serializer = new XmlSerializer(typeof(DataSet));
    return (DataSet)serializer.Deserialize(stream);
}

public MyClass PerformDeserialization2(Stream stream) {
    XmlSerializer serializer = new XmlSerializer(typeof(MyClass));
    return (MyClass)serializer.Deserialize(stream);
}

public class MyClass
{
    // Property of type DataTable, automatically serialized and
    // deserialized as part of the overall MyClass payload.
    public DataTable MyDataTable { get; set; }
}
```

Nesses casos, o modelo de ameaça e as garantias de segurança são os mesmos da [seção DataSet. ReadXml e DataTable. ReadXml](#dsrdtr)

## <a name="deserialize-a-dataset-or-datatable-via-jsonconvert"></a>Desserializar um DataSet ou DataTable via JsonConvert

A popular biblioteca de Newtonsoft de terceiros [JSON.net](https://www.newtonsoft.com/json) pode ser usada para desserializar `DataSet` e `DataTable` instâncias, conforme mostrado no código a seguir:

```cs
using System.Data;
using Newtonsoft.Json;

public DataSet PerformDeserialization1(string json) {
    return JsonConvert.DeserializeObect<DataSet>(data);
}

public MyClass PerformDeserialization2(string json) {
    return JsonConvert.DeserializeObect<MyClass>(data);
}

public class MyClass
{
    // Property of type DataTable, automatically serialized and
    // deserialized as part of the overall MyClass payload.
    public DataTable MyDataTable { get; set; }
}
```

A desserialização de uma `DataSet` ou `DataTable` dessa maneira de um blob JSON não confiável não é segura. Esse padrão é vulnerável a um ataque de negação de serviço. Tal ataque poderia falhar no aplicativo ou renderizá-lo sem resposta.

> [!NOTE]
> A Microsoft não garante nem dá suporte à implementação de bibliotecas de terceiros, como _Newtonsoft.Jsno_. Essas informações são fornecidas para fins de integridade e são precisas no momento da elaboração deste artigo.

## <a name="deserialize-a-dataset-or-datatable-via-binaryformatter"></a>Desserializar um DataSet ou DataTable via BinaryFormatter

Os desenvolvedores nunca devem usar os `BinaryFormatter` `NetDataContractSerializer` `SoapFormatter` formatadores ***inseguros*** ,, ou relacionados para desserializar uma `DataSet` `DataTable` instância do ou de uma carga não confiável:

* Isso é suscetível a um ataque de execução de código remoto completo.
* O uso de um personalizado `SerializationBinder` não é suficiente para evitar esse tipo de ataque.

## <a name="safe-replacements"></a>Substituições seguras

Para aplicativos que:

* Aceite `DataSet` ou `DataTable` por meio de um ponto de extremidade SOAP. asmx ou um ponto de extremidade do WCF.
* Desserializar dados não confiáveis em uma instância do `DataSet` ou do `DataTable` .

Considere substituir o modelo de objeto para usar [Entity Framework](/ef). Entity Framework:

* É uma estrutura rica, moderna e orientada a objeto que pode representar dados relacionais.
* O traz [um ecossistema diversificado](/ef/core/providers/) de provedores de banco de dados para facilitar o projeto de consultas de banco de dados por meio de seus modelos de objeto Entity Framework.
* Oferece proteções internas ao desserializar dados de fontes não confiáveis.

Para aplicativos que usam `.aspx` pontos de extremidade SOAP, considere alterar esses pontos de extremidade para usar o [WCF](/dotnet/framework/wcf/). O WCF é uma substituição mais completa dos `.asmx` serviços da Web. Os pontos de extremidade do WCF [podem ser expostos por meio de SOAP](../../../wcf/feature-details/how-to-expose-a-contract-to-soap-and-web-clients.md) para compatibilidade com chamadores existentes.

## <a name="code-analyzers"></a>Analisadores de código

As regras de segurança do analisador de código, que são executadas quando o código-fonte é compilado, podem ajudar a encontrar vulnerabilidades relacionadas a esse problema de segurança em C# e Visual Basic código. Microsoft. CodeAnalysis. FxCopAnalyzers é um pacote NuGet de analisadores de código que é distribuído no [NuGet.org](https://www.nuget.org/).

Para obter uma visão geral dos analisadores de código, consulte [visão geral dos analisadores de código-fonte](https://docs.microsoft.com/visualstudio/code-quality/roslyn-analyzers-overview).

Habilite as seguintes regras Microsoft. CodeAnalysis. FxCopAnalyzers:

- [CA2350](https://docs.microsoft.com/visualstudio/code-quality/ca2350): não use DataTable. ReadXml () com dados não confiáveis
- [CA2351](https://docs.microsoft.com/visualstudio/code-quality/ca2351): não use DataSet. ReadXml () com dados não confiáveis
- [CA2352](https://docs.microsoft.com/visualstudio/code-quality/ca2352): DataSet ou DataTable não seguro em tipo serializável pode ser vulnerável a ataques de execução remota de código
- [CA2353](https://docs.microsoft.com/visualstudio/code-quality/ca2353): DataSet ou DataTable não seguro em tipo serializável
- [CA2354](https://docs.microsoft.com/visualstudio/code-quality/ca2354): um DataSet não seguro ou DataTable no grafo de objeto desserializado pode ser vulnerável a ataques de execução remota de código
- [CA2355](https://docs.microsoft.com/visualstudio/code-quality/ca2355): tipo de DataSet ou DataTable não seguro encontrado no grafo de objeto deserializável
- [CA2356](https://docs.microsoft.com/visualstudio/code-quality/ca2356): tipo de DataSet ou DataTable não seguro no grafo de objeto deserializável da Web
- [CA2361](https://docs.microsoft.com/visualstudio/code-quality/ca2361): garanta que a classe gerada automaticamente contendo DataSet. ReadXml () não seja usada com dados não confiáveis
- [CA2362](https://docs.microsoft.com/visualstudio/code-quality/ca2362): DataSet ou DataTable não seguro em tipo serializável gerado automaticamente pode ser vulnerável a ataques de execução remota de código

Para obter mais informações sobre como configurar regras, consulte [usar analisadores de código](https://docs.microsoft.com/visualstudio/code-quality/use-roslyn-analyzers).

As novas regras de segurança estão disponíveis nos seguintes pacotes NuGet:

- Microsoft. CodeAnalysis. FxCopAnalyzers 3.3.0: para Visual Studio 2019 versão 16,3 ou posterior
- Microsoft. CodeAnalysis. FxCopAnalyzers 2.9.11: para Visual Studio 2017 versão 15,9 ou posterior
