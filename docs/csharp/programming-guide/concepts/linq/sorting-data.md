---
title: Classificando dados (C#)
description: Saiba mais sobre operações de classificação e os métodos de operador de consulta padrão que executam operações de classificação em LINQ em C#.
ms.date: 07/20/2015
ms.assetid: d93fa055-2f19-46d2-9898-e2aed628f1c9
ms.openlocfilehash: 5feeb0e2229fc370fdcb9608817f41832bffd7cc
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302328"
---
# <a name="sorting-data-c"></a>Classificando dados (C#)
Uma operação de classificação ordena os elementos de uma sequência com base em um ou mais atributos. O primeiro critério de classificação executa uma classificação primária dos elementos. Especificando um segundo critério de classificação, você pode classificar os elementos dentro de cada grupo de classificação primário.  
  
 A ilustração a seguir mostra os resultados de uma operação de classificação alfabética em uma sequência de caracteres:
  
 ![Gráfico que mostra uma operação de classificação alfabética.](./media/sorting-data/alphabetical-sort-operation.png)  
  
 Os métodos de operador de consulta padrão que classificam dados estão listados na seção a seguir.  
  
## <a name="methods"></a>Métodos  
  
|Nome do método|Descrição|Sintaxe de expressão de consulta C#|Mais informações|  
|-----------------|-----------------|---------------------------------|----------------------|  
|OrderBy|Classifica valores em ordem crescente.|`orderby`|<xref:System.Linq.Enumerable.OrderBy%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OrderBy%2A?displayProperty=nameWithType>|  
|OrderByDescending|Classifica valores em ordem decrescente.|`orderby … descending`|<xref:System.Linq.Enumerable.OrderByDescending%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OrderByDescending%2A?displayProperty=nameWithType>|  
|ThenBy|Executa uma classificação secundária em ordem crescente.|`orderby …, …`|<xref:System.Linq.Enumerable.ThenBy%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ThenBy%2A?displayProperty=nameWithType>|  
|ThenByDescending|Executa uma classificação secundária em ordem decrescente.|`orderby …, … descending`|<xref:System.Linq.Enumerable.ThenByDescending%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ThenByDescending%2A?displayProperty=nameWithType>|  
|Reverse|Inverte a ordem dos elementos em uma coleção.|Não aplicável.|<xref:System.Linq.Enumerable.Reverse%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Reverse%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-examples"></a>Exemplos de sintaxe de expressão de consulta  
  
### <a name="primary-sort-examples"></a>Exemplos de classificação primária  
  
#### <a name="primary-ascending-sort"></a>Classificação crescente primária  
 O exemplo a seguir demonstra como usar a cláusula `orderby` em uma consulta de LINQ para classificar as cadeias de caracteres em uma matriz segundo o tamanho da cadeia de caracteres, em ordem crescente.  
  
```csharp  
string[] words = { "the", "quick", "brown", "fox", "jumps" };  
  
IEnumerable<string> query = from word in words  
                            orderby word.Length  
                            select word;  
  
foreach (string str in query)  
    Console.WriteLine(str);  
  
/* This code produces the following output:  
  
    the  
    fox  
    quick  
    brown  
    jumps  
*/  
```  
  
#### <a name="primary-descending-sort"></a>Classificação decrescente primária  
 O exemplo seguinte demonstra como usar a cláusula `orderby descending` em uma consulta de LINQ para classificar as cadeias de caracteres segundo sua primeira letra, em ordem decrescente.  
  
```csharp  
string[] words = { "the", "quick", "brown", "fox", "jumps" };  
  
IEnumerable<string> query = from word in words  
                            orderby word.Substring(0, 1) descending  
                            select word;  
  
foreach (string str in query)  
    Console.WriteLine(str);  
  
/* This code produces the following output:  
  
    the  
    quick  
    jumps  
    fox  
    brown  
*/  
```  
  
### <a name="secondary-sort-examples"></a>Exemplos de classificação secundária  
  
#### <a name="secondary-ascending-sort"></a>Classificação crescente secundária  
 O exemplo a seguir demonstra como usar a cláusula `orderby` em uma consulta de LINQ para executar uma classificação primária e uma classificação secundária das cadeias de caracteres em uma matriz. As cadeias de caracteres são classificadas primeiro segundo o tamanho e, depois, segundo a primeira letra da cadeia de caracteres, em ordem crescente.  
  
```csharp  
string[] words = { "the", "quick", "brown", "fox", "jumps" };  
  
IEnumerable<string> query = from word in words  
                            orderby word.Length, word.Substring(0, 1)  
                            select word;  
  
foreach (string str in query)  
    Console.WriteLine(str);  
  
/* This code produces the following output:  
  
    fox  
    the  
    brown  
    jumps  
    quick  
*/  
```  
  
#### <a name="secondary-descending-sort"></a>Classificação decrescente secundária  
 O exemplo seguinte demonstra como usar a cláusula `orderby descending` em uma consulta de LINQ para executar uma classificação primária, em ordem crescente e uma classificação secundária, em ordem decrescente. As cadeias de caracteres são classificadas primeiro segundo o tamanho e, depois, segundo a primeira letra da cadeia de caracteres.  
  
```csharp  
string[] words = { "the", "quick", "brown", "fox", "jumps" };  
  
IEnumerable<string> query = from word in words  
                            orderby word.Length, word.Substring(0, 1) descending  
                            select word;  
  
foreach (string str in query)  
    Console.WriteLine(str);  
  
/* This code produces the following output:  
  
    the  
    fox  
    quick  
    jumps  
    brown  
*/  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md)
- [Cláusula orderby](../../../language-reference/keywords/orderby-clause.md)
- [Ordenar os resultados de uma cláusula join](../../../linq/order-the-results-of-a-join-clause.md)
- [Como classificar ou filtrar dados de texto por qualquer palavra ou campo (LINQ) (C#)](./how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)
