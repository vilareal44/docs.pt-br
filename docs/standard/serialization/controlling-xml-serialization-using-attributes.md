---
title: Controlando a serialização XML usando atributos
description: Os atributos podem ser usados para controlar a serialização XML de um objeto ou criar um fluxo XML alternativo do mesmo conjunto de classes.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- classes, serializing
- XML serialization, examples
- derived classes, serializing
- arrays, serializing
- XML serialization, attributes
- preventing serialization
- attributes [.NET Framework], XML serialization
- serialization, examples
- serialization, attributes
ms.assetid: 47d4c39d-30e1-4c7b-8a2e-301325390647
ms.openlocfilehash: 79c5541b4c384e91fbec8c8f1b2130887e79a252
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84289675"
---
# <a name="controlling-xml-serialization-using-attributes"></a>Controlando a serialização XML usando atributos

Os atributos podem ser usados para controlar a serialização XML de um objeto ou criar um fluxo XML alternativo do mesmo conjunto de classes. Para obter mais detalhes sobre como criar um fluxo XML alternativo, consulte [Como especificar um nome de elemento alternativo para um fluxo XML](how-to-specify-an-alternate-element-name-for-an-xml-stream.md).

> [!NOTE]
> Se o XML gerado precisar estar de acordo com a seção 5 do documento World Wide Web Consortium (W3C) intitulado [Simple Object Access Protocol (SOAP) 1,1](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/), use os atributos listados em [atributos que controlam a serialização SOAP codificada](attributes-that-control-encoded-soap-serialization.md).

Por padrão, um nome de elemento XML é determinado pela classe ou nome do membro. Em uma classe simples denominada `Book` , um campo chamado `ISBN` produzirá uma marca de elemento XML \<ISBN> , conforme mostrado no exemplo a seguir.

```vb
Public Class Book
    Public ISBN As String
End Class
' When an instance of the Book class is serialized, it might
' produce this XML:
' <ISBN>1234567890</ISBN>.
```

```csharp
public class Book
{
    public string ISBN;
}
// When an instance of the Book class is serialized, it might
// produce this XML:
// <ISBN>1234567890</ISBN>.
```

Esse comportamento padrão pode ser modificado se você quiser dar ao elemento um novo nome. O código a seguir mostra como um atributo permite isso definindo a propriedade <xref:System.Xml.Serialization.XmlElementAttribute.ElementName%2A> de um <xref:System.Xml.Serialization.XmlElementAttribute>.

```vb
Public Class TaxRates
   < XmlElement(ElementName = "TaxRate")> _
    Public ReturnTaxRate As Decimal
End Class
```

```csharp
public class TaxRates {
    [XmlElement(ElementName = "TaxRate")]
    public decimal ReturnTaxRate;
}
```

Para obter mais informações sobre atributos, consulte [Atributos](../attributes/index.md). Para obter uma lista de atributos que controlam a serialização XML, consulte [Atributos que controlam a serialização XML](attributes-that-control-xml-serialization.md).

## <a name="controlling-array-serialization"></a>Controlando a serialização de matriz

Os atributos <xref:System.Xml.Serialization.XmlArrayAttribute> e <xref:System.Xml.Serialization.XmlArrayItemAttribute> são criados para controlar a serialização de matrizes. Usando esses atributos, você pode controlar o nome do elemento, namespace e o tipo de dados do esquema XSD (como definido no documento "Parte 2 do esquema XML: tipos de dados" do World Wide Web Consortium [www.w3.org]). Você também pode especificar os tipos que podem ser incluídos em uma matriz.

O <xref:System.Xml.Serialization.XmlArrayAttribute> determinará as propriedades do elemento XML incluído que resulta quando uma matriz é serializada. Por exemplo, por padrão, serializar a matriz abaixo resultará em um elemento XML denominado `Employees`. O elemento `Employees` conterá uma série de elementos nomeados de acordo com o tipo de matriz `Employee`.

```vb
Public Class Group
    Public Employees() As Employee
End Class
Public Class Employee
    Public Name As String
End Class
```

```csharp
public class Group {
    public Employee[] Employees;
}
public class Employee {
    public string Name;
}
```

Uma instância serializada pode parecer com o seguinte.

```xml
<Group>
<Employees>
    <Employee>
        <Name>Haley</Name>
    </Employee>
</Employees>
</Group>
```

Aplicando um <xref:System.Xml.Serialization.XmlArrayAttribute>, você pode alterar o nome do elemento XML, da seguinte maneira.

```vb
Public Class Group
    <XmlArray("TeamMembers")> _
    Public Employees() As Employee
End Class
```

```csharp
public class Group {
    [XmlArray("TeamMembers")]
    public Employee[] Employees;
}
```

O XML resultante pode parecer com o seguinte.

```xml
<Group>
<TeamMembers>
    <Employee>
        <Name>Haley</Name>
    </Employee>
</TeamMembers>
</Group>
```

O <xref:System.Xml.Serialization.XmlArrayItemAttribute>, por outro lado, controla como os itens contidos na matriz são serializados. Observe que o atributo é aplicado ao campo que retorna a matriz.

```vb
Public Class Group
    <XmlArrayItem("MemberName")> _
    Public Employee() As Employees
End Class
```

```csharp
public class Group {
    [XmlArrayItem("MemberName")]
    public Employee[] Employees;
}
```

O XML resultante pode parecer com o seguinte.

```xml
<Group>
<Employees>
    <MemberName>Haley</MemberName>
</Employees>
</Group>
```

## <a name="serializing-derived-classes"></a>Serializando classes derivadas

Outro uso do <xref:System.Xml.Serialization.XmlArrayItemAttribute> é permitir a serialização de classes derivadas. Por exemplo, outra classe denominada `Manager` que é derivada de `Employee` pode ser adicionada ao exemplo anterior. Se você não aplicar o <xref:System.Xml.Serialization.XmlArrayItemAttribute>, o código falhará em tempo de execução porque o tipo da classe derivada não será reconhecido. Para corrigir isso, aplique o atributo duas vezes, cada vez que definir a propriedade <xref:System.Xml.Serialization.XmlArrayItemAttribute.Type%2A> para cada tipo aceitável (base e derivada).

```vb
Public Class Group
    <XmlArrayItem(Type:=GetType(Employee)), _
    XmlArrayItem(Type:=GetType(Manager))> _
    Public Employees() As Employee
End Class
Public Class Employee
    Public Name As String
End Class
Public Class Manager
Inherits Employee
    Public Level As Integer
End Class
```

```csharp
public class Group {
    [XmlArrayItem(Type = typeof(Employee)),
    XmlArrayItem(Type = typeof(Manager))]
    public Employee[] Employees;
}
public class Employee {
    public string Name;
}
public class Manager:Employee {
    public int Level;
}
```

Uma instância serializada pode parecer com o seguinte.

```xml
<Group>
<Employees>
    <Employee>
        <Name>Haley</Name>
    </Employee>
    <Employee xsi:type = "Manager">
        <Name>Ann</Name>
        <Level>3</Level>
    </Employee>
</Employees>
</Group>
```

## <a name="serializing-an-array-as-a-sequence-of-elements"></a>Serializando uma matriz como uma sequência de elementos

Você também pode serializar uma matriz como uma sequência plana dos elementos XML aplicando um <xref:System.Xml.Serialization.XmlElementAttribute> ao campo que retorna a matriz da seguinte maneira.

```vb
Public Class Group
    <XmlElement> _
    Public Employees() As Employee
End Class
```

```csharp
public class Group {
    [XmlElement]
    public Employee[] Employees;
}
```

Uma instância serializada pode parecer com o seguinte.

```xml
<Group>
<Employees>
    <Name>Haley</Name>
</Employees>
<Employees>
    <Name>Noriko</Name>
</Employees>
<Employees>
    <Name>Marco</Name>
</Employees>
</Group>
```

Outra maneira para diferenciar os dois fluxos XML é usar a Ferramenta de Definição de Esquema XML para gerar os arquivos de documento de esquema XSD de código compilado. (Para obter mais detalhes sobre como usar a ferramenta, consulte [a ferramenta de definição de esquema XML e a SERIALIZAÇÃO XML](the-xml-schema-definition-tool-and-xml-serialization.md).) Quando nenhum atributo é aplicado ao campo, o esquema descreve o elemento da seguinte maneira.

```xml
<xs:element minOccurs="0" maxOccurs ="1" name="Employees" type="ArrayOfEmployee" />
```

Quando o <xref:System.Xml.Serialization.XmlElementAttribute> é aplicado ao campo, o esquema resultante descreve o elemento da seguinte maneira.

```xml
<xs:element minOccurs="0" maxOccurs="unbounded" name="Employees" type="Employee" />
```

## <a name="serializing-an-arraylist"></a>Serializando um ArrayList

A classe <xref:System.Collections.ArrayList> pode conter uma coleção de vários objetos. Você pode, portanto, usar um <xref:System.Collections.ArrayList> da mesma maneira que usa uma matriz. Em vez de criar um campo que retorna uma matriz de objetos tipados, no entanto, você pode criar um campo que retorna um único <xref:System.Collections.ArrayList>. No entanto, da mesma forma que ocorre com matrizes, você deverá informar o <xref:System.Xml.Serialization.XmlSerializer> dos tipos de objetos que o <xref:System.Collections.ArrayList> contém. Para fazer isso, atribua várias instâncias do <xref:System.Xml.Serialization.XmlElementAttribute> ao campo, conforme mostrado no exemplo a seguir.

```vb
Public Class Group
    <XmlElement(Type:=GetType(Employee)), _
    XmlElement(Type:=GetType(Manager))> _
    Public Info As ArrayList
End Class
```

```csharp
public class Group {
    [XmlElement(Type = typeof(Employee)),
    XmlElement(Type = typeof(Manager))]
    public ArrayList Info;
}
```

## <a name="controlling-serialization-of-classes-using-xmlrootattribute-and-xmltypeattribute"></a>Controlando a serialização de classes usando XmlRootAttribute e XmlTypeAttribute

Há dois atributos que podem ser aplicados a uma classe (e somente a uma classe): <xref:System.Xml.Serialization.XmlRootAttribute> e <xref:System.Xml.Serialization.XmlTypeAttribute>. Esses atributos são muito semelhantes. O <xref:System.Xml.Serialization.XmlRootAttribute> pode ser aplicado somente a uma classe: a classe que, quando serializada, representa o elemento de abertura e fechamento do documento XML. Em outras palavras, o elemento raiz. O <xref:System.Xml.Serialization.XmlTypeAttribute>, por outro lado, pode ser aplicado a qualquer classe, incluindo a classe raiz.

Por exemplo, nos exemplos anteriores, a classe `Group` é a classe raiz, e todos os seus campos públicos e propriedades tornam-se elementos XML encontrados no documento XML. Portanto, pode haver apenas uma classe de raiz. Aplicando o <xref:System.Xml.Serialization.XmlRootAttribute>, você pode controlar o fluxo XML produzido pelo <xref:System.Xml.Serialization.XmlSerializer>. Por exemplo, você pode alterar o nome e o namespace do elemento.

O <xref:System.Xml.Serialization.XmlTypeAttribute> permite que você controle o esquema do XML gerado. Esse recurso é útil quando você precisa publicar o esquema por um serviço Web XML. O exemplo a seguir aplica o <xref:System.Xml.Serialization.XmlTypeAttribute> e o <xref:System.Xml.Serialization.XmlRootAttribute> à mesma classe.

```vb
<XmlRoot("NewGroupName"), _
XmlType("NewTypeName")> _
Public Class Group
    Public Employees() As Employee
End Class
```

```csharp
[XmlRoot("NewGroupName")]
[XmlType("NewTypeName")]
public class Group {
    public Employee[] Employees;
}
```

Se essa classe for criada, e a ferramenta de Definição de Esquema XML for usada para gerar seu esquema, você descobriria o seguinte XML descrevendo o `Group`.

```xml
<xs:element name="NewGroupName" type="NewTypeName" />
```

Por outro lado, se você quisesse serializar uma instância da classe, somente `NewGroupName` seria encontrado no documento XML.

```xml
<NewGroupName>
    . . .
</NewGroupName>
```

## <a name="preventing-serialization-with-the-xmlignoreattribute"></a>Evitando a serialização com o XmlIgnoreAttribute

Pode haver situações quando uma propriedade pública ou um campo não precisam ser serializados. Por exemplo, um campo ou propriedade podem ser usados para conter metadados. Nesses casos, aplicar o <xref:System.Xml.Serialization.XmlIgnoreAttribute> ao campo ou propriedade e o <xref:System.Xml.Serialization.XmlSerializer> o ignorarão.

## <a name="see-also"></a>Confira também

- [Atributos que controlam a serialização de XML](attributes-that-control-xml-serialization.md)
- [Atributos que controlam a serialização SOAP codificada](attributes-that-control-encoded-soap-serialization.md)
- [Apresentando a serialização XML](introducing-xml-serialization.md)
- [Exemplos de serialização XML](examples-of-xml-serialization.md)
- [Como especificar um nome de elemento alternativo para um fluxo XML](how-to-specify-an-alternate-element-name-for-an-xml-stream.md)
- [Como serializar um objeto](how-to-serialize-an-object.md)
- [Como desserializar um objeto](how-to-deserialize-an-object.md)
