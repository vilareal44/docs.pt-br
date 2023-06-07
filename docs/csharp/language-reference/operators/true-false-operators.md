---
title: Operadores true e false – referência do C#
ms.date: 12/10/2018
helpviewer_keywords:
- false operator [C#]
- true operator [C#]
ms.assetid: 81a888fd-011e-4589-b242-6c261fea505e
ms.openlocfilehash: 15342c3d9cd66195639e38265875a7ed4008dd51
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916619"
---
# <a name="true-and-false-operators-c-reference"></a>Operadores true e false (referência do C#)

O `true` operador retorna o [bool](../builtin-types/bool.md) valor bool `true` para indicar que seu operando é definitivamente verdadeiro. O `false` operador retorna o `bool` valor `true` para indicar que seu operando é definitivamente falso. Não há garantia de que os operadores `true` e `false` se complementarão. Ou seja, ambos os operadores `true` e `false` podem retornar o valor `bool``false` para o mesmo operando. Se um tipo define um dos dois operadores, ele também deve definir outro operador.

> [!TIP]
> Use o `bool?` tipo, se você precisar dar suporte à lógica de três valores (por exemplo, ao trabalhar com bancos de dados que dão suporte a um tipo booliano de três valores). C# fornece os operadores `&` e `|` que suportam a lógica de três valores com os operandos `bool?`. Para obter mais informações, confira a seção [Operadores lógicos booleanos anuláveis](boolean-logical-operators.md#nullable-boolean-logical-operators) do artigo [Operadores lógicos boolianos](boolean-logical-operators.md).

## <a name="boolean-expressions"></a>Expressões boolianas

Um tipo com o operador `true` definido pode ser o tipo de resultado de uma expressão condicional de controle nas instruções [if](../keywords/if-else.md), [do](../keywords/do.md), [while](../keywords/while.md) e [for](../keywords/for.md) e no [operador condicional `?:`](conditional-operator.md). Para saber mais, confira a seção [Expressões boolianas](~/_csharplang/spec/expressions.md#boolean-expressions) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="user-defined-conditional-logical-operators"></a>Operadores lógicos condicionais definidos pelo usuário

Se um tipo com os operadores `true` e `false` definidos [sobrecarregar](operator-overloading.md) o [operador OR lógico](boolean-logical-operators.md#logical-or-operator-) `|` ou o [operador AND lógico](boolean-logical-operators.md#logical-and-operator-) `&` de uma determinada maneira, o [operador OR lógico condicional](boolean-logical-operators.md#conditional-logical-or-operator-) `||` ou [operador AND lógico condicional](boolean-logical-operators.md#conditional-logical-and-operator-) `&&`, respectivamente, poderá ser avaliado para os operandos desse tipo. Para obter mais informações, veja a seção [Operadores lógicos condicionais definidos pelo usuário](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators) na [especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="example"></a>Exemplo

O exemplo a seguir apresenta o tipo que define os dois operadores, `true` e `false`. O tipo também sobrecarrega o operador AND lógico de `&` forma que o `&&` operador também possa ser avaliado para os operandos desse tipo.

[!code-csharp[true and false operators example](snippets/shared/TrueFalseOperators.cs)]

Observe o comportamento de curto-circuito do operador `&&`. Quando o método `GetFuelLaunchStatus` retorna `LaunchStatus.Red`, o operando à direita do operador `&&` não é avaliado. Isso ocorre porque `LaunchStatus.Red` é, definitivamente, false. Depois, o resultado do AND lógico não depende do valor do operando à direita. A saída do exemplo é a seguinte:

```console
Getting fuel launch status...
Wait!
```

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
