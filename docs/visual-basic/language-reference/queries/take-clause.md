---
title: Cláusula Take
ms.date: 07/20/2015
f1_keywords:
- vb.QueryTake
helpviewer_keywords:
- Take statement [Visual Basic]
- queries [Visual Basic], Take
- Take clause [Visual Basic]
ms.assetid: 77bf87b2-1476-4456-957f-fee922fbad8c
ms.openlocfilehash: 25dd06905525a96bc1504f033eb4f19af6d454a2
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359626"
---
# <a name="take-clause-visual-basic"></a>Cláusula Take (Visual Basic)
Retorna um número especificado de elementos contíguos do início de uma coleção.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Take count  
```  
  
## <a name="parts"></a>Partes  
 `count`  
 Obrigatórios. Um valor ou uma expressão que é avaliada como o número de elementos da sequência a ser retornado.  
  
## <a name="remarks"></a>Comentários  
 A `Take` cláusula faz com que uma consulta inclua um número especificado de elementos contíguos desde o início de uma lista de resultados. O número de elementos a serem incluídos é especificado pelo `count` parâmetro.  
  
 Você pode usar a `Take` cláusula com a `Skip` cláusula para retornar um intervalo de dados de qualquer segmento de uma consulta. Para fazer isso, passe o índice do primeiro elemento do intervalo para a `Skip` cláusula e o tamanho do intervalo para a `Take` cláusula. Nesse caso, a `Take` cláusula deve ser especificada após a `Skip` cláusula.  
  
 Quando você usa a `Take` cláusula em uma consulta, também pode ser necessário garantir que os resultados sejam retornados em uma ordem que permitirá que a `Take` cláusula inclua os resultados pretendidos. Para obter mais informações sobre como ordenar os resultados da consulta, consulte [cláusula order by](order-by-clause.md).  
  
 Você pode usar a `TakeWhile` cláusula para especificar que apenas determinados elementos sejam retornados, dependendo de uma condição fornecida.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir usa a `Take` cláusula junto com a `Skip` cláusula para retornar dados de uma consulta em páginas. A função GetCustomers usa a `Skip` cláusula para ignorar os clientes na lista até o valor de índice inicial fornecido e usa a `Take` cláusula para retornar uma página de clientes que inicia com esse valor de índice.  
  
 [!code-vb[VbSimpleQuerySamples#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#1)]  
  
## <a name="see-also"></a>Confira também

- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Consultas](index.md)
- [Cláusula SELECT](select-clause.md)
- [Cláusula from](from-clause.md)
- [Cláusula Order By](order-by-clause.md)
- [Cláusula Take While](take-while-clause.md)
- [Cláusula Skip](skip-clause.md)
