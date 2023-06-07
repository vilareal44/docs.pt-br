---
title: Operações de projeção
ms.date: 07/20/2015
ms.assetid: b8d38e6d-21cf-4619-8dbb-94476f4badc7
ms.openlocfilehash: 4795bdaba53949b34fe380ea9c51025ce43c40db
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396331"
---
# <a name="projection-operations-visual-basic"></a>Operações de projeção (Visual Basic)

Projeção refere-se à operação de transformar um objeto em um novo formulário que geralmente consiste apenas nas propriedades que serão usadas posteriormente. Usando a projeção, você pode construir um novo tipo que é criado de cada objeto. É possível projetar uma propriedade e executar uma função matemática nela. Também é possível projetar o objeto original sem alterá-lo.

Os métodos de operador de consulta padrão que realizam a projeção estão listados na seção a seguir.

## <a name="methods"></a>Métodos

|Nome do método|Descrição|Visual Basic sintaxe de expressão de consulta|Mais informações|
|-----------------|-----------------|------------------------------------------|----------------------|
|Selecionar|Projeta valores com base em uma função de transformação.|`Select`|<xref:System.Linq.Enumerable.Select%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Select%2A?displayProperty=nameWithType>|
|SelectMany|Projeta sequências de valores baseados em uma função de transformação e os mescla em uma sequência.|Use várias cláusulas `From`|<xref:System.Linq.Enumerable.SelectMany%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SelectMany%2A?displayProperty=nameWithType>|

## <a name="query-expression-syntax-examples"></a>Exemplos de sintaxe de expressão de consulta

### <a name="select"></a>Selecionar

O exemplo a seguir usa a cláusula `Select` para projetar a primeira letra de cada cadeia de caracteres em uma lista de cadeias de caracteres.

```vb
Dim words = New List(Of String) From {"an", "apple", "a", "day"}

Dim query = From word In words
            Select word.Substring(0, 1)

Dim sb As New System.Text.StringBuilder()
For Each letter As String In query
    sb.AppendLine(letter)
Next

' Display the output.
MsgBox(sb.ToString())

' This code produces the following output:

' a
' a
' a
' d
```

### <a name="selectmany"></a>SelectMany

O exemplo a seguir usa várias `From` cláusulas para projetar cada palavra de cada uma delas em uma lista de cadeias de caracteres.

```vb
Dim phrases = New List(Of String) From {"an apple a day", "the quick brown fox"}

Dim query = From phrase In phrases
            From word In phrase.Split(" "c)
            Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In query
    sb.AppendLine(str)
Next

' Display the output.
MsgBox(sb.ToString())

' This code produces the following output:

' an
' apple
' a
' day
' the
' quick
' brown
' fox
```

## <a name="select-versus-selectmany"></a>Select versus SelectMany

O trabalho de `Select()` e `SelectMany()` é produzir um valor (ou valores) de resultado dos valores de origem. `Select()` produz um valor de resultado para cada valor de origem. O resultado geral, portanto, é uma coleção que tem o mesmo número de elementos que a coleção de origem. Por outro lado, `SelectMany()` produz um único resultado geral que contém subcoleções concatenadas de cada valor de origem. A função de transformação passada como um argumento para `SelectMany()` deve retornar uma sequência enumerável de valores para cada valor de origem. Essas sequências enumeráveis, então, são concatenadas por `SelectMany()` para criar uma sequência grande.

As duas ilustrações a seguir mostram a diferença conceitual entre as ações desses dois métodos. Em cada caso, presuma que a função de seletor (transformação) seleciona a matriz de flores de cada valor de origem.

A ilustração mostra como `Select()` retorna uma coleção que tem o mesmo número de elementos que a coleção de origem.

![Gráfico que mostra a ação de Select&#40;&#41;](./media/projection-operations/select-action-graphic.png)

Esta ilustração mostra como `SelectMany()` concatena a sequência intermediária de matrizes em um valor de resultado final que contém cada valor de cada matriz intermediária.

![Gráfico mostrando a ação de SelectMany&#40;&#41;.](./media/projection-operations/select-many-action-graphic.png )

### <a name="code-example"></a>Exemplo de código

O exemplo a seguir compara o comportamento de `Select()` e de `SelectMany()`. O código cria um "buquê" de flores usando os dois primeiros itens de cada lista de nomes de flor na coleção de origem. Neste exemplo, o "valor único" que a função de transformação <xref:System.Linq.Enumerable.Select%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%29> usa é uma coleção de valores. Isso requer o loop `For Each` extra para enumerar cada cadeia de caracteres em cada subsequência.

```vb
Class Bouquet
    Public Flowers As List(Of String)
End Class

Sub SelectVsSelectMany()
    Dim bouquets = New List(Of Bouquet) From {
        New Bouquet With {.Flowers = New List(Of String)(New String() {"sunflower", "daisy", "daffodil", "larkspur"})},
        New Bouquet With {.Flowers = New List(Of String)(New String() {"tulip", "rose", "orchid"})},
        New Bouquet With {.Flowers = New List(Of String)(New String() {"gladiolis", "lily", "snapdragon", "aster", "protea"})},
        New Bouquet With {.Flowers = New List(Of String)(New String() {"larkspur", "lilac", "iris", "dahlia"})}}

    Dim output As New System.Text.StringBuilder

    ' Select()
    Dim query1 = bouquets.Select(Function(b) b.Flowers)

    output.AppendLine("Using Select():")
    For Each flowerList In query1
        For Each str As String In flowerList
            output.AppendLine(str)
        Next
    Next

    ' SelectMany()
    Dim query2 = bouquets.SelectMany(Function(b) b.Flowers)

    output.AppendLine(vbCrLf & "Using SelectMany():")
    For Each str As String In query2
        output.AppendLine(str)
    Next

    ' Display the output
    MsgBox(output.ToString())

    ' This code produces the following output:
    '
    ' Using Select():
    ' sunflower
    ' daisy
    ' daffodil
    ' larkspur
    ' tulip
    ' rose
    ' orchid
    ' gladiolis
    ' lily
    ' snapdragon
    ' aster
    ' protea
    ' larkspur
    ' lilac
    ' iris
    ' dahlia

    ' Using SelectMany()
    ' sunflower
    ' daisy
    ' daffodil
    ' larkspur
    ' tulip
    ' rose
    ' orchid
    ' gladiolis
    ' lily
    ' snapdragon
    ' aster
    ' protea
    ' larkspur
    ' lilac
    ' iris
    ' dahlia

End Sub
```

## <a name="see-also"></a>Confira também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md)
- [Cláusula SELECT](../../../language-reference/queries/select-clause.md)
- [Como combinar dados com junções](../../language-features/linq/how-to-combine-data-with-linq-by-using-joins.md)
- [Como: popular coleções de objetos de várias fontes (LINQ) (Visual Basic)](how-to-populate-object-collections-from-multiple-sources-linq.md)
- [Como retornar um resultado de consulta LINQ como um tipo específico](../../language-features/linq/how-to-return-a-linq-query-result-as-a-specific-type.md)
- [Como dividir um arquivo em vários arquivos usando grupos (LINQ) (Visual Basic)](how-to-split-a-file-into-many-files-by-using-groups-linq.md)
