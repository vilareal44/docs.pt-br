---
title: Operadores de igualdade - Referência de C#
description: Saiba mais sobre os operadores de comparação de igualdade C# e a igualdade do tipo C#.
ms.date: 06/26/2019
author: pkulikov
f1_keywords:
- ==_CSharpKeyword
- '!=_CSharpKeyword'
helpviewer_keywords:
- comparison operators [C#]
- relational operators [C#]
- equality operator [C#]
- equals operator [C#]
- == operator [C#]
- inequality operator [C#]
- not equals operator [C#]
- '!= operator [C#]'
ms.openlocfilehash: 33215e2440b14fb888a6f0df5c220c891ebed0e2
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063088"
---
# <a name="equality-operators-c-reference"></a>Operadores de igualdade (Referência de C#)

Os operadores [ `==` (igualdade)](#equality-operator-) e [ `!=` (desigualdade)](#inequality-operator-) verificam se os operandos são iguais ou não.

## <a name="equality-operator-"></a>Operador de igualdade ==

O operador de igualdade `==` retornará `true` se seus operandos forem iguais; caso contrário, `false`.

### <a name="value-types-equality"></a>Igualdade de tipos de valor

Os operandos dos [tipos de valor internos](../builtin-types/value-types.md#built-in-value-types) serão iguais se seus valores forem iguais:

[!code-csharp-interactive[value types equality](snippets/shared/EqualityOperators.cs#ValueTypesEquality)]

> [!NOTE]
> Para os `==` operadores,, [ `<` `>` , `<=` e `>=` ](comparison-operators.md) , se qualquer um dos operandos não for um número ( <xref:System.Double.NaN?displayProperty=nameWithType> ou <xref:System.Single.NaN?displayProperty=nameWithType> ), o resultado da operação será `false` . Isso significa que o valor `NaN` não é superior, inferior nem igual a nenhum outro valor `double` (ou `float`), incluindo `NaN`. Para obter mais informações e exemplos, consulte o artigo de referência <xref:System.Double.NaN?displayProperty=nameWithType> ou <xref:System.Single.NaN?displayProperty=nameWithType>.

Dois operandos do mesmo tipo de [enum](../builtin-types/enum.md) serão iguais se os valores correspondentes do tipo integral subjacente forem iguais.

Os tipos [struct](../builtin-types/struct.md) definidos pelo usuário não dão suporte ao operador `==`, por padrão. Para dar suporte ao operador `==`, um struct definido pelo usuário precisa [sobrecarregá-lo](operator-overloading.md).

Começando com C# 7.3, os operadores `==`, `!=`,  e  são compatíveis com as [tuplas](../builtin-types/value-tuples.md) de C#. Para obter mais informações, consulte a seção de [igualdade de tupla](../builtin-types/value-tuples.md#tuple-equality) do artigo [tipos de tupla](../builtin-types/value-tuples.md) .

### <a name="reference-types-equality"></a>Igualdade de tipos de referência

Por padrão, dois operandos do tipo de referência são iguais quando se referem ao mesmo objeto:

[!code-csharp[reference type equality](snippets/shared/EqualityOperators.cs#ReferenceTypesEquality)]

Como mostra o exemplo, os tipos de referência definidos pelo usuário dão suporte ao operador `==`, por padrão. No entanto, um tipo de referência pode sobrecarregar o operador `==`. Se um tipo de referência sobrecarregar o operador `==`, use o método <xref:System.Object.ReferenceEquals%2A?displayProperty=nameWithType> para verificar se as duas referências desse tipo dizem respeito ao mesmo objeto.

### <a name="string-equality"></a>Igualdade da cadeia de caracteres

Dois operandos da [cadeia de caracteres](../builtin-types/reference-types.md#the-string-type) serão iguais quando ambos forem `null` ou ambas as instâncias da cadeia de caracteres tiverem o mesmo comprimento e caracteres idênticos em cada posição de caractere:

[!code-csharp-interactive[string equality](snippets/shared/EqualityOperators.cs#StringEquality)]

Essa é uma comparação ordinal que diferencia maiúsculas de minúsculas. Para obter mais informações sobre a comparação de cadeias de caracteres, confira [Como comparar cadeias de caracteres no C#](../../how-to/compare-strings.md).

### <a name="delegate-equality"></a>Igualdade de delegado

Os dois operandos [delegar](../../programming-guide/delegates/index.md) do mesmo tipo de runtime são iguais quando ambos são `null` ou suas listas de invocação são do mesmo comprimento e tem entradas iguais em cada posição:

[!code-csharp-interactive[delegate equality](snippets/shared/EqualityOperators.cs#DelegateEquality)]

Saiba mais na seção [Operadores de igualdade de delegados](~/_csharplang/spec/expressions.md#delegate-equality-operators) na [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

Os delegados produzidos pela avaliação de [expressões lambda](lambda-expressions.md) semanticamente idênticas não são iguais, como mostra o exemplo a seguir:

[!code-csharp-interactive[from identical lambdas](snippets/shared/EqualityOperators.cs#IdenticalLambdas)]

## <a name="inequality-operator-"></a>Operador de desigualdade !=

O operador de desigualdade `!=` retornará `true` se seus operandos não forem iguais; caso contrário, `false`. No caso dos operandos de [tipos internos](../builtin-types/built-in-types.md), a expressão `x != y` gera o mesmo resultado que a expressão `!(x == y)`. Para obter mais informações sobre a igualdade de tipos, confira a seção [Operador de igualdade](#equality-operator-).

O exemplo a seguir demonstra o uso do operador `!=`:

[!code-csharp-interactive[non-equality examples](snippets/shared/EqualityOperators.cs#NonEquality)]

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

Os tipos definidos pelo usuário podem [sobrecarregar](operator-overloading.md) os operadores `==` e `!=`. Se um tipo sobrecarrega um dos dois operadores, ele também deve sobrecarregar o outro.

## <a name="c-language-specification"></a>Especificação da linguagem C#

Para obter mais informações, consulte a seção [Operadores de teste de tipo e relacional](~/_csharplang/spec/expressions.md#relational-and-type-testing-operators) na [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- <xref:System.IEquatable%601?displayProperty=nameWithType>
- <xref:System.Object.Equals%2A?displayProperty=nameWithType>
- <xref:System.Object.ReferenceEquals%2A?displayProperty=nameWithType>
- [Comparações de igualdade](../../programming-guide/statements-expressions-operators/equality-comparisons.md)
- [Operadores de comparação](comparison-operators.md)
