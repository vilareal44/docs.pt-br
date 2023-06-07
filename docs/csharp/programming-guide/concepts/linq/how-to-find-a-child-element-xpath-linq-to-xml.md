---
title: Como encontrar um elemento filho (XPath-LINQ to XML) (C#)
description: Saiba como encontrar um elemento filho comparando o eixo do elemento filho XPath com o método do elemento LINQ to XML.
ms.date: 07/20/2015
ms.assetid: 4fa6182d-6196-4ed1-9c9e-82949ff89c71
ms.openlocfilehash: 57d1a4e636e3443512020129a76cc2de7bb3f244
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301730"
---
# <a name="how-to-find-a-child-element-xpath-linq-to-xml-c"></a>Como encontrar um elemento filho (XPath-LINQ to XML) (C#)
Este tópico compara o eixo do elemento filho XPath com o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] <xref:System.Xml.Linq.XContainer.Element%2A> método.  
  
 A expressão XPath é `DeliveryNotes`.  
  
## <a name="example"></a>Exemplo  
 Este exemplo localiza o elemento filho `DeliveryNotes`.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: vários pedidos de compra (LINQ to XML)](./sample-xml-file-multiple-purchase-orders-linq-to-xml.md).  
  
```csharp  
XDocument cpo = XDocument.Load("PurchaseOrders.xml");  
XElement po = cpo.Root.Element("PurchaseOrder");  
  
// LINQ to XML query  
XElement el1 = po.Element("DeliveryNotes");  
  
// XPath expression  
XElement el2 = po.XPathSelectElement("DeliveryNotes");  
// same as "child::DeliveryNotes"  
// same as "./DeliveryNotes"  
  
if (el1 == el2)  
    Console.WriteLine("Results are identical");  
else  
    Console.WriteLine("Results differ");  
Console.WriteLine(el1);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
Results are identical  
<DeliveryNotes>Please leave packages in shed by driveway.</DeliveryNotes>  
```  
  