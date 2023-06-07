---
title: 'Como: projetar um novo tipo (LINQ to XML)'
ms.date: 07/20/2015
ms.assetid: 8cfb24f5-89b2-4cfb-b85d-e7963f8f1845
ms.openlocfilehash: 48fb82e870a4fc4fa16cfb48a127f364e6d81f13
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396500"
---
# <a name="how-to-project-a-new-type-linq-to-xml-visual-basic"></a>Como projetar um novo tipo (LINQ to XML) (Visual Basic)
Outros exemplos nesta seção mostraram consultas que os resultados de retorno como <xref:System.Collections.Generic.IEnumerable%601> de <xref:System.Xml.Linq.XElement>, <xref:System.Collections.Generic.IEnumerable%601> de `string`, e <xref:System.Collections.Generic.IEnumerable%601> de `int`. Esses são tipos comuns de resultado, mas não são adequados para cada cenário. Em muitos casos você desejará suas consultas para retornar <xref:System.Collections.Generic.IEnumerable%601> de qualquer outro tipo.  
  
## <a name="example"></a>Exemplo  
 Este exemplo mostra como instanciar objetos na cláusula `Select` . O código define primeiro uma nova classe com um construtor, e altera a declaração de `Select` de modo que a expressão é uma nova instância da nova classe.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: ordem de compra típica (LINQ to XML)](sample-xml-file-typical-purchase-order-linq-to-xml.md).  
  
```vb  
Public Class NameQty  
    Public name As String  
    Public qty As Integer  
    Public Sub New(ByVal n As String, ByVal q As Integer)  
        name = n  
        qty = q  
    End Sub  
End Class  
  
Public Class Program  
    Shared Sub Main()  
        Dim po As XElement = XElement.Load("PurchaseOrder.xml")  
  
        Dim nqList As IEnumerable(Of NameQty) = _  
            From n In po...<Item> _  
            Select New NameQty( _  
                n.<ProductName>.Value, CInt(n.<Quantity>.Value))  
  
        For Each n As NameQty In nqList  
            Console.WriteLine(n.name & ":" & n.qty)  
        Next  
    End Sub  
End Class  
```  
  
 Este exemplo usa o `M:System.Xml.Linq.XElement.Element` método que foi introduzido no tópico [como: recuperar um único elemento filho (LINQ to XML) (Visual Basic)](how-to-retrieve-a-single-child-element-linq-to-xml.md). Também usa conversões para recuperar os valores dos elementos que são retornados pelo método de `M:System.Xml.Linq.XElement.Element` .  
  
 Esse exemplo gera a saída a seguir:  
  
```console  
Lawnmower:1  
Baby Monitor:2  
```  
  
## <a name="see-also"></a>Confira também

- [Projeções e transformações (LINQ to XML) (Visual Basic)](projections-and-transformations-linq-to-xml.md)
