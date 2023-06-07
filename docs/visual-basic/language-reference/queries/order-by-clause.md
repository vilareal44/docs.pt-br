---
title: Cláusula Order By
ms.date: 07/20/2015
f1_keywords:
- vb.QueryOrderBy
- vb.QueryAscending
- vb.QueryDescending
helpviewer_keywords:
- queries [Visual Basic], Order By
- Order By clause [Visual Basic]
- Order By statement [Visual Basic]
ms.assetid: fa911282-6b81-44c7-acfa-46b5bb93df75
ms.openlocfilehash: 63f454064e88bc18f252940f3abd8e59b8900e5b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359742"
---
# <a name="order-by-clause-visual-basic"></a>Cláusula Order By (Visual Basic)
Especifica a ordem de classificação para um resultado de consulta.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Order By orderExp1 [ Ascending | Descending ] [, orderExp2 [...] ]  
```  
  
## <a name="parts"></a>Partes  
 `orderExp1`  
 Obrigatórios. Um ou mais campos do resultado da consulta atual que identificam como ordenar os valores retornados. Os nomes de campo devem ser separados por vírgulas (,). Você pode identificar cada campo como classificado em ordem crescente ou decrescente usando as `Ascending` `Descending` palavras-chave ou. Se nenhuma `Ascending` `Descending` palavra-chave ou for especificada, a ordem de classificação padrão será crescente. Os campos de ordem de classificação recebem precedência da esquerda para a direita.  
  
## <a name="remarks"></a>Comentários  
 Você pode usar a `Order By` cláusula para classificar os resultados de uma consulta. A `Order By` cláusula só pode classificar um resultado com base na variável de intervalo para o escopo atual. Por exemplo, a `Select` cláusula apresenta um novo escopo em uma expressão de consulta com novas variáveis de iteração para esse escopo. Variáveis de intervalo definidas antes de uma `Select` cláusula em uma consulta não estão disponíveis após a `Select` cláusula. Portanto, se você quiser ordenar os resultados por um campo que não está disponível na `Select` cláusula, deverá colocar a `Order By` cláusula antes da `Select` cláusula. Um exemplo de quando você precisa fazer isso é quando deseja classificar sua consulta por campos que não são retornados como parte do resultado.  
  
 Ordem crescente e decrescente para um campo é determinada pela implementação da <xref:System.IComparable> interface para o tipo de dados do campo. Se o tipo de dados não implementar a <xref:System.IComparable> interface, a ordem de classificação será ignorada.  
  
## <a name="example"></a>Exemplo  
 A expressão de consulta a seguir usa uma `From` cláusula para declarar uma variável `book` de intervalo para a `books` coleção. A `Order By` cláusula classifica o resultado da consulta por preço em ordem crescente (o padrão). Os livros com o mesmo preço são classificados por título em ordem crescente. A `Select` cláusula seleciona as `Title` `Price` Propriedades e como os valores retornados pela consulta.  
  
 [!code-vb[VbSimpleQuerySamples#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#24)]  
  
## <a name="example"></a>Exemplo  
 A expressão de consulta a seguir usa a `Order By` cláusula para classificar o resultado da consulta por preço em ordem decrescente. Os livros com o mesmo preço são classificados por título em ordem crescente.  
  
 [!code-vb[VbSimpleQuerySamples#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#25)]  
  
## <a name="example"></a>Exemplo  
 A expressão de consulta a seguir usa uma `Select` cláusula para selecionar o título do livro, o preço, a data de publicação e o autor. Em seguida, ele preenche `Title` os `Price` campos,, `PublishDate` e `Author` da variável de intervalo para o novo escopo. A `Order By` cláusula ordena a nova variável de intervalo por nome do autor, título do livro e, em seguida, Price. Cada coluna é classificada na ordem padrão (em ordem crescente).  
  
 [!code-vb[VbSimpleQuerySamples#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#26)]  
  
## <a name="see-also"></a>Confira também

- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Consultas](index.md)
- [Cláusula SELECT](select-clause.md)
- [Cláusula from](from-clause.md)
