---
title: tipo bool-referência C#
ms.date: 11/26/2019
f1_keywords:
- bool
- bool_CSharpKeyword
- "true"
- "false"
- true_CSharpKeyword
- false_CSharpKeyword
helpviewer_keywords:
- bool data type [C#]
- Boolean [C#]
ms.assetid: 551cfe35-2632-4343-af49-33ad12da08e2
ms.openlocfilehash: 4623dc7d6c8c6c437c78aee45f0eeee8a92e3200
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87854874"
---
# <a name="bool-c-reference"></a>bool (referência C#)

A `bool` palavra-chave Type é um alias para o <xref:System.Boolean?displayProperty=nameWithType> tipo de estrutura .NET que representa um valor booliano, que pode ser `true` ou `false` .

Para executar operações lógicas com valores do `bool` tipo, use operadores [lógicos boolianos](../operators/boolean-logical-operators.md) . O `bool` tipo é o tipo de resultado dos operadores de [comparação](../operators/comparison-operators.md) e de [igualdade](../operators/equality-operators.md) . Uma `bool` expressão pode ser uma expressão condicional de controle nas [if](../keywords/if-else.md)instruções if [do](../keywords/do.md), do, [while](../keywords/while.md)e [for](../keywords/for.md) e no [operador `?:` condicional ](../operators/conditional-operator.md).

O valor padrão do `bool` tipo é `false` .

## <a name="literals"></a>Literais

Você pode usar os `true` `false` literais e para inicializar uma `bool` variável ou para passar um `bool` valor:

[!code-csharp-interactive[bool literals](snippets/BoolType.cs#Literals)]

## <a name="three-valued-boolean-logic"></a>Lógica booleana de três valores

Use o `bool?` tipo anulável, se você precisar dar suporte à lógica de três valores, por exemplo, quando trabalhar com bancos de dados que dão suporte a um tipo booliano de três valores. Para os operandos `bool?`, os operadores `&` e `|` predefinidos oferecem suporte à lógica de três valores. Para obter mais informações, confira a seção [Operadores lógicos booleanos anuláveis](../operators/boolean-logical-operators.md#nullable-boolean-logical-operators) do artigo [Operadores lógicos boolianos](../operators/boolean-logical-operators.md).

Para obter mais informações sobre tipos de valor anulável, consulte [tipos de valor anulável](nullable-value-types.md).

## <a name="conversions"></a>Conversões

O C# fornece apenas duas conversões que envolvem o `bool` tipo. Essas são uma conversão implícita para o tipo anulável correspondente `bool?` e uma conversão explícita do `bool?` tipo. No entanto, o .NET fornece métodos adicionais que você pode usar para converter de ou para o `bool` tipo. Para obter mais informações, consulte a seção [convertendo de valores booleanos](/dotnet/api/system.boolean#converting-to-and-from-boolean-values) da <xref:System.Boolean?displayProperty=nameWithType> página de referência da API.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, consulte [a seção tipo bool](~/_csharplang/spec/types.md#the-bool-type) da [especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Confira também

- [Referência de C#](../index.md)
- [Tipos de valor](value-types.md)
- [operadores true e false](../operators/true-false-operators.md)
