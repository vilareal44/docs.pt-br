---
title: Como corresponder uma cadeia de caracteres a um padrão
ms.date: 07/20/2015
helpviewer_keywords:
- comparison operators [Visual Basic], comparing strings
- pattern matching
- strings [Visual Basic], comparing
- string comparison [Visual Basic], operators
- Visual Basic code, operators
- pattern matching [Visual Basic], string comparison
- string comparison [Visual Basic]
- Like operator [Visual Basic], pattern matching
- pattern matching, empty strings
- operators [Visual Basic], comparison
ms.assetid: 19a83804-b5af-4739-928b-ac93e64e457f
ms.openlocfilehash: d8a3c363d1a443db4a0b7633e380562af1913aca
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403440"
---
# <a name="how-to-match-a-string-against-a-pattern-visual-basic"></a>Como corresponder uma cadeia de caracteres a um padrão (Visual Basic)

Se você quiser descobrir se uma expressão do [tipo de dados String](../../../language-reference/data-types/string-data-type.md) satisfaz um padrão, você pode usar o [operador Like](../../../language-reference/operators/like-operator.md).

`Like`usa dois operandos. O operando esquerdo é uma expressão de cadeia de caracteres e o operando à direita é uma cadeia de caracteres que contém o padrão a ser usado para correspondência. `Like`Retorna um `Boolean` valor que indica se a expressão de cadeia de caracteres atende ao padrão.

Você pode corresponder cada caractere na expressão de cadeia de caracteres em relação a um caractere específico, um caractere curinga, uma lista de caracteres ou um intervalo de caracteres. As posições das especificações na cadeia de caracteres de padrão correspondem às posições dos caracteres a serem correspondidos na expressão de cadeia de caracteres.

## <a name="to-match-a-character-in-the-string-expression-against-a-specific-character"></a>Para corresponder um caractere na expressão de cadeia de caracteres em relação a um caractere específico

Coloque o caractere específico diretamente na cadeia de caracteres de padrão. Determinados caracteres especiais devem ser colocados entre colchetes ( `[ ]` ). Para obter mais informações, consulte [operador Like](../../../language-reference/operators/like-operator.md).

O exemplo a seguir testa se o `myString` consiste exatamente em um único caractere `H` .

[!code-vb[VbVbalrOperators#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#70)]

## <a name="to-match-a-character-in-the-string-expression-against-a-wildcard-character"></a>Para corresponder um caractere na expressão de cadeia de caracteres a um caractere curinga

Coloque um ponto de interrogação ( `?` ) na cadeia de caracteres de padrão. Qualquer caractere válido nessa posição faz uma correspondência bem-sucedida.

O exemplo a seguir testa se `myString` consiste em um único caractere `W` seguido de exatamente dois caracteres de qualquer valor.

[!code-vb[VbVbalrOperators#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#71)]

## <a name="to-match-a-character-in-the-string-expression-against-a-list-of-characters"></a>Para corresponder a um caractere na expressão de cadeia de caracteres em relação a uma lista de personagens

Coloque colchetes ( `[ ]` ) na cadeia de caracteres de padrão e dentro dos colchetes Coloque a lista de caracteres. Não separe os caracteres com vírgulas ou qualquer outro separador. Qualquer caractere único na lista faz uma correspondência bem-sucedida.

O exemplo a seguir testa se `myString` consiste em qualquer caractere válido seguido por exatamente um dos caracteres `A` , `C` ou `E` .

[!code-vb[VbVbalrOperators#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#72)]

Observe que essa correspondência diferencia maiúsculas de minúsculas.

## <a name="to-match-a-character-in-the-string-expression-against-a-range-of-characters"></a>Para corresponder um caractere na expressão de cadeia de caracteres em relação a um intervalo de personagens

Coloque colchetes ( `[ ]` ) na cadeia de caracteres de padrão e, dentro dos colchetes, coloque os caracteres mais baixos e mais altos no intervalo, separados por um hífen ( `–` ). Qualquer caractere único dentro do intervalo faz uma correspondência bem-sucedida.

O exemplo a seguir testa se o `myString` consiste nos caracteres `num` seguidos de exatamente um dos caracteres `i` ,,,, `j` `k` `l` `m` ou `n` .

[!code-vb[VbVbalrOperators#73](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#73)]

Observe que essa correspondência diferencia maiúsculas de minúsculas.

## <a name="matching-empty-strings"></a>Correspondência de cadeias de caracteres vazias

`Like`trata a sequência `[]` como uma cadeia de caracteres de comprimento zero ( `""` ). Você pode usar `[]` para testar se a expressão de cadeia de caracteres inteira está vazia, mas não pode usá-la para testar se uma determinada posição na expressão de cadeia de caracteres está vazia. Se uma posição vazia for uma das opções que você precisa testar, você poderá usar `Like` mais de uma vez.

### <a name="to-match-a-character-in-the-string-expression-against-a-list-of-characters-or-no-character"></a>Para corresponder a um caractere na expressão de cadeia de caracteres em uma lista ou nenhum caractere

1. Chame o `Like` operador duas vezes na mesma expressão de cadeia de caracteres e conecte as duas chamadas com o [operador OR](../../../language-reference/operators/or-operator.md) ou o [Operador OrElse](../../../language-reference/operators/orelse-operator.md).

2. Na cadeia de caracteres de padrão da primeira `Like` cláusula, inclua a lista de caracteres entre colchetes ( `[ ]` ).

3. Na cadeia de caracteres de padrão da segunda `Like` cláusula, não coloque nenhum caractere na posição em questão.

    O exemplo a seguir testa o número de telefone de sete dígitos `phoneNum` para exatamente três dígitos numéricos, seguido por um espaço, um hífen ( `–` ), um ponto ( `.` ) ou nenhum caractere, seguido de exatamente quatro dígitos numéricos.

    [!code-vb[VbVbalrOperators#74](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#74)]

## <a name="see-also"></a>Confira também

- [Operadores de comparação](../../../language-reference/operators/comparison-operators.md)
- [Operadores e expressões](index.md)
- [Operador Like](../../../language-reference/operators/like-operator.md)
- [Tipo de dados da cadeia de caracteres](../../../language-reference/data-types/string-data-type.md)
