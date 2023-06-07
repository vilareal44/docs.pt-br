---
title: Como recuperar um único atributo (LINQ to XML) (C#)
description: Saiba como usar LINQ to XML recuperar um único atributo de um elemento em C#, dado o nome do atributo.
ms.date: 07/20/2015
ms.assetid: 1b6b07b9-933f-47e9-874e-e790cab49dc5
ms.openlocfilehash: 4efcae5324ad5a2e4664e68e35e15ec2053daece
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103438"
---
# <a name="how-to-retrieve-a-single-attribute-linq-to-xml-c"></a>Como recuperar um único atributo (LINQ to XML) (C#)
Este tópico explica como recuperar um único atributo de um elemento, dado o nome do atributo. Isso é útil para gravar as expressões de consulta onde você deseja localizar um elemento que possui um atributo específico.  
  
 O método de <xref:System.Xml.Linq.XElement.Attribute%2A> da classe de <xref:System.Xml.Linq.XElement> retorna <xref:System.Xml.Linq.XAttribute> com o nome especificado.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o método <xref:System.Xml.Linq.XElement.Attribute%2A>.  
  
```csharp  
XElement cust = new XElement("PhoneNumbers",  
    new XElement("Phone",  
        new XAttribute("type", "home"),  
        "555-555-5555"),  
    new XElement("Phone",  
        new XAttribute("type", "work"),  
        "555-555-6666")  
);  
IEnumerable<XElement> elList =  
    from el in cust.Descendants("Phone")  
    select el;  
foreach (XElement el in elList)  
    Console.WriteLine((string)el.Attribute("type"));  
```  
  
 Este exemplo localiza os descendentes na árvore chamada `Phone`, e localiza o atributo chamado `type`.  
  
 Este código produz a seguinte saída:  
  
```output  
home  
work  
```  
  
## <a name="example"></a>Exemplo  
 Se você deseja recuperar o valor do atributo, você pode convertê-lo, exatamente como você faz para com objetos de <xref:System.Xml.Linq.XElement> . O exemplo a seguir demonstra isso.  
  
```csharp  
XElement cust = new XElement("PhoneNumbers",  
    new XElement("Phone",  
        new XAttribute("type", "home"),  
        "555-555-5555"),  
    new XElement("Phone",  
        new XAttribute("type", "work"),  
        "555-555-6666")  
);  
IEnumerable<XElement> elList =
    from el in cust.Descendants("Phone")  
    select el;  
foreach (XElement el in elList)  
    Console.WriteLine((string)el.Attribute("type"));  
```  
  
 Este código produz a seguinte saída:  
  
```output  
home  
work  
```  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] fornece operadores cast explícitos para a classe <xref:System.Xml.Linq.XAttribute> para `string`, `bool`, `bool?`, `int`, `int?`, `uint`, `uint?`, `long`, `long?`, `ulong`, `ulong?`, `float`, `float?`, `double`, `double?`, `decimal`, `decimal?`, `DateTime`, `DateTime?`, `TimeSpan`, `TimeSpan?`, `GUID` e `GUID?`.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra o mesmo código para um atributo que está em um namespace. Para obter mais informações, consulte [Visão geral de namespaces (LINQ to XML) (C#)](namespaces-overview-linq-to-xml.md).  
  
```csharp  
XNamespace aw = "http://www.adventure-works.com";  
XElement cust = new XElement(aw + "PhoneNumbers",  
    new XElement(aw + "Phone",  
        new XAttribute(aw + "type", "home"),  
        "555-555-5555"),  
    new XElement(aw + "Phone",  
        new XAttribute(aw + "type", "work"),  
        "555-555-6666")  
);  
IEnumerable<XElement> elList =  
    from el in cust.Descendants(aw + "Phone")  
    select el;  
foreach (XElement el in elList)  
    Console.WriteLine((string)el.Attribute(aw + "type"));  
```  
  
 Este código produz a seguinte saída:  
  
```output  
home  
work  
```  
  
## <a name="see-also"></a>Confira também

- [Eixos do LINQ to XML (C#)](./linq-to-xml-axes-overview.md)
