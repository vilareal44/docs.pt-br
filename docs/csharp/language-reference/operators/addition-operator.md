---
title: + Operadores e += – referência de C#
ms.date: 04/23/2020
f1_keywords:
- +_CSharpKeyword
- +=_CSharpKeyword
helpviewer_keywords:
- addition operator [C#]
- concatenation operator [C#]
- delegate combination [C#]
- + operator [C#]
- addition assignment operator [C#]
- event subscription [C#]
- += operator [C#]
ms.assetid: 93e56486-bb42-43c1-bd43-60af11e64e67
ms.openlocfilehash: f1db0054ad2411bfe23f10b64bc2727a71ad7463
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916953"
---
# <a name="-and--operators-c-reference"></a>Operadores + e += (referência de C#)

Os `+` `+=` operadores and são suportados pelos tipos numéricos de [ponto flutuante](../builtin-types/floating-point-numeric-types.md) e [integral](../builtin-types/integral-numeric-types.md) internos, o tipo de [cadeia de caracteres](../builtin-types/reference-types.md#the-string-type) e os tipos [delegados](../builtin-types/reference-types.md#the-delegate-type) .

Para obter informações sobre o operador `+` aritmético, consulte as seções [Operadores de adição e subtração unários](arithmetic-operators.md#unary-plus-and-minus-operators) e [Operador de adição +](arithmetic-operators.md#addition-operator-) do artigo [Operadores aritméticos](arithmetic-operators.md).

## <a name="string-concatenation"></a>Concatenação de cadeia de caracteres

Quando um ou ambos os operandos são do tipo [cadeia de caracteres](../builtin-types/reference-types.md#the-string-type), o `+` operador concatena as representações de cadeia de caracteres de seus operandos (a representação de cadeia de caracteres `null` é uma cadeia de caracteres vazia):

[!code-csharp-interactive[string concatenation](snippets/shared/AdditionOperator.cs#AddStrings)]

A partir do C# 6, a [interpolação de cadeia de caracteres](../tokens/interpolated.md) fornece uma maneira mais conveniente de Formatar cadeias de texto:

[!code-csharp-interactive[string interpolation](snippets/shared/AdditionOperator.cs#UseStringInterpolation)]

## <a name="delegate-combination"></a>Combinação de delegados

Para operandos do mesmo tipo [delegado](../builtin-types/reference-types.md#the-delegate-type), o operador `+` retorna uma nova instância de delegado que, quando invocada, chama o operando esquerdo e, em seguida, chama o operando direito. Se qualquer um dos operandos for `null`, o operador `+` retornará o valor de outro operando (que também pode ser `null`). O exemplo a seguir mostra como os delegados podem ser combinados com o operador `+`:

[!code-csharp-interactive[delegate combination](snippets/shared/AdditionOperator.cs#AddDelegates)]

Para executar a remoção de delegado, use o [ `-` operador](subtraction-operator.md#delegate-removal).

Para obter mais informações sobre tipos de delegado, veja [Delegados](../../programming-guide/delegates/index.md).

## <a name="addition-assignment-operator-"></a>Operador de atribuição de adição +=

Uma expressão que usa o operador `+=`, como

```csharp
x += y
```

é equivalente a

```csharp
x = x + y
```

exceto que `x` é avaliado apenas uma vez.

O exemplo a seguir demonstra o uso do operador `+=`:

[!code-csharp-interactive[+= examples](snippets/shared/AdditionOperator.cs#AddAndAssign)]

Você também usará o operador `+=` para especificar um método de manipulador de eventos ao assinar um [evento](../keywords/event.md). Para obter mais informações, confira [Como assinar e cancelar a assinatura de eventos](../../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md).

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

Um tipo definido pelo usuário pode [sobrecarregar](operator-overloading.md) o operador `+`. Quando um operador `+` binário é sobrecarregado, o operador `+=` também é implicitamente sobrecarregado. Um tipo definido pelo usuário não pode sobrecarregar explicitamente o operador `+=`.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, veja as seções [Operador de adição unário](~/_csharplang/spec/expressions.md#unary-plus-operator) e [Operador de adição](~/_csharplang/spec/expressions.md#addition-operator) da [Especificação de linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- [Como concatenar várias cadeias de caracteres](../../how-to/concatenate-multiple-strings.md)
- [Eventos](../../programming-guide/events/index.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [-e-= operadores](subtraction-operator.md)
