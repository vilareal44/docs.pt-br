---
title: Cláusula Distinct
ms.date: 07/20/2015
f1_keywords:
- vb.QueryDistinct
helpviewer_keywords:
- Distinct clause [Visual Basic]
- Distinct statement [Visual Basic]
- queries [Visual Basic], Distinct
ms.assetid: 86f42614-0d8f-4ffc-b888-ce8a37a8d36a
ms.openlocfilehash: 5aecffbce036500d294d03a925798d51f1269af6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401388"
---
# <a name="distinct-clause-visual-basic"></a>Cláusula Distinct (Visual Basic)
Restringe os valores da variável de intervalo atual para eliminar valores duplicados em cláusulas de consulta subsequentes.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Distinct  
```  
  
## <a name="remarks"></a>Comentários  
 Você pode usar a `Distinct` cláusula para retornar uma lista de itens exclusivos. A `Distinct` cláusula faz com que a consulta ignore resultados de consulta duplicados. A `Distinct` cláusula aplica-se a valores duplicados para todos os campos de retorno especificados pela `Select` cláusula. Se nenhuma `Select` cláusula for especificada, a `Distinct` cláusula será aplicada à variável de intervalo para a consulta identificada na `From` cláusula. Se a variável de intervalo não for um tipo imutável, a consulta irá ignorar apenas um resultado de consulta se todos os membros do tipo corresponderem a um resultado de consulta existente.  
  
## <a name="example"></a>Exemplo  
 A expressão de consulta a seguir une uma lista de clientes e uma lista de pedidos de clientes. A `Distinct` cláusula é incluída para retornar uma lista de nomes de clientes e datas de pedido exclusivos.  
  
 [!code-vb[VbSimpleQuerySamples#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#20)]  
  
## <a name="see-also"></a>Confira também

- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Consultas](index.md)
- [Cláusula from](from-clause.md)
- [Cláusula SELECT](select-clause.md)
- [Cláusula WHERE](where-clause.md)
