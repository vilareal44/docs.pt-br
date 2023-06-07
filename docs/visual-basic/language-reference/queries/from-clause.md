---
title: Cláusula From
ms.date: 07/20/2015
f1_keywords:
- vb.QueryFrom
- vb.QueryFromIn
- vb.QueryFromLet
helpviewer_keywords:
- queries [Visual Basic], From
- From clause [Visual Basic]
- From statement [Visual Basic]
ms.assetid: 83e3665e-68a0-4540-a3a3-3d777a0f95d5
ms.openlocfilehash: 33680f49247b3b2a6082b3a6b27ca64f8401e42d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396175"
---
# <a name="from-clause-visual-basic"></a>Cláusula From (Visual Basic)
Especifica uma ou mais variáveis de intervalo e uma coleção a ser consultada.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
From element [ As type ] In collection [ _ ]  
  [, element2 [ As type2 ] In collection2 [, ... ] ]  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`element`|Obrigatórios. Uma *variável de intervalo* usada para iterar pelos elementos da coleção. Uma variável de intervalo é usada para fazer referência a cada membro do `collection` à medida que a consulta é iterada por meio do `collection` . Deve ser um tipo enumerável.|  
|`type`|Opcional. O tipo de `element`. Se não `type` for especificado, o tipo de `element` será inferido de `collection` .|  
|`collection`|Obrigatórios. Refere-se à coleção a ser consultada. Deve ser um tipo enumerável.|  
  
## <a name="remarks"></a>Comentários  
 A `From` cláusula é usada para identificar os dados de origem de uma consulta e as variáveis que são usadas para fazer referência a um elemento da coleção de origem. Essas variáveis são chamadas de *variáveis de intervalo*. A `From` cláusula é necessária para uma consulta, exceto quando a `Aggregate` cláusula é usada para identificar uma consulta que retorna apenas resultados agregados. Para obter mais informações, consulte [cláusula Aggregate](aggregate-clause.md).  
  
 Você pode especificar várias `From` cláusulas em uma consulta para identificar várias coleções a serem unidas. Quando várias coleções são especificadas, elas são iteradas de forma independente ou você pode associá-las se estiverem relacionadas. Você pode unir coleções implicitamente usando a `Select` cláusula ou explicitamente usando as `Join` `Group Join` cláusulas ou. Como alternativa, você pode especificar várias variáveis de intervalo e coleções em uma única `From` cláusula, com cada variável de intervalo relacionada e uma coleção separada das outras por uma vírgula. O exemplo de código a seguir mostra ambas as opções de sintaxe para a `From` cláusula.  
  
 [!code-vb[VbSimpleQuerySamples#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#21)]  
  
 A `From` cláusula define o escopo de uma consulta, que é semelhante ao escopo de um `For` loop. Portanto, cada `element` variável de intervalo no escopo de uma consulta deve ter um nome exclusivo. Como você pode especificar várias `From` cláusulas para uma consulta, as `From` cláusulas subsequentes podem se referir a variáveis de intervalo na `From` cláusula, ou podem se referir a variáveis de intervalo em uma `From` cláusula anterior. Por exemplo, o exemplo a seguir mostra uma `From` cláusula aninhada onde a coleção na segunda cláusula é baseada em uma propriedade da variável Range na primeira cláusula.  
  
 [!code-vb[VbSimpleQuerySamples#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#22)]  
  
 Cada `From` cláusula pode ser seguida por qualquer combinação de cláusulas de consulta adicionais para refinar a consulta. Você pode refinar a consulta das seguintes maneiras:  
  
- Combine várias coleções implicitamente usando as `From` `Select` cláusulas e, ou explicitamente, usando as `Join` `Group Join` cláusulas ou.  
  
- Use a `Where` cláusula para filtrar o resultado da consulta.  
  
- Classifique o resultado usando a `Order By` cláusula.  
  
- Agrupe resultados semelhantes em conjunto usando a `Group By` cláusula.  
  
- Use a `Aggregate` cláusula para identificar funções de agregação a serem avaliadas para todo o resultado da consulta.  
  
- Use a `Let` cláusula para introduzir uma variável de iteração cujo valor é determinado por uma expressão em vez de uma coleção.  
  
- Use a `Distinct` cláusula para ignorar os resultados de consulta duplicados.  
  
- Identifique partes do resultado a serem retornadas usando as `Skip` `Take` `Skip While` cláusulas,, e `Take While` .  
  
## <a name="example"></a>Exemplo  
 A expressão de consulta a seguir usa uma `From` cláusula para declarar uma variável `cust` de intervalo para cada `Customer` objeto na `customers` coleção. A `Where` cláusula usa a variável Range para restringir a saída aos clientes da região especificada. O `For Each` loop exibe o nome da empresa para cada cliente no resultado da consulta.  
  
 [!code-vb[VbSimpleQuerySamples#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#23)]  
  
## <a name="see-also"></a>Confira também

- [Consultas](index.md)
- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Instrução For Each...Next](../statements/for-each-next-statement.md)
- [Instrução For...Next](../statements/for-next-statement.md)
- [Cláusula SELECT](select-clause.md)
- [Cláusula WHERE](where-clause.md)
- [Cláusula Aggregate](aggregate-clause.md)
- [Cláusula Distinct](distinct-clause.md)
- [Cláusula JOIN](join-clause.md)
- [Cláusula Group Join](group-join-clause.md)
- [Cláusula Order By](order-by-clause.md)
- [Cláusula Let](let-clause.md)
- [Cláusula Skip](skip-clause.md)
- [Cláusula Take](take-clause.md)
- [Cláusula Skip While](skip-while-clause.md)
- [Cláusula Take While](take-while-clause.md)
