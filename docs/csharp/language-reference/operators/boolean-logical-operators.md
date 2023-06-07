---
title: Operadores lógicos boolianos – referência do C#
description: Saiba mais sobre operadores C# que executam operações de negação lógica, conjunção (AND) e disjunção inclusiva e exclusiva (OR) com operandos booleanos.
ms.date: 06/29/2020
author: pkulikov
f1_keywords:
- '!_CSharpKeyword'
- '&_CSharpKeyword'
- ^_CSharpKeyword
- '|_CSharpKeyword'
- '&&_CSharpKeyword'
- '||_CSharpKeyword'
- '|=_CSharpKeyword'
- ^=_CSharpKeyword
- '&=_CSharpKeyword'
helpviewer_keywords:
- logical operators [C#]
- short-circuiting operators [C#]
- logical negation operator [C#]
- NOT operator [C#]
- '! operator [C#]'
- AND operator [C#]
- ampersand operator [C#]
- conjunction operator [C#]
- '& operator [C#]'
- exclusive OR operator [C#]
- XOR operator [C#]
- ^ operator [C#]
- OR operator [C#]
- disjunction operator [C#]
- '| operator [C#]'
- conditional AND operator [C#]
- short-circuiting AND operator [C#]
- '&& operator [C#]'
- conditional OR operator [C#]
- short-circuiting OR operator [C#]
- '|| operator [C#]'
ms.openlocfilehash: 00b1523029ed6562fda6947415029cd3b7a9b405
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916897"
---
# <a name="boolean-logical-operators-c-reference"></a>Operadores lógicos boolianos (referência do C#)

Os operadores a seguir executam operações lógicas com operandos [bool](../builtin-types/bool.md) :

- Operador unário [ `!` (negação lógica)](#logical-negation-operator-) .
- Operadores Binary [ `&` (and lógico)](#logical-and-operator-), [ `|` (OR lógico)](#logical-or-operator-)e [ `^` (exclusivo lógico)](#logical-exclusive-or-operator-) . Esses operadores sempre avaliam os dois operandos.
- Operadores Binary [ `&&` (and lógico condicional and)](#conditional-logical-and-operator-) e [ `||` (OR lógico condicional)](#conditional-logical-or-operator-) . Esses operadores avaliam o operando à direita apenas se for necessário.

Para operandos dos [tipos numéricos inteiros](../builtin-types/integral-numeric-types.md), os `&` `|` operadores, e `^` executam operações lógicas de bit-nte. Para obter mais informações, veja [Operadores bit a bit e shift](bitwise-and-shift-operators.md).

## <a name="logical-negation-operator-"></a>Operador de negação lógica !

O operador de prefixo unário `!` computa a negação lógica de seu operando. Ou seja, ele produz `true`, se o operando for avaliado como `false`, e `false`, se o operando for avaliado como `true`:

[!code-csharp-interactive[logical negation](snippets/shared/BooleanLogicalOperators.cs#Negation)]

A partir do C# 8,0, o operador de sufixo unário `!` é o [operador NULL-tolerante](null-forgiving.md).

## <a name="logical-and-operator-amp"></a><a name="logical-and-operator-"></a>Operador AND lógico&amp;

O operador `&` computa o AND lógico de seus operandos. O resultado de `x & y` será `true` se ambos `x` e `y` forem avaliados como `true`. Caso contrário, o resultado será `false`.

O `&` operador avalia os dois operandos mesmo se o operando esquerdo for avaliado `false` , de modo que o resultado da operação seja `false` independente do valor do operando à direita.

No exemplo a seguir, o operando à direita do operador `&` é uma chamada de método, que é executada independentemente do valor do operando à esquerda:

[!code-csharp-interactive[logical AND](snippets/shared/BooleanLogicalOperators.cs#And)]

O [operador AND lógico condicional](#conditional-logical-and-operator-) `&&` também computa o AND lógico e seus operandos, mas não avalia o operando à direita se o operando à esquerda for avaliado como `false`.

Para operandos dos [tipos numéricos inteiros](../builtin-types/integral-numeric-types.md), o `&` operador computa a [lógica e os opera bits de bit e](bitwise-and-shift-operators.md#logical-and-operator-) de seus operando. O operador `&` unário é o [operador address-of](pointer-related-operators.md#address-of-operator-).

## <a name="logical-exclusive-or-operator-"></a>Operador OR exclusivo lógico ^

O operador `^` computa o OR exclusivo lógico, também conhecido como o XOR lógico, de seus operandos. O resultado de `x ^ y` é `true` se `x` é avaliado como `true` e `y` avaliado como `false`, ou `x` avaliado como `false` e `y` avaliado como `true`. Caso contrário, o resultado será `false`. Ou seja, para os operandos `bool`, o operador `^` computa o mesmo resultado que o [operador de desigualdade](equality-operators.md#inequality-operator-) `!=`.

[!code-csharp-interactive[logical exclusive OR](snippets/shared/BooleanLogicalOperators.cs#Xor)]

Para operandos dos [tipos numéricos inteiros](../builtin-types/integral-numeric-types.md), o `^` operador computa o [bit lógico Exclusive ou](bitwise-and-shift-operators.md#logical-exclusive-or-operator-) de seus operandos.

## <a name="logical-or-operator-"></a>Operador OR lógico |

O operador `|` computa o OR lógico de seus operandos. O resultado de `x | y` será `true` se `x` ou `y` for avaliado como `true`. Caso contrário, o resultado será `false`.

O `|` operador avalia os dois operandos mesmo se o operando esquerdo for avaliado `true` , de modo que o resultado da operação seja `true` independente do valor do operando à direita.

No exemplo a seguir, o operando à direita do operador `|` é uma chamada de método, que é executada independentemente do valor do operando à esquerda:

[!code-csharp-interactive[logical OR](snippets/shared/BooleanLogicalOperators.cs#Or)]

O [operador OR lógico condicional](#conditional-logical-or-operator-) `||` também computa o OR lógico e seus operandos, mas não avalia o operando à direita se o operando à esquerda for avaliado como `true`.

Para os operandos dos [tipos numéricos inteiros](../builtin-types/integral-numeric-types.md), o `|` operador computa o [bit lógico ou](bitwise-and-shift-operators.md#logical-or-operator-) de seus operandos.

## <a name="conditional-logical-and-operator-ampamp"></a><a name="conditional-logical-and-operator-"></a>Operador AND lógico condicional&amp;&amp;

O operador AND lógico condicional `&&`, também conhecido como operador AND lógico de "curto-circuito", computa o AND lógico de seus operandos. O resultado de `x && y` será `true` se ambos `x` e `y` forem avaliados como `true`. Caso contrário, o resultado será `false`. Se `x` for avaliado como `false`, `y` não será avaliado.

No exemplo a seguir, o operando à direita do operador `&&` é uma chamada de método, que não é executada se o operando à esquerda for avaliado como `false`:

[!code-csharp-interactive[conditional logical AND](snippets/shared/BooleanLogicalOperators.cs#ConditionalAnd)]

O [operador AND lógico](#logical-and-operator-) `&` também computa o AND lógico de seus operandos, mas sempre avalia os dois operandos.

## <a name="conditional-logical-or-operator-"></a>Operador OR lógico condicional ||

O operador OR lógico condicional `||`, também conhecido como operador OR lógico de "curto-circuito", computa o OR lógico de seus operandos. O resultado de `x || y` será `true` se `x` ou `y` for avaliado como `true`. Caso contrário, o resultado será `false`. Se `x` for avaliado como `true`, `y` não será avaliado.

No exemplo a seguir, o operando à direita do operador `||` é uma chamada de método, que não é executada se o operando à esquerda for avaliado como `true`:

[!code-csharp-interactive[conditional logical OR](snippets/shared/BooleanLogicalOperators.cs#ConditionalOr)]

O [operador OR lógico](#logical-or-operator-) `|` também computa o OR lógico de seus operandos, mas sempre avalia os dois operandos.

## <a name="nullable-boolean-logical-operators"></a>Operadores lógicos booleanos anuláveis

Para `bool?` operandos, os operadores [ `&` (and lógico)](#logical-and-operator-) e [ `|` (lógico](#logical-or-operator-) ) oferecem suporte à lógica de três valores, da seguinte maneira:

- O `&` operador só produzirá `true` se ambos os operandos forem avaliados como `true` . Se `x` ou `y` for avaliada como `false` , `x & y` produz `false` (mesmo que outro operando seja avaliado como `null` ). Caso contrário, o resultado de `x & y` é `null` .

- O `|` operador só produzirá `false` se ambos os operandos forem avaliados como `false` . Se `x` ou `y` for avaliada como `true` , `x | y` produz `true` (mesmo que outro operando seja avaliado como `null` ). Caso contrário, o resultado de `x | y` é `null` .

A tabela a seguir apresenta essa semântica:

|x|a|x&y|x&#124;y|  
|----|----|----|----|  
|true|true|true|true|  
|true|false|false|true|  
|true|null|null|true|  
|false|true|false|true|  
|false|false|false|false|  
|false|null|false|null|  
|null|true|null|true|  
|null|false|false|null|  
|null|null|null|null|  

O comportamento desses operadores difere do comportamento típico do operador com tipos de valores anuláveis. Normalmente, um operador que é definido para operandos de um tipo de valor também pode ser usado com operandos do tipo de valor anulável correspondente. Tal operador produz `null` se qualquer um de seus operandos é avaliado como `null` . No entanto, os `&` `|` operadores e podem produzir não nulo mesmo se um dos operandos for avaliado como `null` . Para obter mais informações sobre o comportamento do operador com tipos de valores anuláveis, consulte a seção [operadores levantados](../builtin-types/nullable-value-types.md#lifted-operators) do artigo [tipos de valores anuláveis](../builtin-types/nullable-value-types.md) .

Você também pode usar os `!` `^` operadores e com `bool?` operandos, como mostra o exemplo a seguir:

[!code-csharp-interactive[lifted negation and xor](snippets/shared/BooleanLogicalOperators.cs#WithNullableBoolean)]

Os operadores lógicos condicionais `&&` e `||` não dão suporte a `bool?` operandos.

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

Os operadores `&`, `|` e `^` suportam a atribuição de compostos, conforme mostrado no exemplo a seguir:

[!code-csharp-interactive[compound assignment](snippets/shared/BooleanLogicalOperators.cs#CompoundAssignment)]

> [!NOTE]
> Os operadores lógicos condicionais `&&` e `||` não suportam a atribuição composta.

## <a name="operator-precedence"></a>Precedência do operador

A lista a seguir ordena os operadores lógicos, começando da mais alta precedência até a mais baixa:

- Operador de negação lógica `!`
- Operador AND lógico `&`
- Operador OR exclusivo lógico `^`
- Operador OR lógico `|`
- Operador AND lógico condicional `&&`
- Operador OR lógico condicional `||`

Use parênteses, `()`, para alterar a ordem de avaliação imposta pela precedência do operador:

[!code-csharp-interactive[operator precedence](snippets/shared/BooleanLogicalOperators.cs#Precedence)]

Para obter a lista completa de operadores C# ordenados por nível de precedência, consulte a seção [precedência de operador](index.md#operator-precedence) do artigo sobre [operadores do c#](index.md) .

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

Um tipo definido pelo usuário pode [sobrecarregar](operator-overloading.md) os `!` operadores,, `&` `|` e `^` . Quando um operador binário está sobrecarregado, o operador de atribuição composta correspondente também é implicitamente sobrecarregado. Um tipo definido pelo usuário não pode sobrecarregar explicitamente um operador de atribuição composta.

Um tipo definido pelo usuário não pode sobrecarregar os operadores lógicos condicionais `&&` e `||`. No entanto, se um tipo definido pelo usuário sobrecarregar os operadores [true e false](true-false-operators.md) e o operador `&` ou `|` de uma determinada maneira, a operação `&&` ou `||`, respectivamente, pode ser avaliada para os operandos desse tipo. Para obter mais informações, veja a seção [Operadores lógicos condicionais definidos pelo usuário](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators) na [especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, confira as seguintes seções da [especificação da linguagem C#](~/_csharplang/spec/introduction.md):

- [Operador de negação lógica](~/_csharplang/spec/expressions.md#logical-negation-operator)
- [Operadores lógicos](~/_csharplang/spec/expressions.md#logical-operators)
- [Operadores lógicos condicionais](~/_csharplang/spec/expressions.md#conditional-logical-operators)
- [Atribuição composta](~/_csharplang/spec/expressions.md#compound-assignment)

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- [Operadores shift e bit a bit](bitwise-and-shift-operators.md)
