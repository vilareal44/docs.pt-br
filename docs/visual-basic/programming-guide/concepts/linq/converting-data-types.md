---
title: Converter tipos de dados
ms.date: 07/20/2015
ms.assetid: 9b0cf1ab-de48-4c6e-9f00-05b40fade46e
ms.openlocfilehash: 1394f53923ba850ae11fbc326a25c279589c3be1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410845"
---
# <a name="converting-data-types-visual-basic"></a>Convertendo tipos de dados (Visual Basic)

Os métodos de conversão alteram o tipo dos objetos de entrada.

 As operações de conversão em consultas LINQ são úteis em diversas aplicações. Veja a seguir alguns exemplos:

- O método <xref:System.Linq.Enumerable.AsEnumerable%2A?displayProperty=nameWithType> pode ser usado para ocultar a implementação personalizada de um tipo de um operador de consulta padrão.

- O método <xref:System.Linq.Enumerable.OfType%2A?displayProperty=nameWithType> pode ser usado para habilitar coleções sem parâmetros para consulta LINQ.

- Os métodos <xref:System.Linq.Enumerable.ToArray%2A?displayProperty=nameWithType>, <xref:System.Linq.Enumerable.ToDictionary%2A?displayProperty=nameWithType>, <xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> e <xref:System.Linq.Enumerable.ToLookup%2A?displayProperty=nameWithType> podem ser usados para forçar a execução de consulta imediata em vez de adiá-la até que a consulta seja enumerada.

## <a name="methods"></a>Métodos

A tabela a seguir lista os métodos de operador de consulta padrão que realizam conversões de tipo de dados.

Os métodos de conversão nesta tabela cujos nomes começam com "As" alteram o tipo estático da coleção de origem, mas não a enumeram. Os métodos cujos nomes começam com "To" enumeram a coleção de origem e colocam os itens na coleção de tipo correspondente.

|Nome do método|Descrição|Visual Basic sintaxe de expressão de consulta|Mais informações|
|-----------------|-----------------|------------------------------------------|----------------------|
|AsEnumerable|Retorna a entrada digitada como <xref:System.Collections.Generic.IEnumerable%601>.|Não aplicável.|<xref:System.Linq.Enumerable.AsEnumerable%2A?displayProperty=nameWithType>|
|AsQueryable|Converte um <xref:System.Collections.IEnumerable> (genérico) em um <xref:System.Linq.IQueryable> (genérico).|Não aplicável.|<xref:System.Linq.Queryable.AsQueryable%2A?displayProperty=nameWithType>|
|Conversão|Converte os elementos de uma coleção em um tipo especificado.|`From … As …`|<xref:System.Linq.Enumerable.Cast%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Cast%2A?displayProperty=nameWithType>|
|OfType|Filtra valores, dependendo da capacidade de serem convertidos em um tipo especificado.|Não aplicável.|<xref:System.Linq.Enumerable.OfType%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OfType%2A?displayProperty=nameWithType>|
|ToArray|Converte uma coleção em uma matriz. Esse método força a execução de consulta.|Não aplicável.|<xref:System.Linq.Enumerable.ToArray%2A?displayProperty=nameWithType>|
|ToDictionary|Coloca os elementos em um <xref:System.Collections.Generic.Dictionary%602> com base em uma função de seletor de chave. Esse método força a execução de consulta.|Não aplicável.|<xref:System.Linq.Enumerable.ToDictionary%2A?displayProperty=nameWithType>|
|ToList|Converte uma coleção em um <xref:System.Collections.Generic.List%601>. Esse método força a execução de consulta.|Não aplicável.|<xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType>|
|ToLookup|Coloca os elementos em um <xref:System.Linq.Lookup%602> (um dicionário one-to-many) com base em uma função de seletor de chave. Esse método força a execução de consulta.|Não aplicável.|<xref:System.Linq.Enumerable.ToLookup%2A?displayProperty=nameWithType>|

## <a name="query-expression-syntax-example"></a>Exemplo de sintaxe de expressão de consulta

O exemplo de código a seguir usa a `From As` cláusula para converter um tipo em um subtipo antes de acessar um membro que está disponível somente no subtipo.

```vb
Class Plant
    Public Property Name As String
End Class

Class CarnivorousPlant
    Inherits Plant
    Public Property TrapType As String
End Class

Sub Cast()

    Dim plants() As Plant = {
        New CarnivorousPlant With {.Name = "Venus Fly Trap", .TrapType = "Snap Trap"},
        New CarnivorousPlant With {.Name = "Pitcher Plant", .TrapType = "Pitfall Trap"},
        New CarnivorousPlant With {.Name = "Sundew", .TrapType = "Flypaper Trap"},
        New CarnivorousPlant With {.Name = "Waterwheel Plant", .TrapType = "Snap Trap"}}

    Dim query = From plant As CarnivorousPlant In plants
                Where plant.TrapType = "Snap Trap"
                Select plant

    Dim sb As New System.Text.StringBuilder()
    For Each plant In query
        sb.AppendLine(plant.Name)
    Next

    ' Display the results.
    MsgBox(sb.ToString())

    ' This code produces the following output:

    ' Venus Fly Trap
    ' Waterwheel Plant

End Sub
```

## <a name="see-also"></a>Confira também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md)
- [Cláusula from](../../../language-reference/queries/from-clause.md)
- [Como consultar um ArrayList com LINQ (Visual Basic)](how-to-query-an-arraylist-with-linq.md)
