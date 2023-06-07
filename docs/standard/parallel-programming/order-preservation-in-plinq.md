---
title: Preservação da ordem em PLINQ
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, order preservation
ms.assetid: 10d202bc-19e1-4b5c-bbf1-9a977322a9ca
ms.openlocfilehash: 45752f3ffa64079079505934afd76e812daad7bd
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290649"
---
# <a name="order-preservation-in-plinq"></a>Preservação da ordem em PLINQ
Em PLINQ, o objetivo é maximizar o desempenho mantendo a exatidão. Uma consulta deve ser executada o mais rápido possível, mas ainda produzir os resultados corretos. Em alguns casos, a exatidão requer que a ordem da sequência de origem seja preservada. No entanto, a ordenação pode ser dispendiosa. Portanto, por padrão, o PLINQ não preserva a ordem da sequência de origem. Nesse sentido, o PLINQ assemelha-se a [!INCLUDE[vbtecdlinq](../../../includes/vbtecdlinq-md.md)], mas é diferente do LINQ to Objects que preserva a ordenação.  
  
 Para substituir o comportamento padrão, ative a preservação da ordem usando o operador <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> na sequência de origem. Posteriormente, você pode desativar a preservação da ordem na consulta usando o método <xref:System.Linq.ParallelEnumerable.AsUnordered%2A>. Nos dois métodos, a consulta é processada com base na heurística que determina se a consulta deverá ser executada como paralela ou sequencial. Para saber mais, veja [Noções básicas sobre agilização em PLINQ](understanding-speedup-in-plinq.md).  
  
 O exemplo a seguir mostra uma consulta paralela não ordenada que filtra todos os elementos que correspondem a uma condição, sem tentar ordenar os resultados de qualquer forma.  
  
 [!code-csharp[PLINQ#8](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#8)]
 [!code-vb[PLINQ#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#8)]  
  
 Essa consulta não necessariamente gera as primeiras 1.000 cidades que atendem à condição na sequência de origem, mas em vez disso, gera alguns conjuntos de 1.000 cidades que atendem à condição. Operadores de consulta PLINQ particionam a sequência de origem em várias subsequências que são processadas como tarefas simultâneas. Se a preservação da ordem não for especificada, os resultados de cada partição serão enviados para a próxima fase da consulta em uma ordem arbitrária. Além disso, uma partição pode gerar um subconjunto de seus resultados antes de continuar a processar os elementos restantes. A ordem resultante pode ser sempre diferente. O aplicativo não pode controlar isso porque depende da forma como o sistema operacional agenda os threads.  
  
 O exemplo a seguir substitui o comportamento padrão usando o operador <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> na sequência de origem. Isso garante que o método <xref:System.Linq.ParallelEnumerable.Take%2A> retornará as primeiras 1.000 cidades na sequência de origem que atendam à condição.  
  
 [!code-csharp[PLINQ#9](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#9)]
 [!code-vb[PLINQ#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#9)]  
  
 No entanto, essa consulta provavelmente não é executada tão rápido quanto a versão não ordenada já que ela deve controlar a ordenação original em toda as partições e, no tempo de mesclagem, verificar se a ordenação é consistente. Portanto, é recomendável usar <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> somente quando necessário e apenas para as partes da consulta que o exigem. Quando a preservação da ordem já não for necessária, use <xref:System.Linq.ParallelEnumerable.AsUnordered%2A> para desativá-la. O exemplo a seguir realiza isso compondo duas consultas.  
  
 [!code-csharp[PLINQ#6](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#6)]
 [!code-vb[PLINQ#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#6)]  
  
 Observe que o PLINQ preserva a ordenação de uma sequência produzida por operadores de imposição da ordem para o restante da consulta. Ou seja, operadores como <xref:System.Linq.ParallelEnumerable.OrderBy%2A> e <xref:System.Linq.ParallelEnumerable.ThenBy%2A> são tratados como se fossem seguidos por uma chamada para <xref:System.Linq.ParallelEnumerable.AsOrdered%2A>.  
  
## <a name="query-operators-and-ordering"></a>Operadores de consulta e ordenação  
 Os operadores de consulta a seguir apresentam a preservação da ordem em todas as demais operações de uma consulta ou até que <xref:System.Linq.ParallelEnumerable.AsUnordered%2A> seja chamado:  
  
- <xref:System.Linq.ParallelEnumerable.OrderBy%2A>  
  
- <xref:System.Linq.ParallelEnumerable.OrderByDescending%2A>  
  
- <xref:System.Linq.ParallelEnumerable.ThenBy%2A>  
  
- <xref:System.Linq.ParallelEnumerable.ThenByDescending%2A>  
  
 Os seguintes operadores de consulta PLINQ podem, em alguns casos, exigir sequências ordenadas de origem para produzir resultados corretos:  
  
- <xref:System.Linq.ParallelEnumerable.Reverse%2A>  
  
- <xref:System.Linq.ParallelEnumerable.SequenceEqual%2A>  
  
- <xref:System.Linq.ParallelEnumerable.TakeWhile%2A>  
  
- <xref:System.Linq.ParallelEnumerable.SkipWhile%2A>  
  
- <xref:System.Linq.ParallelEnumerable.Zip%2A>  
  
 Alguns operadores de consulta PLINQ se comportam de forma diferente, dependendo se a sequência de origem for ordenada ou desordenada. A tabela a seguir lista esses operadores.  
  
|Operador|Resultado quando a sequência de origem é ordenada|Resultado quando a sequência de origem é desordenada|  
|--------------|------------------------------------------------|--------------------------------------------------|  
|<xref:System.Linq.ParallelEnumerable.Aggregate%2A>|Saída não determinística para operações não associativas ou não comutativas|Saída não determinística para operações não associativas ou não comutativas|  
|<xref:System.Linq.ParallelEnumerable.All%2A>|Não aplicável|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.Any%2A>|Não aplicável|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.AsEnumerable%2A>|Não aplicável|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.Average%2A>|Saída não determinística para operações não associativas ou não comutativas|Saída não determinística para operações não associativas ou não comutativas|  
|<xref:System.Linq.ParallelEnumerable.Cast%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.Concat%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.Count%2A>|Não aplicável|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.DefaultIfEmpty%2A>|Não aplicável|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.Distinct%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.ElementAt%2A>|Retorna o elemento especificado|Elemento arbitrário|  
|<xref:System.Linq.ParallelEnumerable.ElementAtOrDefault%2A>|Retorna o elemento especificado|Elemento arbitrário|  
|<xref:System.Linq.ParallelEnumerable.Except%2A>|Resultados não ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.First%2A>|Retorna o elemento especificado|Elemento arbitrário|  
|<xref:System.Linq.ParallelEnumerable.FirstOrDefault%2A>|Retorna o elemento especificado|Elemento arbitrário|  
|<xref:System.Linq.ParallelEnumerable.ForAll%2A>|Executa de forma não determinística em paralelo|Executa de forma não determinística em paralelo|  
|<xref:System.Linq.ParallelEnumerable.GroupBy%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.GroupJoin%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.Intersect%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.Join%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.Last%2A>|Retorna o elemento especificado|Elemento arbitrário|  
|<xref:System.Linq.ParallelEnumerable.LastOrDefault%2A>|Retorna o elemento especificado|Elemento arbitrário|  
|<xref:System.Linq.ParallelEnumerable.LongCount%2A>|Não aplicável|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.Min%2A>|Não aplicável|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.OrderBy%2A>|Reordena a sequência|Inicia nova seção ordenada|  
|<xref:System.Linq.ParallelEnumerable.OrderByDescending%2A>|Reordena a sequência|Inicia nova seção ordenada|  
|<xref:System.Linq.ParallelEnumerable.Range%2A>|Não aplicável (mesmo padrão de <xref:System.Linq.ParallelEnumerable.AsParallel%2A>)|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.Repeat%2A>|Não aplicável (mesmo padrão de <xref:System.Linq.ParallelEnumerable.AsParallel%2A>)|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.Reverse%2A>|Inverte|Não agir|  
|<xref:System.Linq.ParallelEnumerable.Select%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.Select%2A> (indexado)|Resultados ordenados|Resultados não ordenados.|  
|<xref:System.Linq.ParallelEnumerable.SelectMany%2A>|Resultados ordenados.|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.SelectMany%2A> (indexado)|Resultados ordenados.|Resultados não ordenados.|  
|<xref:System.Linq.ParallelEnumerable.SequenceEqual%2A>|Comparação ordenada|Comparação não ordenada|  
|<xref:System.Linq.ParallelEnumerable.Single%2A>|Não aplicável|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.SingleOrDefault%2A>|Não aplicável|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.Skip%2A>|Ignora os primeiros *n* elementos|Ignora os *n* elementos|  
|<xref:System.Linq.ParallelEnumerable.SkipWhile%2A>|Resultados ordenados.|Não determinístico. Executa SkipWhile na ordem arbitrária atual|  
|<xref:System.Linq.ParallelEnumerable.Sum%2A>|Saída não determinística para operações não associativas ou não comutativas|Saída não determinística para operações não associativas ou não comutativas|  
|<xref:System.Linq.ParallelEnumerable.Take%2A>|Usa os primeiros `n` elementos|Usa quaisquer `n` elementos|  
|<xref:System.Linq.ParallelEnumerable.TakeWhile%2A>|Resultados ordenados|Não determinístico. Executa TakeWhile na ordem arbitrária atual|  
|<xref:System.Linq.ParallelEnumerable.ThenBy%2A>|Suplementos `OrderBy`|Suplementos `OrderBy`|  
|<xref:System.Linq.ParallelEnumerable.ThenByDescending%2A>|Suplementos `OrderBy`|Suplementos `OrderBy`|  
|<xref:System.Linq.ParallelEnumerable.ToArray%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.ToDictionary%2A>|Não aplicável|Não aplicável|  
|<xref:System.Linq.ParallelEnumerable.ToList%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.ToLookup%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.Union%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.Where%2A>|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.Where%2A> (indexado)|Resultados ordenados|Resultados não ordenados|  
|<xref:System.Linq.ParallelEnumerable.Zip%2A>|Resultados ordenados|Resultados não ordenados|  
  
 Os resultados não ordenados não são ativamente embaralhados. Eles simplesmente não têm qualquer lógica de ordenação especial aplicada a eles. Em alguns casos, uma consulta não ordenada pode manter a ordenação da sequência de origem. No caso das consultas que usam o operador Select indexado, o PLINQ garante que os elementos de saída serão apresentados na ordem de índices crescentes, mas não garante quais índices serão atribuídos a quais elementos.  
  
## <a name="see-also"></a>Veja também

- [LINQ paralelo (PLINQ)](introduction-to-plinq.md)
- [Programação paralela](index.md)
