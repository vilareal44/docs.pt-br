---
title: Consultas XPath e namespaces
description: Saiba mais sobre as consultas XPath & namespaces. As consultas XPath conhecem os namespaces em um documento XML & podem usar prefixos de namespace para qualificar nomes de atributos de & de elemento.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: ef6402be-2f8e-4be2-8d3e-a80891cdef8b
ms.openlocfilehash: e8533d372a747432201dfbc4d879ecd3fbceaf8e
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769243"
---
# <a name="xpath-queries-and-namespaces"></a>Consultas XPath e namespaces
As consultas XPath reconhecem os namespaces em um documento XML e podem usar prefixos de namespace para qualificar nomes de elementos e atributos. A qualificação de nomes de elemento e atributo com um prefixo de namespace limita os nós retornados por uma consulta XPath somente aos nós que pertencem a um namespace específico.  
  
 Por exemplo, se o `books` de prefixo for mapeado para o namespace `http://www.contoso.com/books`, a seguinte consulta XPath `/books:books/books:book` selecionará somente os elementos `book` no namespace `http://www.contoso.com/books`.  
  
## <a name="the-xmlnamespacemanager"></a>XmlNamespaceManager  
 Para usar namespaces em uma consulta XPath, um objeto derivado da interface <xref:System.Xml.IXmlNamespaceResolver> como a classe <xref:System.Xml.XmlNamespaceManager> será construído com o URI e o prefixo do namespace que serão incluídos na consulta XPath.  
  
 O objeto <xref:System.Xml.XmlNamespaceManager> pode ser usado na consulta das seguintes maneiras.  
  
- O objeto <xref:System.Xml.XmlNamespaceManager> é associado a um objeto <xref:System.Xml.XPath.XPathExpression> existente usando o método <xref:System.Xml.XPath.XPathExpression.SetContext%2A> do objeto <xref:System.Xml.XPath.XPathExpression>. Você também pode compilar um novo objeto <xref:System.Xml.XPath.XPathExpression> usando o método <xref:System.Xml.XPath.XPathExpression.Compile%2A> estático que usa uma cadeia de caracteres representando a expressão XPath e um objeto <xref:System.Xml.XmlNamespaceManager> como parâmetros, e retorna um novo objeto <xref:System.Xml.XPath.XPathExpression>.  
  
- O próprio objeto <xref:System.Xml.XmlNamespaceManager> é passado como um parâmetro para um método da classe <xref:System.Xml.XPath.XPathNavigator> de aceitação juntamente com uma cadeia de caracteres que representa a expressão XPath.  
  
 Estes são os métodos da classe <xref:System.Xml.XPath.XPathNavigator> que aceitam um objeto derivado da interface <xref:System.Xml.IXmlNamespaceResolver> como um parâmetro.  
  
- <xref:System.Xml.XPath.XPathNavigator.Evaluate%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.Select%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A>  
  
### <a name="the-default-namespace"></a>O namespace padrão  
 No documento XML a seguir, o namespace padrão com um prefixo vazio é usada para declarar o namespace `http://www.contoso.com/books`.  
  
```xml  
<books xmlns="http://www.contoso.com/books">  
    <book>  
        <title>Title</title>  
        <author>Author Name</author>  
        <price>5.50</price>  
    </book>  
</books>  
```  
  
 O XPath trata o prefixo vazio como o namespace `null`. Em outras palavras, somente os prefixos mapeados para os namespaces podem ser usados em consultas XPath. Isso significa que, se você quiser fazer uma consulta em um namespace em um documento XML, mesmo que ele seja o namespace padrão, será necessário definir um prefixo para ele.  
  
 Por exemplo, sem definir um prefixo para o documento XML acima, a consulta XPath `/books/book` não retornará nenhum resultado.  
  
 Um prefixo deve ser associado para evitar a ambiguidade durante a consulta de documentos com alguns nós que não estão em um namespace e outros que estão em um namespace padrão.  
  
 O código a seguir define um prefixo para o namespace padrão e seleciona todos os elementos `book` do namespace `http://www.contoso.com/books`.  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
Dim navigator As XPathNavigator = document.CreateNavigator()  
Dim query As XPathExpression = navigator.Compile("/books:books/books:book")  
Dim manager As XmlNamespaceManager = New XmlNamespaceManager(navigator.NameTable)  
manager.AddNamespace("books", "http://www.contoso.com/books")  
query.SetContext(manager)  
Dim nodes As XPathNodeIterator = navigator.Select(query)  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
XPathNavigator navigator = document.CreateNavigator();  
XPathExpression query = navigator.Compile("/books:books/books:book");  
XmlNamespaceManager manager = new XmlNamespaceManager(navigator.NameTable);  
manager.AddNamespace("books", "http://www.contoso.com/books");  
query.SetContext(manager);  
XPathNodeIterator nodes = navigator.Select(query);  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Selecionar dados XML usando XPathNavigator](select-xml-data-using-xpathnavigator.md)
- [Avalia as expressões XPath usando XPathNavigator](evaluate-xpath-expressions-using-xpathnavigator.md)
- [Nós compatíveis usando XPathNavigator](matching-nodes-using-xpathnavigator.md)
- [Tipos de nós reconhecidos com consultas XPath](node-types-recognized-with-xpath-queries.md)
- [Expressões XPath compilados](compiled-xpath-expressions.md)
