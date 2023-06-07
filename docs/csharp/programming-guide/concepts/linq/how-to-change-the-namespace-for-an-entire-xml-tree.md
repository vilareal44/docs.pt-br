---
title: Como alterar o namespace de uma árvore XML inteira (C#)
description: Saiba como alterar programaticamente o namespace para um elemento ou um atributo em LINQ to XML em C#.
ms.date: 07/20/2015
ms.assetid: 1584ff3b-c77d-4241-ab62-80adfb7bfc1b
ms.openlocfilehash: 0bf4c9d8f3cf569f14b654dfd0c4291a7eb647df
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105374"
---
# <a name="how-to-change-the-namespace-for-an-entire-xml-tree-c"></a>Como alterar o namespace de uma árvore XML inteira (C#)
Às vezes você tem que alterar programaticamente ao namespace para um elemento ou atributo. LINQ to XML faz isso fácil. A propriedade de <xref:System.Xml.Linq.XElement.Name%2A?displayProperty=nameWithType> pode ser definida. A propriedade de <xref:System.Xml.Linq.XAttribute.Name%2A?displayProperty=nameWithType> não pode ser definida, mas você pode facilmente copiar os atributos em <xref:System.Collections.Generic.List%601?displayProperty=nameWithType>, remover os atributos existentes, e então adiciona novos atributos que estão no novo namespace desejada.  
  
 Para obter mais informações, consulte [Visão geral de namespaces (LINQ to XML) (C#)](namespaces-overview-linq-to-xml.md).  
  
## <a name="example"></a>Exemplo  
 O código a seguir cria duas árvores XML em qualquer namespace. Altera o namespace de cada uma das árvores, e as combina em uma única árvore.  
  
```csharp  
XElement tree1 = new XElement("Data",  
    new XElement("Child", "content",  
        new XAttribute("MyAttr", "content")  
    )  
);  
XElement tree2 = new XElement("Data",  
    new XElement("Child", "content",  
        new XAttribute("MyAttr", "content")  
    )  
);  
XNamespace aw = "http://www.adventure-works.com";  
XNamespace ad = "http://www.adatum.com";  
// change the namespace of every element and attribute in the first tree  
foreach (XElement el in tree1.DescendantsAndSelf())  
{  
    el.Name = aw.GetName(el.Name.LocalName);  
    List<XAttribute> atList = el.Attributes().ToList();  
    el.Attributes().Remove();  
    foreach (XAttribute at in atList)  
        el.Add(new XAttribute(aw.GetName(at.Name.LocalName), at.Value));  
}  
// change the namespace of every element and attribute in the second tree  
foreach (XElement el in tree2.DescendantsAndSelf())  
{  
    el.Name = ad.GetName(el.Name.LocalName);  
    List<XAttribute> atList = el.Attributes().ToList();  
    el.Attributes().Remove();  
    foreach (XAttribute at in atList)  
        el.Add(new XAttribute(ad.GetName(at.Name.LocalName), at.Value));  
}  
// add attribute namespaces so that the tree will be serialized with  
// the aw and ad namespace prefixes  
tree1.Add(  
    new XAttribute(XNamespace.Xmlns + "aw", "http://www.adventure-works.com")  
);  
tree2.Add(  
    new XAttribute(XNamespace.Xmlns + "ad", "http://www.adatum.com")  
);  
// create a new composite tree  
XElement root = new XElement("Root",  
    tree1,  
    tree2  
);  
Console.WriteLine(root);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root>  
  <aw:Data xmlns:aw="http://www.adventure-works.com">  
    <aw:Child aw:MyAttr="content">content</aw:Child>  
  </aw:Data>  
  <ad:Data xmlns:ad="http://www.adatum.com">  
    <ad:Child ad:MyAttr="content">content</ad:Child>  
  </ad:Data>  
</Root>  
```  
