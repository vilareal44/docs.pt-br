---
title: Extrair dados XML usando XPathNavigator
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 095b0987-ee4b-4595-a160-da1c956ad576
ms.openlocfilehash: e639931204a416c3cde87044730364a4f387799a
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287760"
---
# <a name="extract-xml-data-using-xpathnavigator"></a>Extrair dados XML usando XPathNavigator
Há várias maneiras diferentes de representar um documento XML no Microsoft .NET Framework. Isso inclui usar um <xref:System.String> ou usar as classes <xref:System.Xml.XmlReader>, <xref:System.Xml.XmlWriter>, <xref:System.Xml.XmlDocument> ou <xref:System.Xml.XPath.XPathDocument>. Para facilitar a movimentação entre essas diferentes representações de um documento XML, a classe <xref:System.Xml.XPath.XPathNavigator> fornece alguns métodos e propriedades para extrair XML como um objeto <xref:System.String>, <xref:System.Xml.XmlReader> ou <xref:System.Xml.XmlWriter>.  
  
## <a name="convert-an-xpathnavigator-to-a-string"></a>Converter um XPathNavigator em uma cadeia de caracteres  
 A propriedade <xref:System.Xml.XPath.XPathNavigator.OuterXml%2A> da classe <xref:System.Xml.XPath.XPathNavigator> é usada para obter a marcação do documento XML inteiro ou apenas a marcação de um único nó e seus nós filho.  
  
> [!NOTE]
> A propriedade <xref:System.Xml.XPath.XPathNavigator.InnerXml%2A> obtém a marcação de apenas os nós filho de um nó.  
  
 O exemplo de código a seguir mostra como salvar um documento XML inteiro contido em um objeto <xref:System.Xml.XPath.XPathNavigator> como um <xref:System.String>, bem como um único nó e seus nós filho.  
  
```vb  
Dim document As XPathDocument = New XPathDocument("input.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Save the entire input.xml document to a string.  
Dim xml As String = navigator.OuterXml  
  
' Now save the Root element and its child nodes to a string.  
navigator.MoveToChild(XPathNodeType.Element)  
Dim root As String = navigator.OuterXml  
```  
  
```csharp  
XPathDocument document = new XPathDocument("input.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Save the entire input.xml document to a string.  
string xml = navigator.OuterXml;  
  
// Now save the Root element and its child nodes to a string.  
navigator.MoveToChild(XPathNodeType.Element);  
string root = navigator.OuterXml;  
```  
  
## <a name="convert-an-xpathnavigator-to-an-xmlreader"></a>Converter um XPathNavigator em um XmlReader  
 O método <xref:System.Xml.XPath.XPathNavigator.ReadSubtree%2A> é usado para transmitir o conteúdo inteiro de um documento XML ou apenas um único nó e seus nós filho para um objeto <xref:System.Xml.XmlReader>.  
  
 Quando o objeto <xref:System.Xml.XmlReader> é criado com o nó atual e seus nós filho, a propriedade <xref:System.Xml.XmlReader> do objeto <xref:System.Xml.XmlReader.ReadState%2A> é definida como <xref:System.Xml.ReadState.Initial>. Quando o método <xref:System.Xml.XmlReader> do objeto <xref:System.Xml.XmlReader.Read%2A> é chamado pela primeira vez, o <xref:System.Xml.XmlReader> é movido para o nó atual <xref:System.Xml.XPath.XPathNavigator>. O novo objeto <xref:System.Xml.XmlReader> continua a leitura até o final da árvore XML. Neste ponto, o método <xref:System.Xml.XmlReader.Read%2A> retorna `false` e a propriedade <xref:System.Xml.XmlReader> do objeto <xref:System.Xml.XmlReader.ReadState%2A> é definida como <xref:System.Xml.ReadState.EndOfFile>.  
  
 A posição do objeto <xref:System.Xml.XPath.XPathNavigator> é inalterada pela criação ou movimento do objeto <xref:System.Xml.XmlReader>. O método <xref:System.Xml.XPath.XPathNavigator.ReadSubtree%2A> é válido somente quando posicionado em um elemento ou um nó raiz.  
  
 O exemplo a seguir mostra como obter um objeto <xref:System.Xml.XmlReader> que contém o documento XML inteiro em um objeto <xref:System.Xml.XPath.XPathDocument> assim como um único nó e seus nós filho.  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Stream the entire XML document to the XmlReader.  
Dim xml As XmlReader = navigator.ReadSubtree()  
  
While xml.Read()  
    Console.WriteLine(xml.ReadInnerXml())  
End While  
  
xml.Close()  
  
' Stream the book element and its child nodes to the XmlReader.  
navigator.MoveToChild("bookstore", "")  
navigator.MoveToChild("book", "")  
  
Dim book As XmlReader = navigator.ReadSubtree()  
  
While book.Read()  
    Console.WriteLine(book.ReadInnerXml())  
End While  
  
book.Close()  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Stream the entire XML document to the XmlReader.  
XmlReader xml = navigator.ReadSubtree();  
  
while (xml.Read())  
{  
    Console.WriteLine(xml.ReadInnerXml());  
}  
  
xml.Close();  
  
// Stream the book element and its child nodes to the XmlReader.  
navigator.MoveToChild("bookstore", "");  
navigator.MoveToChild("book", "");  
  
XmlReader book = navigator.ReadSubtree();  
  
while (book.Read())  
{  
    Console.WriteLine(book.ReadInnerXml());  
}  
  
book.Close();  
```  
  
 O exemplo usa o arquivo `books.xml` como entrada.  
  
 [!code-xml[XPathXMLExamples#1](../../../../samples/snippets/xml/VS_Snippets_Data/XPathXMLExamples/XML/books.xml#1)]  
  
## <a name="converting-an-xpathnavigator-to-an-xmlwriter"></a>Convertendo um XPathNavigator em um XmlWriter  
 O método <xref:System.Xml.XPath.XPathNavigator.WriteSubtree%2A> é usado para transmitir o conteúdo inteiro de um documento XML ou apenas um único nó e seus nós filho para um objeto <xref:System.Xml.XmlWriter>.  
  
 A posição do objeto <xref:System.Xml.XPath.XPathNavigator> é inalterada pela criação do objeto <xref:System.Xml.XmlWriter>.  
  
 O exemplo a seguir mostra como obter um objeto <xref:System.Xml.XmlWriter> que contém o documento XML inteiro em um objeto <xref:System.Xml.XPath.XPathDocument> assim como um único nó e seus nós filho.  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
  
' Stream the entire XML document to the XmlWriter.  
Dim xml As XmlWriter = XmlWriter.Create("newbooks.xml")  
navigator.WriteSubtree(xml)  
xml.Close()  
  
' Stream the book element and its child nodes to the XmlWriter.  
navigator.MoveToChild("bookstore", "")  
navigator.MoveToChild("book", "")  
  
Dim book As XmlWriter = XmlWriter.Create("book.xml")  
navigator.WriteSubtree(book)  
book.Close()  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
  
// Stream the entire XML document to the XmlWriter.  
XmlWriter xml = XmlWriter.Create("newbooks.xml");  
navigator.WriteSubtree(xml);  
xml.Close();  
  
// Stream the book element and its child nodes to the XmlWriter.  
navigator.MoveToChild("bookstore", "");  
navigator.MoveToChild("book", "");  
  
XmlWriter book = XmlWriter.Create("book.xml");  
navigator.WriteSubtree(book);  
book.Close();  
```  
  
 O exemplo usa o arquivo `books.xml` localizado anteriormente neste tópico como entrada.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Navegação do nó usando XPathNavigator](node-set-navigation-using-xpathnavigator.md)
- [Navegação do nó de atributo e do namespace usando XPathNavigator](attribute-and-namespace-node-navigation-using-xpathnavigator.md)
- [Acessando dados fortemente tipados XML usando XPathNavigator](accessing-strongly-typed-xml-data-using-xpathnavigator.md)
