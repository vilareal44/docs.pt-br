---
title: Como filtrar em um atributo (XPath-LINQ to XML) (C#)
description: Saiba como filtrar elementos descendentes com um nome e valor de atributo especificados para XPath-LINQ to XML.
ms.date: 07/20/2015
ms.assetid: 208d6256-1bd7-4237-b2c9-909f26dfd0e2
ms.openlocfilehash: 80c43b8485314c6a711b574b5d6c23b56533833d
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302887"
---
# <a name="how-to-filter-on-an-attribute-xpath-linq-to-xml-c"></a>Como filtrar em um atributo (XPath-LINQ to XML) (C#)
Este tópico mostra como obter os elementos descendentes com um nome especificado e, com um atributo com um valor especificado.  
  
 A expressão XPath é:  
  
 `.//Address[@Type='Shipping']`  
  
## <a name="example"></a>Exemplo  
 Este exemplo localiza os elementos dos descendentes com o nome de `Address`, e com um atributo de `Type` com um valor de “enviar”.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: vários pedidos de compra (LINQ to XML)](./sample-xml-file-multiple-purchase-orders-linq-to-xml.md).  
  
```csharp  
XDocument po = XDocument.Load("PurchaseOrders.xml");  
  
// LINQ to XML query  
IEnumerable<XElement> list1 =  
    from el in po.Descendants("Address")  
    where (string)el.Attribute("Type") == "Shipping"  
    select el;  
  
// XPath expression  
IEnumerable<XElement> list2 = po.XPathSelectElements(".//Address[@Type='Shipping']");  
  
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
<Address Type="Shipping">  
  <Name>Ellen Adams</Name>  
  <Street>123 Maple Street</Street>  
  <City>Mill Valley</City>  
  <State>CA</State>  
  <Zip>10999</Zip>  
  <Country>USA</Country>  
</Address>  
<Address Type="Shipping">  
  <Name>Cristian Osorio</Name>  
  <Street>456 Main Street</Street>  
  <City>Buffalo</City>  
  <State>NY</State>  
  <Zip>98112</Zip>  
  <Country>USA</Country>  
</Address>  
<Address Type="Shipping">  
  <Name>Jessica Arnold</Name>  
  <Street>4055 Madison Ave</Street>  
  <City>Seattle</City>  
  <State>WA</State>  
  <Zip>98112</Zip>  
  <Country>USA</Country>  
</Address>  
```  
