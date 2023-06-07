---
title: Operador => – referência do C#
ms.date: 01/22/2019
f1_keywords:
- =>_CSharpKeyword
helpviewer_keywords:
- lambda operator [C#]
- => operator [C#]
- lambda expressions [C#], => operator
ms.openlocfilehash: 30e1a3546f83a0a1ba5b1363238878868e94ab93
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063127"
---
# <a name="-operator-c-reference"></a>Operador => (referência do C#)

O `=>` token tem suporte em duas formas: como o [operador lambda](#lambda-operator) e como um separador de um nome de membro e a implementação de membro em uma [definição de corpo de expressão](#expression-body-definition).

## <a name="lambda-operator"></a>Operador lambda

Em [expressões lambda](lambda-expressions.md), o operador lambda `=>` separa os parâmetros de entrada no lado esquerdo do corpo lambda no lado direito.

O seguinte exemplo usa o recurso [LINQ](../../programming-guide/concepts/linq/index.md) com a sintaxe de método para demonstrar o uso de expressões lambda:

[!code-csharp-interactive[infer types of input variables](snippets/shared/LambdaOperator.cs#InferredTypes)]

Os parâmetros de entrada de uma expressão lambda são fortemente tipados no momento da compilação. Quando o compilador pode inferir os tipos de parâmetros de entrada, como no exemplo anterior, você pode omitir declarações de tipo. Se você precisar especificar o tipo de parâmetros de entrada, deverá fazer isso para cada parâmetro, como mostra o exemplo a seguir:

[!code-csharp-interactive[specify types of input variables](snippets/shared/LambdaOperator.cs#ExplicitTypes)]

O exemplo a seguir mostra como definir uma expressão lambda sem parâmetros de entrada:

[!code-csharp-interactive[without input variables](snippets/shared/LambdaOperator.cs#WithoutInput)]

Para obter mais informações, consulte [expressões lambda](lambda-expressions.md).

## <a name="expression-body-definition"></a>Definição de corpo da expressão

Uma definição de corpo da expressão tem a seguinte sintaxe geral:

```csharp
member => expression;
```

em que `expression` é uma expressão válida. O tipo de retorno de `expression` deve ser implicitamente conversível para o tipo de retorno do membro. Se o tipo de retorno do membro for `void` ou se o membro for um construtor, um finalizador ou um acessador de propriedade ou indexador `set` , `expression` deverá ser uma [*expressão de instrução*](~/_csharplang/spec/statements.md#expression-statements). Como o resultado da expressão é Descartado, o tipo de retorno dessa expressão pode ser qualquer tipo.

O seguinte exemplo mostra uma definição de corpo da expressão para um método `Person.ToString`:

```csharp
public override string ToString() => $"{fname} {lname}".Trim();
```

É uma versão abreviada da seguinte definição de método:

```csharp
public override string ToString()
{
   return $"{fname} {lname}".Trim();
}
```

As definições de corpo de expressão para métodos, operadores e propriedades somente leitura têm suporte a partir do C# 6. As definições de corpo de expressão para construtores, finalizadores e acessadores de propriedade e indexador têm suporte a partir do C# 7,0.

Para obter mais informações, consulte [Membros aptos para expressão](../../programming-guide/statements-expressions-operators/expression-bodied-members.md).

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

O operador `=>` não pode ser sobrecarregado.

## <a name="c-language-specification"></a>Especificação da linguagem C#

Para obter mais informações sobre o operador lambda, consulte a seção [expressões de função anônimas](~/_csharplang/spec/expressions.md#anonymous-function-expressions) da [especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
