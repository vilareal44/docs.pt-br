---
title: Operadores de atribuição-referência C#
ms.date: 09/10/2019
f1_keywords:
- =_CSharpKeyword
helpviewer_keywords:
- = operator [C#]
ms.assetid: d802a6d5-32f0-42b8-b180-12f5a081bfc1
ms.openlocfilehash: 7b4f3b3f4d6b697903461f08435552f2df36bfe4
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916937"
---
# <a name="assignment-operators-c-reference"></a>Operadores de atribuição (referência C#)

O operador de atribuição `=` atribui o valor do operando do lado direito a uma variável, uma [propriedade](../../programming-guide/classes-and-structs/properties.md) ou um elemento [indexador](../../programming-guide/indexers/index.md) fornecido pelo operando do lado esquerdo. O resultado de uma expressão de atribuição é o valor atribuído a um operando do lado esquerdo. O tipo do operandos do lado direito deve ser do mesmo tipo ou implicitamente conversível para o operando do lado esquerdo.

O operador de atribuição `=` é associativo à direita, ou seja, uma expressão do formulário

```csharp
a = b = c
```

é avaliada como

```csharp
a = (b = c)
```

O exemplo a seguir demonstra o uso do operador de atribuição com uma variável local, uma propriedade e um elemento do indexador como seu operando esquerdo:

[!code-csharp-interactive[simple assignment](snippets/shared/AssignmentOperator.cs#Simple)]

## <a name="ref-assignment-operator"></a>Operador de atribuição ref

Começando pelo C# 7.3, você pode usar o operador de atribuição ref `= ref` para reatribuir uma variável [ref local](../keywords/ref.md#ref-locals) ou [ref readonly local](../keywords/ref.md#ref-readonly-locals). O exemplo a seguir demonstra o uso do operador de atribuição ref:

[!code-csharp[ref assignment operator](snippets/shared/AssignmentOperator.cs#RefAssignment)]

No caso do operador de atribuição de referência, os dois operandos devem ser do mesmo tipo.

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

A atribuição composta é tem suporte dos operadores [aritmético](arithmetic-operators.md#compound-assignment), [lógico booliano](boolean-logical-operators.md#compound-assignment) e [bit a bit lógico e shift](bitwise-and-shift-operators.md#compound-assignment).

## <a name="null-coalescing-assignment"></a>Atribuição de União nula

A partir do C# 8,0, você pode usar o operador de atribuição de União nula `??=` para atribuir o valor de seu operando à direita para seu operando à esquerda somente se o operando à esquerda for avaliado como `null` . Para obter mais informações, consulte [?? e?? =](null-coalescing-operator.md) artigo de operadores.

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

Um tipo definido pelo usuário não pode [sobrecarregar](operator-overloading.md) o operador de atribuição. No entanto, um tipo definido pelo usuário pode definir uma conversão implícita em outro tipo. Dessa forma, o valor de um tipo definido pelo usuário pode ser atribuído a uma variável, uma propriedade ou um elemento do indexador de outro tipo. Para saber mais, confira [Operadores de conversão definidos pelo usuário](user-defined-conversion-operators.md).

Um tipo definido pelo usuário não pode sobrecarregar explicitamente um operador de atribuição composta. No entanto, se um tipo definido pelo usuário sobrecarregar um operador binário `op` , o `op=` operador, se existir, também será sobrecarregado implicitamente.

## <a name="c-language-specification"></a>especificação da linguagem C#

Saiba mais na seção [Operadores de atribuição](~/_csharplang/spec/expressions.md#assignment-operators) na [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

Para obter mais informações sobre o operador de atribuição de referência `= ref` , consulte a [Nota de proposta de recurso](~/_csharplang/proposals/csharp-7.3/ref-local-reassignment.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- [ref keyword](../keywords/ref.md)
