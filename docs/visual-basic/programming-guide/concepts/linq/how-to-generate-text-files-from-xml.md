---
title: 'Como: Gerar arquivos de texto no XML'
ms.date: 07/20/2015
ms.assetid: 3b33f191-4abe-4419-b81b-3cb81d9a317f
ms.openlocfilehash: 9d89d8d5e929574fa26cc5c11346980e8b235ac5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396565"
---
# <a name="how-to-generate-text-files-from-xml-visual-basic"></a>Como gerar arquivos de texto do XML (Visual Basic)
Este exemplo mostra como gerar um arquivo separado por vírgulas de (CSV) dos valores de um arquivo XML.  
  
## <a name="example"></a>Exemplo  
 A versão Visual Basic usa o código de procedimento para agregar a coleção de cadeias de caracteres em uma única cadeia.  
  
 Este exemplo usa o seguinte documento XML: [Arquivo XML de exemplo: clientes e pedidos (LINQ to XML)](sample-xml-file-customers-and-orders-linq-to-xml.md).  
  
```vb  
Dim custOrd As XElement = XElement.Load("CustomersOrders.xml")  
Dim strCollection As IEnumerable(Of String) = _  
    From el In custOrd.<Customers>.<Customer> _  
    Select _  
        String.Format("{0},{1},{2},{3},{4},{5},{6},{7},{8},{9}{10}", _  
            el.@CustomerID, _  
            el.<CompanyName>.Value, _  
            el.<ContactName>.Value, _  
            el.<ContactTitle>.Value, _  
            el.<Phone>.Value, _  
            el.<FullAddress>.<Address>.Value, _  
            el.<FullAddress>.<City>.Value, _  
            el.<FullAddress>.<Region>.Value, _  
            el.<FullAddress>.<PostalCode>.Value, _  
            el.<FullAddress>.<Country>.Value, _  
            Environment.NewLine _  
        )  
Dim sb As StringBuilder = New StringBuilder()  
For Each str As String In strCollection  
    sb.Append(str)  
Next  
Console.WriteLine(sb.ToString())  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
GREAL,Great Lakes Food Market,Howard Snyder,Marketing Manager,(503) 555-7555,2732 Baker Blvd.,Eugene,OR,97403,USA  
HUNGC,Hungry Coyote Import Store,Yoshi Latimer,Sales Representative,(503) 555-6874,City Center Plaza 516 Main St.,Elgin,OR,97827,USA  
LAZYK,Lazy K Kountry Store,John Steel,Marketing Manager,(509) 555-7969,12 Orchestra Terrace,Walla Walla,WA,99362,USA  
LETSS,Let's Stop N Shop,Jaime Yorres,Owner,(415) 555-5938,87 Polk St. Suite 5,San Francisco,CA,94117,USA  
```  
  
## <a name="see-also"></a>Confira também

- [Projeções e transformações (LINQ to XML) (Visual Basic)](projections-and-transformations-linq-to-xml.md)
