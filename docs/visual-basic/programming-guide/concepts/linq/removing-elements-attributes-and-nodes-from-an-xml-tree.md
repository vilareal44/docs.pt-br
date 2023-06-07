---
title: Remoção de elementos, atributos e nós de uma árvore XML
ms.date: 07/20/2015
ms.assetid: 5cf21919-4360-4b49-b29d-58ea3164ac72
ms.openlocfilehash: f8be7fe521fb3c2662b105e34fd96fea1d1ac6e7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413395"
---
# <a name="removing-elements-attributes-and-nodes-from-an-xml-tree-visual-basic"></a>Removendo elementos, atributos e nós de uma árvore XML (Visual Basic)
Você pode modificar uma árvore XML, remover elementos, atributos e outros tipos de nós.  
  
 Remover um único elemento ou um único atributo de um documento XML é simples. No entanto, ao remover coleções de elementos ou atributos, você deve primeiro materializar uma coleção em uma lista e depois excluir os elementos ou os atributos da lista. A melhor abordagem é usar o método de extensão <xref:System.Xml.Linq.Extensions.Remove%2A>, que fará isso para você.  
  
 O principal motivo para fazer isso é que a maioria das coleções que você recupera de uma árvore XML é gerada usando a execução adiada. Se você não materializar essas coleções primeiro em uma lista, ou não usar os métodos de extensão, poderá encontrar uma determinada classe de bugs. Para obter mais informações, consulte [código declarativo misto/bugs de código imperativo (LINQ to XML) (Visual Basic)](mixed-declarative-code-imperative-code-bugs-linq-to-xml.md).  
  
 Os métodos a seguir removem nós e atributos de uma árvore XML.  
  
|Método|Descrição|  
|------------|-----------------|  
|<xref:System.Xml.Linq.XAttribute.Remove%2A?displayProperty=nameWithType>|Remove uma classe <xref:System.Xml.Linq.XAttribute> de seu pai.|  
|<xref:System.Xml.Linq.XContainer.RemoveNodes%2A?displayProperty=nameWithType>|Remove os nós filho de uma classe <xref:System.Xml.Linq.XContainer>.|  
|<xref:System.Xml.Linq.XElement.RemoveAll%2A?displayProperty=nameWithType>|Remove o conteúdo e os atributos de uma classe <xref:System.Xml.Linq.XElement>.|  
|<xref:System.Xml.Linq.XElement.RemoveAttributes%2A?displayProperty=nameWithType>|Remove os atributos de uma classe <xref:System.Xml.Linq.XElement>.|  
|<xref:System.Xml.Linq.XElement.SetAttributeValue%2A?displayProperty=nameWithType>|Se você passar `null` para o valor, esse método removerá o atributo.|  
|<xref:System.Xml.Linq.XElement.SetElementValue%2A?displayProperty=nameWithType>|Se você passar `null` para o valor, esse método removerá o elemento filho.|  
|<xref:System.Xml.Linq.XNode.Remove%2A?displayProperty=nameWithType>|Remove uma classe <xref:System.Xml.Linq.XNode> de seu pai.|  
|<xref:System.Xml.Linq.Extensions.Remove%2A?displayProperty=nameWithType>|Remove cada atributo ou elemento na coleção de origem do respectivo elemento pai.|  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 Este exemplo demonstra três abordagens para remover elementos. Primeiro, ele remove um único elemento. Segundo, ele recupera uma coleção de elementos, materializa essa coleção usando o operador <xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> e remove a coleção. Por último, recupera uma coleção de elementos e remove-a usando o método de extensão <xref:System.Xml.Linq.Extensions.Remove%2A>.  
  
 Para obter mais informações sobre o <xref:System.Linq.Enumerable.ToList%2A> operador, consulte [convertendo tipos de dados (Visual Basic)](converting-data-types.md).  
  
### <a name="code"></a>Código  
  
```vb  
Dim root As XElement = _  
    <Root>  
        <Child1>  
            <GrandChild1/>  
            <GrandChild2/>  
            <GrandChild3/>  
        </Child1>  
        <Child2>  
            <GrandChild4/>  
            <GrandChild5/>  
            <GrandChild6/>  
        </Child2>  
        <Child3>  
            <GrandChild7/>  
            <GrandChild8/>  
            <GrandChild9/>  
        </Child3>  
    </Root>  
root.<Child1>.<GrandChild1>.Remove()  
root.<Child2>.Elements().ToList().Remove()  
root.<Child3>.Elements().Remove()  
Console.WriteLine(root)  
```  
  
### <a name="comments"></a>Comentários  
 Esse código gera a seguinte saída:  
  
```xml  
<Root>  
  <Child1>  
    <GrandChild2 />  
    <GrandChild3 />  
  </Child1>  
  <Child2 />  
  <Child3 />  
</Root>  
```  
  
 Observe que o primeiro elemento neto foi removido de `Child1`. Todos os elementos neto foram removidos de `Child2` e de `Child3`.  
  
## <a name="see-also"></a>Confira também

- [Modificando árvores XML (LINQ to XML) (Visual Basic)](modifying-xml-trees-linq-to-xml.md)
