---
title: Operações de conjunto
ms.date: 07/20/2015
ms.assetid: 2b06e822-e030-438f-9db7-ee402bd3a706
ms.openlocfilehash: b6ff14794343ae7623ee38894cef02cfc0a2a597
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406814"
---
# <a name="set-operations-visual-basic"></a>Operações Set (Visual Basic)

As operações de conjunto na LINQ referem-se a operações de consulta que geram um conjunto de resultados baseado na presença ou ausência de elementos equivalentes dentro da mesma ou de coleções (ou conjuntos) separadas.

Os métodos de operador de consulta padrão que executam operações de conjunto estão listados na seção a seguir.

## <a name="methods"></a>Métodos

|Nome do método|Descrição|Visual Basic sintaxe de expressão de consulta|Mais informações|
|-----------------|-----------------|------------------------------------------|----------------------|
|Distinct|Remove os valores duplicados de uma coleção.|`Distinct`|<xref:System.Linq.Enumerable.Distinct%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Distinct%2A?displayProperty=nameWithType>|
|Except|Retorna a diferença de conjunto, que significa os elementos de uma coleção que não aparecem em uma segunda coleção.|Não aplicável.|<xref:System.Linq.Enumerable.Except%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Except%2A?displayProperty=nameWithType>|
|Intersect|Retorna a interseção de conjunto, o que significa os elementos que aparecem em cada uma das duas coleções.|Não aplicável.|<xref:System.Linq.Enumerable.Intersect%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Intersect%2A?displayProperty=nameWithType>|
|Union|Retorna a união de conjunto, o que significa os elementos únicos que aparecem em qualquer uma das duas coleções.|Não aplicável.|<xref:System.Linq.Enumerable.Union%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Union%2A?displayProperty=nameWithType>|

## <a name="comparison-of-set-operations"></a>Comparação de operações de conjuntos

### <a name="distinct"></a>Distinct

A ilustração a seguir mostra o comportamento do método <xref:System.Linq.Enumerable.Distinct%2A?displayProperty=nameWithType> em uma sequência de caracteres. A sequência retornada contém os elementos exclusivos da sequência de entrada.

![Gráfico mostrando o comportamento de Distinct&#40;&#41;.](./media/set-operations/distinct-method-behavior.png)

### <a name="except"></a>Except

A ilustração a seguir mostra o comportamento de <xref:System.Linq.Enumerable.Except%2A?displayProperty=nameWithType>. A sequência retornada contém apenas os elementos da primeira sequência de entrada que não estão na segunda sequência de entrada.

![Gráfico mostrando a ação de, exceto&#40;&#41;.](./media/set-operations/except-behavior-graphic.png "Mostra o comportamento de Except.")

### <a name="intersect"></a>Intersect

A ilustração a seguir mostra o comportamento de <xref:System.Linq.Enumerable.Intersect%2A?displayProperty=nameWithType>. A sequência retornada contém os elementos que são comuns a ambas as sequências de entrada.

![Gráfico mostrando a interseção de duas sequências.](./media/set-operations/intersection-two-sequences.png)

### <a name="union"></a>Union

A ilustração a seguir descreve uma operação de união em duas sequências de caracteres. A sequência retornada contém os elementos exclusivos das duas sequências de entrada.

![Gráfico mostrando a união de duas sequências.](./media/set-operations/union-operation-two-sequences.png)

## <a name="query-expression-syntax-example"></a>Exemplo de sintaxe de expressão de consulta

O exemplo a seguir usa a `Distinct` cláusula em uma consulta LINQ para retornar os números exclusivos de uma lista de inteiros.

[!code-vb[CsLINQSetOps#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQSetOps/VB/setops.vb#1)]

## <a name="see-also"></a>Confira também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md)
- [Cláusula Distinct](../../../language-reference/queries/distinct-clause.md)
- [Como: combinar e comparar coleções de cadeias de caracteres (LINQ) (Visual Basic)](how-to-combine-and-compare-string-collections-linq.md)
- [Como localizar a diferença de conjunto entre duas listas (LINQ) (Visual Basic)](how-to-find-the-set-difference-between-two-lists-linq.md)
