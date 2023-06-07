---
title: 'Como: filtrar em um elemento opcional'
ms.date: 07/20/2015
ms.assetid: a74b76ad-6889-4185-a189-d6ef2c63841e
ms.openlocfilehash: 9f03eee479e7c47e528a145d370472de6dfddd39
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84394357"
---
# <a name="how-to-filter-on-an-optional-element-visual-basic"></a>Como filtrar em um elemento opcional (Visual Basic)
Às vezes você deseja filtrar um elemento mesmo que você não tenha certeza ele existe em seu documento XML. A pesquisa deve ser executada de modo que se o elemento específico não tem o elemento filho, você não dispare uma exceção de referência nula filtragem para ele. No exemplo a seguir, o elemento de `Child5` não tiver um elemento filho de `Type` , mas a consulta ainda executa corretamente.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa o método de extensão de <xref:System.Xml.Linq.Extensions.Elements%2A> .  
  
```vb  
Dim root As XElement = _
    <Root>  
        <Child1>  
            <Text>Child One Text</Text>  
            <Type Value="Yes"/>  
        </Child1>  
        <Child2>  
            <Text>Child Two Text</Text>  
            <Type Value="Yes"/>  
        </Child2>  
        <Child3>  
            <Text>Child Three Text</Text>  
            <Type Value="No"/>  
        </Child3>  
        <Child4>  
            <Text>Child Four Text</Text>  
            <Type Value="Yes"/>  
        </Child4>  
        <Child5>  
            <Text>Child Five Text</Text>  
        </Child5>  
    </Root>  
Dim cList As IEnumerable(Of String) = _  
    From typeElement In root.Elements().<Type> _  
    Where typeElement.@Value = "Yes" _  
    Select typeElement.Parent.<Text>.Value  
Dim str As String  
For Each str In cList  
    Console.WriteLine(str)  
Next  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
Child One Text  
Child Two Text  
Child Four Text  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra a mesma consulta para XML que está em um namespace. Para obter mais informações, consulte [visão geral de namespaces (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md).  
  
```vb  
Imports <xmlns='http://www.adatum.com'>  
  
Module Module1  
    Sub Main()  
        Dim root As XElement = _  
            <Root>  
                <Child1>  
                    <Text>Child One Text</Text>  
                    <Type Value="Yes"/>  
                </Child1>  
                <Child2>  
                    <Text>Child Two Text</Text>  
                    <Type Value="Yes"/>  
                </Child2>  
                <Child3>  
                    <Text>Child Three Text</Text>  
                    <Type Value="No"/>  
                </Child3>  
                <Child4>  
                    <Text>Child Four Text</Text>  
                    <Type Value="Yes"/>  
                </Child4>  
                <Child5>  
                    <Text>Child Five Text</Text>  
                </Child5>  
            </Root>  
        Dim cList As IEnumerable(Of String) = _  
            From typeElement In root.Elements().<Type> _  
            Where typeElement.@Value = "Yes" _  
            Select typeElement.Parent.<Text>.Value  
        Dim str As String  
        For Each str In cList  
            Console.WriteLine(str)  
        Next  
    End Sub  
End Module  
```  
  
 Esse código gera a seguinte saída:  
  
```console  
Child One Text  
Child Two Text  
Child Four Text  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XElement.Attribute%2A?displayProperty=nameWithType>
- <xref:System.Xml.Linq.XContainer.Elements%2A?displayProperty=nameWithType>
- <xref:System.Xml.Linq.Extensions.Elements%2A?displayProperty=nameWithType>
- [Consultas básicas (LINQ to XML) (Visual Basic)](basic-queries-linq-to-xml.md)
- [Propriedade do Eixo Filho XML](../../../language-reference/xml-axis/xml-child-axis-property.md)
- [Propriedade de Eixo do Atributo XML](../../../language-reference/xml-axis/xml-attribute-axis-property.md)
- [Propriedade do Valor XML](../../../language-reference/xml-axis/xml-value-property.md)
- [Visão geral de operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md)
- [Operações de projeção (Visual Basic)](projection-operations.md)
