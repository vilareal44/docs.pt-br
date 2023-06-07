---
title: Como converter com segurança usando correspondência de padrões e os operadores is e as
description: Aprenda a usar técnicas de correspondência de padrões para converter variáveis com segurança em um tipo diferente. É possível usar a correspondência de padrões, assim como os operadores is e as para converter tipos com segurança.
ms.date: 09/05/2018
helpviewer_keywords:
- cast operators [C#], as and is operators
- as operator [C#]
- is operator [C#]
ms.openlocfilehash: f10ce837057cc61b84130f237a13af708849dfc5
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662960"
---
# <a name="how-to-safely-cast-by-using-pattern-matching-and-the-is-and-as-operators"></a>Como converter com segurança usando correspondência de padrões e os operadores is e as

Como os objetos são polimórficos, é possível que uma variável de tipo de classe base tenha um [tipo](../programming-guide/types/index.md) derivado. Para acessar os métodos de instância do tipo derivado, é necessário [converter](../programming-guide/types/casting-and-type-conversions.md) o valor de volta no tipo derivado. No entanto, uma conversão cria o risco de lançar um <xref:System.InvalidCastException>. O C# fornece instruções de [correspondência de padrões](../pattern-matching.md) que executarão uma conversão condicionalmente somente quando ela tiver êxito. O C# também oferece os operadores [is](../language-reference/operators/type-testing-and-cast.md#is-operator) e [as](../language-reference/operators/type-testing-and-cast.md#as-operator) para testar se um valor é de um determinado tipo.

O exemplo a seguir mostra como usar a instrução de correspondência de padrões `is` :

:::code language="csharp" source="../../../samples/snippets/csharp/how-to/safelycast/patternmatching/Program.cs" id="PatternMatchingIs":::

O exemplo anterior demonstra alguns recursos da sintaxe de correspondência de padrões. A `if (a is Mammal m)` instrução combina o teste com uma atribuição de inicialização. A atribuição ocorre apenas quando o teste é bem-sucedido. A variável `m` está somente no escopo na instrução `if` inserida em ela foi atribuída. Não é possível acessar `m` posteriormente no mesmo método. O exemplo anterior também mostra como usar o [ `as` operador](../language-reference/operators/type-testing-and-cast.md#as-operator) para converter um objeto em um tipo especificado.

Você também pode usar a mesma sintaxe para testar se um [tipo de valor anulável](../language-reference/builtin-types/nullable-value-types.md) tem um valor, conforme mostrado no exemplo a seguir:

:::code language="csharp" source="../../../samples/snippets/csharp/how-to/safelycast/nullablepatternmatching/Program.cs" id="PatternMatchingNullable":::

O exemplo anterior demonstra outros recursos da correspondência de padrões a serem usados com conversões. É possível testar se uma variável tem padrão nulo verificando especificamente o valor `null`. Quando o valor de runtime da variável é `null`, uma instrução `is` que verifica um tipo retorna sempre `false`. A instrução `is` da correspondência de padrões não permite um tipo de valor anulável, como `int?` ou `Nullable<int>`, mas é possível testar qualquer outro tipo de valor. Os `is` padrões do exemplo anterior não estão limitados aos tipos de valor anulável. Você também pode usar esses padrões para testar se uma variável de um tipo de referência tem um valor ou é `null` .

O exemplo anterior também mostra como você usa o padrão de tipo em uma `switch` instrução em que a variável pode ser um dos muitos tipos diferentes.

Se você quiser testar se uma variável é um tipo específico, mas não atribuí-la a uma nova variável, você pode usar os `is` operadores e para tipos de `as` referência e tipos de valor anulável. O seguinte código mostra como usar as instruções `is` e `as` que faziam parte da linguagem C# antes que a correspondência de padrões fosse introduzida para testar se uma variável é de um determinado tipo:

:::code language="csharp" source="../../../samples/snippets/csharp/how-to/safelycast/asandis/Program.cs" id="IsAndAs":::

Como você pode ver na comparação desse código com o de correspondência de padrões, a sintaxe de correspondência de padrões oferece recursos mais robustos combinando o teste e a atribuição em uma única instrução. Use a sintaxe de correspondência de padrões sempre que possível.
