---
title: Operações de agregação (C#)
description: Saiba mais sobre os métodos para executar uma operação de agregação. Uma operação de agregação computa um único valor de uma coleção de valores.
ms.date: 07/20/2015
ms.assetid: 6fc035e5-7639-48b8-bc7f-b093dd31b039
ms.openlocfilehash: 3d5a52249520478571db2fcfd7aa5d10fb013565
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104305"
---
# <a name="aggregation-operations-c"></a>Operações de agregação (C#)
Uma operação de agregação computa um único valor de uma coleção de valores. Um exemplo de uma operação de agregação é o cálculo da temperatura média diária dos valores válidos de temperatura diária de um mês.  
  
 A ilustração a seguir mostra os resultados de duas operações de agregação diferentes em uma sequência de números. A primeira operação soma os números. A segunda operação retorna o valor máximo na sequência.  
  
 ![Ilustração que mostra operações de agregação LINQ.](./media/aggregation-operations/linq-aggregation-operations.png)  
  
 Os métodos de operador de consulta padrão que realizam operações de agregação são listados na seção a seguir.  
  
## <a name="methods"></a>Métodos  
  
|Nome do método|Descrição|Sintaxe de expressão de consulta C#|Mais informações|  
|-----------------|-----------------|---------------------------------|----------------------|  
|Agregado|Executa uma operação de agregação personalizada nos valores de uma coleção.|Não aplicável.|<xref:System.Linq.Enumerable.Aggregate%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Aggregate%2A?displayProperty=nameWithType>|  
|Média|Calcula o valor médio de uma coleção de valores.|Não aplicável.|<xref:System.Linq.Enumerable.Average%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Average%2A?displayProperty=nameWithType>|  
|Contagem|Conta os elementos em uma coleção e, opcionalmente, apenas os elementos que satisfazem a uma função de predicado.|Não aplicável.|<xref:System.Linq.Enumerable.Count%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Count%2A?displayProperty=nameWithType>|  
|LongCount|Conta os elementos em uma coleção grande e, opcionalmente, apenas os elementos que satisfazem a uma função de predicado.|Não aplicável.|<xref:System.Linq.Enumerable.LongCount%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.LongCount%2A?displayProperty=nameWithType>|  
|Máx|Determina o valor máximo em uma coleção.|Não aplicável.|<xref:System.Linq.Enumerable.Max%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Max%2A?displayProperty=nameWithType>|  
|Mín|Retorna o valor mínimo em uma coleção.|Não aplicável.|<xref:System.Linq.Enumerable.Min%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Min%2A?displayProperty=nameWithType>|  
|Somar|Calcula a soma dos valores em uma coleção.|Não aplicável.|<xref:System.Linq.Enumerable.Sum%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Sum%2A?displayProperty=nameWithType>|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md)
- [Como computar valores de coluna em um arquivo de texto CSV (LINQ) (C#)](./how-to-compute-column-values-in-a-csv-text-file-linq.md)
- [Como consultar o maior arquivo ou arquivos em uma árvore de diretório (LINQ) (C#)](./how-to-query-for-the-largest-file-or-files-in-a-directory-tree-linq.md)
- [Como consultar o número total de bytes em um conjunto de pastas (LINQ) (C#)](./how-to-query-for-the-total-number-of-bytes-in-a-set-of-folders-linq.md)
