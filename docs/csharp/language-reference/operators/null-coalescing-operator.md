---
title: ?? e?? = Operators-referência C#
ms.date: 09/10/2019
f1_keywords:
- ??_CSharpKeyword
- ??=_CSharpKeyword
helpviewer_keywords:
- null-coalescing operator [C#]
- ?? operator [C#]
- null-coalescing assignment [C#]
- ??= operator [C#]
ms.assetid: 088b1f0d-c1af-4fe1-b4b8-196fd5ea9132
ms.openlocfilehash: 58c60dad3badc62f850f737a3d210ec486809272
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916741"
---
# <a name="-and--operators-c-reference"></a>?? e?? = Operators (referência C#)

O operador de coalescência nula `??` retornará o valor do operando esquerdo se não for `null`; caso contrário, ele avaliará o operando direito e retornará seu resultado. O operador `??` não avaliará o operando do lado direito se o operando esquerdo for avaliado como não nulo.

Disponível em C# 8,0 e posterior, o operador de atribuição de União nula `??=` atribui o valor de seu operando à direita para seu operando à esquerda somente se o operando esquerdo for avaliado como `null` . O operador `??=` não avaliará o operando do lado direito se o operando esquerdo for avaliado como não nulo.

[!code-csharp[null-coalescing assignment](snippets/shared/NullCoalescingOperator.cs#Assignment)]

O operando da esquerda do `??=` operador deve ser uma variável, uma [Propriedade](../../programming-guide/classes-and-structs/properties.md)ou um elemento do [indexador](../../programming-guide/indexers/index.md) .

No C# 7,3 e anterior, o tipo do operando à esquerda do `??` operador deve ser um [tipo de referência](../keywords/reference-types.md) ou um [tipo de valor anulável](../builtin-types/nullable-value-types.md). A partir do C# 8,0, esse requisito é substituído pelo seguinte: o tipo do operando esquerdo dos `??` `??=` operadores e não pode ser um tipo de valor não anulável. Em particular, a partir do C# 8,0, você pode usar os operadores de União nulo com parâmetros de tipo irrestrito:

[!code-csharp[unconstrained type parameter](snippets/shared/NullCoalescingOperator.cs#UnconstrainedType)]

Os operadores de União nulos são associativos à direita. Ou seja, expressões do formulário

```csharp
a ?? b ?? c
d ??= e ??= f
```

são avaliados como

```csharp
a ?? (b ?? c)
d ??= (e ??= f)
```

## <a name="examples"></a>Exemplos

Os `??` `??=` operadores e podem ser úteis nos seguintes cenários:

- Em expressões com [operadores condicionais nulos?. e? []](member-access-operators.md#null-conditional-operators--and-), você pode usar o `??` operador para fornecer uma expressão alternativa a ser avaliada, caso o resultado da expressão com operações condicionais nulas seja `null` :

  [!code-csharp-interactive[with null-conditional](snippets/shared/NullCoalescingOperator.cs#WithNullConditional)]

- Quando você trabalha com [tipos de valor anulável](../builtin-types/nullable-value-types.md) e precisa fornecer um valor de um tipo de valor subjacente, use o `??` operador para especificar o valor a ser fornecido, caso um valor de tipo anulável seja `null` :

  [!code-csharp-interactive[with nullable types](snippets/shared/NullCoalescingOperator.cs#WithNullableTypes)]

  Use o método <xref:System.Nullable%601.GetValueOrDefault?displayProperty=nameWithType> se o valor a ser usado quando um valor de tipo que permite valor nulo for `null` tiver que ser o valor padrão do tipo de valor subjacente.

- A partir do C# 7,0, você pode usar uma [ `throw` expressão](../keywords/throw.md#the-throw-expression) como o operando à direita do `??` operador para tornar o código de verificação de argumento mais conciso:

  [!code-csharp[with throw expression](snippets/shared/NullCoalescingOperator.cs#WithThrowExpression)]

  O exemplo anterior também demonstra como usar [membros aptos para expressão](../../programming-guide/statements-expressions-operators/expression-bodied-members.md) para definir uma propriedade.

- A partir do C# 8,0, você pode usar o `??=` operador para substituir o código do formulário

  ```csharp
  if (variable is null)
  {
      variable = expression;
  }
  ```

  pelo código a seguir:

  ```csharp
  variable ??= expression;
  ```

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

Os operadores `??` e `??=` não podem ser sobrecarregados.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações sobre o `??` operador, consulte [a seção operador de União nula](~/_csharplang/spec/expressions.md#the-null-coalescing-operator) da [especificação da linguagem C#](~/_csharplang/spec/introduction.md).

Para obter mais informações sobre o `??=` operador, consulte a [Nota de proposta de recurso](~/_csharplang/proposals/csharp-8.0/null-coalescing-assignment.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- [Operadores ?. e ?[]](member-access-operators.md#null-conditional-operators--and-)
- [operador?:](conditional-operator.md)
