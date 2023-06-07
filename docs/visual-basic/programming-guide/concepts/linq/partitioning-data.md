---
title: Particionar dados
ms.date: 07/20/2015
ms.assetid: 69c59379-b66e-422c-b324-5b5c07760ef7
ms.openlocfilehash: 34749f9d7b137bade66b6103650871246c3cc532
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406840"
---
# <a name="partitioning-data-visual-basic"></a>Dados de particionamento (Visual Basic)
Particionamento em LINQ refere-se à operação de dividir uma sequência de entrada em duas seções sem reorganizar os elementos e, depois, retornar uma das seções.  
  
 A ilustração a seguir mostra os resultados de três operações de particionamento diferentes em uma sequência de caracteres. A primeira operação retorna os três primeiros elementos na sequência. A segunda operação ignora os três primeiros elementos e retorna os elementos restantes. A terceira operação ignora os dois primeiros elementos na sequência e retorna os três elementos seguintes.  
  
 ![Ilustração que mostra as três operações de particionamento do LINQ.](./media/partitioning-data/linq-partitioning-operations.png)  
  
 Os métodos de operador de consulta padrão que particionam sequências estão listados na seção a seguir.  
  
## <a name="operators"></a>Operadores  
  
|Nome do operador|Descrição|Visual Basic sintaxe de expressão de consulta|Mais informações|  
|-------------------|-----------------|------------------------------------------|----------------------|  
|Ignorar|Ignora elementos até uma posição especificada na sequência.|`Skip`|<xref:System.Linq.Enumerable.Skip%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Skip%2A?displayProperty=nameWithType>|  
|SkipWhile|Ignora elementos com base em uma função de predicado até que um elemento não satisfaça a condição.|`Skip While`|<xref:System.Linq.Enumerable.SkipWhile%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SkipWhile%2A?displayProperty=nameWithType>|  
|Take|Aceita elementos até uma posição especificada na sequência.|`Take`|<xref:System.Linq.Enumerable.Take%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Take%2A?displayProperty=nameWithType>|  
|TakeWhile|Aceita elementos com base em uma função de predicado até que um elemento não satisfaça a condição.|`Take While`|<xref:System.Linq.Enumerable.TakeWhile%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.TakeWhile%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-examples"></a>Exemplos de sintaxe de expressão de consulta  
  
### <a name="skip"></a>Ignorar  
 O exemplo de código a seguir usa a `Skip` cláusula em Visual Basic para ignorar as primeiras quatro cadeias de caracteres em uma matriz de cadeias de caracteres antes de retornar as cadeias de caracteres restantes na matriz.  
  
 [!code-vb[CsLINQPartitioning#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQPartitioning/VB/Partitioning.vb#1)]  
  
### <a name="skipwhile"></a>SkipWhile  
 O exemplo de código a seguir usa a `Skip While` cláusula em Visual Basic para ignorar as cadeias de caracteres em uma matriz, enquanto a primeira letra da cadeia é "a". As cadeias de caracteres restantes na matriz são retornadas.  
  
 [!code-vb[CsLINQPartitioning#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQPartitioning/VB/Partitioning.vb#2)]  
  
### <a name="take"></a>Take  
 O exemplo de código a seguir usa a `Take` cláusula em Visual Basic para retornar as duas primeiras cadeias de caracteres em uma matriz de cadeias de caracteres.  
  
 [!code-vb[CsLINQPartitioning#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQPartitioning/VB/Partitioning.vb#3)]  
  
### <a name="takewhile"></a>TakeWhile  
 O exemplo de código a seguir usa a `Take While` cláusula em Visual Basic para retornar cadeias de caracteres de uma matriz, enquanto o comprimento da cadeia de caracteres é de cinco ou menos.  
  
 [!code-vb[CsLINQPartitioning#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQPartitioning/VB/Partitioning.vb#4)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Linq>
- [Visão geral de operadores de consulta padrão (Visual Basic)](standard-query-operators-overview.md)
- [Cláusula Skip](../../../language-reference/queries/skip-clause.md)
- [Cláusula Skip While](../../../language-reference/queries/skip-while-clause.md)
- [Cláusula Take](../../../language-reference/queries/take-clause.md)
- [Cláusula Take While](../../../language-reference/queries/take-while-clause.md)
