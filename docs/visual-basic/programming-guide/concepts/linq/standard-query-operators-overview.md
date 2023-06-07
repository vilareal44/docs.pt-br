---
title: Visão geral de operadores de consulta padrão
ms.date: 07/20/2015
ms.assetid: 302bd39e-2ec1-495b-94bf-37d370d6f05f
ms.openlocfilehash: 7c229a576f6695282473352d6253d2c699c76604
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406775"
---
# <a name="standard-query-operators-overview-visual-basic"></a>Visão geral de operadores de consulta padrão (Visual Basic)

Os *operadores de consulta padrão* são os métodos que formam o padrão LINQ. A maioria desses métodos opera em sequências; neste contexto, uma sequência é um objeto cujo tipo implementa a interface <xref:System.Collections.Generic.IEnumerable%601> ou a interface <xref:System.Linq.IQueryable%601>. Os operadores de consulta padrão fornecem recursos de consulta incluindo filtragem, projeção, agregação, classificação e muito mais.

Há dois conjuntos de operadores de consulta padrão LINQ, um que opera em objetos do tipo <xref:System.Collections.Generic.IEnumerable%601> e o outro que opera em objetos do tipo <xref:System.Linq.IQueryable%601>. Os métodos que compõem a cada conjunto são os membros estáticos das classes <xref:System.Linq.Enumerable> e <xref:System.Linq.Queryable>, respectivamente. Eles são definidos como *métodos de extensão* do tipo nos quais operam. Isso significa que eles podem ser chamados usando a sintaxe de método estático ou sintaxe de método de instância.

Além disso, vários métodos de operador de consulta padrão operam em tipos diferentes daqueles baseados em <xref:System.Collections.Generic.IEnumerable%601> ou <xref:System.Linq.IQueryable%601>. O tipo <xref:System.Linq.Enumerable> define dois métodos tais que ambos operam em objetos do tipo <xref:System.Collections.IEnumerable>. Esses métodos, <xref:System.Linq.Enumerable.Cast%60%601%28System.Collections.IEnumerable%29> e <xref:System.Linq.Enumerable.OfType%60%601%28System.Collections.IEnumerable%29>, permitem que você habilite uma coleção sem parâmetros ou não genérica, a ser consultada no padrão LINQ. Eles fazem isso criando uma coleção fortemente tipada de objetos. A classe <xref:System.Linq.Queryable> define dois métodos semelhantes, <xref:System.Linq.Queryable.Cast%60%601%28System.Linq.IQueryable%29> e <xref:System.Linq.Queryable.OfType%60%601%28System.Linq.IQueryable%29>, que operam em objetos do tipo <xref:System.Linq.Queryable>.

Os operadores de consulta padrão são diferentes no momento de sua execução, dependendo de se eles retornam um valor singleton ou uma sequência de valores. Esses métodos que retornam um valor singleton (por exemplo, <xref:System.Linq.Enumerable.Average%2A> e <xref:System.Linq.Enumerable.Sum%2A>) são executados imediatamente. Os métodos que retornam uma sequência adiam a execução da consulta e retornam um objeto enumerável.

No caso de métodos que operam em coleções na memória, ou seja, os métodos que estendem <xref:System.Collections.Generic.IEnumerable%601>, o objeto enumerável retornado captura o argumento que foi passado para o método. Quando esse objeto é enumerado, a lógica do operador de consulta é empregada e os resultados da consulta são retornados.

Por outro lado, os métodos que estendem <xref:System.Linq.IQueryable%601> não implementam nenhum comportamento de consulta, mas criam uma árvore de expressão que representa a consulta a ser executada. O processamento de consulta é tratado pelo objeto <xref:System.Linq.IQueryable%601> de origem.

Chamadas para métodos de consulta podem ser encadeadas em uma consulta, o que permite que consultas se tornem arbitrariamente complexas.

O exemplo de código a seguir demonstra como os operadores de consulta padrão podem ser usados para obter informações sobre uma sequência.

```vb
Dim sentence = "the quick brown fox jumps over the lazy dog"
' Split the string into individual words to create a collection.
Dim words = sentence.Split(" "c)

Dim query = From word In words
            Group word.ToUpper() By word.Length Into gr = Group
            Order By Length _
            Select Length, GroupedWords = gr

Dim output As New System.Text.StringBuilder
For Each obj In query
    output.AppendLine(String.Format("Words of length {0}:", obj.Length))
    For Each word As String In obj.GroupedWords
        output.AppendLine(word)
    Next
Next

'Display the output
MsgBox(output.ToString())

' This code example produces the following output:
'
' Words of length 3:
' THE
' FOX
' THE
' DOG
' Words of length 4:
' OVER
' LAZY
' Words of length 5:
' QUICK
' BROWN
' JUMPS
```

## <a name="query-expression-syntax"></a>Sintaxe de expressão de consulta

Alguns dos operadores de consulta padrão mais usados têm uma sintaxe de palavra-chave de linguagem C# e Visual Basic dedicada que possibilita que eles sejam chamados como parte de uma *expressão* *de consulta*. Para obter mais informações sobre operadores de consulta padrão que têm palavras-chave dedicadas e suas sintaxes correspondentes, consulte [sintaxe de expressão de consulta para operadores de consulta padrão (Visual Basic)](query-expression-syntax-for-standard-query-operators.md).

## <a name="extending-the-standard-query-operators"></a>Estendendo os operadores de consulta padrão

Você pode aumentar o conjunto de operadores de consulta padrão criando métodos específicos de domínio apropriados para o domínio ou tecnologia de destino. Você também pode substituir os operadores de consulta padrão por suas próprias implementações que fornecem serviços adicionais, como avaliação remota, conversão de consulta e otimização. Para ver um exemplo, consulte <xref:System.Linq.Enumerable.AsEnumerable%2A>.

## <a name="related-sections"></a>Seções relacionadas

Os links a seguir levam você a tópicos que fornecem informações adicionais sobre os vários operadores de consulta padrão com base na funcionalidade.

- [Classificar dados](sorting-data.md)

- [Operações Set (Visual Basic)](set-operations.md)

- [Filtrando dados (Visual Basic)](filtering-data.md)

- [Operações do quantificador (Visual Basic)](quantifier-operations.md)

- [Operações de projeção (Visual Basic)](projection-operations.md)

- [Dados de particionamento (Visual Basic)](partitioning-data.md)

- [Operações de junção (Visual Basic)](join-operations.md)

- [Agrupando dados (Visual Basic)](grouping-data.md)

- [Operações de geração (Visual Basic)](generation-operations.md)

- [Operações de igualdade (Visual Basic)](equality-operations.md)

- [Operações de elemento (Visual Basic)](element-operations.md)

- [Convertendo tipos de dados (Visual Basic)](converting-data-types.md)

- [Operações de concatenação (Visual Basic)](concatenation-operations.md)

- [Operações de agregação (Visual Basic)](aggregation-operations.md)

## <a name="see-also"></a>Confira também

- <xref:System.Linq.Enumerable>
- <xref:System.Linq.Queryable>
- [Introdução ao LINQ (Visual Basic)](introduction-to-linq.md)
- [Sintaxe de expressão de consulta para operadores de consulta padrão (Visual Basic)](query-expression-syntax-for-standard-query-operators.md)
- [Classificação de operadores de consulta padrão por meio de execução (Visual Basic)](classification-of-standard-query-operators-by-manner-of-execution.md)
- [Métodos de Extensão](../../language-features/procedures/extension-methods.md)
