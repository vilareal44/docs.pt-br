---
title: Classificar dados
ms.date: 07/20/2015
ms.assetid: 6f81065c-0c89-4bf3-a6d8-442273f8810e
ms.openlocfilehash: a5ccff745995ed7f41731cf98fb7c49d3247d994
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406788"
---
# <a name="sorting-data-visual-basic"></a>Classificando dados (Visual Basic)

Uma operação de classificação ordena os elementos de uma sequência com base em um ou mais atributos. O primeiro critério de classificação executa uma classificação primária dos elementos. Especificando um segundo critério de classificação, você pode classificar os elementos dentro de cada grupo de classificação primário.

A ilustração a seguir mostra os resultados de uma operação de classificação alfabética em uma sequência de caracteres.

![Gráfico que mostra uma operação de classificação alfabética.](./media/sorting-data/alphabetical-sort-operation.png)

Os métodos de operador de consulta padrão que classificam dados estão listados na seção a seguir.

## <a name="methods"></a>Métodos

|Nome do método|Descrição|Visual Basic sintaxe de expressão de consulta|Mais informações|
|-----------------|-----------------|------------------------------------------|----------------------|
|OrderBy|Classifica valores em ordem crescente.|`Order By`|<xref:System.Linq.Enumerable.OrderBy%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OrderBy%2A?displayProperty=nameWithType>|
|OrderByDescending|Classifica valores em ordem decrescente.|`Order By … Descending`|<xref:System.Linq.Enumerable.OrderByDescending%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OrderByDescending%2A?displayProperty=nameWithType>|
|ThenBy|Executa uma classificação secundária em ordem crescente.|`Order By …, …`|<xref:System.Linq.Enumerable.ThenBy%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ThenBy%2A?displayProperty=nameWithType>|
|ThenByDescending|Executa uma classificação secundária em ordem decrescente.|`Order By …, … Descending`|<xref:System.Linq.Enumerable.ThenByDescending%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ThenByDescending%2A?displayProperty=nameWithType>|
|Reverse|Inverte a ordem dos elementos em uma coleção.|Não aplicável.|<xref:System.Linq.Enumerable.Reverse%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Reverse%2A?displayProperty=nameWithType>|

## <a name="query-expression-syntax-examples"></a>Exemplos de sintaxe de expressão de consulta

### <a name="primary-sort-examples"></a>Exemplos de classificação primária

#### <a name="primary-ascending-sort"></a>Classificação crescente primária

O exemplo a seguir demonstra como usar a cláusula `Order By` em uma consulta de LINQ para classificar as cadeias de caracteres em uma matriz segundo o tamanho da cadeia de caracteres, em ordem crescente.

```vb
Dim words = {"the", "quick", "brown", "fox", "jumps"}

Dim sortQuery = From word In words
                Order By word.Length
                Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In sortQuery
    sb.AppendLine(str)
Next

' Display the results.
MsgBox(sb.ToString())

' This code produces the following output:

' the
' fox
' quick
' brown
' jumps
```

#### <a name="primary-descending-sort"></a>Classificação decrescente primária

O exemplo seguinte demonstra como usar a cláusula `Order By Descending` em uma consulta de LINQ para classificar as cadeias de caracteres segundo sua primeira letra, em ordem decrescente.

```vb
Dim words = {"the", "quick", "brown", "fox", "jumps"}

Dim sortQuery = From word In words
                Order By word.Substring(0, 1) Descending
                Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In sortQuery
    sb.AppendLine(str)
Next

' Display the results.
MsgBox(sb.ToString())

' This code produces the following output:

' the
' quick
' jumps
' fox
' brown
```

### <a name="secondary-sort-examples"></a>Exemplos de classificação secundária

#### <a name="secondary-ascending-sort"></a>Classificação crescente secundária

O exemplo a seguir demonstra como usar a cláusula `Order By` em uma consulta de LINQ para executar uma classificação primária e uma classificação secundária das cadeias de caracteres em uma matriz. As cadeias de caracteres são classificadas primeiro segundo o tamanho e, depois, segundo a primeira letra da cadeia de caracteres, em ordem crescente.

```vb
Dim words = {"the", "quick", "brown", "fox", "jumps"}

Dim sortQuery = From word In words
                Order By word.Length, word.Substring(0, 1)
                Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In sortQuery
    sb.AppendLine(str)
Next

' Display the results.
MsgBox(sb.ToString())

' This code produces the following output:

' fox
' the
' brown
' jumps
' quick
```

#### <a name="secondary-descending-sort"></a>Classificação decrescente secundária

O exemplo seguinte demonstra como usar a cláusula `Order By Descending` em uma consulta de LINQ para executar uma classificação primária, em ordem crescente e uma classificação secundária, em ordem decrescente. As cadeias de caracteres são classificadas primeiro segundo o tamanho e, depois, segundo a primeira letra da cadeia de caracteres.

```vb
Dim words = {"the", "quick", "brown", "fox", "jumps"}

Dim sortQuery = From word In words
                Order By word.Length, word.Substring(0, 1) Descending
                Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In sortQuery
    sb.AppendLine(str)
Next

' Display the results.
MsgBox(sb.ToString())

' This code produces the following output:

' fox
' the
' quick
' jumps
' brown
```

## <a name="see-also"></a>Confira também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md)
- [Cláusula Order By](../../../language-reference/queries/order-by-clause.md)
- [Como classificar resultados de consulta](../../language-features/linq/how-to-sort-query-results-by-using-linq.md)
- [Como classificar ou filtrar dados de texto por qualquer palavra ou campo (LINQ) (Visual Basic)](how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)
