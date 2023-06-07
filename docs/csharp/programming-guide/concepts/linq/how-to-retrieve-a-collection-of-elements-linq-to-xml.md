---
title: Como recuperar uma coleção de elementos (LINQ to XML) (C#)
description: O método Elements em C# recupera uma coleção dos elementos filho de um elemento. Este exemplo de LINQ to XML itera por meio de elementos filho de um elemento.
ms.date: 07/20/2015
ms.assetid: b849668c-7976-4974-b8e1-1cd587d34258
ms.openlocfilehash: 4f9b6ae4713af9ce1a4eeb5257f57cd9724f68b2
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103350"
---
# <a name="how-to-retrieve-a-collection-of-elements-linq-to-xml-c"></a>Como recuperar uma coleção de elementos (LINQ to XML) (C#)
Este tópico demonstra o método de <xref:System.Xml.Linq.XContainer.Elements%2A> . Esse método retorna uma coleção de elementos filho de um elemento.  
  
## <a name="example"></a>Exemplo  
 Este exemplo efetua iterações através dos elementos filho do elemento de `purchaseOrder` .  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: ordem de compra típica (LINQ to XML)](./sample-xml-file-typical-purchase-order-linq-to-xml-1.md).  
  
```csharp  
XElement po = XElement.Load("PurchaseOrder.xml");  
IEnumerable<XElement> childElements =  
    from el in po.Elements()  
    select el;  
foreach (XElement el in childElements)  
    Console.WriteLine("Name: " + el.Name);  
```  
  
 Este exemplo gerencia a seguinte saída.  
  
```output  
Name: Address  
Name: Address  
Name: DeliveryNotes  
Name: Items  
```  
  
## <a name="see-also"></a>Confira também

- [Eixos do LINQ to XML (C#)](./linq-to-xml-axes-overview.md)
