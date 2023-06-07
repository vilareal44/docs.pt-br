---
title: Visão geral do modelo de programação atribuído (MEF)
description: Introdução ao modelo de programação atribuído, que é o modelo de programação padrão para o Managed Extensibility Framework (MEF) no .NET.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- MEF, attributed programming model
- attributed programming model [MEF]
ms.assetid: 49b787ff-2741-4836-ad51-c3017dc592d4
ms.openlocfilehash: aea3a19ffe6f177901e5c0839b618bb36f573beb
ms.sourcegitcommit: 97ce5363efa88179dd76e09de0103a500ca9b659
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2020
ms.locfileid: "86281362"
---
# <a name="attributed-programming-model-overview-mef"></a>Visão geral do modelo de programação atribuído (MEF)

No MEF (Managed Extensibility Framework), um *modelo de programação* é um método específico para definir o conjunto de objetos conceituais no qual o MEF opera. Esses objetos conceituais incluem partes, importações e exportações. O MEF usa esses objetos, mas não especifica como eles devem ser representados. Portanto, uma grande variedade de modelos de programação são possíveis, incluindo modelos de programação personalizados.

O modelo de programação padrão usado no MEF é o *modelo de programação atribuído*. No modelo de programação atribuída, partes, importações, exportações e outros objetos são definidos com atributos que decoram classes comuns do .NET Framework. Este tópico explica como usar os atributos fornecidos pelo modelo de programação atribuída para criar um aplicativo MEF.

<a name="import_and_export_basics"></a>

## <a name="import-and-export-basics"></a>Fundamentos sobre importações e exportações

Uma *exportação* é um valor que uma parte fornece a outras partes no contêiner, e uma *importação* é um requisito que uma parte expressa ao contêiner, a ser preenchido com as exportações disponíveis. No modelo de programação atribuída, importações e exportações são declaradas decorando classes ou membros com os atributos `Import` e `Export`. O atributo `Export` pode decorar uma classe, um campo, propriedade ou método, enquanto o atributo `Import` pode decorar um parâmetro de campo, propriedade ou construtor.

Para que uma importação seja correspondida a uma exportação, a importação e a exportação devem ter o mesmo *contrato*. O contrato é formado por uma cadeia de caracteres, chamada *nome do contrato*, e pelo tipo de objeto exportado ou importado, chamado de *tipo de contrato*. Somente se o nome do contrato e o tipo de contrato corresponderem, uma exportação é considerada para atender uma importação específica.

Um ou os dois parâmetros do contrato podem ser implícitos ou explícitos. O código a seguir mostra uma classe que declara uma importação básica.

```vb
Public Class MyClass1
    <Import()>
    Public Property MyAddin As IMyAddin
End Class
```

```csharp
public class MyClass
{
    [Import]
    public IMyAddin MyAddin { get; set; }
}
```

Nessa importação, o atributo `Import` não tem um parâmetro de tipo de contrato, nem de nome de contrato anexado. Portanto, ambos serão inferidos da propriedade decorada. Nesse caso, o tipo de contrato será `IMyAddin`, e o nome do contrato será uma cadeia de caracteres exclusiva criada do tipo de contrato. (Em outras palavras, o nome do contrato corresponderá somente às exportações cujos nomes também sejam inferidos do tipo `IMyAddin`.)

Segue uma exportação que corresponde à importação anterior.

```vb
<Export(GetType(IMyAddin))>
Public Class MyLogger
    Implements IMyAddin

End Class
```

```csharp
[Export(typeof(IMyAddin))]
public class MyLogger : IMyAddin { }
```

Nessa exportação, o tipo de contrato é `IMyAddin`, pois ele é especificado como parâmetro do atributo `Export`. O tipo exportado deve ser igual ao tipo de contrato, ser derivado do tipo de contrato ou implementar o tipo de contrato, caso seja uma instância. Nessa exportação, o tipo real `MyLogger` implementa a interface `IMyAddin`. O nome do contrato é inferido do tipo de contrato, o que significa que essa exportação corresponderá à importação anterior.

> [!NOTE]
> Exportações e importações geralmente devem ser declaradas em classes ou membros públicos. É oferecido suporte a outras declarações, mas exportar ou importar um membro privado, protegido ou interno quebra o modelo de isolamento para a parte e, portanto, isso não é recomendado.

O tipo de contrato deve corresponder exatamente para que a exportação e a importação sejam consideradas uma correspondência. Considere a seguinte exportação.

```vb
<Export()> 'WILL NOT match the previous import!
Public Class MyLogger
    Implements IMyAddin

End Class
```

```csharp
[Export] //WILL NOT match the previous import!
public class MyLogger : IMyAddin { }
```

Nessa exportação, o tipo de contrato é `MyLogger` ao invés de `IMyAddin`. Embora `MyLogger` implemente `IMyAddin` e, portanto, possa ser convertido em um objeto `IMyAddin`, essa exportação não corresponderá à importação anterior porque os tipos de contrato não são iguais.

Em geral, não é necessário especificar o nome do contrato, e a maioria dos contratos deve ser definida em relação ao tipo de contrato e metadados. No entanto, em determinadas circunstâncias, é importante especificar o nome do contrato diretamente. O caso mais comum é quando uma classe exporta vários valores que compartilham um tipo em comum, como primitivos. O nome do contrato pode ser especificado como primeiro parâmetro do atributo `Import` ou `Export`. O código a seguir mostra uma importação e uma exportação com um nome de contrato especificado de `MajorRevision`.

```vb
Public Class MyExportClass

    'This one will match
    <Export("MajorRevision")>
    Public ReadOnly Property MajorRevision As Integer
        Get
            Return 4
        End Get
    End Property

    <Export("MinorRevision")>
    Public ReadOnly Property MinorRevision As Integer
        Get
            Return 16
        End Get
    End Property
End Class
```

```csharp
public class MyClass
{
    [Import("MajorRevision")]
    public int MajorRevision { get; set; }
}

public class MyExportClass
{
    [Export("MajorRevision")] //This one will match.
    public int MajorRevision = 4;

    [Export("MinorRevision")]
    public int MinorRevision = 16;
}
```

Se o tipo de contrato não for especificado, ele ainda será inferido do tipo da importação ou exportação. Entretanto, mesmo se o nome do contrato for especificado explicitamente, o tipo de contrato também deve corresponder exatamente para que a importação e a exportação sejam consideradas uma correspondência. Por exemplo, se o campo `MajorRevision` fosse uma cadeia de caracteres, os tipos de contrato inferidos não corresponderiam e a exportação não corresponderia à importação, apesar de ter o mesmo nome de contrato.

### <a name="importing-and-exporting-a-method"></a>Importando e exportando um método

O atributo `Export` também pode decorar um método, da mesma maneira que uma classe, propriedade ou função. Exportações de métodos devem especificar um tipo ou nome de contrato, uma vez que o tipo não pode ser inferido. O tipo especificado pode ser um representante personalizado ou um tipo genérico, como `Func`. A classe a seguir exporta um método denominado `DoSomething`.

```vb
Public Class MyAddin

    'Explicitly specifying a generic type
    <Export(GetType(Func(Of Integer, String)))>
    Public Function DoSomething(ByVal TheParam As Integer) As String
        Return Nothing 'Function body goes here
    End Function

End Class
```

```csharp
public class MyAddin
{
    //Explicitly specifying a generic type.
    [Export(typeof(Func<int, string>))]
    public string DoSomething(int TheParam);
}
```

Nessa classe, o método `DoSomething` considera um único parâmetro `int` e retorna um `string`. Para corresponder a essa exportação, a parte da importação deve declarar um membro adequado. A classe a seguir importa o método `DoSomething`.

```vb
Public Class MyClass1

    'Contract name must match!
    <Import()>
    Public Property MajorRevision As Func(Of Integer, String)
End Class
```

```csharp
public class MyClass
{
    [Import] //Contract name must match!
    public Func<int, string> DoSomething { get; set; }
}
```

Para obter mais informações sobre como usar o objeto `Func<T, T>`, consulte <xref:System.Func%602>.

<a name="types_of_imports"></a>

## <a name="types-of-imports"></a>Tipos de importações

O MEF oferece suporte a vários tipos de importação, incluindo importação dinâmica, lenta, obrigatória e opcional.

### <a name="dynamic-imports"></a>Importações dinâmicas

Em alguns casos, a classe de importação pode querer corresponder exportações de qualquer tipo que tenham um nome de contrato específico. Nesse cenário, a classe pode declarar uma *importação dinâmica*. A importação a seguir corresponde qualquer exportação com o nome de contrato "TheString".

```vb
Public Class MyClass1

    <Import("TheString")>
    Public Property MyAddin

End Class
```

```csharp
public class MyClass
{
    [Import("TheString")]
    public dynamic MyAddin { get; set; }
}
```

Quando o tipo de contrato é inferido da palavra-chave `dynamic`, ele corresponderá a qualquer tipo de contrato. Nesse caso, uma importação deve **sempre** especificar um nome de contrato para correspondência. (Se nenhum nome de contrato for especificado, a importação será considerada como não corresponder a exportações.) Ambas as exportações a seguir corresponderão à importação anterior.

```vb
<Export("TheString", GetType(IMyAddin))>
Public Class MyLogger
    Implements IMyAddin

End Class

<Export("TheString")>
Public Class MyToolbar

End Class
```

```csharp
[Export("TheString", typeof(IMyAddin))]
public class MyLogger : IMyAddin { }

[Export("TheString")]
public class MyToolbar { }
```

Obviamente, a classe de importação deve ser preparada para lidar com um objeto de tipo arbitrário.

### <a name="lazy-imports"></a>Importações lentas

Em alguns casos, a classe de importação pode exigir uma referência indireta ao objeto importado, de modo que o objeto não seja instanciado imediatamente. Nesse cenário, a classe pode declarar uma *importação lenta* usando o tipo de contrato `Lazy<T>`. A propriedade de importação a seguir declara uma importação lenta.

```vb
Public Class MyClass1

    <Import()>
    Public Property MyAddin As Lazy(Of IMyAddin)

End Class
```

```csharp
public class MyClass
{
    [Import]
    public Lazy<IMyAddin> MyAddin { get; set; }
}
```

A partir do ponto de vista do mecanismo de composição, um tipo de contrato `Lazy<T>` é considerado idêntico ao tipo de contrato `T`. Portanto, a importação anterior corresponderia à exportação seguinte.

```vb
<Export(GetType(IMyAddin))>
Public Class MyLogger
    Implements IMyAddin

End Class
```

```csharp
[Export(typeof(IMyAddin))]
public class MyLogger : IMyAddin { }
```

O nome e o tipo do contrato podem ser especificados no atributo `Import` para uma importação lenta, como descrito anteriormente na seção "Importações e exportações básicas".

### <a name="prerequisite-imports"></a>Importações obrigatórias

Partes de MEF exportadas geralmente são criadas pelo mecanismo de composição, em resposta a uma solicitação direta ou à necessidade de preencher uma importação correspondida. Por padrão, ao criar uma parte, o mecanismo de composição usa o construtor sem parâmetro. Para fazer com que o mecanismo use um construtor diferente, você pode marcá-lo com o atributo `ImportingConstructor`.

Cada parte pode ter somente um construtor para uso por parte do mecanismo de composição. Não fornecer nenhum construtor sem parâmetros e nenhum atributo `ImportingConstructor`, ou fornecer mais de um atributo `ImportingConstructor`, gerará um erro.

Para preencher os parâmetros de um construtor marcado com o atributo `ImportingConstructor`, todos esses parâmetros são automaticamente declarados como importações. Essa é uma maneira conveniente de declarar importações que são usadas durante da inicialização da parte. A classe a seguir usa `ImportingConstructor` para declarar uma importação.

```vb
Public Class MyClass1

    Private _theAddin As IMyAddin

    'Parameterless constructor will NOT be used
    'because the ImportingConstructor
    'attribute is present.
    Public Sub New()

    End Sub

    'This constructor will be used.
    'An import with contract type IMyAddin
    'is declared automatically.
    <ImportingConstructor()>
    Public Sub New(ByVal MyAddin As IMyAddin)
        _theAddin = MyAddin
    End Sub

End Class
```

```csharp
public class MyClass
{
    private IMyAddin _theAddin;

    //Parameterless constructor will NOT be
    //used because the ImportingConstructor
    //attribute is present.
    public MyClass() { }

    //This constructor will be used.
    //An import with contract type IMyAddin is
    //declared automatically.
    [ImportingConstructor]
    public MyClass(IMyAddin MyAddin)
    {
        _theAddin = MyAddin;
    }
}
```

Por padrão, o atributo `ImportingConstructor` usa tipos e nomes de contrato inferidos para todas as importações de parâmetros. É impossível substituir isso decorando os parâmetros com atributos `Import`, que podem então definir o tipo e o nome do contrato explicitamente. O código a seguir demonstra um construtor que usa essa sintaxe para importar uma classe derivada ao invés de uma classe pai.

```vb
<ImportingConstructor()>
Public Sub New(<Import(GetType(IMySubAddin))> ByVal MyAddin As IMyAddin)

End Sub
```

```csharp
[ImportingConstructor]
public MyClass([Import(typeof(IMySubAddin))]IMyAddin MyAddin)
{
    _theAddin = MyAddin;
}
```

Em particular, você deve tomar cuidado com parâmetros de coleção. Por exemplo, se você especificar `ImportingConstructor` em um construtor com o parâmetro do tipo `IEnumerable<int>`, a importação corresponderá a uma única exportação do tipo `IEnumerable<int>`, ao invés de a um conjunto de exportações do tipo `int`. Para corresponder a um conjunto de exportações do tipo `int`, você precisa decorar o parâmetro com o atributo `ImportMany`.

Os parâmetros declarados como importações pelo atributo `ImportingConstructor` também são marcados como *importações de pré-requisito*. O MEF normalmente permite exportações e importações para formar um *ciclo*. Por exemplo, um ciclo é quando o objeto A importa o objeto B, que por sua vez importa o objeto A. Em circunstância normais, um ciclo não é um problema e o contêiner de composição constrói os dois objetos normalmente.

Quando um valor importado é solicitado pelo construtor de uma parte, esse objeto não pode participar de um ciclo. Se o objeto A exigir que o objeto B seja construído antes que ele próprio possa ser construído, e o objeto B importar o objeto A, então será impossível resolver o ciclo e um erro de composição ocorrerá. Importações declaradas nos parâmetros do construtor são, portanto, importações obrigatórias, que devem ser preenchidas antes que qualquer exportação do objeto que as requer possa ser usada.

### <a name="optional-imports"></a>Importações opcionais

O atributo `Import` especifica um requisito para a parte funcionar. Se uma importação não puder ser atendida, a composição dessa parte falhará e a parte não estará disponível.

Você pode especificar que uma importação é *optional* usando a propriedade `AllowDefault`. Nesse caso, a composição terá sucesso mesmo se a importação não corresponder a nenhuma exportação disponível, e a propriedade de importação será definida como o padrão para seu tipo de propriedade ( `null` para propriedades de objeto, `false` para boolianos ou zero para propriedades numéricas). A classe a seguir usa uma importação opcional.

```vb
Public Class MyClass1

    <Import(AllowDefault:=True)>
    Public Property thePlugin As Plugin

    'If no matching export is available,
    'thePlugin will be set to null.
End Class
```

```csharp
public class MyClass
{
    [Import(AllowDefault = true)]
    public Plugin thePlugin { get; set; }

    //If no matching export is available,
    //thePlugin will be set to null.
}
```

### <a name="importing-multiple-objects"></a>Importando vários objetos

O atributo `Import` só será composto com êxito quando ele corresponder a somente uma exportação. Outros casos produzirão um erro de composição. Para importar mais de uma exportação que corresponda ao mesmo contrato, use o atributo `ImportMany`. Importações marcadas com esse atributo são sempre opcionais. Por exemplo, a composição não falhará se nenhuma exportação correspondente estiver presente. A classe a seguir importa qualquer número de exportações do tipo `IMyAddin`.

```vb
Public Class MyClass1

    <ImportMany()>
    Public Property MyAddin As IEnumerable(Of IMyAddin)

End Class
```

```csharp
public class MyClass
{
    [ImportMany]
    public IEnumerable<IMyAddin> MyAddin { get; set; }
}
```

A matriz importada pode ser acessada usando sintaxe e métodos comuns `IEnumerable<T>`. Também é impossível usar uma matriz comum (`IMyAddin[]`).

O padrão pode ser bastante importante quando usado combinado à sintaxe `Lazy<T>`. Por exemplo, ao usar `ImportMany`, `IEnumerable<T>` e `Lazy<T>`, você pode importar referências indiretas a qualquer número de objetos e apenas instanciar aqueles que são necessários. A classe a seguir mostra esse padrão.

```vb
Public Class MyClass1

    <ImportMany()>
    Public Property MyAddin As IEnumerable(Of Lazy(Of IMyAddin))

End Class
```

```csharp
public class MyClass
{
    [ImportMany]
    public IEnumerable<Lazy<IMyAddin>> MyAddin { get; set; }
}
```

<a name="avoiding_discovery"></a>

## <a name="avoiding-discovery"></a>Evitando descobertas

Em alguns casos, talvez você queira impedir que uma parte seja descoberta como parte de um catálogo. Por exemplo, a parte pode ser uma classe base para fins de herança, mas não para ser usada. Há duas maneiras de fazer isso. Primeiramente, você pode usar a palavra-chave `abstract` na classe da parte. Classes abstratas nunca oferecem exportações, embora possam fornecer exportações herdadas a classes derivadas.

Se a classe não puder se tornar abstrata, você poderá decorá-la com o atributo `PartNotDiscoverable`. Uma parte decorada com esse atributo não será incluída em nenhum catálogo. O exemplo a seguir demonstra esses padrões. `DataOne` será descoberto pelo catálogo. Uma vez que `DataTwo` é abstrato, ele não será descoberto. Uma vez que `DataThree` usou o atributo `PartNotDiscoverable`, ele não será descoberto.

```vb
<Export()>
Public Class DataOne
    'This part will be discovered
    'as normal by the catalog.
End Class

<Export()>
Public MustInherit Class DataTwo
    'This part will not be discovered
    'by the catalog.
End Class

<PartNotDiscoverable()>
<Export()>
Public Class DataThree
    'This part will also not be discovered
    'by the catalog.
End Class
```

```csharp
[Export]
public class DataOne
{
    //This part will be discovered
    //as normal by the catalog.
}

[Export]
public abstract class DataTwo
{
    //This part will not be discovered
    //by the catalog.
}

[PartNotDiscoverable]
[Export]
public class DataThree
{
    //This part will also not be discovered
    //by the catalog.
}
```

<a name="metadata_and_metadata_views"></a>

## <a name="metadata-and-metadata-views"></a>Metadados e exibições de metadados

As exportações podem fornecer informações adicionais sobre elas mesmas, conhecidas como *metadados*. Metadados podem ser usados para transmitir propriedades do objeto exportado à parte que realizará a importação. A parte que realizará a importação pode usar esses dados para decidir quais exportações utilizar, ou coletar informações sobre uma exportação sem precisar construí-la. Por esse motivo, uma importação deve ser lenta para usar metadados.

Para usar metadados, geralmente você declara uma interface conhecida como *exibição de metadados*, que declara quais metadados estarão disponíveis. A interface de exibição de metadados deve conter somente propriedades, e essas propriedades devem ter acessadores `get`. A interface a seguir é um exemplo de exibição de metadados.

```vb
Public Interface IPluginMetadata

    ReadOnly Property Name As String

    <DefaultValue(1)>
    ReadOnly Property Version As Integer

End Interface
```

```csharp
public interface IPluginMetadata
{
    string Name { get; }

    [DefaultValue(1)]
    int Version { get; }
}
```

Também é possível usar uma coleção genérica, `IDictionary<string, object>`, como uma exibição de metadados, mas isso não inclui os benefícios da verificação de tipo e deve ser evitado.

Normalmente, todas as propriedades denominadas na exibição de metadados são necessárias, e qualquer exportação que não as forneça não será considerada correspondente. O atributo `DefaultValue` especifica que uma propriedade é opcional. Se a propriedade não for incluída, o valor padrão especificado como parâmetro de `DefaultValue` será atribuído a ela. A seguir, há duas classes diferentes decoradas com metadados. Essas duas classes corresponderiam à exibição de metadados anterior.

```vb
<Export(GetType(IPlugin))>
<ExportMetadata("Name", "Logger")>
<ExportMetadata("Version", 4)>
Public Class MyLogger
    Implements IPlugin

End Class

'Version is not required because of the DefaultValue
<Export(GetType(IPlugin))>
<ExportMetadata("Name", "Disk Writer")>
Public Class DWriter
    Implements IPlugin

End Class
```

```csharp
[Export(typeof(IPlugin)),
    ExportMetadata("Name", "Logger"),
    ExportMetadata("Version", 4)]
public class Logger : IPlugin
{
}

[Export(typeof(IPlugin)),
    ExportMetadata("Name", "Disk Writer")]
    //Version is not required because of the DefaultValue
public class DWriter : IPlugin
{
}
```

Os metadados são expressados após o atributo `Export` usando o atributo `ExportMetadata`. Cada metadado é composto de um par de nome/valor. A parte do nome dos metadados deve corresponder ao nome da propriedade adequada na exibição de metadados, e o valor será atribuído a essa propriedade.

É o importador que especifica qual exibição de metadados será usada, se houver. Uma importação com metadados é declarada como uma importação lenta, com a interface dos metadados como o segundo parâmetro de tipo para `Lazy<T,T>`. A classe a seguir importa a parte anterior com os metadados.

```vb
Public Class Addin

    <Import()>
    Public Property plugin As Lazy(Of IPlugin, IPluginMetadata)
End Class
```

```csharp
public class Addin
{
    [Import]
    public Lazy<IPlugin, IPluginMetadata> plugin;
}
```

Em muitos casos, você vai querer combinar metadados com o atributo `ImportMany`, a fim de analisar as importações disponíveis, escolher e instanciar apenas uma ou filtrar uma coleção para corresponder a uma condição específica. A classe a seguir instancia somente `IPlugin` objetos que possuem o valor `Name` "Agente".

```vb
Public Class User

    <ImportMany()>
    Public Property plugins As IEnumerable(Of Lazy(Of IPlugin, IPluginMetadata))

    Public Function InstantiateLogger() As IPlugin

        Dim logger As IPlugin
        logger = Nothing

        For Each Plugin As Lazy(Of IPlugin, IPluginMetadata) In plugins
            If Plugin.Metadata.Name = "Logger" Then
                logger = Plugin.Value
            End If
        Next
        Return logger
    End Function

End Class
```

```csharp
public class User
{
    [ImportMany]
    public IEnumerable<Lazy<IPlugin, IPluginMetadata>> plugins;

    public IPlugin InstantiateLogger()
    {
        IPlugin logger = null;

        foreach (Lazy<IPlugin, IPluginMetadata> plugin in plugins)
        {
            if (plugin.Metadata.Name == "Logger")
                logger = plugin.Value;
        }
        return logger;
    }
}
```

<a name="import_and_export_inheritance"></a>

## <a name="import-and-export-inheritance"></a>Herança de importações e exportações

Se uma classe herdar de uma parte, essa classe também pode se tornar uma parte. As importações são sempre herdadas por subclasses. Portanto, uma subclasse de uma parte sempre será uma parte, com as mesmas importações que sua classe pai.

Exportações declaradas usando o atributo `Export` não são herdadas por subclasses. Entretanto, uma parte pode exportar a si mesma usando o atributo `InheritedExport`. Subclasses da parte herdarão e fornecerão a mesma exportação, incluindo o nome e o tipo do contrato. Diferentemente de um atributo `Export`, `InheritedExport` pode ser aplicado somente no nível da classe e não no nível do membro. Portanto, exportações no nível do membro nunca podem ser herdadas.

As quatro classes a seguir demonstram os princípios da herança de importação e exportação. A `NumTwo` é herdada da `NumOne`, portanto a `NumTwo` importará a `IMyData`. Exportações comuns não são herdadas, por isso, `NumTwo` não exportará nada. `NumFour` herda de `NumThree`. Como `NumThree` usou `InheritedExport`, `NumFour` tem uma exportação com o tipo de contrato `NumThree`. Exportações no nível do membro nunca são herdadas, portanto, `IMyData` não é exportado.

```vb
<Export()>
Public Class NumOne
    <Import()>
    Public Property MyData As IMyData
End Class

Public Class NumTwo
    Inherits NumOne

    'Imports are always inherited, so NumTwo will
    'Import IMyData

    'Ordinary exports are not inherited, so
    'NumTwo will NOT export anything.  As a result it
    'will not be discovered by the catalog!

End Class

<InheritedExport()>
Public Class NumThree

    <Export()>
    Public Property MyData As IMyData

    'This part provides two exports, one of
    'contract type NumThree, and one of
    'contract type IMyData.

End Class

Public Class NumFour
    Inherits NumThree

    'Because NumThree used InheritedExport,
    'this part has one export with contract
    'type NumThree.

    'Member-level exports are never inherited,
    'so IMyData is not exported.

End Class
```

```csharp
[Export]
public class NumOne
{
    [Import]
    public IMyData MyData { get; set; }
}

public class NumTwo : NumOne
{
    //Imports are always inherited, so NumTwo will
    //import IMyData.

    //Ordinary exports are not inherited, so
    //NumTwo will NOT export anything. As a result it
    //will not be discovered by the catalog!
}

[InheritedExport]
public class NumThree
{
    [Export]
    Public IMyData MyData { get; set; }

    //This part provides two exports, one of
    //contract type NumThree, and one of
    //contract type IMyData.
}

public class NumFour : NumThree
{
    //Because NumThree used InheritedExport,
    //this part has one export with contract
    //type NumThree.

    //Member-level exports are never inherited,
    //so IMyData is not exported.
}
```

Se houver metadados associados a um atributo `InheritedExport`, esses metadados também serão herdados. (Para obter mais informações, consulte a seção "exibições de metadados e metadados" anteriores.) Os metadados herdados não podem ser modificados pela subclasse. No entanto, ao declarar novamente o atributo `InheritedExport` com o mesmo nome e tipo de contrato, mas com novos metadados, a subclasse pode substituir os metadados herdados por novos metadados. A classe a seguir demonstra esse princípio. A parte `MegaLogger` herda de `Logger` e inclui o atributo `InheritedExport`. Uma vez que `MegaLogger` declara novamente novos metadados nomeados como Status, ele não herda os metadados de Nome e Versão de `Logger`.

```vb
<InheritedExport(GetType(IPlugin))>
<ExportMetadata("Name", "Logger")>
<ExportMetadata("Version", 4)>
Public Class Logger
    Implements IPlugin

    'Exports with contract type IPlugin
    'and metadata "Name" and "Version".
End Class

Public Class SuperLogger
    Inherits Logger

    'Exports with contract type IPlugin and
    'metadata "Name" and "Version", exactly the same
    'as the Logger class.

End Class

<InheritedExport(GetType(IPlugin))>
<ExportMetadata("Status", "Green")>
Public Class MegaLogger
    Inherits Logger

    'Exports with contract type IPlugin and
    'metadata "Status" only. Re-declaring
    'the attribute replaces all metadata.

End Class
```

```csharp
[InheritedExport(typeof(IPlugin)),
    ExportMetadata("Name", "Logger"),
    ExportMetadata("Version", 4)]
public class Logger : IPlugin
{
    //Exports with contract type IPlugin and
    //metadata "Name" and "Version".
}

public class SuperLogger : Logger
{
    //Exports with contract type IPlugin and
    //metadata "Name" and "Version", exactly the same
    //as the Logger class.
}

[InheritedExport(typeof(IPlugin)),
    ExportMetadata("Status", "Green")]
public class MegaLogger : Logger        {
    //Exports with contract type IPlugin and
    //metadata "Status" only. Re-declaring
    //the attribute replaces all metadata.
}
```

Ao declarar novamente o atributo `InheritedExport` para substituir metadados, assegure-se de que os tipos de contrato sejam iguais. (No exemplo anterior, `IPlugin` é o tipo de contrato.) Se forem diferentes, em vez de substituir, o segundo atributo criará uma segunda exportação independente da parte. Em geral, isso significa que você terá que especificar explicitamente o tipo de contrato ao substituir um atributo `InheritedExport`, como mostrado no exemplo anterior.

Uma vez que interfaces não podem ser instanciadas diretamente, elas geralmente não podem ser decoradas com atributos `Export` ou `Import`. No entanto, uma interface pode ser decorada com um atributo `InheritedExport` no nível da interface, e essa exportação, juntamente com quaisquer metadados associados, serão herdados por qualquer classe de implementação. Entretanto, a interface em si não estará disponível como uma parte.

<a name="custom_export_attributes"></a>

## <a name="custom-export-attributes"></a>Atributos de exportação personalizados

Os atributos de exportação básicos, `Export` e `InheritedExport`, podem ser estendidos para incluir metadados como propriedades de atributo. Essa técnica é útil para aplicar metadados semelhantes a diversas partes ou criar uma árvore de herança de atributos de metadados.

Um atributo personalizado pode especificar o tipo de contrato, o nome do contrato ou qualquer outro metadado. Para definir um atributo personalizado, uma classe que herda de `ExportAttribute` (ou `InheritedExportAttribute`) deve ser declarada com o atributo `MetadataAttribute`. A classe a seguir define um atributo personalizado.

```vb
<MetadataAttribute()>
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=false)>
Public Class MyAttribute
    Inherits ExportAttribute

    Public Property MyMetadata As String

    Public Sub New(ByVal myMetadata As String)
        MyBase.New(GetType(IMyAddin))

        myMetadata = myMetadata
    End Sub

End Class
```

```csharp
[MetadataAttribute]
[AttributeUsage(AttributeTargets.Class, AllowMultiple=false)]
public class MyAttribute : ExportAttribute
{
    public MyAttribute(string myMetadata)
        : base(typeof(IMyAddin))
    {
        MyMetadata = myMetadata;
    }

    public string MyMetadata { get; private set; }
}
```

Essa classe define um atributo personalizado denominado `MyAttribute` com o tipo de contrato `IMyAddin` e alguns metadados denominados `MyMetadata`. Todas as propriedades em uma classe marcada com o atributo `MetadataAttribute` são consideradas como sendo metadados definidos no atributo personalizado. As duas declarações a seguir são equivalentes.

```vb
<Export(GetType(IMyAddin))>
<ExportMetadata("MyMetadata", "theData")>
Public Property myAddin As MyAddin
```

```vb
<MyAttribute("theData")>
Public Property myAddin As MyAddin
```

```csharp
[Export(typeof(IMyAddin)),
    ExportMetadata("MyMetadata", "theData")]
public MyAddin myAddin { get; set; }
```

```csharp
[MyAttribute("theData")]
public MyAddin myAddin { get; set; }
```

Na primeira declaração, o tipo de contrato e os metadados são definidos explicitamente. Na segunda declaração, o tipo de contrato e os metadados são implícitos no atributo personalizado. Especialmente em casos em que uma grande quantidade de metadados idênticos devem ser aplicados a várias partes (por exemplo, informações autorais ou de copyright), usar um atributo personalizado pode economizar bastante tempo e duplicação. Além disso, é possível criar árvores de herança de atributos personalizados para permitir variações.

Para criar metadados opcionais em um atributo personalizado, você pode usar o atributo `DefaultValue`. Quando esse atributo é aplicado a uma propriedade em uma classe de atributos personalizados, ele especifica que a propriedade decorada é opcional e não precisa ser fornecida por um exportador. Se um valor da propriedade não for fornecido, o valor padrão será designado a ela no tipo de propriedade (geralmente `null`, `false` ou 0.)

<a name="creation_policies"></a>

## <a name="creation-policies"></a>Políticas de criação

Quando uma parte especifica uma importação e a composição é realizada, o contêiner da composição tenta encontrar uma exportação correspondente. Se a importação for correspondida a uma exportação com êxito, o membro da importação é definido como uma instância do objeto exportado. O lugar de origem dessa instância é controlado pela *política de criação* da parte exportadora.

As duas políticas de criação possíveis são *compartilhada* e *não compartilhada*. Uma parte com uma política de criação compartilhada será compartilhada entre cada importação no contêiner para uma parte com esse contrato. Quando o mecanismo de composição encontra uma correspondência e precisa definir uma propriedade de importação, ele instanciará uma nova cópia da parte somente se ainda não existir uma; caso contrário, a cópia existente será fornecida. Isso significa que vários objetos possuem referências para a mesma parte. Essas partes não devem depender do estado interno que pode ser alterado a partir de diversos locais. Essa política é adequada para partes estáticas, partes que oferecem serviços e partes que consomem muita memória ou outros recursos.

Uma parte com a política de criação não compartilhada será criada toda vez que uma importação correspondente para uma das exportações for encontrada. Uma nova cópia será, portanto, instanciada para cada importação no contêiner que corresponder a um dos contratos exportados da parte. O estado interno dessas cópias não será compartilhado. Essa política é adequada para partes em que cada importação exigir seu próprio estado interno.

A importação e a exportação podem especificar a política de criação de uma parte, dentre os valores `Shared`, `NonShared` ou `Any`. O padrão é `Any` para importações e exportações. Uma exportação que especifica `Shared` ou `NonShared` será correspondente somente a uma importação com a mesma especificação, ou que especificar `Any`. Da mesma forma, uma importação que especifica `Shared` ou `NonShared` será correspondente somente a uma exportação com a mesma especificação, ou que especificar `Any`. Importações e exportações com políticas de criação incompatíveis não são consideradas correspondentes, da mesma maneira que uma importação e uma exportação cujo nome ou tipo de contrato não são correspondentes. Se a importação e a exportação especificarem `Any`, ou não especificarem uma política de criação e o padrão for definido como `Any`, a política de criação será definida como compartilhada por padrão.

O exemplo a seguir mostra importações e exportações que especificam políticas de criação. `PartOne` não especifica uma política de criação, portanto, o padrão é `Any`. `PartTwo` não especifica uma política de criação, portanto, o padrão é `Any`. Como o padrão da importação e da exportação é `Any`, `PartOne` será compartilhada. `PartThree` especifica uma política de criação `Shared`, portanto, `PartTwo` e `PartThree` compartilharão a mesma cópia de `PartOne`. `PartFour` especifica uma política de criação `NonShared`, portanto, `PartFour` não será compartilhada em `PartFive`. `PartSix` especifica uma política de criação `NonShared`. `PartFive` e `PartSix` receberão cópias separadas de `PartFour`. `PartSeven` especifica uma política de criação `Shared`. Como não há `PartFour` exportado com uma política de criação de `Shared`, a importação `PartSeven` não é correspondente a nada e não será preenchida.

```vb
<Export()>
Public Class PartOne
    'The default creation policy for an export is Any.
End Class

Public Class PartTwo

    <Import()>
    Public Property partOne As PartOne

    'The default creation policy for an import is Any.
    'If both policies are Any, the part will be shared.

End Class

Public Class PartThree

    <Import(RequiredCreationPolicy:=CreationPolicy.Shared)>
    Public Property partOne As PartOne

    'The Shared creation policy is explicitly specified.
    'PartTwo and PartThree will receive references to the
    'SAME copy of PartOne.

End Class

<Export()>
<PartCreationPolicy(CreationPolicy.NonShared)>
Public Class PartFour
    'The NonShared creation policy is explicitly specified.
End Class

Public Class PartFive

    <Import()>
    Public Property partFour As PartFour

    'The default creation policy for an import is Any.
    'Since the export's creation policy was explicitly
    'defined, the creation policy for this property will
    'be non-shared.

End Class

Public Class PartSix

    <Import(RequiredCreationPolicy:=CreationPolicy.NonShared)>
    Public Property partFour As PartFour

    'Both import and export specify matching creation
    'policies.  PartFive and PartSix will each receive
    'SEPARATE copies of PartFour, each with its own
    'internal state.

End Class

Public Class PartSeven

    <Import(RequiredCreationPolicy:=CreationPolicy.Shared)>
    Public Property partFour As PartFour

    'A creation policy mismatch.  Because there is no
    'exported PartFour with a creation policy of Shared,
    'this import does not match anything and will not be
    'filled.

End Class
```

```csharp
[Export]
public class PartOne
{
    //The default creation policy for an export is Any.
}

public class PartTwo
{
    [Import]
    public PartOne partOne { get; set; }

    //The default creation policy for an import is Any.
    //If both policies are Any, the part will be shared.
}

public class PartThree
{
    [Import(RequiredCreationPolicy = CreationPolicy.Shared)]
    public PartOne partOne { get; set; }

    //The Shared creation policy is explicitly specified.
    //PartTwo and PartThree will receive references to the
    //SAME copy of PartOne.
}

[Export]
[PartCreationPolicy(CreationPolicy.NonShared)]
public class PartFour
{
    //The NonShared creation policy is explicitly specified.
}

public class PartFive
{
    [Import]
    public PartFour partFour { get; set; }

    //The default creation policy for an import is Any.
    //Since the export's creation policy was explicitly
    //defined, the creation policy for this property will
    //be non-shared.
}

public class PartSix
{
    [Import(RequiredCreationPolicy = CreationPolicy.NonShared)]
    public PartFour partFour { get; set; }

    //Both import and export specify matching creation
    //policies.  PartFive and PartSix will each receive
    //SEPARATE copies of PartFour, each with its own
    //internal state.
}

public class PartSeven
{
    [Import(RequiredCreationPolicy = CreationPolicy.Shared)]
    public PartFour partFour { get; set; }

    //A creation policy mismatch.  Because there is no
    //exported PartFour with a creation policy of Shared,
    //this import does not match anything and will not be
    //filled.
}
```

<a name="life_cycle_and_disposing"></a>

## <a name="life-cycle-and-disposing"></a>Ciclo de vida e descarte

Uma vez que as partes estão hospedadas no contêiner de composição, seu ciclo de vida pode ser mais complexo do que objetos comuns. As partes podem implementar duas interfaces importantes relacionadas ao ciclo de vida: `IDisposable` e `IPartImportsSatisfiedNotification`.

Partes que precisam que um trabalho seja realizado no desligamento ou que precisam ter recursos liberados devem implementar `IDisposable`, como é comum para objetos do .NET Framework. No entanto, uma vez que o contêiner cria e mantém referências a partes, somente o contêiner que possui uma parte deve chamar o método `Dispose` em si. O contêiner em si implementa `IDisposable` e, como parte de sua limpeza em `Dispose`, ele chamará `Dispose` em todas as partes que possui. Por esse motivo, você deve sempre descartar o contêiner de composição quando ele, e qualquer uma de suas partes, não for mais necessário.

Para contêineres de composição de longa vida, o consumo de memória pelas partes com uma política de criação não compartilhada pode se tornar um problema. Essas partes não compartilhadas podem ser criadas diversas vezes e não serão descartadas até que o contêiner em si seja descartado. Para lidar com isso, o contêiner oferece o método `ReleaseExport`. Chamar esse método em uma exportação não compartilhada remove essa exportação do contêiner de composição e o descarta. Partes que são usadas somente pela exportação removida, e assim por diante na árvore, também são removidas e descartadas. Dessa maneira, os recursos podem ser reivindicados novamente sem descartar o contêiner de composição em si.

`IPartImportsSatisfiedNotification` contém um método chamado `OnImportsSatisfied`. Esse método é chamado pelo contêiner de composição em qualquer parte que implementar a interface quando a composição tiver sido concluída e as importações da parte estiverem prontas para uso. As partes são criadas pelo mecanismo de composição para preencher as importações de outras partes. Antes de as importações de uma parte terem sido definidas, você não pode realizar nenhuma inicialização que dependa de ou manipule valores importados no construtor da parte, a menos que esses valores tenham sido especificados como pré-requisitos usando o atributo `ImportingConstructor`. Normalmente, esse é o método preferido, mas, em alguns casos, a injeção do construtor pode não estar disponível. Na maioria dos casos, a inicialização pode ser realizada em `OnImportsSatisfied`, e a parte deve implementar `IPartImportsSatisfiedNotification`.

## <a name="see-also"></a>Confira também

- [Vídeo do Channel 9: Open Up Your Applications with the Managed Extensibility Framework (Abra seus aplicativos com o Managed Extensibility Framework)](https://channel9.msdn.com/events/TechEd/NorthAmerica/2009/DTL328)
- [Vídeo do Channel 9: Managed Extensibility Framework (MEF) 2.0 [MEF (Managed Extensibility Framework) 2.0]](https://channel9.msdn.com/posts/NET-45-Oleg-Lvovitch-and-Kevin-Ransom-Managed-Extensibility-Framework-MEF-20)
