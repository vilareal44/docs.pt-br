---
title: Como recuperar uma coleção de atributos (LINQ to XML) (C#)
description: O método Attributes em C# recupera os atributos de um elemento. Esse LINQ to XML exemplo itera através da coleção de atributos de um elemento.
ms.date: 07/20/2015
ms.assetid: a49ee7a3-b2c2-4d49-9b5c-b7c6c41f4f13
ms.openlocfilehash: 5994086db6133530e63eb1328a7b524d30a0797d
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103380"
---
# <a name="how-to-retrieve-a-collection-of-attributes-linq-to-xml-c"></a>Como recuperar uma coleção de atributos (LINQ to XML) (C#)
Este tópico apresenta o método de <xref:System.Xml.Linq.XElement.Attributes%2A> . Esse método recupera os atributos de um elemento.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como iterar através da coleção de atributos de um elemento.  
  
```csharp  
XElement val = new XElement("Value",  
    new XAttribute("ID", "1243"),  
    new XAttribute("Type", "int"),  
    new XAttribute("ConvertableTo", "double"),  
    "100");  
IEnumerable<XAttribute> listOfAttributes =  
    from att in val.Attributes()  
    select att;  
foreach (XAttribute a in listOfAttributes)  
    Console.WriteLine(a);  
```  
  
 Este código produz a seguinte saída:  
  
```output  
ID="1243"  
Type="int"  
ConvertableTo="double"  
```  
  
## <a name="see-also"></a>Confira também

- [Eixos do LINQ to XML (C#)](./linq-to-xml-axes-overview.md)
