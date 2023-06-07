---
title: Cláusula Take While
ms.date: 07/20/2015
f1_keywords:
- vb.QueryTakeWhile
helpviewer_keywords:
- queries [Visual Basic], Take While
- Take While clause [Visual Basic]
- Take While statement [Visual Basic]
ms.assetid: db8f9f2f-fc9f-4a6c-b0b8-1bf048147e11
ms.openlocfilehash: 4b6133efdbd9c46ab85201ad454671e5538b6a81
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359574"
---
# <a name="take-while-clause-visual-basic"></a>Cláusula Take While (Visual Basic)
Inclui elementos em uma coleção desde que uma condição especificada seja `true` e ignore os elementos restantes.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Take While expression  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`expression`|Obrigatórios. Uma expressão que representa uma condição para os elementos de teste. A expressão deve retornar um `Boolean` valor ou um equivalente funcional, como um `Integer` a ser avaliado como um `Boolean` .|  
  
## <a name="remarks"></a>Comentários  
 A `Take While` cláusula inclui elementos do início de um resultado da consulta até que o `expression` retorno fornecido `false` . Após o `expression` retorno `false` , a consulta irá ignorar todos os elementos restantes. O `expression` é ignorado para os resultados restantes.  
  
 A `Take While` cláusula é diferente da cláusula, pois `Where` a `Where` cláusula pode ser usada para incluir todos os elementos de uma consulta que atenda a uma condição específica. A `Take While` cláusula só inclui elementos até a primeira vez que a condição não for satisfeita. A `Take While` cláusula é mais útil quando você está trabalhando com um resultado de consulta ordenado.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir usa a `Take While` cláusula para recuperar resultados até que o primeiro cliente sem nenhum pedido seja encontrado.  
  
 [!code-vb[VbSimpleQuerySamples#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#2)]  
  
## <a name="see-also"></a>Confira também

- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Consultas](index.md)
- [Cláusula SELECT](select-clause.md)
- [Cláusula from](from-clause.md)
- [Cláusula Take](take-clause.md)
- [Cláusula Skip While](skip-while-clause.md)
- [Cláusula WHERE](where-clause.md)
