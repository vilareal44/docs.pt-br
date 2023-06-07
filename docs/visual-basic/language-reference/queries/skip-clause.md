---
title: Cláusula Skip
ms.date: 07/20/2015
f1_keywords:
- vb.QuerySkip
helpviewer_keywords:
- queries [Visual Basic], Skip
- Skip statement [Visual Basic]
- Skip clause [Visual Basic]
ms.assetid: f00eb172-3907-4c43-9745-d8546ab86234
ms.openlocfilehash: 427d14453260a54bd3f2ab9a8ac75dedacd291f4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359652"
---
# <a name="skip-clause-visual-basic"></a>Cláusula Skip (Visual Basic)
Ignora um número especificado de elementos em uma coleção e, em seguida, retorna os elementos restantes.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Skip count  
```  
  
## <a name="parts"></a>Partes  
 `count`  
 Obrigatórios. Um valor ou uma expressão que é avaliada como o número de elementos da sequência a serem ignorados.  
  
## <a name="remarks"></a>Comentários  
 A `Skip` cláusula faz com que uma consulta ignore os elementos no início de uma lista de resultados e retorne os elementos restantes. O número de elementos a serem ignorados é identificado pelo `count` parâmetro.  
  
 Você pode usar a `Skip` cláusula com a `Take` cláusula para retornar um intervalo de dados de qualquer segmento de uma consulta. Para fazer isso, passe o índice do primeiro elemento do intervalo para a `Skip` cláusula e o tamanho do intervalo para a `Take` cláusula.  
  
 Quando você usa a `Skip` cláusula em uma consulta, também pode ser necessário garantir que os resultados sejam retornados em uma ordem que permitirá que a `Skip` cláusula ignore os resultados pretendidos. Para obter mais informações sobre como ordenar os resultados da consulta, consulte [cláusula order by](order-by-clause.md).  
  
 Você pode usar a `SkipWhile` cláusula para especificar que apenas determinados elementos sejam ignorados, dependendo de uma condição fornecida.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir usa a `Skip` cláusula junto com a `Take` cláusula para retornar dados de uma consulta em páginas. A `GetCustomers` função usa a `Skip` cláusula para ignorar os clientes na lista até o valor de índice inicial fornecido e usa a `Take` cláusula para retornar uma página de clientes que inicia com esse valor de índice.  
  
 [!code-vb[VbSimpleQuerySamples#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#1)]  
  
## <a name="see-also"></a>Confira também

- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Consultas](index.md)
- [Cláusula SELECT](select-clause.md)
- [Cláusula from](from-clause.md)
- [Cláusula Order By](order-by-clause.md)
- [Cláusula Skip While](skip-while-clause.md)
- [Cláusula Take](take-clause.md)
