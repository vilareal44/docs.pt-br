---
title: Acessando atributos no DOM
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: ce2df341-a1a4-4e97-8e1b-cd45b8e3e71e
ms.openlocfilehash: a77780621032e2ce59b9db04a179c7086588219b
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291637"
---
# <a name="accessing-attributes-in-the-dom"></a>Acessando atributos no DOM

Os atributos são propriedades do elemento, não filhos do elemento. Essa distinção é importante por causa dos métodos usados para navegar por nós irmãos, pai e filho do DOM (Document Object Model) XML. Por exemplo, os métodos **PreviousSibling** e **NextSibling** não são usados para navegar de um elemento para um atributo ou entre atributos. Em vez disso, um atributo é uma propriedade de um elemento e pertence a um elemento, possui uma propriedade **OwnerElement**, não uma propriedade **parentNode**, e possui métodos distintos de navegação.

Quando o nó atual for um elemento, use o método **HasAttribute** para verificar se há algum atributo associado ao elemento. Quando se sabe que um elemento tem atributos, há vários métodos para acessar atributos. Para recuperar um único atributo do elemento, use os métodos **GetAttribute** e **GetAttributeNode** do **XmlElement** ou obtenha todos os atributos de uma coleção. Obter a coleção é útil se você precisar iterar sobre a coleção. Se desejar todos os atributos do elemento, use a propriedade **Attributes** do elemento para recuperar todos os atributos de uma coleção.

## <a name="retrieving-all-attributes-into-a-collection"></a>Recuperando todos os atributos em uma coleção

Se desejar que todos os atributos de um nó do elemento sejam colocados em uma coleção, chame a propriedade **XmlElement.Attributes**. Isso obtém o **XmlAttributeCollection**, que contém todos os atributos de um elemento. A classe **XmlAttributeCollection** herda do mapa **XmlNamedNode**. Portanto, os métodos e as propriedades disponíveis na coleção incluem os que estão disponíveis em um mapa de nó nomeado além dos métodos e das propriedades específicos à classe **XmlAttributeCollection**, como a propriedade **ItemOf** ou o método **Append**. Cada item da coleção de atributo representa um nó **XmlAttribute**. Para localizar o número de atributos em um elemento, obtenha o **XmlAttributeCollection** e use a propriedade **Count** para verificar quantos nós **XmlAttribute** estão na coleção.

O exemplo de código a seguir mostra como recuperar uma coleção de atributos e, usando o método **Count** para o índice de loop, iterar sobre ela. Em seguida, o código mostra como recuperar um único atributo da coleção e exibir seu valor.

```vb
Imports System.IO
Imports System.Xml

Public Class Sample

    Public Shared Sub Main()

        Dim doc As XmlDocument = New XmlDocument()
        doc.LoadXml("<book genre='novel' ISBN='1-861001-57-5' misc='sale item'>" & _
               "<title>The Handmaid's Tale</title>" & _
               "<price>14.95</price>" & _
               "</book>")

        ' Move to an element.
        Dim myElement As XmlElement = doc.DocumentElement

        ' Create an attribute collection from the element.
        Dim attrColl As XmlAttributeCollection = myElement.Attributes

        ' Show the collection by iterating over it.
        Console.WriteLine("Display all the attributes in the collection...")
        Dim i As Integer
        For i = 0 To attrColl.Count - 1
            Console.Write("{0} = ", attrColl.ItemOf(i).Name)
            Console.Write("{0}", attrColl.ItemOf(i).Value)
            Console.WriteLine()
        Next

        ' Retrieve a single attribute from the collection; specifically, the
        ' attribute with the name "misc".
        Dim attr As XmlAttribute = attrColl("misc")

        ' Retrieve the value from that attribute.
        Dim miscValue As String = attr.InnerXml

        Console.WriteLine("Display the attribute information.")
        Console.WriteLine(miscValue)

    End Sub
End Class
```

```csharp
using System;
using System.IO;
using System.Xml;

public class Sample
{

    public static void Main()
    {
        XmlDocument doc = new XmlDocument();
        doc.LoadXml("<book genre='novel' ISBN='1-861001-57-5' misc='sale item'>" +
                      "<title>The Handmaid's Tale</title>" +
                      "<price>14.95</price>" +
                      "</book>");

        // Move to an element.
        XmlElement myElement = doc.DocumentElement;

        // Create an attribute collection from the element.
        XmlAttributeCollection attrColl = myElement.Attributes;

        // Show the collection by iterating over it.
        Console.WriteLine("Display all the attributes in the collection...");
        for (int i = 0; i < attrColl.Count; i++)
        {
            Console.Write("{0} = ", attrColl[i].Name);
            Console.Write("{0}", attrColl[i].Value);
            Console.WriteLine();
        }

        // Retrieve a single attribute from the collection; specifically, the
        // attribute with the name "misc".
        XmlAttribute attr = attrColl["misc"];

        // Retrieve the value from that attribute.
        String miscValue = attr.InnerXml;

        Console.WriteLine("Display the attribute information.");
        Console.WriteLine(miscValue);

    }
}
```

Esse exemplo exibe a seguinte saída:

**Saída**

Display all the attributes in the collection.

```console
genre = novel
ISBN = 1-861001-57-5
misc = sale item
Display the attribute information.
sale item
```

As informações em uma coleção de atributos podem ser recuperadas por nome ou número de índice. O exemplo acima mostra como recuperar dados por nome. O próximo exemplo mostra como recuperar dados por número de índice.

Como **XmlAttributeCollection** é uma coleção e pode ser iterada por nome ou índice, este exemplo mostra como selecionar o primeiro atributo da coleção usando um índice com base em zero e usando o seguinte arquivo, **baseuri.xml**, como entrada.

### <a name="input"></a>Entrada

```xml
<!-- XML fragment -->
<book genre="novel">
  <title>Pride And Prejudice</title>
</book>
```

```vb
Option Explicit On
Option Strict On

Imports System.IO
Imports System.Xml

Public Class Sample

    Public Shared Sub Main()
        ' Create the XmlDocument.
        Dim doc As New XmlDocument()
        doc.Load("http://localhost/baseuri.xml")

        ' Display information on the attribute node. The value
        ' returned for BaseURI is 'http://localhost/baseuri.xml'.
        Dim attr As XmlAttribute = doc.DocumentElement.Attributes(0)
        Console.WriteLine("Name of the attribute:  {0}", attr.Name)
        Console.WriteLine("Base URI of the attribute:  {0}", attr.BaseURI)
        Console.WriteLine("The value of the attribute:  {0}", attr.InnerText)
    End Sub 'Main
End Class 'Sample
```

```csharp
using System;
using System.IO;
using System.Xml;

public class Sample
{
  public static void Main()
  {
    // Create the XmlDocument.
    XmlDocument doc = new XmlDocument();

    doc.Load("http://localhost/baseuri.xml");

    // Display information on the attribute node. The value
    // returned for BaseURI is 'http://localhost/baseuri.xml'.
    XmlAttribute attr = doc.DocumentElement.Attributes[0];
    Console.WriteLine("Name of the attribute:  {0}", attr.Name);
    Console.WriteLine("Base URI of the attribute:  {0}", attr.BaseURI);
    Console.WriteLine("The value of the attribute:  {0}", attr.InnerText);
  }
}
```

## <a name="retrieving-an-individual-attribute-node"></a>Recuperando um único nó de atributo

Para recuperar um único nó de atributo de um elemento, o método <xref:System.Xml.XmlElement.GetAttributeNode%2A?displayProperty=nameWithType> é usado. Retorna um objeto do tipo **XmlAttribute**. Quando você tem um **XmlAttribute**, todos os métodos e propriedades disponíveis na classe <xref:System.Xml.XmlAttribute?displayProperty=nameWithType> estão disponíveis naquele objeto, como localizar o **OwnerElement**.

```vb
Imports System.IO
Imports System.Xml

Public Class Sample

    Public Shared Sub Main()

        Dim doc As XmlDocument = New XmlDocument()
        doc.LoadXml("<book genre='novel' ISBN='1-861001-57-5' misc='sale item'>" & _
               "<title>The Handmaid's Tale</title>" & _
               "<price>14.95</price>" & _
               "</book>")

        ' Move to an element.
        Dim root As XmlElement
        root = doc.DocumentElement

        ' Get an attribute.
        Dim attr As XmlAttribute
        attr = root.GetAttributeNode("ISBN")

        ' Display the value of the attribute.
        Dim attrValue As String
        attrValue = attr.InnerXml
        Console.WriteLine(attrValue)

    End Sub
End Class
```

```csharp
using System;
using System.IO;
using System.Xml;

 public class Sample
 {
      public static void Main()
      {
    XmlDocument doc = new XmlDocument();
     doc.LoadXml("<book genre='novel' ISBN='1-861003-78' misc='sale item'>" +
                   "<title>The Handmaid's Tale</title>" +
                   "<price>14.95</price>" +
                   "</book>");

    // Move to an element.
     XmlElement root = doc.DocumentElement;

    // Get an attribute.
     XmlAttribute attr = root.GetAttributeNode("ISBN");

    // Display the value of the attribute.
     String attrValue = attr.InnerXml;
     Console.WriteLine(attrValue);

    }
}
```

Você também pode fazer como mostrado no exemplo anterior, onde um único nó de atributo é recuperado da coleção de atributos. O exemplo de código a seguir mostra como uma linha de código pode ser escrita para recuperar um único atributo pelo número de índice da raiz da árvore de documentos XML, também conhecida como a propriedade **DocumentElement**.

```csharp
XmlAttribute attr = doc.DocumentElement.Attributes[0];
```

## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
