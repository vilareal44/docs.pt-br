---
title: Como calcular valores numéricos
ms.date: 07/20/2015
helpviewer_keywords:
- operator precedence
- operators [Visual Basic]
- expressions [Visual Basic], numeric
- calculations [Visual Basic], numeric expressions
- precedence [Visual Basic], of operators
- Visual Basic code, operators
- Visual Basic code, expressions
- numeric expressions
ms.assetid: ba6bf43d-bd96-49b8-b1de-4a7797551372
ms.openlocfilehash: 94b02693f308dcfcfa6983f2750a26d9d419f7be
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403453"
---
# <a name="how-to-calculate-numeric-values-visual-basic"></a>Como calcular valores numéricos (Visual Basic)
Você pode calcular valores numéricos com o uso de expressões numéricas. Uma *expressão numérica* é uma expressão que contém literais, constantes e variáveis que representam valores numéricos e operadores que atuam nesses valores.  
  
## <a name="calculating-numeric-values"></a>Calculando valores numéricos  
  
#### <a name="to-calculate-a-numeric-value"></a>Para calcular um valor numérico  
  
- Combine uma ou mais literais, constantes e variáveis numéricas em uma expressão numérica. O exemplo a seguir mostra algumas expressões numéricas válidas.  
  
     `93.217`  
  
     `System.Math.PI`  
  
     `counter`  
  
     `4 * (67 + i)`  
  
     As três primeiras linhas mostram um literal, uma constante e uma variável. Cada uma forma uma expressão numérica válida por si só. A linha final mostra uma combinação de uma variável com dois literais.  
  
     Observe que uma expressão numérica não forma uma instrução Visual Basic completa por si só. Você deve usar a expressão como parte de uma instrução completa.  
  
#### <a name="to-store-a-numeric-value"></a>Para armazenar um valor numérico  
  
- Você pode usar uma instrução de atribuição para atribuir o valor representado por uma expressão numérica a uma variável, como demonstra o exemplo a seguir.  
  
     [!code-vb[VbVbalrOperators#82](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#82)]  
  
     No exemplo anterior, o valor da expressão no lado direito do operador EQUAL ( `=` ) é atribuído à variável `j` no lado esquerdo do operador, portanto, é `j` avaliado como 276.  
  
     Para obter mais informações, consulte [Instruções](../../../language-reference/statements/index.md).  
  
## <a name="multiple-operators"></a>Vários operadores  
 Se a expressão numérica contiver mais de um operador, a ordem na qual eles são avaliados será determinada pelas regras de precedência de operador. Para substituir as regras de precedência de operador, você deve colocar expressões entre parênteses, como no exemplo acima; as expressões embutidas são avaliadas primeiro.  
  
#### <a name="to-override-normal-operator-precedence"></a>Para substituir a precedência de operador normal  
  
- Use parênteses para colocar as operações que você deseja executar primeiro. O exemplo a seguir mostra dois resultados diferentes com os mesmos operandos e operadores.  
  
     [!code-vb[VbVbalrOperators#83](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#83)]  
  
     No exemplo anterior, o cálculo para `j` executa o operador de adição ( `+` ) primeiro porque os parênteses em volta da `(67 + i)` precedência normal de substituição e o valor atribuído a `j` é 276 (4 vezes 69). O cálculo para o `k` executa os operadores em sua precedência normal ( `*` antes `+` ) e o valor atribuído a `k` é 270 (268 mais 2).  
  
     Para obter mais informações, consulte [precedência de operador em Visual Basic](../../../language-reference/operators/operator-precedence.md).  
  
## <a name="see-also"></a>Confira também

- [Operadores e expressões](index.md)
- [Comparações de valor](value-comparisons.md)
- [Instruções](../../../language-reference/statements/index.md)
- [Precedência do operador no Visual Basic](../../../language-reference/operators/operator-precedence.md)
- [Operadores aritméticos](../../../language-reference/operators/arithmetic-operators.md)
- [Combinação eficiente de operadores](efficient-combination-of-operators.md)
