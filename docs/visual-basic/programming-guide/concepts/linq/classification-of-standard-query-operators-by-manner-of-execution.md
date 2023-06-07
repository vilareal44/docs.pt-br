---
title: Classificação de operadores de consulta padrão por maneira de execução
ms.date: 07/20/2015
ms.assetid: 7f55b0be-9f6e-44f8-865c-6afbea50cc54
ms.openlocfilehash: e1ba5d8bdc2b7a521a11ca5c055323fde4bcb9d9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410897"
---
# <a name="classification-of-standard-query-operators-by-manner-of-execution-visual-basic"></a>Classificação de operadores de consulta padrão por meio de execução (Visual Basic)
As implementações de LINQ to Objects dos métodos de operador de consulta padrão em uma das duas maneiras principais: imediata ou adiada. Os operadores de consulta que usam a execução adiada podem ser divididos em mais duas categorias: streaming e não streaming. Se você souber como os operadores de consulta diferentes são executados, isso poderá ajudá-lo a entender os resultados que serão obtidos de uma determinada consulta. Isso é especialmente verdadeiro se a fonte de dados está sendo alterada ou se você estiver criando uma consulta sobre outra consulta. Este tópico classifica os operadores de consulta padrão de acordo com o modo de execução.  
  
## <a name="manners-of-execution"></a>Modos de execução  
  
### <a name="immediate"></a>Imediata  
 A execução imediata significa que a fonte de dados é lida e a operação é realizada no ponto do código em que a consulta é declarada. Todos os operadores de consulta padrão que retornam um resultado único e não enumerável são executados imediatamente.  
  
### <a name="deferred"></a>Adiado  
 A execução adiada significa que a operação não será realizada no ponto do código em que a consulta estiver declarada. A operação será realizada somente quando a variável de consulta for enumerada, por exemplo, usando uma instrução `For Each`. Isso significa que os resultados da execução da consulta dependerão do conteúdo da fonte de dados quando a consulta for executada em vez de quando a consulta for definida. Se a variável de consulta for enumerada várias vezes, os resultados poderão ser diferentes a cada vez. Quase todos os operadores de consulta padrão cujo tipo de retorno é <xref:System.Collections.Generic.IEnumerable%601> ou <xref:System.Linq.IOrderedEnumerable%601> executam de maneira adiada.  
  
 Os operadores de consulta que usam a execução adiada podem ser, adicionalmente, classificados como streaming ou não streaming.  
  
#### <a name="streaming"></a>Streaming  
 Operadores streaming não precisam ler todos os dados de origem antes de gerar elementos. No momento da execução, um operador streaming realiza sua operação em cada elemento de origem enquanto eles são lidos, gerando o elemento, se apropriado. Um operador streaming continua a ler os elementos de origem até que um elemento de resultado possa ser produzido. Isso significa que mais de um elemento de origem poderá ser lido para produzir um elemento de resultado.  
  
#### <a name="non-streaming"></a>Não streaming  
 Os operadores não streaming devem ler todos os dados de origem antes de produzirem um elemento de resultado. Operações como classificação ou agrupamento se enquadram nesta categoria. No momento da execução, os operadores de consulta não streaming leem todos os dados de origem, colocam-nos em uma estrutura de dados, realizam a operação e geram os elementos de resultado.  
  
## <a name="classification-table"></a>Tabela de classificação  
 A tabela a seguir classifica cada método de operador de consulta padrão de acordo com o respectivo método de execução.  
  
> [!NOTE]
> Se um operador estiver marcado em duas colunas, duas sequências de entrada estarão envolvidas na operação e cada sequência será avaliada de forma diferente. Nesses casos, a primeira sequência na lista de parâmetros é a que sempre será avaliada de maneira adiada e em modo streaming.  
  
|Operador de consulta padrão|Tipo de retorno|Execução Imediata|Execução adiada de streaming|Execução adiada de não streaming|  
|-----------------------------|-----------------|-------------------------|----------------------------------|---------------------------------------|  
|<xref:System.Linq.Enumerable.Aggregate%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.All%2A>|<xref:System.Boolean>|X|||  
|<xref:System.Linq.Enumerable.Any%2A>|<xref:System.Boolean>|X|||  
|<xref:System.Linq.Enumerable.AsEnumerable%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Average%2A>|Valor numérico único|X|||  
|<xref:System.Linq.Enumerable.Cast%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Concat%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Contains%2A>|<xref:System.Boolean>|X|||  
|<xref:System.Linq.Enumerable.Count%2A>|<xref:System.Int32>|X|||  
|<xref:System.Linq.Enumerable.DefaultIfEmpty%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Distinct%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.ElementAt%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.ElementAtOrDefault%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.Empty%2A>|<xref:System.Collections.Generic.IEnumerable%601>|X|||  
|<xref:System.Linq.Enumerable.Except%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X|X|  
|<xref:System.Linq.Enumerable.First%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.FirstOrDefault%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.GroupBy%2A>|<xref:System.Collections.Generic.IEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.GroupJoin%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X|X|  
<xref:System.Linq.Enumerable.Intersect%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X|X|  
|<xref:System.Linq.Enumerable.Join%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X|X|  
|<xref:System.Linq.Enumerable.Last%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.LastOrDefault%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.LongCount%2A>|<xref:System.Int64>|X|||  
|<xref:System.Linq.Enumerable.Max%2A>|Valor numérico único, TSource ou TResult|X|||  
|<xref:System.Linq.Enumerable.Min%2A>|Valor numérico único, TSource ou TResult|X|||  
|<xref:System.Linq.Enumerable.OfType%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.OrderBy%2A>|<xref:System.Linq.IOrderedEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.OrderByDescending%2A>|<xref:System.Linq.IOrderedEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.Range%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Repeat%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Reverse%2A>|<xref:System.Collections.Generic.IEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.Select%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.SelectMany%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.SequenceEqual%2A>|<xref:System.Boolean>|X|||  
|<xref:System.Linq.Enumerable.Single%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.SingleOrDefault%2A>|TSource|X|||  
|<xref:System.Linq.Enumerable.Skip%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.SkipWhile%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Sum%2A>|Valor numérico único|X|||  
|<xref:System.Linq.Enumerable.Take%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
<xref:System.Linq.Enumerable.TakeWhile%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.ThenBy%2A>|<xref:System.Linq.IOrderedEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.ThenByDescending%2A>|<xref:System.Linq.IOrderedEnumerable%601>|||X|  
|<xref:System.Linq.Enumerable.ToArray%2A>|Matriz de TSource|X|||  
|<xref:System.Linq.Enumerable.ToDictionary%2A>|<xref:System.Collections.Generic.Dictionary%602>|X|||  
|<xref:System.Linq.Enumerable.ToList%2A>|<xref:System.Collections.Generic.IList%601>|X|||  
|<xref:System.Linq.Enumerable.ToLookup%2A>|<xref:System.Linq.ILookup%602>|X|||  
|<xref:System.Linq.Enumerable.Union%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
|<xref:System.Linq.Enumerable.Where%2A>|<xref:System.Collections.Generic.IEnumerable%601>||X||  
  
## <a name="see-also"></a>Confira também

- <xref:System.Linq.Enumerable>
- [Visão geral de operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md)
- [Sintaxe de expressão de consulta para operadores de consulta padrão (Visual Basic)](query-expression-syntax-for-standard-query-operators.md)
- [LINQ to Objects (Visual Basic)](linq-to-objects.md)
