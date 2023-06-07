---
title: Como consultar LINQ to XML usando XPath (C#)
description: Você pode usar métodos de extensão em C# para consultar uma árvore XML usando XPath. Em geral, não recomendamos o uso de XPath com LINQ to XML.
ms.date: 07/20/2015
ms.assetid: ee5af263-4ab1-45e5-b792-33a3221b426d
ms.openlocfilehash: fff45a93380b5af85aa640fc690783cc95e6298b
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104341"
---
# <a name="how-to-query-linq-to-xml-using-xpath-c"></a>Como consultar LINQ to XML usando XPath (C#)
Este tópico apresenta os métodos de extensão que permitem ver uma árvore XML usando o XPath. Para obter informações detalhadas sobre como usar esses métodos de extensão, consulte <xref:System.Xml.XPath.Extensions?displayProperty=nameWithType>.  
  
 A menos que você tenha um motivo específico para muito consulte o XPath em uso, como o uso extensivo de código herdado, usando o XPath com LINQ to XML não é recomendada. Consultas de XPath não são executadas tão bem quanto consultas de [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria uma árvore XML pequena e usa <xref:System.Xml.XPath.Extensions.XPathSelectElements%2A> para selecionar um conjunto de elementos.  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("Child1", 1),  
    new XElement("Child1", 2),  
    new XElement("Child1", 3),  
    new XElement("Child2", 4),  
    new XElement("Child2", 5),  
    new XElement("Child2", 6)  
);  
IEnumerable<XElement> list = root.XPathSelectElements("./Child2");  
foreach (XElement el in list)  
    Console.WriteLine(el);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Child2>4</Child2>  
<Child2>5</Child2>  
<Child2>6</Child2>  
```  
  