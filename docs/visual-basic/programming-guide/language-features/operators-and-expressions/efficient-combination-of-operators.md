---
title: Combinação eficiente de operadores
ms.date: 07/20/2015
helpviewer_keywords:
- expressions [Visual Basic], parentheses
- operators [Visual Basic], associativity
- expressions [Visual Basic], operators
- operators [Visual Basic], precedence
- Visual Basic code, operators
- Visual Basic code, expressions
- operators [Visual Basic], complex expressions
- expressions [Visual Basic], complex
- parentheses [Visual Basic], complex expressions
- numeric expressions
ms.assetid: bd22340e-b5be-458b-8772-3916c02309a4
ms.openlocfilehash: 3088072646278dac13e4d483cb4f99297eaad9ca
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403466"
---
# <a name="efficient-combination-of-operators-visual-basic"></a>Combinação eficiente de operadores (Visual Basic)
Expressões complexas podem conter muitos operadores diferentes. O exemplo a seguir ilustra isto.  
  
 `x = (45 * (y + z)) ^ (2 / 85) * 5 + z`  
  
 A criação de expressões complexas, como a do exemplo anterior, requer uma compreensão completa das regras de precedência de operador. Para obter mais informações, consulte [precedência de operador em Visual Basic](../../../language-reference/operators/operator-precedence.md).  
  
## <a name="parenthetical-expressions"></a>Expressões entre parênteses  
 Muitas vezes você deseja que as operações continuem em uma ordem diferente da determinada por precedência de operador. Considere o exemplo a seguir.  
  
 `x = z * y + 4`  
  
 O exemplo anterior multiplica `z` por `y` e, em seguida, adiciona o resultado a `4` . Mas se você quiser adicionar `y` e `4` antes de multiplicar o resultado por `z` , poderá substituir a precedência de operador normal usando parênteses. Ao colocar uma expressão entre parênteses, você força essa expressão a ser avaliada primeiro, independentemente da precedência do operador. Para forçar o exemplo anterior a fazer a adição primeiro, você pode reescrevê-lo como no exemplo a seguir.  
  
 `x = z * (y + 4)`  
  
 O exemplo anterior adiciona `y` e `4` , em seguida, multiplica essa soma por `z` .  
  
### <a name="nested-parenthetical-expressions"></a>Expressões de parênteses aninhadas  
 Você pode aninhar expressões em vários níveis de parênteses para substituir a precedência ainda mais. As expressões que mais profundamente são aninhadas em parênteses são avaliadas primeiro, seguidas pelo próximo aninhado mais profundamente e assim por diante para o menos aninhado e finalmente as expressões fora dos parênteses. O exemplo a seguir ilustra isto.  
  
 `x = (z * 4) ^ (y * (z + 2))`  
  
 No exemplo anterior, `z + 2` é avaliado primeiro e, em seguida, as outras expressões entre parênteses. A exponenciação, que normalmente tem maior precedência do que adição ou multiplicação, é avaliada por último neste exemplo porque as outras expressões são colocadas entre parênteses.  
  
## <a name="see-also"></a>Confira também

- [Operadores aritméticos no Visual Basic](arithmetic-operators.md)
- [Operadores de comparação no Visual Basic](comparison-operators.md)
- [Operadores lógicos e bit a bit no Visual Basic](logical-and-bitwise-operators.md)
- [Operadores lógicos/bit a bit (Visual Basic)](../../../language-reference/operators/logical-bitwise-operators.md)
- [Expressões booleanas](boolean-expressions.md)
- [Comparações de valor](value-comparisons.md)
- [Como calcular valores numéricos](how-to-calculate-numeric-values.md)
- [Precedência do operador no Visual Basic](../../../language-reference/operators/operator-precedence.md)
