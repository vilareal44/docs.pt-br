---
title: Expressões lentas
description: 'Saiba como as expressões lentas em F # podem melhorar o desempenho de seus aplicativos e bibliotecas.'
ms.date: 08/15/2020
ms.openlocfilehash: 71c466ca3b74c9e92b81a3c268e07438ec944905
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558082"
---
# <a name="lazy-expressions"></a>Expressões lentas

As *expressões lentas* são expressões que não são avaliadas imediatamente, mas, em vez disso, são avaliadas quando o resultado é necessário. Isso pode ajudar a melhorar o desempenho do seu código.

## <a name="syntax"></a>Sintaxe

```fsharp
let identifier = lazy ( expression )
```

## <a name="remarks"></a>Comentários

Na sintaxe anterior, *expression* é o código que é avaliado somente quando um resultado é necessário e o *identificador* é um valor que armazena o resultado. O valor é do tipo [`Lazy<'T>`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-lazy-1-0.html) , onde o tipo real usado para `'T` é determinado a partir do resultado da expressão.

As expressões lentas permitem melhorar o desempenho restringindo a execução de expressões para apenas as situações em que um resultado é necessário.

Para forçar a execução das expressões, você chama o método `Force` . `Force` faz com que a execução seja executada apenas uma vez. As chamadas subsequentes para `Force` retornar o mesmo resultado, mas não executam nenhum código.

O código a seguir ilustra o uso de expressões lentas e o uso de `Force` . Nesse código, o tipo de `result` é `Lazy<int>` e o `Force` método retorna um `int` .

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet73011.fs)]

A avaliação lenta, mas não o `Lazy` tipo, também é usada para sequências. Para obter mais informações, consulte [sequences](sequences.md).

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
- [Módulo LazyExtensions](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-lazyextensions.html)
