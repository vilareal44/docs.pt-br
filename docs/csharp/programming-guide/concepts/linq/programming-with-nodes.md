---
title: Programando com nós (C#)
description: Saiba mais sobre a programação com nós. Isso permite aos desenvolvedores escrever programas que funcionam em um nível mais preciso de detalhes do que elementos e atributos.
ms.date: 07/20/2015
ms.assetid: c38df0f2-c805-431a-93ff-9103a4284c2f
ms.openlocfilehash: 8a31d6459ab644a682d480239cabc3d88fd7dc14
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301678"
---
# <a name="programming-with-nodes-c"></a>Programando com nós (C#)
desenvolvedores de[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] que precisam geralmente escrever programas como um editor XML, um sistema uma transformação, ou uma necessidade o gravador de relatório escrever programas que funcionam no nível mais fino de granularidade dos elementos e atributos. Freqüentemente necessitam de trabalhar no nível de nó, em nós de manipulação de texto, as instruções de processamento, e os comentários. Este tópico fornece alguns detalhes sobre programação no nível do nó.  
  
## <a name="node-details"></a>Detalhes do nó  
 Há um número de detalhes de programação que um programador que funciona a nível do nó deve conhecer.  
  
### <a name="parent-property-of-children-nodes-of-xdocument-is-set-to-null"></a>A propriedade pai de nós de filhos de XDocument é definida como nula  
 A propriedade de <xref:System.Xml.Linq.XObject.Parent%2A> contém <xref:System.Xml.Linq.XElement>pai, não o nó pai. Os nós filho de <xref:System.Xml.Linq.XDocument> não têm nenhum <xref:System.Xml.Linq.XElement>pai. Seu pai é o documento, assim que a propriedade de <xref:System.Xml.Linq.XObject.Parent%2A> para os nós é definida como nula.  
  
 O exemplo a seguir demonstra este:  
  
```csharp  
XDocument doc = XDocument.Parse(@"<!-- a comment --><Root/>");  
Console.WriteLine(doc.Nodes().OfType<XComment>().First().Parent == null);  
Console.WriteLine(doc.Root.Parent == null);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
True  
True  
```  
  
### <a name="adjacent-text-nodes-are-possible"></a>Nós adjacentes de texto são possíveis  
 Em um número XML que programa modelos, nós adjacentes de texto sempre são mesclados. Isso é às vezes chamado normalização de nós de texto. [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] não normalizará nós de texto. Se você adicionar dois nós de texto ao mesmo elemento, resultará a nós adjacentes de texto. Entretanto, se você adicionar o conteúdo especificado como uma cadeia de caracteres em vez de como um nó de <xref:System.Xml.Linq.XText>, [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] poderá mesclar a cadeia de caracteres com um nó de texto adjacente.  
  
 O exemplo a seguir demonstra este:  
  
```csharp  
XElement xmlTree = new XElement("Root", "Content");  
  
Console.WriteLine(xmlTree.Nodes().OfType<XText>().Count());  
  
// this does not add a new text node  
xmlTree.Add("new content");  
Console.WriteLine(xmlTree.Nodes().OfType<XText>().Count());  
  
// this does add a new, adjacent text node  
xmlTree.Add(new XText("more text"));  
Console.WriteLine(xmlTree.Nodes().OfType<XText>().Count());  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
1  
1  
2  
```  
  
### <a name="empty-text-nodes-are-possible"></a>Os nós vazios de texto são possíveis  
 Em algum XML que programa modelos, os nós de texto são garantidos para não conter a cadeia de caracteres vazia. O raciocínio é que um nó de texto não tem impacto na serialização XML. No entanto, pela mesma razão nós adjacentes do texto são possíveis, se você remover o texto de um nó de texto definindo seu valor para a cadeia de caracteres vazia, o nó de texto em si não serão excluídos.  
  
```csharp  
XElement xmlTree = new XElement("Root", "Content");  
XText textNode = xmlTree.Nodes().OfType<XText>().First();  
  
// the following line does not cause the removal of the text node.  
textNode.Value = "";  
  
XText textNode2 = xmlTree.Nodes().OfType<XText>().First();  
Console.WriteLine(">>{0}<<", textNode2);
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
>><<  
```  
  
### <a name="an-empty-text-node-impacts-serialization"></a>Um nó de texto vazio afeta a serialização  
 Se um elemento contém apenas um nó filho do texto que está vazio, é serializado com longa sintaxe de marca: `<Child></Child>`. Se um elemento não contém nenhum nó filho, qualquer é serializado pela sintaxe abreviada de marca: `<Child />`.  
  
```csharp  
XElement child1 = new XElement("Child1",  
    new XText("")  
);  
XElement child2 = new XElement("Child2");  
Console.WriteLine(child1);  
Console.WriteLine(child2);
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Child1></Child1>  
<Child2 />  
```  
  
### <a name="namespaces-are-attributes-in-the-linq-to-xml-tree"></a>Namespaces são atributos na árvore LINQ to XML  
 Mesmo que as declarações namespace tenham a sintaxe idêntica a atributos, em algumas interfaces de programação, como XSLT e o XPath, as declarações namespace não são consideradas como atributos. No entanto, em [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], namespaces são armazenadas como objetos de <xref:System.Xml.Linq.XAttribute> na árvore XML. Se você itera através de atributos para um elemento que contém uma declaração de namespace, você verá a declaração de namespace como um dos itens na coleção retornada.  
  
 A propriedade de <xref:System.Xml.Linq.XAttribute.IsNamespaceDeclaration%2A> indica se um atributo é uma declaração de namespace.  
  
```csharp  
XElement root = XElement.Parse(  
@"<Root  
    xmlns='http://www.adventure-works.com'  
    xmlns:fc='www.fourthcoffee.com'  
    AnAttribute='abc'/>");  
foreach (XAttribute att in root.Attributes())  
    Console.WriteLine("{0}  IsNamespaceDeclaration:{1}", att, att.IsNamespaceDeclaration);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
xmlns="http://www.adventure-works.com"  IsNamespaceDeclaration:True  
xmlns:fc="www.fourthcoffee.com"  IsNamespaceDeclaration:True  
AnAttribute="abc"  IsNamespaceDeclaration:False  
```  
  
### <a name="xpath-axis-methods-do-not-return-child-white-space-of-xdocument"></a>Os métodos do eixo XPath retornam espaço em branco filho de XDocument  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] permite nós filhos de texto de <xref:System.Xml.Linq.XDocument>, desde que os nós de texto contém somente espaço em branco. No entanto, o modelo de objeto XPath não inclui espaço em branco como nós filho de um documento, para que quando você itera através de filhos de <xref:System.Xml.Linq.XDocument> usando o eixo <xref:System.Xml.Linq.XContainer.Nodes%2A>, os nós de texto de espaço em branco serão retornados. No entanto, quando você itera através de filhos de <xref:System.Xml.Linq.XDocument> usando os métodos do eixo XPath, os nós de texto de espaço em branco não serão retornados.  
  
```csharp  
// Create a document with some white-space child nodes of the document.  
XDocument root = XDocument.Parse(  
@"<?xml version='1.0' encoding='utf-8' standalone='yes'?>  
  
<Root/>  
  
<!--a comment-->  
", LoadOptions.PreserveWhitespace);  
  
// count the white-space child nodes using LINQ to XML  
Console.WriteLine(root.Nodes().OfType<XText>().Count());  
  
// count the white-space child nodes using XPathEvaluate  
Console.WriteLine(((IEnumerable)root.XPathEvaluate("text()")).OfType<XText>().Count());
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
3  
0  
```  
  
### <a name="xdeclaration-objects-are-not-nodes"></a>Os objetos de XDeclaration não são nós  
 Quando você itera através de nós de filhos de <xref:System.Xml.Linq.XDocument>, você não verá o objeto da declaração XML. É uma propriedade do documento, não um nó filho deles.  
  
```csharp  
XDocument doc = new XDocument(  
    new XDeclaration("1.0", "utf-8", "yes"),  
    new XElement("Root")  
);  
doc.Save("Temp.xml");  
Console.WriteLine(File.ReadAllText("Temp.xml"));  
  
// this shows that there is only one child node of the document  
Console.WriteLine(doc.Nodes().Count());  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
<?xml version="1.0" encoding="utf-8" standalone="yes"?>  
<Root />  
1  
```  
  