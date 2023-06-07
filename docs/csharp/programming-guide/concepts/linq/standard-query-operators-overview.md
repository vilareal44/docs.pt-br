---
title: Visão geral de operadores de consulta padrão (C#)
description: Os operadores de consulta padrão do LINQ fornecem recursos de consulta, incluindo filtragem, projeção, agregação e classificação em C#.
ms.date: 07/20/2015
ms.assetid: 812fa119-5f65-4139-b4fa-55dccd8dc3ac
ms.openlocfilehash: 8a399f52881e10f8d94263843b5992101f96a5ea
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302315"
---
# <a name="standard-query-operators-overview-c"></a>Visão geral de operadores de consulta padrão (C#)
Os *operadores de consulta padrão* são os métodos que formam o padrão LINQ. A maioria desses métodos opera em sequências; neste contexto, uma sequência é um objeto cujo tipo implementa a interface <xref:System.Collections.Generic.IEnumerable%601> ou a interface <xref:System.Linq.IQueryable%601>. Os operadores de consulta padrão fornecem recursos de consulta incluindo filtragem, projeção, agregação, classificação e muito mais.  
  
 Há dois conjuntos de operadores de consulta padrão do LINQ: um que opera em objetos do tipo <xref:System.Collections.Generic.IEnumerable%601> , outro que opera em objetos do tipo <xref:System.Linq.IQueryable%601> . Os métodos que compõem a cada conjunto são os membros estáticos das classes <xref:System.Linq.Enumerable> e <xref:System.Linq.Queryable>, respectivamente. Eles são definidos como *métodos de extensão* do tipo nos quais operam. Os métodos de extensão podem ser chamados usando a sintaxe do método estático ou a sintaxe do método de instância.  
  
 Além disso, vários métodos de operador de consulta padrão operam em tipos diferentes daqueles baseados em <xref:System.Collections.Generic.IEnumerable%601> ou <xref:System.Linq.IQueryable%601>. O tipo <xref:System.Linq.Enumerable> define dois métodos tais que ambos operam em objetos do tipo <xref:System.Collections.IEnumerable>. Esses métodos, <xref:System.Linq.Enumerable.Cast%60%601%28System.Collections.IEnumerable%29> e <xref:System.Linq.Enumerable.OfType%60%601%28System.Collections.IEnumerable%29>, permitem que você habilite uma coleção sem parâmetros ou não genérica, a ser consultada no padrão LINQ. Eles fazem isso criando uma coleção fortemente tipada de objetos. A classe <xref:System.Linq.Queryable> define dois métodos semelhantes, <xref:System.Linq.Queryable.Cast%60%601%28System.Linq.IQueryable%29> e <xref:System.Linq.Queryable.OfType%60%601%28System.Linq.IQueryable%29>, que operam em objetos do tipo <xref:System.Linq.Queryable>.  
  
 Os operadores de consulta padrão são diferentes no momento de sua execução, dependendo de se eles retornam um valor singleton ou uma sequência de valores. Esses métodos que retornam um valor singleton (por exemplo, <xref:System.Linq.Enumerable.Average%2A> e <xref:System.Linq.Enumerable.Sum%2A>) são executados imediatamente. Os métodos que retornam uma sequência adiam a execução da consulta e retornam um objeto enumerável.  
  
 Para métodos que operam em coleções na memória, ou seja, os métodos que se estendem <xref:System.Collections.Generic.IEnumerable%601> , o objeto Enumerable retornado captura os argumentos que foram passados para o método. Quando esse objeto é enumerado, a lógica do operador de consulta é empregada e os resultados da consulta são retornados.  
  
 Por outro lado, os métodos que se estendem <xref:System.Linq.IQueryable%601> não implementam nenhum comportamento de consulta. Eles criam uma árvore de expressão que representa a consulta a ser executada. O processamento de consulta é tratado pelo objeto <xref:System.Linq.IQueryable%601> de origem.  
  
 Chamadas para métodos de consulta podem ser encadeadas em uma consulta, o que permite que consultas se tornem arbitrariamente complexas.  
  
 O exemplo de código a seguir demonstra como os operadores de consulta padrão podem ser usados para obter informações sobre uma sequência.  
  
```csharp  
string sentence = "the quick brown fox jumps over the lazy dog";  
// Split the string into individual words to create a collection.  
string[] words = sentence.Split(' ');  
  
// Using query expression syntax.  
var query = from word in words  
            group word.ToUpper() by word.Length into gr  
            orderby gr.Key  
            select new { Length = gr.Key, Words = gr };  
  
// Using method-based query syntax.  
var query2 = words.  
    GroupBy(w => w.Length, w => w.ToUpper()).  
    Select(g => new { Length = g.Key, Words = g }).  
    OrderBy(o => o.Length);  
  
foreach (var obj in query)  
{  
    Console.WriteLine("Words of length {0}:", obj.Length);  
    foreach (string word in obj.Words)  
        Console.WriteLine(word);  
}  
  
// This code example produces the following output:  
//  
// Words of length 3:  
// THE  
// FOX  
// THE  
// DOG  
// Words of length 4:  
// OVER  
// LAZY  
// Words of length 5:  
// QUICK  
// BROWN  
// JUMPS
```  
  
## <a name="query-expression-syntax"></a>Sintaxe de expressão de consulta  
 Alguns dos operadores de consulta padrão mais usados têm uma sintaxe de palavra-chave de linguagem C# e Visual Basic dedicada que possibilita que eles sejam chamados como parte de uma *expressão* *de consulta*. Para obter mais informações sobre operadores de consulta padrão que têm palavras-chave dedicadas e suas sintaxes correspondentes, consulte [Sintaxe de expressão de consulta para operadores de consulta padrão (C#)](./query-expression-syntax-for-standard-query-operators.md).  
  
## <a name="extending-the-standard-query-operators"></a>Estendendo os operadores de consulta padrão  
 Você pode aumentar o conjunto de operadores de consulta padrão criando métodos específicos de domínio apropriados para o domínio ou tecnologia de destino. Você também pode substituir os operadores de consulta padrão por suas próprias implementações que fornecem serviços adicionais, como avaliação remota, conversão de consulta e otimização. Para ver um exemplo, consulte <xref:System.Linq.Enumerable.AsEnumerable%2A>.  
  
## <a name="related-sections"></a>Seções relacionadas  
 Os links a seguir levam você a artigos que fornecem informações adicionais sobre os vários operadores de consulta padrão com base na funcionalidade.  
  
 [Classificando dados (C#)](./sorting-data.md)  
  
 [Operações de conjunto (C#)](./set-operations.md)  
  
 [Filtrando dados (C#)](./filtering-data.md)  
  
 [Operações de quantificador (C#)](./quantifier-operations.md)  
  
 [Operações de projeção (C#)](./projection-operations.md)  
  
 [Particionando dados (C#)](./partitioning-data.md)  
  
 [Operações de junção (C#)](./join-operations.md)  
  
 [Agrupando dados (C#)](./grouping-data.md)  
  
 [Operações de geração (C#)](./generation-operations.md)  
  
 [Operações de Igualdade (C#)](./equality-operations.md)  
  
 [Operações de elemento (C#)](./element-operations.md)  
  
 [Convertendo Tipos de Dados (C#)](./converting-data-types.md)  
  
 [Operações de concatenação (C#)](./concatenation-operations.md)  
  
 [Operações de agregação (C#)](./aggregation-operations.md)  
  
## <a name="see-also"></a>Veja também

- <xref:System.Linq.Enumerable>
- <xref:System.Linq.Queryable>
- [Introdução a consultas LINQ (C#)](./introduction-to-linq-queries.md)
- [Sintaxe de expressão de consulta para operadores de consulta padrão (C#)](./query-expression-syntax-for-standard-query-operators.md)
- [Classificação de operadores de consulta padrão pelo modo de execução (C#)](./classification-of-standard-query-operators-by-manner-of-execution.md)
- [Métodos de Extensão](../../classes-and-structs/extension-methods.md)
