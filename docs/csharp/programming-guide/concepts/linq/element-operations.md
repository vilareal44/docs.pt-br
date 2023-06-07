---
title: Operações de elemento (C#)
description: Saiba mais sobre os métodos de operador de consulta padrão que fazem operações de elemento, que retornam um único elemento de uma sequência em C#.
ms.date: 10/03/2018
ms.assetid: 283206c9-3246-4c48-b01a-d9de409a7231
ms.openlocfilehash: 8cd34146a3b93205ec01ed128aacb79d566a03c6
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105448"
---
# <a name="element-operations-c"></a>Operações de elemento (C#)

Operações de elemento retornam um único elemento específico de uma sequência.  
  
 Os métodos de operador de consulta padrão que executam operações de elemento estão listados na seção a seguir.  
  
## <a name="methods"></a>Métodos  
  
|Nome do método|Descrição|Sintaxe de expressão de consulta C#|Mais informações|  
|-----------------|-----------------|---------------------------------|----------------------|  
|ElementAt|Retorna o elemento em um índice especificado em uma coleção.|Não aplicável.|<xref:System.Linq.Enumerable.ElementAt%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ElementAt%2A?displayProperty=nameWithType>|  
|ElementAtOrDefault|Retorna o elemento em um índice especificado em uma coleção ou um valor padrão se o índice estiver fora do intervalo.|Não aplicável.|<xref:System.Linq.Enumerable.ElementAtOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ElementAtOrDefault%2A?displayProperty=nameWithType>|  
|Primeiro|Retorna o primeiro elemento de uma coleção ou o primeiro elemento que satisfaz uma condição.|Não aplicável.|<xref:System.Linq.Enumerable.First%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.First%2A?displayProperty=nameWithType>|  
|FirstOrDefault|Retorna o primeiro elemento de uma coleção ou o primeiro elemento que satisfaz uma condição. Retorna um valor padrão se esse elemento não existir.|Não aplicável.|<xref:System.Linq.Enumerable.FirstOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.FirstOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.FirstOrDefault%60%601%28System.Linq.IQueryable%7B%60%600%7D%29?displayProperty=nameWithType>|  
|Último|Retorna o último elemento de uma coleção ou o último elemento que satisfaz uma condição.|Não aplicável.|<xref:System.Linq.Enumerable.Last%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Last%2A?displayProperty=nameWithType>|  
|LastOrDefault|Retorna o último elemento de uma coleção ou o último elemento que satisfaz uma condição. Retorna um valor padrão se esse elemento não existir.|Não aplicável.|<xref:System.Linq.Enumerable.LastOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.LastOrDefault%2A?displayProperty=nameWithType>|  
|Single|Retorna o único elemento de uma coleção ou o único elemento que satisfaz uma condição. Gera um <xref:System.InvalidOperationException> se não houver elemento ou mais de um elemento a ser retornado. |Não aplicável.|<xref:System.Linq.Enumerable.Single%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Single%2A?displayProperty=nameWithType>|  
|SingleOrDefault|Retorna o único elemento de uma coleção ou o único elemento que satisfaz uma condição. Retorna um valor padrão se não houver elemento a ser retornado. Gera um <xref:System.InvalidOperationException> se houver mais de um elemento a ser retornado. |Não aplicável.|<xref:System.Linq.Enumerable.SingleOrDefault%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SingleOrDefault%2A?displayProperty=nameWithType>|  
  
## <a name="see-also"></a>Confira também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (C#)](./standard-query-operators-overview.md)
- [Como consultar o maior arquivo ou arquivos em uma árvore de diretório (LINQ) (C#)](./how-to-query-for-the-largest-file-or-files-in-a-directory-tree-linq.md)
