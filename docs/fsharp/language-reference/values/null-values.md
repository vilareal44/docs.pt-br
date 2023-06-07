---
title: Valores nulos
description: 'Saiba como o valor nulo é usado na linguagem de programação F #.'
ms.date: 08/15/2020
ms.openlocfilehash: 3b2c7ebe7b8eff08f7c3e76b9715ccab1bbd5d36
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558342"
---
# <a name="null-values"></a>Valores nulos

Este tópico descreve como o valor nulo é usado em F #.

## <a name="null-value"></a>Valor nulo

O valor nulo normalmente não é usado em F # para valores ou variáveis. No entanto, NULL aparece como um valor anormal em determinadas situações. Se um tipo for definido em F #, NULL não será permitido como um valor regular, a menos que o atributo [AllowNullLiteral](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-allownullliteralattribute.html#Value) seja aplicado ao tipo. Se um tipo for definido em alguma outra linguagem .NET, NULL é um valor possível e, quando você estiver interoperante com esses tipos, o código F # poderá encontrar valores nulos.

Para um tipo definido em F # e usado estritamente de F #, a única maneira de criar um valor nulo usando a biblioteca F # diretamente é usar [Unchecked. defaultof](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators-unchecked.html#defaultof) ou [array. zeroCreate](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#zeroCreate). No entanto, para um tipo F # que é usado de outras linguagens .NET, ou se você estiver usando esse tipo com uma API que não é escrita em F #, como o .NET Framework, podem ocorrer valores nulos.

Você pode usar o `option` tipo em F # quando você pode usar uma variável de referência com um valor nulo possível em outra linguagem .net. Em vez de NULL, com um tipo F # `option` , você usará o valor da opção `None` se não houver nenhum objeto. Você usa o valor da opção `Some(obj)` com um objeto `obj` quando há um objeto. Para obter mais informações, veja [Opções](../options.md). Observe que você ainda pode empacotar um `null` valor em uma opção se, para `Some x` , for `x` `null` . Por isso, é importante que você use `None` quando um valor é `null` .

A `null` palavra-chave é uma palavra-chave válida na linguagem F # e você precisa usá-la quando estiver trabalhando com apis .NET Framework ou outras APIs escritas em outra linguagem .net. As duas situações nas quais você pode precisar de um valor nulo são quando você chama uma API .NET e passa um valor nulo como um argumento e quando você interpreta o valor de retorno ou um parâmetro de saída de uma chamada de método .NET.

Para passar um valor nulo para um método .NET, basta usar a `null` palavra-chave no código de chamada. O exemplo de código a seguir ilustra isso.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet701.fs)]

Para interpretar um valor nulo que é obtido de um método .NET, use a correspondência de padrões, se possível. O exemplo de código a seguir mostra como usar a correspondência de padrões para interpretar o valor nulo que é retornado `ReadLine` quando ele tenta ler após o final de um fluxo de entrada.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet702.fs)]

Valores nulos para tipos F # também podem ser gerados de outras maneiras, como quando você usa `Array.zeroCreate` , que chama `Unchecked.defaultof` . Você deve ter cuidado com esse código para manter os valores nulos encapsulados. Em uma biblioteca destinada apenas a F #, você não precisa verificar se há valores nulos em cada função. Se você estiver escrevendo uma biblioteca para interoperação com outras linguagens .NET, talvez seja necessário adicionar verificações para parâmetros de entrada nulos e lançar um `ArgumentNullException` , assim como faria no código C# ou Visual Basic.

Você pode usar o código a seguir para verificar se um valor arbitrário é nulo.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet703.fs)]

## <a name="see-also"></a>Confira também

- [Valores](index.md)
- [Expressões de correspondência](../match-expressions.md)
