---
title: Expressões e operadores do c# – referência C#
description: Saiba mais sobre operadores e expressões de C#, precedência de operador e Associação de operador
ms.date: 08/04/2020
f1_keywords:
- cs.operators
helpviewer_keywords:
- operators [C#]
- operator precedence [C#]
- operator associativity [C#]
- expressions [C#]
ms.assetid: 0301e31f-22ad-49af-ac3c-d5eae7f0ac43
ms.openlocfilehash: 854d7c1278319869104e1758ba91eb3594741126
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063179"
---
# <a name="c-operators-and-expressions-c-reference"></a>Operadores e expressões do c# (referência C#)

O C# fornece vários operadores. Muitos deles têm suporte dos [tipos internos](../builtin-types/built-in-types.md) e permitem que você execute operações básicas com valores desses tipos. Esses operadores incluem os seguintes grupos:

- [Operadores aritméticos](arithmetic-operators.md) que executam operações aritméticas com operandos numéricos
- [Operadores de comparação](comparison-operators.md) que comparam operandos numéricos
- [Operadores lógicos boolianos](boolean-logical-operators.md) que executam operações lógicas com [`bool`](../builtin-types/bool.md) operandos
- [Operadores de tecla e bit de alternância](bitwise-and-shift-operators.md) que executam operações de bit ou de Shift com operandos dos tipos integrais
- [Operadores de igualdade](equality-operators.md) que verificam se os operandos são iguais ou não

Normalmente, você pode [sobrecarregar](operator-overloading.md) esses operadores, ou seja, especificar o comportamento do operador para os operandos de um tipo definido pelo usuário.

As expressões C# mais simples são literais (por exemplo, números [inteiros](../builtin-types/integral-numeric-types.md#integer-literals) e [reais](../builtin-types/floating-point-numeric-types.md#real-literals) ) e nomes de variáveis. Você pode combiná-los em expressões complexas usando operadores. A [precedência](#operator-precedence) de operador e a [Associação](#operator-associativity) determinam a ordem na qual as operações em uma expressão são executadas. Você pode usar parênteses para alterar a ordem de avaliação imposta pela prioridade e pela associação dos operadores.

No código a seguir, exemplos de expressões estão no lado direito das atribuições:

[!code-csharp[expression examples](snippets/shared/Overview.cs#Expressions)]

Normalmente, uma expressão produz um resultado e pode ser incluída em outra expressão. Uma [`void`](../builtin-types/void.md) chamada de método é um exemplo de uma expressão que não produz um resultado. Ele pode ser usado apenas como uma [instrução](../../programming-guide/statements-expressions-operators/statements.md), como mostra o exemplo a seguir:

```csharp
Console.WriteLine("Hello, world!");
```

Aqui estão alguns outros tipos de expressões que o C# fornece:

- [Expressões de cadeia de caracteres interpoladas](../tokens/interpolated.md) que fornecem uma sintaxe conveniente para criar cadeias formatadas:

  [!code-csharp-interactive[interpolated string](snippets/shared/Overview.cs#InterpolatedString)]

- [Expressões lambda](lambda-expressions.md) que permitem criar funções anônimas:

  [!code-csharp-interactive[lambda expression](snippets/shared/Overview.cs#Lambda)]

- [Expressões de consulta](../keywords/query-keywords.md) que permitem que você use recursos de consulta diretamente em C#:

  [!code-csharp-interactive[query expression](snippets/shared/Overview.cs#Query)]

Você pode usar uma [definição de corpo de expressão](../../programming-guide/statements-expressions-operators/expression-bodied-members.md) para fornecer uma definição concisa para um método, Construtor, propriedade, indexador ou finalizador.

## <a name="operator-precedence"></a>Precedência do operador

Em uma expressão com vários operadores, os operadores com maior precedência são avaliados antes dos operadores com menor precedência. No exemplo a seguir, a multiplicação é executada primeiro porque tem uma precedência mais alta do que a adição:

```csharp-interactive
var a = 2 + 2 * 2;
Console.WriteLine(a); //  output: 6
```

Use parênteses para alterar a ordem de avaliação imposta pela precedência do operador:

```csharp-interactive
var a = (2 + 2) * 2;
Console.WriteLine(a); //  output: 8
```

A tabela a seguir lista os operadores C#, começando com a precedência mais alta até a mais baixa. Os operadores em cada linha têm a mesma precedência.

| Operadores | Categoria ou nome |
| --------- | ---------------- |
| [x. y](member-access-operators.md#member-access-expression-), [f (x)](member-access-operators.md#invocation-expression-), [a&#91;i&#93;](member-access-operators.md#indexer-operator-), [`x?.y`](member-access-operators.md#null-conditional-operators--and-) , [`x?[y]`](member-access-operators.md#null-conditional-operators--and-) , [x + +](arithmetic-operators.md#increment-operator-), [x--](arithmetic-operators.md#decrement-operator---), [x!](null-forgiving.md), [novo](new-operator.md), [typeof](type-testing-and-cast.md#typeof-operator), [Checked](../keywords/checked.md), [desmarcado](../keywords/unchecked.md), [padrão](default.md), [nameof](nameof.md), [delegate](delegate-operator.md), [sizeof](sizeof.md), [stackalloc](stackalloc.md), [x->y](pointer-related-operators.md#pointer-member-access-operator--) | Primário |
| [+ x](arithmetic-operators.md#unary-plus-and-minus-operators), [-x](arithmetic-operators.md#unary-plus-and-minus-operators), [ \! x](boolean-logical-operators.md#logical-negation-operator-), [~ x](bitwise-and-shift-operators.md#bitwise-complement-operator-), [+ + x](arithmetic-operators.md#increment-operator-), [--x](arithmetic-operators.md#decrement-operator---), [^ x](member-access-operators.md#index-from-end-operator-), [(T) x](type-testing-and-cast.md#cast-expression), [Await](await.md), [&x](pointer-related-operators.md#address-of-operator-), [* x](pointer-related-operators.md#pointer-indirection-operator-), [true e false](true-false-operators.md) | Unário |
| [x.. Iar](member-access-operators.md#range-operator-) | Intervalo |
| [switch](switch-expression.md) | Expressão `switch` |
| [x * y](arithmetic-operators.md#multiplication-operator-), [x / y](arithmetic-operators.md#division-operator-), [x % y](arithmetic-operators.md#remainder-operator-) | Multiplicativo|
| [x + y](arithmetic-operators.md#addition-operator-), [x – y](arithmetic-operators.md#subtraction-operator--) | Aditiva |
| [x \<\<  y](bitwise-and-shift-operators.md#left-shift-operator-), [x >> y](bitwise-and-shift-operators.md#right-shift-operator-) | Turno |
| [x \< y](comparison-operators.md#less-than-operator-), [x > y](comparison-operators.md#greater-than-operator-), [x \<= y](comparison-operators.md#less-than-or-equal-operator-), [x > = y](comparison-operators.md#greater-than-or-equal-operator-), [é](type-testing-and-cast.md#is-operator), [como](type-testing-and-cast.md#as-operator) | Teste de tipo e relacional |
| [x = = y](equality-operators.md#equality-operator-), [x! = y](equality-operators.md#inequality-operator-) | Igualitário |
| `x & y` | [AND lógico booliano](boolean-logical-operators.md#logical-and-operator-) ou [AND lógico bit a bit](bitwise-and-shift-operators.md#logical-and-operator-) |
| `x ^ y` | [XOR lógico booliano](boolean-logical-operators.md#logical-exclusive-or-operator-) ou [XOR lógico bit a bit](bitwise-and-shift-operators.md#logical-exclusive-or-operator-) |
| <code>x &#124; y</code> | [OR lógico booliano](boolean-logical-operators.md#logical-or-operator-) ou [OR lógico bit a bit](bitwise-and-shift-operators.md#logical-or-operator-) |
| [x && y](boolean-logical-operators.md#conditional-logical-and-operator-) | AND condicional |
| [x &#124;&#124; y](boolean-logical-operators.md#conditional-logical-or-operator-) | OR condicional |
| [x ?? y](null-coalescing-operator.md) | Operador de coalescência nula |
| [c ? t : f](conditional-operator.md) | Operador condicional |
| [x = y](assignment-operator.md), [x + = y](arithmetic-operators.md#compound-assignment), [x-= y](arithmetic-operators.md#compound-assignment), [x * = y](arithmetic-operators.md#compound-assignment), [x/= y](arithmetic-operators.md#compound-assignment), [x% = y](arithmetic-operators.md#compound-assignment), [x &= y](boolean-logical-operators.md#compound-assignment), [x &#124;= y](boolean-logical-operators.md#compound-assignment), [x ^ = y](boolean-logical-operators.md#compound-assignment), [x <<= y](bitwise-and-shift-operators.md#compound-assignment), [x >>= y](bitwise-and-shift-operators.md#compound-assignment), [x?? = y](null-coalescing-operator.md),[=>](lambda-operator.md) | Declaração de atribuição e lambda |

## <a name="operator-associativity"></a>Associação de operador

Quando os operadores têm a mesma precedência, a associação dos operadores determina a ordem na qual as operações são executadas:

- Os operadores *associativos esquerdos* são avaliados na ordem da esquerda para a direita. Exceto para os operadores de [atribuição](assignment-operator.md) e os [operadores de União nulo](null-coalescing-operator.md), todos os operadores binários são associativos à esquerda. Por exemplo, `a + b - c` é avaliado como `(a + b) - c`.
- Os operadores *associativos direitos* são avaliados na ordem da direita para a esquerda. Os operadores de atribuição, os operadores de União nula e o [operador `?:` condicional](conditional-operator.md) são associativos à direita. Por exemplo, `x = y = z` é avaliado como `x = (y = z)`.

Use parênteses para alterar a ordem de avaliação imposta pela associação de operador:

```csharp-interactive
int a = 13 / 5 / 2;
int b = 13 / (5 / 2);
Console.WriteLine($"a = {a}, b = {b}");  // output: a = 1, b = 6
```

## <a name="operand-evaluation"></a>Avaliação do operando

Sem considerar a relação com a precedência e a associação de operadores, os operandos em uma expressão são avaliados da esquerda para a direita. Os exemplos a seguir demonstram a ordem em que os operadores e os operandos são avaliados:

| Expression | Ordem de avaliação |
| ---------- | ------------------- |
|`a + b`|a, b, +|
|`a + b * c`|a, b, c, *, +|
|`a / b + c * d`|a, b, /, c, d, *, +|
|`a / (b + c) * d`|a, b, c, +, /, d, *|

Normalmente, todos os operandos do operador são avaliados. No entanto, alguns operadores avaliam os operandos condicionalmente. Ou seja, o valor do operando mais à esquerda de tal operador define if (ou quais) outros operandos devem ser avaliados. Esses operadores são os operadores lógicos condicional [and ( `&&` )](boolean-logical-operators.md#conditional-logical-and-operator-) e [or ( `||` )](boolean-logical-operators.md#conditional-logical-or-operator-) , os [operadores de União nula `??` e `??=` ](null-coalescing-operator.md), os [operadores condicionais NULL `?.` e `?[]` e ](member-access-operators.md#null-conditional-operators--and-)o [operador `?:` condicional ](conditional-operator.md). Para obter mais informações, consulte a descrição de cada operador.

## <a name="c-language-specification"></a>Especificação da linguagem C#

Para obter mais informações, confira as seguintes seções da [especificação da linguagem C#](~/_csharplang/spec/introduction.md):

- [Expressões](~/_csharplang/spec/expressions.md)
- [Operadores](~/_csharplang/spec/expressions.md#operators)

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Sobrecarga de operador](operator-overloading.md)
- [Árvores de expressão](../../programming-guide/concepts/expression-trees/index.md)
