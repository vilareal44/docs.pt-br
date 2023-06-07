---
title: Cláusula Group Join
ms.date: 07/20/2015
f1_keywords:
- vb.QueryGroupJoinIn
- vb.QueryGroupJoinOn
- vb.QueryGroupJoin
- vb.QueryGroupJoinInto
helpviewer_keywords:
- Group Join clause [Visual Basic]
- Group Join statement [Visual Basic]
- queries [Visual Basic], Group Join
ms.assetid: 37dbf79c-7b5c-421b-bbb7-dadfd2b92a1c
ms.openlocfilehash: 7916e51293c06016b2581b7109df3f0a599404ca
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359821"
---
# <a name="group-join-clause-visual-basic"></a>Cláusula Join Group (Visual Basic)
Combina duas coleções em uma única coleção hierárquica. A operação de junção é baseada em chaves correspondentes.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Group Join element [As type] In collection _  
  On key1 Equals key2 [ And key3 Equals key4 [... ] ] _  
  Into expressionList  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`element`|Obrigatórios. A variável de controle para a coleção que está sendo unida.|  
|`type`|Opcional. O tipo de `element`. Se não `type` for especificado, o tipo de `element` será inferido de `collection` .|  
|`collection`|Obrigatórios. A coleção a ser combinada com a coleção que está no lado esquerdo do `Group Join` operador. Uma `Group Join` cláusula pode ser aninhada em uma `Join` cláusula ou em outra `Group Join` cláusula.|  
|`key1` `Equals` `key2`|Obrigatórios. Identifica as chaves para as coleções que estão sendo Unidas. Você deve usar o `Equals` operador para comparar as chaves das coleções que estão sendo Unidas. Você pode combinar condições de junção usando o `And` operador para identificar várias chaves. O `key1` parâmetro deve ser da coleção no lado esquerdo do `Join` operador. O `key2` parâmetro deve ser da coleção no lado direito do `Join` operador.<br /><br /> As chaves usadas na condição de junção podem ser expressões que incluem mais de um item da coleção. No entanto, cada expressão de chave pode conter apenas itens de sua respectiva coleção.|  
|`expressionList`|Obrigatórios. Uma ou mais expressões que identificam como os grupos de elementos da coleção são agregados. Para identificar um nome de membro para os resultados agrupados, use a `Group` palavra-chave ( `<alias> = Group` ). Você também pode incluir funções de agregação para aplicar ao grupo.|  
  
## <a name="remarks"></a>Comentários  
 A `Group Join` cláusula combina duas coleções com base em valores de chave correspondentes das coleções que estão sendo Unidas. A coleção resultante pode conter um membro que faz referência a uma coleção de elementos da segunda coleção que corresponde ao valor de chave da primeira coleção. Você também pode especificar funções de agregação para aplicar aos elementos agrupados da segunda coleção. Para obter informações sobre funções de agregação, consulte [cláusula Aggregate](aggregate-clause.md).  
  
 Considere, por exemplo, uma coleção de gerentes e uma coleção de funcionários. Os elementos de ambas as coleções têm uma propriedade ManagerID que identifica os funcionários que se reportam a um gerente específico. Os resultados de uma operação de junção contêm um resultado para cada gerente e funcionário com um valor de ManagerID correspondente. Os resultados de uma `Group Join` operação contêm a lista completa de gerentes. Cada resultado do gerente teria um membro que referenciou a lista de funcionários que eram uma correspondência para o gerente específico.  
  
 A coleção resultante de uma `Group Join` operação pode conter qualquer combinação de valores da coleção identificada na `From` cláusula e as expressões identificadas na `Into` cláusula da `Group Join` cláusula. Para obter mais informações sobre expressões válidas para a `Into` cláusula, consulte [cláusula Aggregate](aggregate-clause.md).  
  
 Uma `Group Join` operação retornará todos os resultados da coleção identificada no lado esquerdo do `Group Join` operador. Isso é verdadeiro mesmo se não houver nenhuma correspondência na coleção que está sendo unida. Isso é como um `LEFT OUTER JOIN` no SQL.  
  
 Você pode usar a `Join` cláusula para combinar coleções em uma única coleção. Isso é equivalente a um `INNER JOIN` em SQL.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir une duas coleções usando a `Group Join` cláusula.  
  
 [!code-vb[VbSimpleQuerySamples#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#14)]  
  
## <a name="see-also"></a>Confira também

- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Consultas](index.md)
- [Cláusula SELECT](select-clause.md)
- [Cláusula from](from-clause.md)
- [Cláusula JOIN](join-clause.md)
- [Cláusula WHERE](where-clause.md)
- [Cláusula Group By](group-by-clause.md)
