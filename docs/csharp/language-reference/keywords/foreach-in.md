---
title: Instrução foreach do C#
ms.date: 07/22/2020
f1_keywords:
- foreach
- foreach_CSharpKeyword
helpviewer_keywords:
- foreach keyword [C#]
- foreach statement [C#]
- in keyword [C#]
ms.assetid: 5a9c5ddc-5fd3-457a-9bb6-9abffcd874ec
ms.openlocfilehash: 4af431d29e538c1516efeaad3008eaa3b2229ece
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104245"
---
# <a name="foreach-in-c-reference"></a>foreach, in (Referência em C#)

A `foreach` instrução executa uma instrução ou um bloco de instruções para cada elemento em uma instância do tipo que implementa a <xref:System.Collections.IEnumerable?displayProperty=nameWithType> <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> interface ou, como mostra o exemplo a seguir:

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="1" interactive="try-dotnet-method" :::

A `foreach` instrução não está limitada a esses tipos. Você pode usá-lo com uma instância de qualquer tipo que atenda às seguintes condições:

- um tipo tem o método público sem parâmetros `GetEnumerator` cujo tipo de retorno é Class, struct ou interface Type,
- o tipo de retorno do método `GetEnumerator` tem a propriedade `Current` pública e o método `MoveNext` público sem parâmetros, cujo tipo de retorno é <xref:System.Boolean>.

O exemplo a seguir usa a `foreach` instrução com uma instância do <xref:System.Span%601?displayProperty=nameWithType> tipo, que não implementa nenhuma interface:

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="2" :::

A partir do C# 7,3, se a propriedade do enumerador `Current` retornar um [valor de retorno de referência](ref.md#reference-return-values) ( `ref T` em que `T` é o tipo de um elemento de coleção), você poderá declarar uma variável de iteração com o `ref` `ref readonly` modificador ou, como mostra o exemplo a seguir:

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="RefSpan" :::

A partir do C# 8,0, você pode usar a `await foreach` instrução para consumir um fluxo de dados assíncrono, ou seja, o tipo de coleção que implementa a <xref:System.Collections.Generic.IAsyncEnumerable%601> interface. Cada iteração do loop pode ser suspensa enquanto o próximo elemento é recuperado de forma assíncrona. O exemplo a seguir mostra como usar a `await foreach` instrução:

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="AwaitForeach" :::

Por padrão, os elementos de fluxo são processados no contexto capturado. Se você quiser desabilitar a captura do contexto, use o <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait%2A?displayProperty=nameWithType> método de extensão. Para obter mais informações sobre contextos de sincronização e como capturar o contexto atual, consulte [consumindo o padrão assíncrono baseado em tarefa](../../../standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md). Para obter mais informações sobre fluxos assíncronos, consulte a seção [fluxos assíncronos](../../whats-new/csharp-8.md#asynchronous-streams) do artigo [novidades no C# 8,0](../../whats-new/csharp-8.md) .

Em qualquer ponto dentro do bloco de instrução `foreach`, você pode sair do loop usando a instrução [break](break.md) ou seguir para a próxima iteração no loop usando a instrução [continue](continue.md). Você também pode sair de um `foreach` loop pelas instruções [goto](goto.md), [Return](return.md)ou [throw](throw.md) .

Se a instrução `foreach` for aplicada a `null`, uma <xref:System.NullReferenceException> será gerada. Se a coleção de origem da `foreach` instrução estiver vazia, o corpo do `foreach` loop não será executado e ignorado.

## <a name="type-of-an-iteration-variable"></a>Tipo de uma variável de iteração

Você pode usar a `var` palavra-chave para permitir que o compilador inferir o tipo de uma variável de iteração na `foreach` instrução, como mostra o código a seguir:

```csharp
foreach (var item in collection) { }
```

Você também pode especificar explicitamente o tipo de uma variável de iteração, como mostra o código a seguir:

```csharp
IEnumerable<T> collection = new T[5];
foreach (V item in collection) { }
```

No formulário anterior, o tipo `T` de um elemento de coleção deve ser implicitamente ou explicitamente conversível para `V` o tipo de uma variável de iteração. Se uma conversão explícita de `T` para `V` falhar em tempo de execução, a `foreach` instrução lançará um <xref:System.InvalidCastException> . Por exemplo, se `T` for um tipo de classe não lacrado, `V` pode ser qualquer tipo de interface, até mesmo aquele que `T` não implementa. No tempo de execução, o tipo de um elemento de coleção pode ser o que deriva de `T` e realmente implementa `V` . Se esse não for o caso, um <xref:System.InvalidCastException> será lançado.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, confira a seção [A instrução foreach](~/_csharplang/spec/statements.md#the-foreach-statement) na [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Confira também

- [Referência de C#](../index.md)
- [Palavras-chave do C#](index.md)
- [Usando foreach com matrizes](../../programming-guide/arrays/using-foreach-with-arrays.md)
- [Instrução for](for.md)
