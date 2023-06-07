---
title: Programação connosco
ms.date: 07/20/2015
ms.assetid: d8422a9b-dd37-44a3-8aac-2237ed9561e0
ms.openlocfilehash: 4cae85d99e7e28506faad8ca39347cf8f271718e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396370"
---
# <a name="programming-with-nodes-visual-basic"></a>Programando com nós (Visual Basic)
desenvolvedores de[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] que precisam geralmente escrever programas como um editor XML, um sistema uma transformação, ou uma necessidade o gravador de relatório escrever programas que funcionam no nível mais fino de granularidade dos elementos e atributos. Freqüentemente necessitam de trabalhar no nível de nó, em nós de manipulação de texto, as instruções de processamento, e os comentários. Este tópico fornece alguns detalhes sobre programação no nível do nó.  
  
## <a name="node-details"></a>Detalhes do nó  
 Há um número de detalhes de programação que um programador que funciona a nível do nó deve conhecer.  
  
### <a name="parent-property-of-children-nodes-of-xdocument-is-set-to-null"></a>A propriedade pai de nós de filhos de XDocument é definida como nula  
 A propriedade de <xref:System.Xml.Linq.XObject.Parent%2A> contém <xref:System.Xml.Linq.XElement>pai, não o nó pai. Os nós filho de <xref:System.Xml.Linq.XDocument> não têm nenhum <xref:System.Xml.Linq.XElement>pai. Seu pai é o documento, assim que a propriedade de <xref:System.Xml.Linq.XObject.Parent%2A> para os nós é definida como nula.  
  
 O exemplo a seguir demonstra este:  
  
```vb  
Dim doc As XDocument = XDocument.Parse("<!-- a comment --><Root/>")  
Console.WriteLine(doc.Nodes().OfType(Of XComment).First().Parent Is Nothing)  
Console.WriteLine(doc.Root.Parent Is Nothing)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```console  
True  
True  
```  
  
### <a name="adjacent-text-nodes-are-possible"></a>Nós adjacentes de texto são possíveis  
 Em um número XML que programa modelos, nós adjacentes de texto sempre são mesclados. Isso é às vezes chamado normalização de nós de texto. [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] não normalizará nós de texto. Se você adicionar dois nós de texto ao mesmo elemento, resultará a nós adjacentes de texto. Entretanto, se você adicionar o conteúdo especificado como uma cadeia de caracteres em vez de como um nó de <xref:System.Xml.Linq.XText>, [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] poderá mesclar a cadeia de caracteres com um nó de texto adjacente.  
  
 O exemplo a seguir demonstra este:  
  
```vb  
Dim xmlTree As XElement = <Root>Content</Root>  
Console.WriteLine(xmlTree.Nodes().OfType(Of XText)().Count())  
  
' This does not add a new text node.  
xmlTree.Add("new content")  
Console.WriteLine(xmlTree.Nodes().OfType(Of XText)().Count())  
  
'// This does add a new, adjacent text node.  
xmlTree.Add(New XText("more text"))  
Console.WriteLine(xmlTree.Nodes().OfType(Of XText)().Count())  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```console  
1  
1  
2  
```  
  
### <a name="empty-text-nodes-are-possible"></a>Os nós vazios de texto são possíveis  
 Em algum XML que programa modelos, os nós de texto são garantidos para não conter a cadeia de caracteres vazia. O raciocínio é que um nó de texto não tem impacto na serialização XML. No entanto, pela mesma razão nós adjacentes do texto são possíveis, se você remover o texto de um nó de texto definindo seu valor para a cadeia de caracteres vazia, o nó de texto em si não serão excluídos.  
  
```vb  
Dim xmlTree As XElement = <Root>Content</Root>  
Dim textNode As XText = xmlTree.Nodes().OfType(Of XText)().First()  
  
' The following line does not cause the removal of the text node.  
textNode.Value = ""  
  
Dim textNode2 As XText = xmlTree.Nodes().OfType(Of XText)().First()  
Console.WriteLine(">>{0}<<", textNode2)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```console  
>><<  
```  
  
### <a name="an-empty-text-node-impacts-serialization"></a>Um nó de texto vazio afeta a serialização  
 Se um elemento contém apenas um nó filho do texto que está vazio, é serializado com longa sintaxe de marca: `<Child></Child>`. Se um elemento não contém nenhum nó filho, qualquer é serializado pela sintaxe abreviada de marca: `<Child />`.  
  
```vb  
Dim child1 As XElement = New XElement("Child1", _  
    New XText("") _  
)  
Dim child2 As XElement = New XElement("Child2")  
Console.WriteLine(child1)  
Console.WriteLine(child2)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Child1></Child1>  
<Child2 />  
```  
  
### <a name="namespaces-are-attributes-in-the-linq-to-xml-tree"></a>Namespaces são atributos na árvore LINQ to XML  
 Mesmo que as declarações namespace tenham a sintaxe idêntica a atributos, em algumas interfaces de programação, como XSLT e o XPath, as declarações namespace não são consideradas como atributos. No entanto, em [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], namespaces são armazenadas como objetos de <xref:System.Xml.Linq.XAttribute> na árvore XML. Se você itera através de atributos para um elemento que contém uma declaração de namespace, você verá a declaração de namespace como um dos itens na coleção retornada.  
  
 A propriedade de <xref:System.Xml.Linq.XAttribute.IsNamespaceDeclaration%2A> indica se um atributo é uma declaração de namespace.  
  
```vb  
Dim root As XElement = _
<Root  
    xmlns='http://www.adventure-works.com'  
    xmlns:fc='www.fourthcoffee.com'  
    AnAttribute='abc'/>  
For Each att As XAttribute In root.Attributes()  
    Console.WriteLine("{0}  IsNamespaceDeclaration:{1}", att, _  
                      att.IsNamespaceDeclaration)  
Next  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```console  
xmlns="http://www.adventure-works.com"  IsNamespaceDeclaration:True  
xmlns:fc="www.fourthcoffee.com"  IsNamespaceDeclaration:True  
AnAttribute="abc"  IsNamespaceDeclaration:False  
```  
  
### <a name="xpath-axis-methods-do-not-return-child-white-space-of-xdocument"></a>Os métodos do eixo XPath retornam espaço em branco filho de XDocument  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] permite nós filhos de texto de <xref:System.Xml.Linq.XDocument>, desde que os nós de texto contém somente espaço em branco. No entanto, o modelo de objeto XPath não inclui espaço em branco como nós filho de um documento, para que quando você itera através de filhos de <xref:System.Xml.Linq.XDocument> usando o eixo de <xref:System.Xml.Linq.XContainer.Nodes%2A> , os nós de texto de espaço em branco serão retornados. No entanto, quando você itera através de filhos de <xref:System.Xml.Linq.XDocument> usando os métodos do eixo XPath, os nós de texto de espaço em branco não serão retornados.  
  
```vb  
' Create a document with some white space child nodes of the document.  
Dim root As XDocument = XDocument.Parse( _  
"<?xml version='1.0' encoding='utf-8' standalone='yes'?>" & _  
vbNewLine & "<Root/>" & vbNewLine & "<!--a comment-->" & vbNewLine, _  
LoadOptions.PreserveWhitespace)  
  
' Count the white space child nodes using LINQ to XML.  
Console.WriteLine(root.Nodes().OfType(Of XText)().Count())  
  
' Count the white space child nodes using XPathEvaluate.  
Dim nodes As IEnumerable = CType(root.XPathEvaluate("text()"), IEnumerable)  
Console.WriteLine(nodes.OfType(Of XText)().Count())  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```console  
3  
0  
```  
  
### <a name="xdeclaration-objects-are-not-nodes"></a>Os objetos de XDeclaration não são nós  
 Quando você itera através de nós de filhos de <xref:System.Xml.Linq.XDocument>, você não verá o objeto da declaração XML. É uma propriedade do documento, não um nó filho deles.  
  
```vb  
Dim doc As XDocument = _  
<?xml version='1.0' encoding='utf-8' standalone='yes'?>  
<Root/>  
  
doc.Save("Temp.xml")  
Console.WriteLine(File.ReadAllText("Temp.xml"))  
  
' This shows that there is only one child node of the document.  
Console.WriteLine(doc.Nodes().Count())  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<?xml version="1.0" encoding="utf-8" standalone="yes"?>  
<Root />
1
```  
  
## <a name="see-also"></a>Confira também

- [Programação de LINQ to XML avançada (Visual Basic)](advanced-linq-to-xml-programming.md)
