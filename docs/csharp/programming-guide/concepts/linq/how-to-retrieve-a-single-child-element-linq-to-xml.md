---
title: Como recuperar um único elemento filho (LINQ to XML) (C#)
description: Saiba como usar LINQ to XML para recuperar um único elemento filho por nome. Neste exemplo de C#, o método Element retorna o primeiro elemento filho especificado.
ms.date: 07/20/2015
ms.assetid: ce37db9e-76fa-46eb-b4cc-e8f32d22ad90
ms.openlocfilehash: c3a044f30cf2e411bdff52586c7a370b626f41bc
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103272"
---
# <a name="how-to-retrieve-a-single-child-element-linq-to-xml-c"></a>Como recuperar um único elemento filho (LINQ to XML) (C#)
Este tópico explica como recuperar um único elemento filho, considerando o nome do elemento filho. Quando você souber que o nome do elemento filho e que há apenas um elemento com esse nome, pode ser conveniente recuperar apenas um elemento, em vez de uma coleção.  
  
 O método <xref:System.Xml.Linq.XContainer.Element%2A> retorna o primeiro filho <xref:System.Xml.Linq.XElement> com o <xref:System.Xml.Linq.XName> especificado.  
  
 Se você quiser recuperar um único elemento filho no Visual Basic, uma abordagem comum é usar a propriedade XML e, em seguida, recuperar o primeiro elemento usando a notação do indexador de matriz.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra o uso do método <xref:System.Xml.Linq.XContainer.Element%2A>. Este exemplo usa a árvore XML chamada `po` e localiza o primeiro elemento chamado `Comment`.  
  
 O exemplo do Visual Basic mostra o uso da notação do indexador de matriz para recuperar um único elemento.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: ordem de compra típica (LINQ to XML)](./sample-xml-file-typical-purchase-order-linq-to-xml-1.md).  
  
```csharp  
XElement po = XElement.Load("PurchaseOrder.xml");  
XElement e = po.Element("DeliveryNotes");  
Console.WriteLine(e);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<DeliveryNotes>Please leave packages in shed by driveway.</DeliveryNotes>  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra o mesmo código para XML que está em um namespace. Para obter mais informações, consulte [Visão geral de namespaces (LINQ to XML) (C#)](namespaces-overview-linq-to-xml.md).  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: ordem de compra típica em um namespace](./sample-xml-file-typical-purchase-order-in-a-namespace.md).  
  
```csharp  
XElement po = XElement.Load("PurchaseOrderInNamespace.xml");  
XNamespace aw = "http://www.adventure-works.com";  
XElement e = po.Element(aw + "DeliveryNotes");  
Console.WriteLine(e);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<aw:DeliveryNotes xmlns:aw="http://www.adventure-works.com">Please leave packages in shed by driveway.</aw:DeliveryNotes>  
```  
  
## <a name="see-also"></a>Confira também

- [Eixos do LINQ to XML (C#)](./linq-to-xml-axes-overview.md)
