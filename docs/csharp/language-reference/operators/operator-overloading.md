---
title: Sobrecarga de Operador – referência de C#
description: Saiba como sobrecarregar um operador C# e quais operadores C# podem ser sobrecarregados.
ms.date: 07/05/2019
f1_keywords:
- operator_CSharpKeyword
- operator
helpviewer_keywords:
- operator keyword [C#]
- operator overloading [C#]
ms.openlocfilehash: 4fde25cac21b2cb32efc9282578f32102a0f607f
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916719"
---
# <a name="operator-overloading-c-reference"></a>Sobrecarga de operador (referência de C#)

Um tipo definido pelo usuário pode sobrecarregar um operador C# predefinido. Ou seja, um tipo pode fornecer a implementação personalizada de uma operação caso um ou ambos os operandos sejam desse tipo. A seção [Operadores sobrecarregáveis](#overloadable-operators) mostra quais operadores do C# podem ser sobrecarregados.

Use a palavra-chave `operator` para declarar um operador. Uma declaração de operador deve satisfazer as regras a seguir:

- Ela inclui os modificadores `public` e `static`.
- Um operador unário tem um parâmetro de entrada. Um operador binário tem dois parâmetros de entrada. Em cada caso, pelo menos um parâmetro deve ter o tipo `T` ou `T?`, em que `T` é o tipo que contém a declaração do operador.

O exemplo a seguir define uma estrutura simplificada para representar um número racional. A estrutura sobrecarrega alguns dos [operadores aritméticos](arithmetic-operators.md):

[!code-csharp[fraction example](snippets/shared/OperatorOverloading.cs)]

Você pode estender o exemplo anterior [definindo uma conversão implícita](user-defined-conversion-operators.md) de `int` para `Fraction` . Em seguida, os operadores sobrecarregados seriam compatíveis com os argumentos desses dois tipos. Ou seja, tornaria-se possível adicionar um inteiro a uma fração e obter uma fração como um resultado.

Use também a palavra-chave `operator` para definir uma conversão de tipo personalizado. Para saber mais, confira [Operadores de conversão definidos pelo usuário](user-defined-conversion-operators.md).

## <a name="overloadable-operators"></a>Operadores sobrecarregáveis

A tabela a seguir fornece informações sobre capacidade de sobrecarga de operadores do C#:

| Operadores | Capacidade de sobrecarga |
| --------- | --------------- |
|[+x](arithmetic-operators.md#unary-plus-and-minus-operators), [-x](arithmetic-operators.md#unary-plus-and-minus-operators), [!x](boolean-logical-operators.md#logical-negation-operator-), [~x](bitwise-and-shift-operators.md#bitwise-complement-operator-), [++](arithmetic-operators.md#increment-operator-), [--](arithmetic-operators.md#decrement-operator---), [true](true-false-operators.md), [false](true-false-operators.md)|Esses operadores unários podem ser sobrecarregados.|
|[x + y](addition-operator.md), [x-y](subtraction-operator.md), [x \* y](arithmetic-operators.md#multiplication-operator-), [x/y](arithmetic-operators.md#division-operator-), [x% y](arithmetic-operators.md#remainder-operator-), [x & y](boolean-logical-operators.md#logical-and-operator-), [x &#124; y](boolean-logical-operators.md#logical-or-operator-), [x ^ y](boolean-logical-operators.md#logical-exclusive-or-operator-), [x \<\< y](bitwise-and-shift-operators.md#left-shift-operator-), [x >> y](bitwise-and-shift-operators.md#right-shift-operator-), [x = = y](equality-operators.md#equality-operator-), [x! = y](equality-operators.md#inequality-operator-), [x \< y](comparison-operators.md#less-than-operator-), [x > y](comparison-operators.md#greater-than-operator-), [x \<= y](comparison-operators.md#less-than-or-equal-operator-), [x > = y](comparison-operators.md#greater-than-or-equal-operator-)|Esses operadores binários podem ser sobrecarregados. Determinados operadores devem ser sobrecarregados em pares; para obter mais informações, consulte a observação após esta tabela.|
|[x && y](boolean-logical-operators.md#conditional-logical-and-operator-), [x &#124;&#124; y](boolean-logical-operators.md#conditional-logical-or-operator-)|Operadores lógicos condicionais não podem ser sobrecarregados. No entanto, se um tipo com os [ `true` `false` operadores sobrecarregado e](true-false-operators.md) também sobrecarregar o `&` operador or <code>&#124;</code> de uma determinada maneira, o `&&` operador or <code>&#124;&#124;</code> , respectivamente, poderá ser avaliado para os operandos desse tipo. Para obter mais informações, veja a seção [Operadores lógicos condicionais definidos pelo usuário](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators) na [especificação da linguagem C#](~/_csharplang/spec/introduction.md).|
|[um&#91;eu&#93;](member-access-operators.md#indexer-operator-),[`a?[i]`](member-access-operators.md#null-conditional-operators--and-)|O acesso de elemento não é considerado um operador que pode ser sobrecarregado, mas você pode definir um [indexador](../../programming-guide/indexers/index.md).|
|[(T) x](type-testing-and-cast.md#cast-expression)|O operador cast não pode ser sobrecarregado, mas você pode definir conversões de tipo personalizadas que podem ser executadas por uma expressão de conversão. Para saber mais, confira [Operadores de conversão definidos pelo usuário](user-defined-conversion-operators.md).|
|[+=](arithmetic-operators.md#compound-assignment), [-=](arithmetic-operators.md#compound-assignment) , [\*=](arithmetic-operators.md#compound-assignment) , [/=](arithmetic-operators.md#compound-assignment) , [%=](arithmetic-operators.md#compound-assignment) , [&=](boolean-logical-operators.md#compound-assignment) , [&#124;=](boolean-logical-operators.md#compound-assignment), [^=](boolean-logical-operators.md#compound-assignment) ,[\<\<=](bitwise-and-shift-operators.md#compound-assignment), [>>=](bitwise-and-shift-operators.md#compound-assignment)|Operadores de atribuição compostos não podem ser sobrecarregados explicitamente. No entanto, quando um operador binário estiver sobrecarregado, o operador de atribuição composto correspondente, se houver, também estará implicitamente sobrecarregado. Por exemplo, `+=` é avaliado usando `+`, que pode ser sobrecarregado.|
|[^ x](member-access-operators.md#index-from-end-operator-), [x = y](assignment-operator.md), [x. y](member-access-operators.md#member-access-expression-), [`x?.y`](member-access-operators.md#null-conditional-operators--and-) , [c? t: f](conditional-operator.md), [x?? y](null-coalescing-operator.md), [x?? = y](null-coalescing-operator.md), [x.. y](member-access-operators.md#range-operator-), [x->y](pointer-related-operators.md#pointer-member-access-operator--), [=>](lambda-operator.md) , [f (x)](member-access-operators.md#invocation-expression-), [as](type-testing-and-cast.md#as-operator), [Await](await.md), [marcada](../keywords/checked.md), [desmarcada](../keywords/unchecked.md), [padrão](default.md), [delegado](delegate-operator.md), [is](type-testing-and-cast.md#is-operator), [nameof](nameof.md), [New](new-operator.md), [sizeof](sizeof.md), [stackalloc](stackalloc.md), [switch](switch-expression.md), [typeof](type-testing-and-cast.md#typeof-operator)|Esses operadores não podem ser sobrecarregados.|

> [!NOTE]
> Os operadores de comparação precisam ser sobrecarregados em pares. Ou seja, se o operador de um par está sobrecarregado, o outro operador precisa estar sobrecarregado também. Esses pares são os seguintes:
>
> - Operadores `==` e `!=`
> - Operadores `<` e `>`
> - Operadores `<=` e `>=`

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, confira as seguintes seções da [especificação da linguagem C#](~/_csharplang/spec/introduction.md):

- [Sobrecarga de operador](~/_csharplang/spec/expressions.md#operator-overloading)
- [Operadores](~/_csharplang/spec/classes.md#operators)

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- [Operadores de conversões definidas pelo usuário](user-defined-conversion-operators.md)
- [Diretrizes de design – sobrecargas de operador](../../../standard/design-guidelines/operator-overloads.md)
- [Diretrizes de design-operadores de igualdade](../../../standard/design-guidelines/equality-operators.md)
- [Por que os operadores sobrecarregados sempre são estáticos em C#?](https://docs.microsoft.com/archive/blogs/ericlippert/why-are-overloaded-operators-always-static-in-c)
