---
title: 'Como: filtrar em nomes de elemento (LINQ to XML)'
ms.date: 07/20/2015
ms.assetid: b1437b4a-48aa-4546-834a-d6d3ab015fe1
ms.openlocfilehash: ccb768030e10d73b839df71093a27ff11b918650
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84394331"
---
# <a name="how-to-filter-on-element-names-linq-to-xml-visual-basic"></a>Como filtrar em nomes de elemento (LINQ to XML) (Visual Basic)
Quando você chamar um dos métodos que <xref:System.Collections.Generic.IEnumerable%601> de retorno de <xref:System.Xml.Linq.XElement>, você pode filtrar no nome do elemento.  
  
## <a name="example"></a>Exemplo  
 Este exemplo retorna uma coleção de descendentes que é filtrada para conter somente descendentes com o nome especificado.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: ordem de compra típica (LINQ to XML)](sample-xml-file-typical-purchase-order-linq-to-xml.md).  
  
```vb  
Dim po As XElement = XElement.Load("PurchaseOrder.xml")  
Dim items As IEnumerable(Of XElement) = _  
    From el In po...<ProductName> _  
    Select el  
For Each prdName As XElement In items  
    Console.WriteLine(prdName.Name.ToString & ":" & prdName.Value)  
Next  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
ProductName:Lawnmower  
ProductName:Baby Monitor  
```  
  
 Os outros métodos que <xref:System.Collections.Generic.IEnumerable%601> de retorno de coleções de <xref:System.Xml.Linq.XElement> segue o mesmo padrão. Suas assinaturas são semelhantes a <xref:System.Xml.Linq.XContainer.Elements%2A> e a <xref:System.Xml.Linq.XContainer.Descendants%2A>. O seguinte é a lista completa dos métodos semelhantes que tenham assinaturas de método:  
  
- <xref:System.Xml.Linq.XNode.Ancestors%2A>  
  
- <xref:System.Xml.Linq.XContainer.Descendants%2A>  
  
- <xref:System.Xml.Linq.XContainer.Elements%2A>  
  
- <xref:System.Xml.Linq.XNode.ElementsAfterSelf%2A>  
  
- <xref:System.Xml.Linq.XNode.ElementsBeforeSelf%2A>  
  
- <xref:System.Xml.Linq.XElement.AncestorsAndSelf%2A>  
  
- <xref:System.Xml.Linq.XElement.DescendantsAndSelf%2A>  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra a mesma consulta para XML que está em um namespace. Para obter mais informações, consulte [visão geral de namespaces (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md).  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: ordem de compra típica em um namespace](sample-xml-file-typical-purchase-order-in-a-namespace.md).  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
  
Module Module1  
    Sub Main()  
        Dim po As XElement = XElement.Load("PurchaseOrderInNamespace.xml")  
        Dim items As IEnumerable(Of XElement) = _  
            From el In po...<aw:ProductName> _  
            Select el  
        For Each prdName As XElement In items  
            Console.WriteLine(prdName.Name.ToString & ":" & prdName.Value)  
        Next  
    End Sub  
End Module  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
{http://www.adventure-works.com}ProductName:Lawnmower  
{http://www.adventure-works.com}ProductName:Baby Monitor  
```  
  
## <a name="see-also"></a>Confira também

- [Eixos LINQ to XML (Visual Basic)](linq-to-xml-axes.md)
