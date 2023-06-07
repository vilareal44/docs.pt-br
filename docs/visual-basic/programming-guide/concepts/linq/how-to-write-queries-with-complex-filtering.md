---
title: 'Como: escrever consultas com filtragem complexa'
ms.date: 07/20/2015
ms.assetid: bf286ffc-7990-4b00-a4eb-ee3d70129950
ms.openlocfilehash: 18ed4379b7ec9c8007cc3c189fc45571b5161911
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397616"
---
# <a name="how-to-write-queries-with-complex-filtering-visual-basic"></a>Como: escrever consultas com filtragem complexa (Visual Basic)
Muitas vezes, você deseja escrever consultas LINQ to XML com filtros complexos. Por exemplo, você pode ter que localizar todos os elementos que têm um elemento filho com um nome e um valor específicos. Este tópico dá um exemplo de como escrever uma consulta com filtragem complexa.  
  
## <a name="example"></a>Exemplo  
 Este exemplo mostra como localizar todos os elementos `PurchaseOrder` que têm um elemento filho `Address` que têm um atributo `Type` igual a “Shipping” e um elemento filho `State` igual a “NY”. Ele usa uma consulta aninhada na cláusula `Where` e o operador `Any` retornará `True` se a coleção tiver elementos nele.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: vários pedidos de compra (LINQ to XML)](sample-xml-file-multiple-purchase-orders-linq-to-xml.md).  
  
 Para obter mais informações sobre o `Any` operador, consulte [operações do quantificador (Visual Basic)](quantifier-operations.md).  
  
```vb  
Dim root As XElement = XElement.Load("PurchaseOrders.xml")  
Dim purchaseOrders As IEnumerable(Of XElement) = _  
    From el In root.<PurchaseOrder> _  
    Where _  
        (From add In el.<Address> _  
        Where _  
             add.@Type = "Shipping" And _  
             add.<State>.Value = "NY" _  
        Select add) _  
    .Any() _  
    Select el  
For Each el As XElement In purchaseOrders  
    Console.WriteLine(el.@PurchaseOrderNumber)  
Next  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
99505  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra a mesma consulta para XML que está em um namespace. Para obter mais informações, consulte [visão geral de namespaces (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md).  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: vários pedidos de compra em um namespace](sample-xml-file-multiple-purchase-orders-in-a-namespace.md).  
  
```vb  
Imports <xmlns:aw='http://www.adventure-works.com'>  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = XElement.Load("PurchaseOrdersInNamespace.xml")  
        Dim purchaseOrders As IEnumerable(Of XElement) = _  
            From el In root.<aw:PurchaseOrder> _  
            Where _  
                (From add In el.<aw:Address> _  
                Where _  
                     add.@aw:Type = "Shipping" And _  
                     add.<aw:State>.Value = "NY" _  
                Select add) _  
            .Any() _  
            Select el  
        For Each el As XElement In purchaseOrders  
            Console.WriteLine(el.@aw:PurchaseOrderNumber)  
        Next  
    End Sub  
End Module  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
99505  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XElement.Attribute%2A>
- <xref:System.Xml.Linq.XContainer.Elements%2A>
- [Consultas básicas (LINQ to XML) (Visual Basic)](basic-queries-linq-to-xml.md)
- [Propriedade do Eixo Filho XML](../../../language-reference/xml-axis/xml-child-axis-property.md)
- [Propriedade de Eixo do Atributo XML](../../../language-reference/xml-axis/xml-attribute-axis-property.md)
- [Propriedade do Valor XML](../../../language-reference/xml-axis/xml-value-property.md)
- [Operações de projeção (Visual Basic)](projection-operations.md)
- [Operações do quantificador (Visual Basic)](quantifier-operations.md)
