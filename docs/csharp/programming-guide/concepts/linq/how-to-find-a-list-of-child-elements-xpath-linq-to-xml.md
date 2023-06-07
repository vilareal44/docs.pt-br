---
title: Como localizar uma lista de elementos filho (XPath-LINQ to XML) (C#)
description: Saiba como localizar uma lista de elementos filho usando uma expressão XPath. Examine um exemplo de código que localiza todos os elementos filho de um elemento específico.
ms.date: 07/20/2015
ms.assetid: 7c589dd8-f680-4cdb-9d6a-78d57e2555e8
ms.openlocfilehash: a3025aca7fb1055acd55e5ce98914d8359ebe4b7
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301717"
---
# <a name="how-to-find-a-list-of-child-elements-xpath-linq-to-xml-c"></a>Como localizar uma lista de elementos filho (XPath-LINQ to XML) (C#)
Este tópico compara o eixo dos elementos filho do XPath com o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] <xref:System.Xml.Linq.XContainer.Elements%2A> eixo.  
  
 A expressão XPath é: `./*`  
  
## <a name="example"></a>Exemplo  
 Este exemplo localiza os elementos filho do elemento `Address`.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: vários pedidos de compra (LINQ to XML)](./sample-xml-file-multiple-purchase-orders-linq-to-xml.md).  
  
```csharp  
XDocument cpo = XDocument.Load("PurchaseOrders.xml");  
XElement po = cpo.Root.Element("PurchaseOrder").Element("Address");  
  
// LINQ to XML query  
IEnumerable<XElement> list1 = po.Elements();  
  
// XPath expression  
IEnumerable<XElement> list2 = po.XPathSelectElements("./*");  
  
if (list1.Count() == list2.Count() &&  
        list1.Intersect(list2).Count() == list1.Count())  
    Console.WriteLine("Results are identical");  
else  
    Console.WriteLine("Results differ");  
foreach (XElement el in list1)  
    Console.WriteLine(el);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
Results are identical  
<Name>Ellen Adams</Name>  
<Street>123 Maple Street</Street>  
<City>Mill Valley</City>  
<State>CA</State>  
<Zip>10999</Zip>  
<Country>USA</Country>  
```  
