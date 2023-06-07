---
title: Operadores aritméticos – referência do C#
description: Saiba mais sobre os operadores do C# que executam operações de multiplicação, divisão, resto, adição e subtração com tipos numéricos.
ms.date: 05/11/2020
author: pkulikov
f1_keywords:
- ++_CSharpKeyword
- --_CSharpKeyword
- '*_CSharpKeyword'
- /_CSharpKeyword
- '%_CSharpKeyword'
- +_CSharpKeyword
- -_CSharpKeyword
- '%=_CSharpKeyword'
- '*=_CSharpKeyword'
helpviewer_keywords:
- arithmetic operators [C#]
- increment operator [C#]
- ++ operator [C#]
- decrement operator [C#]
- -- operator [C#]
- multiplication operator [C#]
- '* operator [C#]'
- division operator [C#]
- / operator [C#]
- remainder operator [C#]
- '% operator [C#]'
- addition operator [C#]
- + operator [C#]
- subtraction operator [C#]
- '- operator [C#]'
ms.openlocfilehash: f5da9c4433ffcf3e6a110bbb1543343289240778
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916944"
---
# <a name="arithmetic-operators-c-reference"></a>Operadores aritméticos (referência do C#)

Os operadores a seguir executam operações aritméticas com operandos de tipos numéricos:

- Operadores unários [ `++` (incremento](#increment-operator-)), [ `--` (decrementar)](#decrement-operator---), [ `+` (além)](#unary-plus-and-minus-operators)e [ `-` (menos)](#unary-plus-and-minus-operators)
- Operadores binário [ `*` (multiplicação)](#multiplication-operator-), [ `/` (divisão)](#division-operator-), [ `%` (restante)](#remainder-operator-), [ `+` (adição)](#addition-operator-)e [ `-` (subtração)](#subtraction-operator--)

Esses operadores têm suporte de todos os tipos numéricos [integral](../builtin-types/integral-numeric-types.md) e [de ponto flutuante](../builtin-types/floating-point-numeric-types.md) .

No caso de tipos integrais, esses operadores (exceto os `++` `--` operadores e) são definidos para os `int` `uint` tipos,, `long` e `ulong` . Quando operandos são de outros tipos integrais (,,, `sbyte` `byte` `short` `ushort` ou `char` ), seus valores são convertidos para o `int` tipo, que também é o tipo de resultado de uma operação. Quando operandos são de diferentes tipos de ponto flutuante ou integral, seus valores são convertidos para o tipo recipiente mais próximo, se esse tipo existir. Para saber mais, confira a seção [Promoções numéricas](~/_csharplang/spec/expressions.md#numeric-promotions) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md). Os `++` `--` operadores e são definidos para todos os tipos de inteiro e numérico de ponto flutuante e o tipo [Char](../builtin-types/char.md) .

## <a name="increment-operator-"></a>Operador de incremento ++

O operador de incremento unário `++` incrementa seu operando em 1. O operando precisa ser uma variável, um acesso de [propriedade](../../programming-guide/classes-and-structs/properties.md) ou um acesso de [indexador](../../programming-guide/indexers/index.md).

Há duas formas de suporte para o operador de incremento: o operador de incremento pós-fixado, `x++`, e o operador de incremento pré-fixado, `++x`.

### <a name="postfix-increment-operator"></a>Operador de incremento pós-fixado

O resultado de `x++` é o valor de `x` *antes* da operação, como mostra o exemplo a seguir:

[!code-csharp-interactive[postfix increment](snippets/shared/ArithmeticOperators.cs#PostfixIncrement)]

### <a name="prefix-increment-operator"></a>Operador de incremento de prefixo

O resultado de `++x` é o valor de `x` *após* a operação, como mostra o exemplo a seguir:

[!code-csharp-interactive[prefix increment](snippets/shared/ArithmeticOperators.cs#PrefixIncrement)]

## <a name="decrement-operator---"></a>Operador de decremento --

O operador de decremento unário `--` decrementa o operando em 1. O operando precisa ser uma variável, um acesso de [propriedade](../../programming-guide/classes-and-structs/properties.md) ou um acesso de [indexador](../../programming-guide/indexers/index.md).

Há duas formas de suporte para o operador de decremento: o operador de decremento pós-fixado, `x--`, e o operador de decremento pré-fixado, `--x`.

### <a name="postfix-decrement-operator"></a>Operador de decremento pós-fixado

O resultado de `x--` é o valor de `x` *antes* da operação, como mostra o exemplo a seguir:

[!code-csharp-interactive[postfix decrement](snippets/shared/ArithmeticOperators.cs#PostfixDecrement)]

### <a name="prefix-decrement-operator"></a>Operador de decremento de prefixo

O resultado de `--x` é o valor de `x` *após* a operação, como mostra o exemplo a seguir:

[!code-csharp-interactive[prefix decrement](snippets/shared/ArithmeticOperators.cs#PrefixDecrement)]

## <a name="unary-plus-and-minus-operators"></a>Operadores unários de adição e subtração

O operador unário `+` retorna o valor do operando. O operador unário `-` calcula a negação numérica do operando.

[!code-csharp-interactive[unary plus and minus](snippets/shared/ArithmeticOperators.cs#UnaryPlusAndMinus)]

O tipo [ULONG](../builtin-types/integral-numeric-types.md) não dá suporte ao `-` operador unário.

## <a name="multiplication-operator-"></a>Operador de multiplicação *

O operador de multiplicação `*` calcula o produto dos operandos:

[!code-csharp-interactive[multiplication operator](snippets/shared/ArithmeticOperators.cs#Multiplication)]

O operador unário `*` é o [operador de indireção de ponteiro](pointer-related-operators.md#pointer-indirection-operator-).

## <a name="division-operator-"></a>Operador de divisão /

O operador de divisão `/` divide o operando à esquerda pelo operando à direita.

### <a name="integer-division"></a>Divisão de inteiros

Para os operandos de tipos inteiros, o resultado do operador `/` é de um tipo inteiro e igual ao quociente dos dois operandos arredondados para zero:

[!code-csharp-interactive[integer division](snippets/shared/ArithmeticOperators.cs#IntegerDivision)]

Para obter o quociente dos dois operandos como um número de ponto flutuante, use o tipo `float`, `double` ou `decimal`:

[!code-csharp-interactive[integer as floating-point division](snippets/shared/ArithmeticOperators.cs#IntegerAsFloatingPointDivision)]

### <a name="floating-point-division"></a>Divisão de ponto flutuante

Para os tipos `float`, `double` e `decimal`, o resultado do operador `/` é o quociente dos dois operandos:

[!code-csharp-interactive[floating-point division](snippets/shared/ArithmeticOperators.cs#FloatingPointDivision)]

Se um dos operandos é `decimal`, outro operando não pode ser `float` nem `double`, porque nem `float` ou `double` é implicitamente conversível para `decimal`. Você deve converter explicitamente o operando `float` ou `double` para o tipo `decimal`. Para obter mais informações sobre conversões entre tipos numéricos, consulte [conversões numéricas internas](../builtin-types/numeric-conversions.md).

## <a name="remainder-operator-"></a>Operador de resto %

O operador de resto `%` calcula o resto após dividir o operando à esquerda pelo à direita.

### <a name="integer-remainder"></a>Resto inteiro

Para os operandos de tipos inteiros, o resultado de `a % b` é o valor produzido por `a - (a / b) * b`. O sinal do resto diferente de zero é o mesmo que o do operando à esquerda, conforme mostra o seguinte exemplo:

[!code-csharp-interactive[integer remainder](snippets/shared/ArithmeticOperators.cs#IntegerRemainder)]

Use o método <xref:System.Math.DivRem%2A?displayProperty=nameWithType> para calcular a divisão de inteiros e os resultados do resto.

### <a name="floating-point-remainder"></a>Resto de ponto flutuante

Para os operandos `float` e `double`, o resultado de `x % y` para o `x` e o `y` finitos é o valor `z` tal que

- O sinal de `z`, se diferente de zero, é o mesmo que o sinal de `x`.
- O valor absoluto de `z` é o valor produzido por `|x| - n * |y|`, em que `n` é o maior inteiro possível que é inferior ou igual a `|x| / |y|`, e `|x|` e `|y|` são os valores absolutos de `x` e `y`, respectivamente.

> [!NOTE]
> Esse método de computação do resto é análogo ao usado para operandos inteiros, mas diferente da especificação IEEE 754. Se você precisar da operação restante que está em conformidade com a especificação IEEE 754, use o <xref:System.Math.IEEERemainder%2A?displayProperty=nameWithType> método.

Para saber mais sobre o comportamento do operador `%` com operandos não finitos, confira a seção [Operador de restante](~/_csharplang/spec/expressions.md#remainder-operator) da [especificação da linguagem C#](~/_csharplang/spec/introduction.md).

Para os operandos `decimal`, o operador de resto `%` é equivalente ao [operador de resto](<xref:System.Decimal.op_Modulus(System.Decimal,System.Decimal)>) do tipo <xref:System.Decimal?displayProperty=nameWithType>.

O seguinte exemplo demonstra o comportamento do operador de resto com os operandos de ponto flutuante:

[!code-csharp-interactive[floating-point remainder](snippets/shared/ArithmeticOperators.cs#FloatingPointRemainder)]

## <a name="addition-operator-"></a>Operador de adição +

O operador de adição `+` calcula a soma dos operandos:

[!code-csharp-interactive[addition operator](snippets/shared/ArithmeticOperators.cs#Addition)]

Você também pode usar o `+` operador para concatenação de cadeia de caracteres e combinação de delegado. Para obter mais informações, consulte o artigo [ `+` `+=` operadores e](addition-operator.md) .

## <a name="subtraction-operator--"></a>Operador de subtração -

O operador de subtração `-` subtrai o operando à direita do operando à esquerda:

[!code-csharp-interactive[subtraction operator](snippets/shared/ArithmeticOperators.cs#Subtraction)]

Você também pode usar o `-` operador para remoção de delegado. Para obter mais informações, consulte o artigo [ `-` `-=` operadores e](subtraction-operator.md) .

## <a name="compound-assignment"></a>Atribuição composta

Para um operador binário `op`, uma expressão de atribuição composta do formato

```csharp
x op= y
```

é equivalente a

```csharp
x = x op y
```

exceto que `x` é avaliado apenas uma vez.

O seguinte exemplo demonstra o uso da atribuição composta com operadores aritméticos:

[!code-csharp-interactive[compound assignment](snippets/shared/ArithmeticOperators.cs#CompoundAssignment)]

Devido a [promoções numéricas](~/_csharplang/spec/expressions.md#numeric-promotions), o resultado da operação `op` pode não ser implicitamente conversível no tipo `T` de `x`. Nesse caso, se `op` for um operador predefinido e o resultado da operação for explicitamente convertido no tipo `T` de `x`, uma expressão de atribuição composta da forma `x op= y` será equivalente a `x = (T)(x op y)`, exceto que `x` será avaliada apenas uma vez. O exemplo a seguir demonstra esse comportamento:

[!code-csharp-interactive[compound assignment with cast](snippets/shared/ArithmeticOperators.cs#CompoundAssignmentWithCast)]

Você também usa os `+=` `-=` operadores e para assinar e cancelar a assinatura de um [evento](../keywords/event.md), respectivamente. Para obter mais informações, consulte [como assinar e cancelar a assinatura de eventos](../../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md).

## <a name="operator-precedence-and-associativity"></a>Precedência e associatividade do operador

A seguinte lista ordena os operadores aritméticos da precedência mais alta para a mais baixa:

- Incluir um pós-fixo a operadores de incremento `x++` e decremento `x--`
- Incluir um prefixo a operadores de incremento `++x` e de decremento `--x` e operadores unários `+` e `-`
- Operadores de multiplicação `*`, `/` e `%`
- Operadores de adição `+` e `-`

Operadores aritméticos binários são associativos à esquerda. Ou seja, os operadores com o mesmo nível de precedência são avaliados da esquerda para a direita.

Use parênteses, `()`, para alterar a ordem de avaliação imposta pela precedência e pela capacidade de associação do operador.

[!code-csharp-interactive[precedence and associativity](snippets/shared/ArithmeticOperators.cs#PrecedenceAndAssociativity)]

Para obter a lista completa de operadores C# ordenados por nível de precedência, consulte a seção [precedência de operador](index.md#operator-precedence) do artigo sobre [operadores do c#](index.md) .

## <a name="arithmetic-overflow-and-division-by-zero"></a>Estouro aritmético e divisão por zero

Quando o resultado de uma operação aritmética está fora do intervalo de valores finitos possíveis do tipo numérico envolvido, o comportamento de um operador aritmético depende do tipo dos operandos.

### <a name="integer-arithmetic-overflow"></a>Estouro aritmético de inteiros

A divisão de inteiro por zero sempre lança um <xref:System.DivideByZeroException>.

No caso de um estouro aritmético de inteiros, um contexto de verificação de estouro, que pode ser [verificado ou não verificado](../keywords/checked-and-unchecked.md), controla o comportamento resultante:

- Em um contexto verificado, se o estouro acontece em uma expressão de constante, ocorre um erro em tempo de compilação. Caso contrário, quando a operação é executada em tempo de execução, uma <xref:System.OverflowException> é gerada.
- Em um contexto não verificado, o resultado é truncado pelo descarte dos bits de ordem superior que não se ajustam ao tipo de destino.

Juntamente com as instruções [verificadas e não verificadas](../keywords/checked-and-unchecked.md), você pode usar os operadores `checked` e `unchecked` para controlar o contexto de verificação de estouro, no qual uma expressão é avaliada:

[!code-csharp-interactive[checked and unchecked](snippets/shared/ArithmeticOperators.cs#CheckedUnchecked)]

Por padrão, as operações aritméticas ocorrem em um contexto *não verificado*.

### <a name="floating-point-arithmetic-overflow"></a>Estouro aritmético de ponto flutuante

As operações aritméticas com os tipos `float` e `double` nunca geram uma exceção. O resultado de operações aritméticas com esses tipos pode ser um dos valores especiais que representam o infinito e um valor não é um número:

[!code-csharp-interactive[double non-finite values](snippets/shared/ArithmeticOperators.cs#FloatingPointOverflow)]

Para os operandos do tipo `decimal`, o estouro aritmético sempre gera uma <xref:System.OverflowException> e a divisão por zero sempre gera uma <xref:System.DivideByZeroException>.

## <a name="round-off-errors"></a>Erros de arredondamento

Devido a limitações gerais da representação de ponto flutuante de números reais e aritméticas de ponto flutuante, erros de arredondamento podem ocorrer em cálculos com tipos de ponto flutuante. Ou seja, o resultado produzido de uma expressão pode diferir do resultado matemático esperado. O seguinte exemplo demonstra vários casos desse tipo:

[!code-csharp-interactive[round-off errors](snippets/shared/ArithmeticOperators.cs#RoundOffErrors)]

Para obter mais informações, consulte comentários nas páginas de referência [System. Double](/dotnet/api/system.double#remarks), [System. single](/dotnet/api/system.single#remarks)ou [System. decimal](/dotnet/api/system.decimal#remarks) .

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

Um tipo definido pelo usuário pode [sobrecarregar](operator-overloading.md) os operadores aritméticos unários ( `++` , `--` , `+` , e `-` ) e binary ( `*` , `/` ,, `%` `+` e `-` ). Quando um operador binário está sobrecarregado, o operador de atribuição composta correspondente também é implicitamente sobrecarregado. Um tipo definido pelo usuário não pode sobrecarregar explicitamente um operador de atribuição composta.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, confira as seguintes seções da [especificação da linguagem C#](~/_csharplang/spec/introduction.md):

- [Operadores de incremento e decremento pós-fixados](~/_csharplang/spec/expressions.md#postfix-increment-and-decrement-operators)
- [Operadores de incremento e decremento pré-fixados](~/_csharplang/spec/expressions.md#prefix-increment-and-decrement-operators)
- [Operador de adição de unário](~/_csharplang/spec/expressions.md#unary-plus-operator)
- [Operador de subtração de unário](~/_csharplang/spec/expressions.md#unary-minus-operator)
- [Operador de multiplicação](~/_csharplang/spec/expressions.md#multiplication-operator)
- [Operador de divisão](~/_csharplang/spec/expressions.md#division-operator)
- [Operador de resto](~/_csharplang/spec/expressions.md#remainder-operator)
- [Operador de adição](~/_csharplang/spec/expressions.md#addition-operator)
- [Operador de subtração](~/_csharplang/spec/expressions.md#subtraction-operator)
- [Atribuição composta](~/_csharplang/spec/expressions.md#compound-assignment)
- [Os operadores verificados e não verificados](~/_csharplang/spec/expressions.md#the-checked-and-unchecked-operators)
- [Promoções numéricas](~/_csharplang/spec/expressions.md#numeric-promotions)

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- <xref:System.Math?displayProperty=nameWithType>
- <xref:System.MathF?displayProperty=nameWithType>
- [Numéricos no .NET](../../../standard/numerics.md)
