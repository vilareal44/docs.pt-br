---
title: Como especificar um nome de elemento alternativo para um fluxo XML
description: Saiba como criar um fluxo XML com um nome de elemento alternativo, por exemplo, para serviços Web XML que exigem as mesmas informações com pequenas diferenças.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- XML serialization, overriding
- serialization, overriding
- XML stream, specifying alternate element name for
- overriding XML serialization
- classes, overriding
- overriding classes
ms.assetid: 5cc1c0b0-f94b-4525-9a41-88a582cd6668
ms.openlocfilehash: d7e482ee6e1e1a7318ab05766508537d4b87789e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84289584"
---
# <a name="how-to-specify-an-alternate-element-name-for-an-xml-stream"></a>Como especificar um nome de elemento alternativo para um fluxo XML
  
Usando o <xref:System.Xml.Serialization.XmlSerializer>, você pode gerar mais de um fluxo de XML com o mesmo conjunto de classes. Você deve querer fazer isso porque dois diferentes serviços Web XML exigem as mesmas informações básicas, com apenas poucas diferenças. Por exemplo, imagine dois serviços Web XML que processam pedidos para livros e, portanto, exigem números ISBN. Um serviço usa a marca \<ISBN> enquanto o segundo usa a marca \<BookID> . Você tem uma classe nomeada `Book` que contém um campo nomeado `ISBN`. Quando uma instância da classe `Book` é serializada, ela, por padrão, usará o nome de membro (ISBN) como o nome de elemento da marca. Para o primeiro serviço Web XML, esse é o esperado. Mas, para enviar o fluxo XML para o segundo serviço Web XML, você deverá sobrescrever a serialização para que o nome do elemento da marca seja `BookID`.  
  
## <a name="to-create-an-xml-stream-with-an-alternate-element-name"></a>Para criar um fluxo XML com um nome de elemento alternativo  
  
1. Criar uma instância da classe <xref:System.Xml.Serialization.XmlElementAttribute>.  
  
2. Defina o <xref:System.Xml.Serialization.XmlElementAttribute.ElementName%2A> do <xref:System.Xml.Serialization.XmlElementAttribute> como "BookID".  
  
3. Criar uma instância da classe <xref:System.Xml.Serialization.XmlAttributes>.  
  
4. Adicione o objeto `XmlElementAttribute` à coleção acessada por meio da propriedade <xref:System.Xml.Serialization.XmlAttributes.XmlElements%2A> de <xref:System.Xml.Serialization.XmlAttributes>.  
  
5. Criar uma instância da classe <xref:System.Xml.Serialization.XmlAttributeOverrides>.  
  
6. Adicione o `XmlAttributes` ao <xref:System.Xml.Serialization.XmlAttributeOverrides>, passando o tipo do objeto para sobrescrever e o nome do membro que está sendo sobrescrito.  
  
7. Crie uma instância da classe `XmlSerializer` com `XmlAttributeOverrides`.  
  
8. Crie uma instância da classe `Book` e serialize-a ou desserialize-a.  
  
## <a name="example"></a>Exemplo  
  
```vb  
Public Function SerializeOverride()  
    ' Creates an XmlElementAttribute with the alternate name.  
    Dim myElementAttribute As XmlElementAttribute = _  
    New XmlElementAttribute()  
    myElementAttribute.ElementName = "BookID"  
    Dim myAttributes As XmlAttributes = New XmlAttributes()  
    myAttributes.XmlElements.Add(myElementAttribute)  
    Dim myOverrides As XmlAttributeOverrides = New XmlAttributeOverrides()  
    myOverrides.Add(typeof(Book), "ISBN", myAttributes)  
    Dim mySerializer As XmlSerializer = _  
    New XmlSerializer(GetType(Book), myOverrides)  
    Dim b As Book = New Book()  
    b.ISBN = "123456789"  
    ' Creates a StreamWriter to write the XML stream to.  
    Dim writer As StreamWriter = New StreamWriter("Book.xml")  
    mySerializer.Serialize(writer, b);  
End Class  
```  
  
```csharp  
public void SerializeOverride()  
{  
    // Creates an XmlElementAttribute with the alternate name.  
    XmlElementAttribute myElementAttribute = new XmlElementAttribute();  
    myElementAttribute.ElementName = "BookID";  
    XmlAttributes myAttributes = new XmlAttributes();  
    myAttributes.XmlElements.Add(myElementAttribute);  
    XmlAttributeOverrides myOverrides = new XmlAttributeOverrides();  
    myOverrides.Add(typeof(Book), "ISBN", myAttributes);  
    XmlSerializer mySerializer =
    new XmlSerializer(typeof(Book), myOverrides)  
    Book b = new Book();  
    b.ISBN = "123456789"  
    // Creates a StreamWriter to write the XML stream to.  
    StreamWriter writer = new StreamWriter("Book.xml");  
    mySerializer.Serialize(writer, b);  
}  
```  
  
 O fluxo XML pode ter a seguinte aparência.  
  
```xml  
<Book>  
    <BookID>123456789</BookID>  
</Book>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.Serialization.XmlElementAttribute>
- <xref:System.Xml.Serialization.XmlAttributes>
- <xref:System.Xml.Serialization.XmlAttributeOverrides>
- [Serialização de XML e SOAP](xml-and-soap-serialization.md)
- <xref:System.Xml.Serialization.XmlSerializer>
- [Como serializar um objeto](how-to-serialize-an-object.md)
- [Como desserializar um objeto](how-to-deserialize-an-object.md)
