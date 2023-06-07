---
title: 'Como: recuperar o valor superficial de um elemento'
ms.date: 07/20/2015
ms.assetid: 730a6670-fb8c-41fc-8a1b-eb97a837e432
ms.openlocfilehash: 24e6b128481f56941f0a61da9766f02813a46e97
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397811"
---
# <a name="how-to-retrieve-the-shallow-value-of-an-element-visual-basic"></a>Como recuperar o valor superficial de um elemento (Visual Basic)

Este tópico mostra como obter o valor raso de um elemento. O valor raso é o valor do elemento específico somente, diferentemente de valor maior, que inclui os valores de todos os elementos descendentes concatenados em uma única cadeia de caracteres.

Quando você recupera um valor de elemento usando conversão ou propriedade de <xref:System.Xml.Linq.XElement.Value%2A?displayProperty=nameWithType> , você recupera o valor maior. Para recuperar o valor raso, você pode usar o método de extensão de `ShallowValue`, conforme mostrado no exemplo a seguir. Recuperar o valor raso é útil quando você deseja selecionar elementos com base no conteúdo.

O exemplo a seguir declara um método de extensão que recupera o valor raso de um elemento. Use o método de extensão em uma consulta para listar todos os elementos que contém um valor calculado.

## <a name="example"></a>Exemplo

O seguinte arquivo de texto, Report.xml, é a fonte para esse exemplo.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Report>
  <Section>
    <Heading>
      <Column Name="CustomerId">=Customer.CustomerId.Heading</Column>
      <Column Name="Name">=Customer.Name.Heading</Column>
    </Heading>
    <Detail>
      <Column Name="CustomerId">=Customer.CustomerId</Column>
      <Column Name="Name">=Customer.Name</Column>
    </Detail>
  </Section>
</Report>
```

```vb
Module Module1
    <System.Runtime.CompilerServices.Extension()> _
    Public Function ShallowValue(ByVal xe As XElement)
        Return xe _
               .Nodes() _
               .OfType(Of XText)() _
               .Aggregate(New StringBuilder(), _
                              Function(s, c) s.Append(c), _
                              Function(s) s.ToString())
    End Function

    Sub Main()
        Dim root As XElement = XElement.Load("Report.xml")

        Dim query As IEnumerable(Of XElement) = _
            From el In root.Descendants() _
            Where (el.ShallowValue().StartsWith("=")) _
            Select el

        For Each q As XElement In query
            Console.WriteLine("{0}{1}{2}", _
                q.Name.ToString().PadRight(8), _
                q.Attribute("Name").ToString().PadRight(20), _
                q.ShallowValue())
        Next
    End Sub
End Module
```

Esse exemplo gera a saída a seguir:

```console
Column  Name="CustomerId"   =Customer.CustomerId.Heading
Column  Name="Name"         =Customer.Name.Heading
Column  Name="CustomerId"   =Customer.CustomerId
Column  Name="Name"         =Customer.Name
```

## <a name="see-also"></a>Confira também

- [Eixos LINQ to XML (Visual Basic)](linq-to-xml-axes.md)
