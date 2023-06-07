---
title: Escrever consultas LINQ em C#
description: Saiba como escrever consultas LINQ em C#.
ms.date: 12/01/2016
ms.assetid: 30703f79-cf3a-4d02-b892-c95d58a1d9ed
ms.openlocfilehash: bd7da81f2873c6a25570cab32fafecc66fd98be4
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063439"
---
# <a name="write-linq-queries-in-c"></a>Escrever consultas LINQ em C\#

Este artigo mostra as três maneiras de escrever uma consulta LINQ em C#:

1. Usar a sintaxe de consulta.

2. Usar a sintaxe do método.

3. Usar uma combinação da sintaxe de consulta e da sintaxe de método.

Os exemplos a seguir demonstram algumas consultas LINQ simples usando cada abordagem listada anteriormente. Em geral, a regra é usar (1) sempre que possível e usar (2) e (3) sempre que necessário.

> [!NOTE]
> Essas consultas funcionam em coleções na memória simples, no entanto, a sintaxe básica é idêntica àquela usada no LINQ to Entities e no LINQ to XML.

## <a name="example---query-syntax"></a>Exemplo – sintaxe de consulta

A maneira recomendada de escrever a maioria das consultas é usar a *sintaxe de consulta* para criar *expressões de consulta*. O exemplo a seguir mostra três expressões de consulta. A primeira expressão de consulta demonstra como filtrar ou restringir os resultados aplicando condições com uma cláusula `where`. Ela retorna todos os elementos na sequência de origem cujos valores são maiores que 7 ou menores que 3. A segunda expressão demonstra como ordenar os resultados retornados. A terceira expressão demonstra como agrupar resultados de acordo com uma chave. Esta consulta retorna dois grupos com base na primeira letra da palavra.

[!code-csharp[csProgGuideLINQ#5](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_1.cs)]

Observe que o tipo das consultas é <xref:System.Collections.Generic.IEnumerable%601>. Todas essas consultas poderiam ser escritas usando `var` conforme mostrado no exemplo a seguir:

`var query = from num in numbers...`

Em cada exemplo anterior, as consultas não são de fato executadas até você iterar na variável de consulta em uma instrução `foreach` ou outra instrução. Para obter mais informações, consulte [Introdução a Consultas LINQ](../programming-guide/concepts/linq/introduction-to-linq-queries.md).

## <a name="example---method-syntax"></a>Exemplo – sintaxe de método

Algumas operações de consulta devem ser expressas como uma chamada de método. Os mais comuns desses métodos são aqueles que retornam valores numéricos singleton como <xref:System.Linq.Enumerable.Sum%2A>, <xref:System.Linq.Enumerable.Max%2A>, <xref:System.Linq.Enumerable.Min%2A>, <xref:System.Linq.Enumerable.Average%2A> e assim por diante. Esses métodos devem sempre ser chamados por último em qualquer consulta porque representam apenas um único valor e não podem atuar como a fonte para uma operação de consulta adicional. O exemplo a seguir mostra uma chamada de método em uma expressão de consulta:

[!code-csharp[csProgGuideLINQ#6](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_2.cs)]

Se o método tiver parâmetros Action ou Func, eles serão fornecidos na forma de uma expressão [lambda](../language-reference/operators/lambda-expressions.md) , conforme mostrado no exemplo a seguir:

[!code-csharp[csProgGuideLINQ#7](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_3.cs)]

Nas consultas anteriores, apenas a Query #4 é executada imediatamente. Isso ocorre porque ele retorna um único valor e não uma coleção <xref:System.Collections.Generic.IEnumerable%601> genérica. O próprio método tem que usar `foreach` para calcular seu valor.

Cada uma das consultas anteriores pode ser escrita usando a tipagem implícita com [var](../language-reference/keywords/var.md), como mostrado no exemplo a seguir:

[!code-csharp[csProgGuideLINQ#8](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_4.cs)]

## <a name="example---mixed-query-and-method-syntax"></a>Exemplo – sintaxe mista de consulta e do método

Este exemplo mostra como usar a sintaxe do método nos resultados de uma cláusula de consulta. Simplesmente coloque a expressão de consulta entre parênteses e, em seguida, aplique o operador de ponto e chame o método. No exemplo a seguir, a Query #7 retorna uma contagem dos números cujo valor está entre 3 e 7. Em geral, no entanto, é melhor usar uma segunda variável para armazenar o resultado da chamada do método. Dessa forma, é menos provável que a consulta seja confundida com os resultados da consulta.

[!code-csharp[csProgGuideLINQ#9](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_5.cs)]

Como a Query #7 retorna um único valor e não uma coleção, a consulta é executada imediatamente.

A consulta anterior pode ser escrita usando a tipagem implícita com `var`, da seguinte maneira:

```csharp
var numCount = (from num in numbers...
```

Ela pode ser escrita na sintaxe de método da seguinte maneira:

```csharp
var numCount = numbers.Where(n => n < 3 || n > 7).Count();
```

Ela pode ser escrita usando a tipagem explícita da seguinte maneira:

```csharp
int numCount = numbers.Where(n => n < 3 || n > 7).Count();
```

## <a name="see-also"></a>Consulte também

- [Passo a passo: escrevendo consultas em C#](../programming-guide/concepts/linq/walkthrough-writing-queries-linq.md)
- [LINQ (Consulta Integrada à Linguagem)](index.md)
- [cláusula WHERE](../language-reference/keywords/where-clause.md)
