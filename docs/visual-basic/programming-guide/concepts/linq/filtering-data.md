---
title: Filtrar dados
ms.date: 07/20/2015
ms.assetid: 7749519a-7edc-49fe-aef9-6a353864af6c
ms.openlocfilehash: f7a1aa76dc93cc03952e55f5f8fc3f75176a3f9f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84383411"
---
# <a name="filtering-data-visual-basic"></a>Filtrando dados (Visual Basic)

A filtragem é a operação de restringir o conjunto de resultados de forma que ele contenha apenas os elementos correspondentes a uma condição especificada. Ela também é conhecida como seleção.

A ilustração a seguir mostra os resultados da filtragem de uma sequência de caracteres. O predicado para a operação de filtragem especifica que o caractere deve ser "A".

![Diagrama que mostra uma operação de filtragem do LINQ](./media/filtering-data/linq-filter-operation.png)

Os métodos de operador de consulta padrão que realizam a seleção estão listados na seção a seguir.

## <a name="methods"></a>Métodos

|Nome do método|Descrição|Visual Basic sintaxe de expressão de consulta|Mais informações|
|-----------------|-----------------|------------------------------------------|----------------------|
|OfType|Seleciona valores, dependendo da capacidade de serem convertidos em um tipo especificado.|Não aplicável.|<xref:System.Linq.Enumerable.OfType%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OfType%2A?displayProperty=nameWithType>|
|Where|Seleciona valores com base em uma função de predicado.|`Where`|<xref:System.Linq.Enumerable.Where%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Where%2A?displayProperty=nameWithType>|

## <a name="query-expression-syntax-example"></a>Exemplo de sintaxe de expressão de consulta

O exemplo a seguir usa o `Where` para filtrar de uma matriz de cadeias de caracteres que têm um comprimento específico.

```vb
Dim words() As String = {"the", "quick", "brown", "fox", "jumps"}

Dim query = From word In words
            Where word.Length = 3
            Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In query
    sb.AppendLine(str)
Next

' Display the results.
MsgBox(sb.ToString())

' This code produces the following output:

' the
' fox
```

## <a name="see-also"></a>Confira também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md)
- [Cláusula WHERE](../../../language-reference/queries/where-clause.md)
- [Como filtrar resultados de consulta](../../language-features/linq/how-to-filter-query-results-by-using-linq.md)
- [Como consultar metadados de um assembly com reflexão (LINQ) (Visual Basic)](how-to-query-an-assembly-s-metadata-with-reflection-linq.md)
- [Como consultar arquivos com um atributo ou nome especificado (Visual Basic)](how-to-query-for-files-with-a-specified-attribute-or-name.md)
- [Como classificar ou filtrar dados de texto por qualquer palavra ou campo (LINQ) (Visual Basic)](how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)
