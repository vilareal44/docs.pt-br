---
title: Como qualificar o elemento XML e os nomes de atributo XML
description: Este artigo mostra como qualificar os nomes de elementos XML e atributos XML em documentos XML.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- qualifying XML names
- qualifying XML elements
- XML namespaces, qualifying elements and names in
ms.assetid: 44719f90-7e15-42e8-a9e2-282287e2b5bf
ms.openlocfilehash: 6c29e03d9ce28e5b0abc68a5d7e8d82f4485ac93
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "83378409"
---
# <a name="how-to-qualify-xml-element-and-xml-attribute-names"></a>Como qualificar o elemento XML e os nomes de atributo XML

Os namespaces XML contidos por instâncias da <xref:System.Xml.Serialization.XmlSerializerNamespaces> classe devem estar em conformidade com a especificação do World Wide Web Consortium (W3C) chamada [namespaces em XML](https://www.w3.org/TR/REC-xml-names/).

Os namespaces em XML fornecem um método para qualificar os nomes de elementos XML e atributos XML em documentos XML. Um nome qualificado é composto por um prefixo e por um nome local separados por dois-pontos. O prefixo funciona somente como espaço reservado; é mapeado para um URI que especifica um namespace. A combinação do namespace de URI gerenciado universalmente e o nome local produz um nome que é garantido para ser exclusivo universalmente.

Ao criar uma instância do `XmlSerializerNamespaces` e adicionar os pares de namespace ao objeto, você pode especificar os prefixos usados em um documento XML.

## <a name="to-create-qualified-names-in-an-xml-document"></a>Para criar nomes qualificados em um documento XML

1. Criar uma instância da classe `XmlSerializerNamespaces`.

2. Adicione todos os prefixos e pares de namespace ao `XmlSerializerNamespaces`.

3. Aplique o atributo `System.Xml.Serialization` apropriado a cada membro ou classe que o <xref:System.Xml.Serialization.XmlSerializer> deve serializar em um documento XML.

    Os atributos disponíveis são: <xref:System.Xml.Serialization.XmlAnyElementAttribute>, <xref:System.Xml.Serialization.XmlArrayAttribute>, <xref:System.Xml.Serialization.XmlArrayItemAttribute>, <xref:System.Xml.Serialization.XmlAttributeAttribute>, <xref:System.Xml.Serialization.XmlElementAttribute>, <xref:System.Xml.Serialization.XmlRootAttribute> e <xref:System.Xml.Serialization.XmlTypeAttribute>.

4. Configure a propriedade `Namespace` de cada atributo para um dos valores de namespace do `XmlSerializerNamespaces`.

5. Passe o `XmlSerializerNamespaces` para o método `Serialize` do `XmlSerializer`.

## <a name="example"></a>Exemplo

O exemplo a seguir cria um `XmlSerializerNamespaces` e adiciona dois prefixos e pares de namespace ao objeto. O código cria um `XmlSerializer` que é usado para serializar uma instância da classe `Books`. O código chama o método `Serialize` com o `XmlSerializerNamespaces`, permitindo que o XML contenha namespaces com prefixo.

```vb
Imports System.IO
Imports System.Xml
Imports System.Xml.Serialization

Public Module Program

    Public Sub Main()
        SerializeObject("XmlNamespaces.xml")
    End Sub

    Public Sub SerializeObject(filename As String)
        Dim mySerializer As New XmlSerializer(GetType(Books))
        ' Writing a file requires a TextWriter.
        Dim myWriter As New StreamWriter(filename)

        ' Creates an XmlSerializerNamespaces and adds two
        ' prefix-namespace pairs.
        Dim myNamespaces As New XmlSerializerNamespaces()
        myNamespaces.Add("books", "http://www.cpandl.com")
        myNamespaces.Add("money", "http://www.cohowinery.com")

        ' Creates a Book.
        Dim myBook As New Book()
        myBook.TITLE = "A Book Title"
        Dim myPrice As New Price()
        myPrice.price = CDec(9.95)
        myPrice.currency = "US Dollar"
        myBook.PRICE = myPrice
        Dim myBooks As New Books()
        myBooks.Book = myBook
        mySerializer.Serialize(myWriter, myBooks, myNamespaces)
        myWriter.Close()
    End Sub
End Module

Public Class Books
    <XmlElement([Namespace] := "http://www.cohowinery.com")> _
    Public Book As Book
End Class

<XmlType([Namespace] := "http://www.cpandl.com")> _
Public Class Book
    <XmlElement([Namespace] := "http://www.cpandl.com")> _
    Public TITLE As String
    <XmlElement([Namespace] := "http://www.cohowinery.com")> _
    Public PRICE As Price
End Class

Public Class Price
    <XmlAttribute([Namespace] := "http://www.cpandl.com")> _
    Public currency As String
    <XmlElement([Namespace] := "http://www.cohowinery.com")> _
    Public price As Decimal
End Class
```

```csharp
using System;
using System.IO;
using System.Xml;
using System.Xml.Serialization;

public class Program
{
    public static void Main()
    {
        SerializeObject("XmlNamespaces.xml");
    }

    public static void SerializeObject(string filename)
    {
        var mySerializer = new XmlSerializer(typeof(Books));
        // Writing a file requires a TextWriter.
        TextWriter myWriter = new StreamWriter(filename);

        // Creates an XmlSerializerNamespaces and adds two
        // prefix-namespace pairs.
        var myNamespaces = new XmlSerializerNamespaces();
        myNamespaces.Add("books", "http://www.cpandl.com");
        myNamespaces.Add("money", "http://www.cohowinery.com");

        // Creates a Book.
        var myBook = new Book();
        myBook.TITLE = "A Book Title";
        var myPrice = new Price();
        myPrice.price = (decimal) 9.95;
        myPrice.currency = "US Dollar";
        myBook.PRICE = myPrice;
        var myBooks = new Books();
        myBooks.Book = myBook;
        mySerializer.Serialize(myWriter, myBooks, myNamespaces);
        myWriter.Close();
    }
}

public class Books
{
    [XmlElement(Namespace = "http://www.cohowinery.com")]
    public Book Book;
}

[XmlType(Namespace ="http://www.cpandl.com")]
public class Book
{
    [XmlElement(Namespace = "http://www.cpandl.com")]
    public string TITLE;
    [XmlElement(Namespace ="http://www.cohowinery.com")]
    public Price PRICE;
}

public class Price
{
    [XmlAttribute(Namespace = "http://www.cpandl.com")]
    public string currency;
    [XmlElement(Namespace = "http://www.cohowinery.com")]
    public decimal price;
}
```

## <a name="see-also"></a>Confira também

- <xref:System.Xml.Serialization.XmlSerializer>
- [A ferramenta de definição de esquema XML e a serialização XML](the-xml-schema-definition-tool-and-xml-serialization.md)
- [Apresentando a serialização XML](introducing-xml-serialization.md)
- [Classe XmlSerializer](xref:System.Xml.Serialization.XmlSerializer)
- [Atributos que controlam a serialização de XML](attributes-that-control-xml-serialization.md)
- [Como especificar um nome de elemento alternativo para um fluxo XML](how-to-specify-an-alternate-element-name-for-an-xml-stream.md)
- [Como serializar um objeto](how-to-serialize-an-object.md)
- [Como desserializar um objeto](how-to-deserialize-an-object.md)
