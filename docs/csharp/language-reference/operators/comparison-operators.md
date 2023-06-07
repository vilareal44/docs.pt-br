---
title: Operadores de comparação – referência do C#
description: Saiba mais sobre operadores de comparação em C# que você pode usar para verificar a ordem dos valores numéricos.
ms.date: 05/11/2020
author: pkulikov
f1_keywords:
- <_CSharpKeyword
- '>_CSharpKeyword'
- <=_CSharpKeyword
- '>=_CSharpKeyword'
helpviewer_keywords:
- comparison operators [C#]
- relational operators [C#]
- less than operator [C#]
- < operator [C#]
- greater than operator [C#]
- '> operator [C#]'
- less than or equal to operator [C#]
- <= operator [C#]
- greater than or equal to operator [C#]
- '>= operator [C#]'
ms.openlocfilehash: 9fa739d8b5461d4043f3ae51f5d14949a95c68e5
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916880"
---
# <a name="comparison-operators-c-reference"></a>Operadores de comparação (referência do C#)

A comparação [ `<` (menor que)](#less-than-operator-), [ `>` (maior que)](#greater-than-operator-), [ `<=` (menor que ou igual)](#less-than-or-equal-operator-)e [ `>=` (maior que ou igual)](#greater-than-or-equal-operator-) , também conhecida como operadores relacionais, comparam seus operandos. Esses operadores têm suporte de todos os tipos numéricos [integral](../builtin-types/integral-numeric-types.md) e [de ponto flutuante](../builtin-types/floating-point-numeric-types.md) .

> [!NOTE]
> Para os operadores `==`, `<`, `>`, `<=` e `>=`, se nenhum dos operandos for um número (<xref:System.Double.NaN?displayProperty=nameWithType> ou <xref:System.Single.NaN?displayProperty=nameWithType>), o resultado da operação será `false`. Isso significa que o valor `NaN` não é superior, inferior nem igual a nenhum outro valor `double` (ou `float`), incluindo `NaN`. Para obter mais informações e exemplos, consulte o artigo de referência <xref:System.Double.NaN?displayProperty=nameWithType> ou <xref:System.Single.NaN?displayProperty=nameWithType>.

O tipo [Char](../builtin-types/char.md) também dá suporte a operadores de comparação. No caso dos `char` operandos, os códigos de caractere correspondentes são comparados.

Tipos de enumeração também dão suporte a operadores de comparação. No caso dos operandos do mesmo tipo de [enum](../builtin-types/enum.md), os valores correspondentes do tipo integral subjacente são comparados.

Os [ `==` `!=` operadores e](equality-operators.md) verificam se seus operandos são iguais ou não.

## <a name="less-than-operator-"></a>Operador menor que \<

O operador `<` retornará `true` se o operando à esquerda for menor do que o operando à direita, caso contrário, `false`:

[!code-csharp-interactive[less than example](snippets/shared/ComparisonOperators.cs#Less)]

## <a name="greater-than-operator-"></a>Operador maior que >

O operador `>` retornará `true` se o operando à esquerda for maior do que o operando à direita, caso contrário, `false`:

[!code-csharp-interactive[greater than example](snippets/shared/ComparisonOperators.cs#Greater)]

## <a name="less-than-or-equal-operator-"></a>Operador menor ou igual \<=

O operador `<=` retornará `true` se o operando à esquerda for menor ou igual ao operando à direita, caso contrário, `false`:

[!code-csharp-interactive[less than or equal example](snippets/shared/ComparisonOperators.cs#LessOrEqual)]

## <a name="greater-than-or-equal-operator-"></a>Operador maior ou igual >=

O operador `>=` retornará `true` se o operando à esquerda for maior ou igual ao operando à direita, caso contrário, `false`:

[!code-csharp-interactive[greater than or equal example](snippets/shared/ComparisonOperators.cs#GreaterOrEqual)]

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

Um tipo definido pelo usuário pode [sobrecarregar](operator-overloading.md) os `<` operadores,, `>` `<=` e `>=` .

Se um tipo sobrecarregar um dos operadores `<` ou `>`, ele deverá sobrecarregar tanto `<` quanto `>`. Se um tipo sobrecarregar um dos operadores `<=` ou `>=`, ele deverá sobrecarregar tanto `<=` quanto `>=`.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, consulte a seção [Operadores de teste de tipo e relacional](~/_csharplang/spec/expressions.md#relational-and-type-testing-operators) na [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- <xref:System.IComparable%601?displayProperty=nameWithType>
- [Operadores de igualdade](equality-operators.md)
