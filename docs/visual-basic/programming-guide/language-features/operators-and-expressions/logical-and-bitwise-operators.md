---
title: Operadores lógicos e bit a bit
ms.date: 07/20/2015
helpviewer_keywords:
- short-circuiting
- Boolean expressions
- logical operators [Visual Basic], Boolean expressions
- operators [Visual Basic], logical
- AndAlso operator [Visual Basic]
- Not operator [Visual Basic], Boolean expressions
- Xor operator [Visual Basic], Boolean expressions
- And operator [Visual Basic], logical operators
- logical operators [Visual Basic]
- expressions [Visual Basic], Boolean
- Or operator [Visual Basic], logical operators
- Visual Basic code, operators
- short-circuiting [Visual Basic], logical operators
- logical operators [Visual Basic], short-circuiting
- Visual Basic code, expressions
- logical operators [Visual Basic], binary
- OrElse operator [Visual Basic]
- logical operators [Visual Basic], unary
ms.assetid: ca474e13-567d-4b1d-a18b-301433705e57
ms.openlocfilehash: 19d3511363f19319c2bd8ce872b62c1462b1370f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403402"
---
# <a name="logical-and-bitwise-operators-in-visual-basic"></a>Operadores lógicos e bit a bit no Visual Basic
Os operadores lógicos comparam `Boolean` expressões e retornam um `Boolean` resultado. Os `And` operadores,, `Or` `AndAlso` , `OrElse` e `Xor` são *binários* porque usam dois operandos, enquanto o `Not` operador é *unário* porque usa um único operando. Alguns desses operadores também podem executar operações lógicas de bit a bit em valores integrais.  
  
## <a name="unary-logical-operator"></a>Operador lógico unário  
 O [operador NOT](../../../language-reference/operators/not-operator.md) executa uma *negação* lógica em uma `Boolean` expressão. Ele gera o oposto lógico de seu operando. Se a expressão for avaliada como `True` , `Not` retornará `False` ; se a expressão for avaliada como `False` , `Not` retorna `True` . O exemplo a seguir ilustra isto.  
  
 [!code-vb[VbVbalrOperators#77](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#77)]  
  
## <a name="binary-logical-operators"></a>Operadores lógicos binários  
 O [operador and](../../../language-reference/operators/and-operator.md) executa uma *conjunção* lógica em duas `Boolean` expressões. Se as duas expressões avaliarem como `True` , `And` retornarão `True` . Se pelo menos uma das expressões for avaliada como `False` , `And` retorna `False` .  
  
 O [operador OR](../../../language-reference/operators/or-operator.md) executa a *disjunção* lógica ou a *inclusão* em duas `Boolean` expressões. Se qualquer expressão for avaliada como `True` , ou se for avaliada como `True` , `Or` retorna `True` . Se nenhuma expressão for avaliada como `True` , `Or` retorna `False` .  
  
 O [operador XOR](../../../language-reference/operators/xor-operator.md) executa a *exclusão* lógica em duas `Boolean` expressões. Se exatamente uma expressão for avaliada como `True` , mas não ambos, `Xor` retornará `True` . Se ambas as expressões forem avaliadas como `True` ou ambas forem avaliadas como `False` , `Xor` retorna `False` .  
  
 O exemplo a seguir ilustra `And` os `Or` operadores, e `Xor` .  
  
 [!code-vb[VbVbalrOperators#78](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#78)]  
  
## <a name="short-circuiting-logical-operations"></a>Operações lógicas de curto-circuito  
 O [Operador AndAlso](../../../language-reference/operators/andalso-operator.md) é muito semelhante ao `And` operador, pois ele também executa uma conjunção lógica em duas `Boolean` expressões. A principal diferença entre os dois é que o `AndAlso` exibe o comportamento *de curto-circuito* . Se a primeira expressão em uma `AndAlso` expressão for avaliada como `False` , a segunda expressão não será avaliada porque não pode alterar o resultado final e `AndAlso` retorna `False` .  
  
 Da mesma forma, o [Operador OrElse](../../../language-reference/operators/orelse-operator.md) executa uma disjunção lógica de curto-circuito em duas `Boolean` expressões. Se a primeira expressão em uma `OrElse` expressão for avaliada como `True` , a segunda expressão não será avaliada porque não pode alterar o resultado final e `OrElse` retorna `True` .  
  
### <a name="short-circuiting-trade-offs"></a>Compensações de curto-circuito  
 O curto-circuito pode melhorar o desempenho não avaliando uma expressão que não possa alterar o resultado da operação lógica. No entanto, se essa expressão executar ações adicionais, o circuito curto ignorará essas ações. Por exemplo, se a expressão incluir uma chamada para um `Function` procedimento, esse procedimento não será chamado se a expressão tiver um circuito curto e qualquer código adicional contido no `Function` não for executado. Portanto, a função pode ser executada apenas ocasionalmente e pode não ser testada corretamente. Ou a lógica do programa pode depender do código no `Function` .  
  
 O exemplo a seguir ilustra a diferença entre `And` , `Or` e suas contrapartes de curto-circuito.  
  
 [!code-vb[VbVbalrOperators#81](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#81)]  
  
 [!code-vb[VbVbalrOperators#80](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#80)]  
  
 [!code-vb[VbVbalrOperators#79](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#79)]  
  
 No exemplo anterior, observe que algum código importante dentro não `checkIfValid()` é executado quando a chamada é de curto-circuito. A primeira `If` instrução chama `checkIfValid()` , apesar de `12 > 45` Returns `False` , porque não `And` é um circuito curto. A segunda `If` instrução não chama `checkIfValid()` , porque quando `12 > 45` retorna `False` , a segunda expressão é retornada por `AndAlso` circuitos curtos. A terceira `If` instrução chama `checkIfValid()` , embora `12 < 45` retorne `True` , porque o `Or` não é um circuito curto. A quarta `If` instrução não chama `checkIfValid()` , porque, quando `12 < 45` retorna `True` , `OrElse` a segunda expressão é retornada por circuitos curtos.  
  
## <a name="bitwise-operations"></a>Operações bits  
 As operações bit a bit avaliam dois valores integrais no formulário binário (base 2). Eles comparam os bits em posições correspondentes e, em seguida, atribuem valores com base na comparação. O exemplo a seguir ilustra o `And` operador.  
  
 [!code-vb[VbVbalrConcepts#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConcepts/VB/Class1.vb#2)]  
  
 O exemplo anterior define o valor de `x` como 1. Isso ocorre pelos seguintes motivos:  
  
- Os valores são tratados como binários:  
  
     3 em formato binário = 011  
  
     5 em formato binário = 101  
  
- O `And` operador compara as representações binárias, uma posição binária (bit) por vez. Se ambos os bits em uma determinada posição forem 1, um 1 será colocado nessa posição no resultado. Se um dos dois bits for 0, um 0 será colocado nessa posição no resultado. No exemplo anterior, isso funciona da seguinte maneira:  
  
     011 (3 em formato binário)  
  
     101 (5 em formato binário)  
  
     001 (o resultado, em formato binário)  
  
- O resultado é tratado como Decimal. O valor 001 é a representação binária de 1, portanto `x` = 1.  
  
 Essa `Or` operação é semelhante, exceto pelo fato de que um 1 é atribuído ao bit de resultado se um ou ambos os bits comparados forem 1. `Xor`atribui um 1 ao bit de resultado se exatamente um dos bits comparados (não ambos) for 1. `Not`usa um único operando e inverte todos os bits, incluindo o bit de sinal e atribui esse valor ao resultado. Isso significa que, para números positivos assinados, `Not` sempre retorna um valor negativo e para números negativos, `Not` sempre retorna um valor positivo ou zero.  
  
 Os `AndAlso` operadores e não `OrElse` dão suporte a operações bit a bit.  
  
> [!NOTE]
> As operações de bit a bit podem ser executadas somente em tipos integrais. Os valores de ponto flutuante devem ser convertidos em tipos integrais antes que a operação bit-up possa continuar.  
  
## <a name="see-also"></a>Confira também

- [Operadores lógicos/bit a bit (Visual Basic)](../../../language-reference/operators/logical-bitwise-operators.md)
- [Expressões booleanas](boolean-expressions.md)
- [Operadores aritméticos no Visual Basic](arithmetic-operators.md)
- [Operadores de comparação no Visual Basic](comparison-operators.md)
- [Operadores de concatenação no Visual Basic](concatenation-operators.md)
- [Combinação eficiente de operadores](efficient-combination-of-operators.md)
