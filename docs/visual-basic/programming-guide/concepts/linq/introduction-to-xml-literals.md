---
title: Introdução aos literais XML no Visual Basic2
ms.date: 07/20/2015
ms.assetid: 94fc0e03-978e-4c08-ab6c-0dc3c1e64f10
ms.openlocfilehash: 8b92d22727c50274d57a5e407a0ca42807de3a94
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397578"
---
# <a name="introduction-to-xml-literals-in-visual-basic"></a>Introdução a literais XML no Visual Basic
Esta seção fornece informações sobre a criação de árvores XML no Visual Basic.  
  
 Para obter informações sobre como usar os resultados de consultas LINQ como o conteúdo de uma árvore XML, consulte [construção funcional (LINQ to XML) (Visual Basic)](functional-construction-linq-to-xml.md).  
  
 Para obter mais informações sobre literais XML no Visual Basic, consulte [visão geral de LINQ to XML em Visual Basic](../../language-features/xml/overview-of-linq-to-xml.md).  
  
## <a name="creating-xml-trees"></a>Criando árvores XML  
 O exemplo a seguir mostra como criar <xref:System.Xml.Linq.XElement>, nesse caso `contacts`:  
  
```vb  
Dim contacts As XElement = _  
    <Contacts>  
        <Contact>  
            <Name>Patrick Hines</Name>  
            <Phone>206-555-0144</Phone>  
            <Address>  
                <Street1>123 Main St</Street1>  
                <City>Mercer Island</City>  
                <State>WA</State>  
                <Postal>68042</Postal>  
            </Address>  
        </Contact>  
    </Contacts>  
```  
  
### <a name="creating-an-xelement-with-simple-content"></a>Criando um XElement com conteúdo simples  
 Você pode criar <xref:System.Xml.Linq.XElement> que contém o conteúdo simples, como segue:  
  
```vb  
Dim n as XElement = <Customer>Adventure Works</Customer>  
Console.WriteLine(n)
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Customer>Adventure Works</Customer>  
```  
  
### <a name="creating-an-empty-element"></a>Criando um elemento vazio  
 Você pode criar <xref:System.Xml.Linq.XElement>vazia, como segue:  
  
```vb  
Dim n As XElement = <Customer/>  
Console.WriteLine(n)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Customer />  
```  
  
### <a name="using-embedded-expressions"></a>Usando expressões inseridas  
 Um recurso importante de literais XML é que permitem expressões inseridas. Expressões inseridas permite avaliar uma expressão e para inserir os resultados da expressão na árvore XML. Se a expressão avaliada como um tipo de <xref:System.Xml.Linq.XElement>, um elemento é inserido na árvore. Se a expressão avaliada como um tipo de <xref:System.Xml.Linq.XAttribute>, um atributo é inserido na árvore. Você pode inserir os elementos e atributos na árvore apenas onde são válidos.  
  
 É importante observar que apenas uma única expressão pode inserir em uma expressão inserida. Você não pode inserir várias instruções. Se uma expressão ultrapassa de uma única linha, você deve usar o caractere de continuação de linha.  
  
 Se você usar uma expressão inserida para adicionar nós existentes (incluindo elementos) e atributos para uma nova árvore XML e os nós existentes já parented, os nós são clonados. Os nós recentemente clonados são anexados a nova árvore XML. Se os nós existentes não parented, os nós estão conectados somente para a nova árvore XML. O último exemplo deste tópico demonstra isso.  
  
 O exemplo a seguir usa uma expressão inserida para inserir um elemento na árvore:  
  
```vb  
xmlTree1 As XElement = _  
    <Root>  
        <Child>Contents</Child>  
    </Root>  
Dim xmlTree2 As XElement = _  
    <Root>  
        <%= xmlTree1.<Child> %>  
    </Root>  
Console.WriteLine(xmlTree2)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root>  
  <Child>Contents</Child>  
</Root>  
```  
  
### <a name="using-embedded-expressions-for-content"></a>Usando expressões inseridas para o conteúdo  
 Você pode usar uma expressão inserida para fornecer o conteúdo de um elemento:  
  
```vb  
Dim str As String  
str = "Some content"  
Dim root As XElement = <Root><%= str %></Root>  
Console.WriteLine(root)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root>Some content</Root>  
```  
  
### <a name="using-a-linq-query-in-an-embedded-expression"></a>Usando uma consulta LINQ em uma expressão inserida  
 Você pode usar os resultados de uma consulta LINQ para o conteúdo de um elemento:  
  
```vb  
Dim arr As Integer() = {1, 2, 3}  
  
Dim n As XElement = _  
    <Root>  
        <%= From i In arr Select <Child><%= i %></Child> %>  
    </Root>  
  
Console.WriteLine(n)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root>  
  <Child>1</Child>  
  <Child>2</Child>  
  <Child>3</Child>  
</Root>  
```  
  
### <a name="using-embedded-expressions-for-node-names"></a>Usando expressões inseridas para nomes de nó  
 Você também pode usar expressões inseridas para calcular nomes de atributo, valores de atributo, nomes de elemento, e valores de elemento:  
  
```vb  
Dim eleName As String = "ele"  
Dim attName As String = "att"  
Dim attValue As String = "aValue"  
Dim eleValue As String = "eValue"  
Dim n As XElement = _  
    <Root <%= attName %>=<%= attValue %>>  
        <<%= eleName %>>  
            <%= eleValue %>  
        </>  
    </Root>  
Console.WriteLine(n)  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root att="aValue">  
  <ele>eValue</ele>  
</Root>  
```  
  
### <a name="cloning-vs-attaching"></a>Clonagem contra. anexação  
 Como mencionado anteriormente, se você usar uma expressão inserida para adicionar nós existentes (incluindo elementos) e atributos para uma nova árvore XML, se os nós existentes já parented, os nós é clonados e os nós recentemente clonados são anexados a nova árvore XML. Se os nós existentes não parented, eles estão conectados somente para a nova árvore XML.  
  
```vb  
' Create a tree with a child element.  
Dim xmlTree1 As XElement = _  
    <Root>  
        <Child1>1</Child1>  
    </Root>  
  
' Create an element that is not parented.  
Dim child2 As XElement = <Child2>2</Child2>  
  
' Create a tree and add Child1 and Child2 to it.  
Dim xmlTree2 As XElement = _  
    <Root>  
        <%= xmlTree1.<Child1>(0) %>  
        <%= child2 %>  
    </Root>  
  
' Compare Child1 identity.  
Console.WriteLine("Child1 was {0}", _  
    IIf(xmlTree1.Element("Child1") Is xmlTree2.Element("Child1"), _  
    "attached", "cloned"))  
  
' Compare Child2 identity.  
Console.WriteLine("Child2 was {0}", _  
    IIf(child2 Is xmlTree2.Element("Child2"), _  
    "attached", "cloned"))  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```console  
Child1 was cloned  
Child2 was attached  
```  
  
## <a name="see-also"></a>Confira também

- [Criando árvores XML (Visual Basic)](creating-xml-trees.md)
