---
title: Operadores de comparação
ms.date: 07/20/2015
helpviewer_keywords:
- comparison operators [Visual Basic], comparing strings
- comparison operators [Visual Basic], comparing objects
- strings [Visual Basic], comparing
- comparison operators [Visual Basic]
- string comparison [Visual Basic], operators
- objects [Visual Basic], comparing
- numbers [Visual Basic], comparing
- Visual Basic code, operators
- string comparison [Visual Basic]
- numeric values [Visual Basic], comparing
- comparison operators [Visual Basic], comparing numeric values
- operators [Visual Basic], comparison
ms.assetid: 0b570339-5407-474f-8421-e183a8b303ee
ms.openlocfilehash: 7a93928ff95e307c64149da7ab21476ffd4fa77d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388823"
---
# <a name="comparison-operators-in-visual-basic"></a>Operadores de comparação no Visual Basic
Os operadores de comparação comparam duas expressões e retornam um `Boolean` valor que representa a relação de seus valores. Há operadores para comparar valores numéricos, operadores para comparação de cadeias de caracteres e operadores para comparar objetos. Todos os três tipos de operadores são discutidos neste documento.  
  
## <a name="comparing-numeric-values"></a>Comparando valores numéricos  
 Visual Basic compara valores numéricos usando seis operadores de comparação numérica. Cada operador usa como operandos duas expressões que são avaliadas como valores numéricos. A tabela a seguir lista os operadores e mostra exemplos de cada um.  
  
|Operador|Condição testada|Exemplos|  
|--------------|----------------------|--------------|  
|`=`Igualmente|O valor da primeira expressão é igual ao valor da segunda?|`23`   `=`   `33    ' False`<br /><br /> `23`   `=`   `23    ' True`<br /><br /> `23`   `=`   `12    ' False`|  
|`<>`Desigualdade|O valor da primeira expressão é diferente do valor da segunda?|`23`   `<>`   `33    ' True`<br /><br /> `23`   `<>`   `23    ' False`<br /><br /> `23`   `<>`   `12    ' True`|  
|`<`(Menor que)|O valor da primeira expressão é menor que o valor da segunda?|`23`   `<`   `33    ' True`<br /><br /> `23`   `<`   `23    ' False`<br /><br /> `23`   `<`   `12    ' False`|  
|`>`(Maior que)|O valor da primeira expressão é maior que o valor do segundo?|`23`   `>`   `33    ' False`<br /><br /> `23`   `>`   `23    ' False`<br /><br /> `23`   `>`   `12    ' True`|  
|`<=`(Menor que ou igual a)|O valor da primeira expressão é menor ou igual ao valor do segundo?|`23`   `<=`   `33    ' True`<br /><br /> `23`   `<=`   `23    ' True`<br /><br /> `23`   `<=`   `12    ' False`|  
|`>=`(Maior ou igual a)|O valor da primeira expressão é maior ou igual ao valor do segundo?|`23`   `>=`   `33    ' False`<br /><br /> `23`   `>=`   `23    ' True`<br /><br /> `23`   `>=`   `12    ' True`|  
  
## <a name="comparing-strings"></a>Comparando cadeias de caracteres  
 Visual Basic compara Cadeias de caracteres usando o [operador Like](../../../language-reference/operators/like-operator.md) , bem como os operadores de comparação numéricos. O `Like` operador permite que você especifique um padrão. Em seguida, a cadeia de caracteres é comparada com o padrão e, se corresponder, o resultado será `True` . Caso contrário, o resultado será `False`. Os operadores numéricos permitem comparar `String` valores com base em sua ordem de classificação, como mostra o exemplo a seguir.  
  
 `"73" < "9"`  
  
 `' The result of the preceding comparison is True.`  
  
 O resultado no exemplo anterior é `True` porque o primeiro caractere na primeira cadeia de caracteres é classificado antes do primeiro caractere na segunda cadeia de caracteres. Se os primeiros caracteres forem iguais, a comparação continuaria com o próximo caractere em ambas as cadeias, e assim por diante. Você também pode testar a igualdade de cadeias de caracteres usando o operador de igualdade, como mostra o exemplo a seguir.  
  
 `"734" = "734"`  
  
 `' The result of the preceding comparison is True.`  
  
 Se uma cadeia de caracteres for um prefixo de outro, como "AA" e "AAA", a cadeia de caracteres mais longa será considerada maior que a cadeia de caracteres mais curta. O exemplo a seguir ilustra isto.  
  
 `"aaa" > "aa"`  
  
 `' The result of the preceding comparison is True.`  
  
 A ordem de classificação é baseada em uma comparação binária ou em uma comparação textual, dependendo da configuração de `Option Compare` . Para obter mais informações [, consulte Option Compare Statement](../../../language-reference/statements/option-compare-statement.md).  
  
## <a name="comparing-objects"></a>Comparando objetos  
 Visual Basic compara duas variáveis de referência de objeto com o [operador is](../../../language-reference/operators/is-operator.md) e o [operador IsNot](../../../language-reference/operators/isnot-operator.md). Você pode usar qualquer um desses operadores para determinar se duas variáveis de referência referem-se à mesma instância de objeto. O exemplo a seguir ilustra isto.  
  
 [!code-vb[VbVbalrOperators#65](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#65)]  
  
 No exemplo anterior, `x Is y` é avaliado como `True` , porque ambas as variáveis referem-se à mesma instância. Compare esse resultado com o exemplo a seguir.  
  
 [!code-vb[VbVbalrOperators#66](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#66)]  
  
 No exemplo anterior, `x Is y` é avaliado como `False` , porque embora as variáveis se refiram a objetos do mesmo tipo, eles se referem a diferentes instâncias desse tipo.  
  
 Quando você deseja testar dois objetos que não apontam para a mesma instância, o `IsNot` operador permite evitar uma combinação de e atarefado de forma gramatical de `Not` e `Is` . O exemplo a seguir ilustra isto.  
  
 [!code-vb[VbVbalrOperators#67](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#67)]  
  
 No exemplo anterior, `If a IsNot b` é equivalente a `If Not a Is b` .  
  
### <a name="comparing-object-type"></a>Comparando tipo de objeto  
 Você pode testar se um objeto é de um tipo específico com a `TypeOf` expressão.... `Is` A sintaxe é mostrada a seguir:  
  
 `TypeOf <objectexpression> Is <typename>`  
  
 Quando `typename` especifica um tipo de interface, a `TypeOf` expressão... `Is` retorna `True` se o objeto implementa o tipo de interface. Quando `typename` é um tipo de classe, a expressão retorna `True` se o objeto é uma instância da classe especificada ou de uma classe que deriva da classe especificada. O exemplo a seguir ilustra isto.  
  
 [!code-vb[VbVbalrOperators#68](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#68)]  
  
 No exemplo anterior, a `TypeOf x Is Control` expressão é avaliada como `True` porque o tipo de `x` é `Button` , que é herdado de `Control` .  
  
 Para obter mais informações, consulte [operador typeof](../../../language-reference/operators/typeof-operator.md).  
  
## <a name="see-also"></a>Confira também

- [Comparações de valor](value-comparisons.md)
- [Operadores de comparação](../../../language-reference/operators/comparison-operators.md)
- [Operadores](../../../language-reference/operators/index.md)
- [Operadores aritméticos no Visual Basic](arithmetic-operators.md)
- [Operadores de concatenação no Visual Basic](concatenation-operators.md)
- [Operadores lógicos e bit a bit no Visual Basic](logical-and-bitwise-operators.md)
