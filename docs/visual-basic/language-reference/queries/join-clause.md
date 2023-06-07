---
title: Cláusula Join
ms.date: 07/20/2015
f1_keywords:
- vb.QueryJoinIn
- vb.QueryJoin
- vb.QueryJoinOn
helpviewer_keywords:
- queries [Visual Basic], Join
- Join statement [Visual Basic]
- Join clause [Visual Basic]
ms.assetid: 6dd37936-b27c-4e00-98ad-154b23f4de64
ms.openlocfilehash: f73dc31bbbb9014a8a1a315de406c53fa58d1c65
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359768"
---
# <a name="join-clause-visual-basic"></a>Cláusula Join (Visual Basic)

Combina duas coleções em uma única coleção. A operação de junção é baseada em chaves correspondentes e usa o `Equals` operador.

## <a name="syntax"></a>Sintaxe

```vb
Join element In collection _
  [ joinClause _ ]
  [ groupJoinClause ... _ ]
On key1 Equals key2 [ And key3 Equals key4 [... ]
```

## <a name="parts"></a>Partes

`element` Necessário. A variável de controle para a coleção que está sendo unida.

`collection`  
Obrigatórios. A coleção a ser combinada com a coleção identificada no lado esquerdo do `Join` operador. Uma `Join` cláusula pode ser aninhada em outra `Join` cláusula ou em uma `Group Join` cláusula.

`joinClause`  
Opcional. Uma ou mais `Join` cláusulas adicionais para refinar ainda mais a consulta.

`groupJoinClause`  
Opcional. Uma ou mais `Group Join` cláusulas adicionais para refinar ainda mais a consulta.

`key1` `Equals` `key2`  
Obrigatórios. Identifica as chaves para as coleções que estão sendo Unidas. Você deve usar o `Equals` operador para comparar as chaves das coleções que estão sendo Unidas. Você pode combinar condições de junção usando o `And` operador para identificar várias chaves. `key1`deve ser da coleção no lado esquerdo do `Join` operador. `key2`deve ser da coleção no lado direito do `Join` operador.

As chaves usadas na condição de junção podem ser expressões que incluem mais de um item da coleção. No entanto, cada expressão de chave pode conter apenas itens de sua respectiva coleção.

## <a name="remarks"></a>Comentários

A `Join` cláusula combina duas coleções com base em valores de chave correspondentes das coleções que estão sendo Unidas. A coleção resultante pode conter qualquer combinação de valores da coleção identificada no lado esquerdo do `Join` operador e da coleção identificada na `Join` cláusula. A consulta retornará apenas resultados para os quais a condição especificada pelo `Equals` operador é atendida. Isso é equivalente a um `INNER JOIN` em SQL.

Você pode usar várias `Join` cláusulas em uma consulta para unir duas ou mais coleções em uma única coleção.

Você pode executar uma junção implícita para combinar coleções sem a `Join` cláusula. Para fazer isso, inclua várias `In` cláusulas em sua `From` cláusula e especifique uma `Where` cláusula que identifique as chaves que você deseja usar para a junção.

Você pode usar a `Group Join` cláusula para combinar coleções em uma única coleção hierárquica. Isso é como um `LEFT OUTER JOIN` no SQL.

## <a name="example"></a>Exemplo

O exemplo de código a seguir executa uma junção implícita para combinar uma lista de clientes com seus pedidos.

[!code-vb[VbSimpleQuerySamples#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples1.vb#13)]

## <a name="example"></a>Exemplo

O exemplo de código a seguir une duas coleções usando a `Join` cláusula.

[!code-vb[VbSimpleQuerySamples#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples2.vb#12)]

Este exemplo produzirá uma saída semelhante à seguinte:

`winlogon (968), Windows Logon`

`explorer (2424), File Explorer`

`cmd (5136), Command Window`

## <a name="example"></a>Exemplo

O exemplo de código a seguir une duas coleções usando a `Join` cláusula com duas colunas de chave.

[!code-vb[VbSimpleQuerySamples#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbSimpleQuerySamples/VB/QuerySamples3.vb#17)]

O exemplo produzirá uma saída semelhante à seguinte:

`winlogon (968), Windows Logon, Priority = 13`

`cmd (700), Command Window, Priority = 8`

`explorer (2424), File Explorer, Priority = 8`

## <a name="see-also"></a>Confira também

- [Introdução a LINQ no Visual Basic](../../programming-guide/language-features/linq/introduction-to-linq.md)
- [Consultas](index.md)
- [Cláusula SELECT](select-clause.md)
- [Cláusula from](from-clause.md)
- [Cláusula Group Join](group-join-clause.md)
- [Cláusula WHERE](where-clause.md)
