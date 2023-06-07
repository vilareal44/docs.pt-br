---
title: while – Referência de C#
ms.date: 05/28/2018
f1_keywords:
- while_CSharpKeyword
- while
helpviewer_keywords:
- while keyword [C#]
ms.assetid: 72a0765c-6852-4aca-b327-4a11cb7f5c59
ms.openlocfilehash: af616c2381b993f86296cbfa43a01ba2f9e000c2
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401856"
---
# <a name="while-c-reference"></a>while (Referência de C#)

A instrução `while` executa uma instrução ou um bloco de instruções enquanto uma expressão booliana especificada é avaliada como `true`. Como essa expressão é avaliada antes de cada execução do loop, um loop `while` é executado zero ou mais vezes. Isso difere do loop [do](do.md), que é executado uma ou mais vezes.

A qualquer momento dentro do bloco de instruções `while`, interrompa o loop usando a instrução [break](break.md).

Você pode seguir diretamente para a avaliação da expressão `while` usando a instrução [continue](continue.md). Se a expressão for avaliada como `true`, a execução continuará na primeira instrução do loop. Caso contrário, a execução continuará na primeira instrução após o loop.

Você também pode sair de um `while` loop pelas instruções [goto](goto.md), [Return](return.md)ou [throw](throw.md) .

## <a name="example"></a>Exemplo

O exemplo a seguir mostra o uso da instrução `while`. Selecione **Executar** para executar o código de exemplo. Depois disso, você pode modificar o código e executá-lo novamente.

[!code-csharp-interactive[while loop example](snippets/IterationKeywordsExamples.cs#3)]

## <a name="c-language-specification"></a>especificação da linguagem C#

Para saber mais, confira a seção [A instrução while](~/_csharplang/spec/statements.md#the-while-statement) na [Especificação da linguagem C#](/dotnet/csharp/language-reference/language-specification/introduction).

## <a name="see-also"></a>Confira também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [instrução do](do.md)
