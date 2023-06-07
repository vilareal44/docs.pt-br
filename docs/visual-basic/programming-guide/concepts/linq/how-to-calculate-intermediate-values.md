---
title: 'Como: calcular valores intermediários'
ms.date: 07/20/2015
ms.assetid: 933a97b2-dfe7-4f4d-94ad-e6e20df84abd
ms.openlocfilehash: bc38d4848a26a24aeb00581079c9dd31abb7ba1e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84375090"
---
# <a name="how-to-calculate-intermediate-values-visual-basic"></a>Como calcular valores intermediários (Visual Basic)
Este exemplo mostra como calcular valores intermediários que podem ser usados na classificação, filtragem, e em selecionar.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir utiliza a cláusula de `Let` .  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: dados numéricos (LINQ to XML)](sample-xml-file-numerical-data-linq-to-xml.md).  
  
```vb  
Dim root As XElement = XElement.Load("Data.xml")  
Dim extensions As IEnumerable(Of Decimal) = _  
    From el In root.<Data> _  
    Let extension = CDec(el.<Quantity>.Value) * CDec(el.<Price>.Value) _  
    Where extension > 25 _  
    Order By extension _  
    Select extension  
For Each ex As Decimal In extensions  
    Console.WriteLine(ex)  
Next  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
55.92  
73.50  
89.99  
198.00  
435.00  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra a mesma consulta para XML que está em um namespace. Para obter mais informações, consulte [visão geral de namespaces (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md).  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: dados numéricos em um namespace](sample-xml-file-numerical-data-in-a-namespace.md).  
  
```vb  
Imports <xmlns="http://www.adatum.com">  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = XElement.Load("DataInNamespace.xml")  
        Dim extensions As IEnumerable(Of Decimal) = _  
            From el In root.<Data> _  
            Let extension = CDec(el.<Quantity>.Value) * CDec(el.<Price>.Value) _  
            Where extension > 25 _  
            Order By extension _  
            Select extension  
        For Each ex As Decimal In extensions  
            Console.WriteLine(ex)  
        Next  
    End Sub  
End Module  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
55.92  
73.50  
89.99  
198.00  
435.00  
```  
  
## <a name="see-also"></a>Confira também

- [Consultas básicas (LINQ to XML) (Visual Basic)](basic-queries-linq-to-xml.md)
