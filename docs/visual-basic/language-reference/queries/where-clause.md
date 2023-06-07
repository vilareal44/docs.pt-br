---
title: Cláusula Where
ms.date: 07/20/2015
f1_keywords:
- vb.QueryWhere
helpviewer_keywords:
- Where statement [Visual Basic]
- queries [Visual Basic], Where
- Where clause [Visual Basic]
ms.assetid: 48b5c2c5-3181-429c-8545-894296798c89
ms.openlocfilehash: b80bb047551dee8ab23cfac06b961996992d69b5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359535"
---
# <a name="where-clause-visual-basic"></a>Cláusula Where (Visual Basic)
Especifica a condição de filtragem para uma consulta.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Where condition  
```  
  
## <a name="parts"></a>Partes  
 `condition`  
 Obrigatórios. Uma expressão que determina se os valores para o item atual na coleção são incluídos na coleção de saída. A expressão deve ser avaliada como um `Boolean` valor ou equivalente a um `Boolean` valor. Se a condição for avaliada como `True` , o elemento será incluído no resultado da consulta; caso contrário, o elemento será excluído do resultado da consulta.  
  
## <a name="remarks"></a>Comentários  
 A `Where` cláusula permite filtrar dados de consulta selecionando apenas os elementos que atendem a determinados critérios. Os elementos cujos valores fazem a `Where` cláusula ser avaliada como `True` estão incluídos no resultado da consulta; outros elementos são excluídos. A expressão usada em uma `Where` cláusula deve ser avaliada como um `Boolean` ou equivalente de um `Boolean` , como um inteiro que é avaliado como `False` quando seu valor é zero. Você pode combinar várias expressões em uma `Where` cláusula usando operadores lógicos, como,,,, `And` `Or` `AndAlso` `OrElse` `Is` e `IsNot` .  
  
 Por padrão, as expressões de consulta não são avaliadas até que elas sejam acessadas — por exemplo, quando elas são vinculadas a dados ou iteradas em um `For` loop. Como resultado, a `Where` cláusula não é avaliada até que a consulta seja acessada. Se você tiver valores externos à consulta que são usados na `Where` cláusula, verifique se o valor apropriado é usado na `Where` cláusula no momento em que a consulta é executada. Para obter mais informações sobre a execução da consulta, consulte [escrevendo sua primeira consulta LINQ](../../programming-guide/concepts/linq/writing-your-first-linq-query.md).  
  
 Você pode chamar funções dentro de uma `Where` cláusula para executar um cálculo ou uma operação em um valor do elemento atual na coleção. Chamar uma função em uma `Where` cláusula pode fazer com que a consulta seja executada imediatamente quando ela é definida em vez de quando é acessada. Para obter mais informações sobre a execução da consulta, consulte [escrevendo sua primeira consulta LINQ](../../programming-guide/concepts/linq/writing-your-first-linq-query.md).  
  
## <a name="example"></a>Exemplo  
 A expressão de consulta a seguir usa uma `From` cláusula para declarar uma variável `cust` de intervalo para cada `Customer` objeto na `customers` coleção. A `Where` cláusula usa a variável Range para restringir a saída aos clientes da região especificada. O `For Each` loop exibe o nome da empresa para cada cliente no resultado da consulta.  
  
 [!code-vb[VbSimpleQuerySamples#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#23)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir `And` usa `Or` os operadores lógicos e na `Where` cláusula.  
  
 [!code-vb[VbSimpleQuerySamples#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#31)]  
  
## <a name="see-also"></a>Confira também

- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Consultas](index.md)
- [Cláusula from](from-clause.md)
- [Cláusula SELECT](select-clause.md)
- [Instrução For Each...Next](../statements/for-each-next-statement.md)
