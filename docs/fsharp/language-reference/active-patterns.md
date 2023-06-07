---
title: Padrões ativos
description: 'Saiba como usar padrões ativos para definir partições nomeadas que subdividem dados de entrada na linguagem de programação F #.'
ms.date: 05/16/2016
ms.openlocfilehash: 7b6da38baa35f30ae6de8a930be60a4e4fc0fc0d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493884"
---
# <a name="active-patterns"></a>Padrões ativos

Os *padrões ativos* permitem que você defina partições nomeadas que subdividem dados de entrada, para que você possa usar esses nomes em uma expressão de correspondência de padrões, exatamente como faria para uma união discriminada. Você pode usar padrões ativos para decompor os dados de uma maneira personalizada para cada partição.

## <a name="syntax"></a>Sintaxe

```fsharp
// Active pattern of one choice.
let (|identifier|) [arguments] valueToMatch = expression

// Active Pattern with multiple choices.
// Uses a FSharp.Core.Choice<_,...,_> based on the number of case names. In F#, the limitation n <= 7 applies.
let (|identifer1|identifier2|...|) valueToMatch = expression

// Partial active pattern definition.
// Uses a FSharp.Core.option<_> to represent if the type is satisfied at the call site.
let (|identifier|_|) [arguments ] valueToMatch = expression
```

## <a name="remarks"></a>Comentários

Na sintaxe anterior, os identificadores são nomes de partições dos dados de entrada que são representados por *argumentos*ou, em outras palavras, nomes para subconjuntos do conjunto de todos os valores dos argumentos. Pode haver até sete partições em uma definição de padrão ativo. A *expressão* descreve o formulário no qual os dados são decompostos. Você pode usar uma definição de padrão ativo para definir as regras para determinar a quais das partições nomeadas os valores fornecidos como argumentos pertencem. Os símbolos (| e |) são chamados de *clipes banana* e a função criada por esse tipo de associação de Let é chamada de *reconhecedor ativo*.

Como exemplo, considere o seguinte padrão ativo com um argumento.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5001.fs)]

Você pode usar o padrão ativo em uma expressão de correspondência de padrões, como no exemplo a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5002.fs)]

A saída desse programa é a seguinte:

```console
7 is odd
11 is odd
32 is even
```

Outro uso de padrões ativos é decompor tipos de dados de várias maneiras, como quando os mesmos dados subjacentes têm várias representações possíveis. Por exemplo, um `Color` objeto pode ser decomposto em uma representação RGB ou em uma representação HSB.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5003.fs)]

A saída do programa acima é a seguinte:

```console
Red
 Red: 255 Green: 0 Blue: 0
 Hue: 360.000000 Saturation: 1.000000 Brightness: 0.500000
Black
 Red: 0 Green: 0 Blue: 0
 Hue: 0.000000 Saturation: 0.000000 Brightness: 0.000000
White
 Red: 255 Green: 255 Blue: 255
 Hue: 0.000000 Saturation: 0.000000 Brightness: 1.000000
Gray
 Red: 128 Green: 128 Blue: 128
 Hue: 0.000000 Saturation: 0.000000 Brightness: 0.501961
BlanchedAlmond
 Red: 255 Green: 235 Blue: 205
 Hue: 36.000000 Saturation: 1.000000 Brightness: 0.901961
```

Em combinação, essas duas maneiras de usar padrões ativos permitem que você particione e decompoa dados apenas no formulário apropriado e execute os cálculos apropriados nos dados apropriados na forma mais conveniente para a computação.

As expressões de correspondência de padrões resultantes permitem que os dados sejam gravados de uma maneira conveniente que seja muito legível, simplificando bastante a ramificação potencialmente complexa e o código de análise de dados.

## <a name="partial-active-patterns"></a>Padrões ativos parciais

Às vezes, você precisa particionar apenas parte do espaço de entrada. Nesse caso, você escreve um conjunto de padrões parciais, cada um dos quais corresponde a algumas entradas, mas não corresponde a outras entradas. Padrões ativos que nem sempre produzem um valor são chamados de *padrões ativos parciais*; Eles têm um valor de retorno que é um tipo de opção. Para definir um padrão ativo parcial, use um caractere curinga ( \_ ) no final da lista de padrões dentro dos clipes banana. O código a seguir ilustra o uso de um padrão ativo parcial.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5004.fs)]

A saída do exemplo anterior é a seguinte:

```console
1.100000 : Floating point
0 : Integer
0.000000 : Floating point
10 : Integer
Something else : Not matched.
```

Ao usar padrões ativos parciais, às vezes, as escolhas individuais podem ser separadas ou mutuamente exclusivas, mas não precisam ser. No exemplo a seguir, o quadrado padrão e o cubo de padrão não são contíguos, pois alguns números são quadrados e cubos, como 64. O programa a seguir usa o padrão e para combinar os padrões de cubo e quadrado. Ele imprime todos os inteiros de até 1000 que são quadrados e cubos, bem como aqueles que são apenas cubos.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5005.fs)]

A saída é da seguinte maneira:

```console
1 is a cube and a square
8 is a cube
27 is a cube
64 is a cube and a square
125 is a cube
216 is a cube
343 is a cube
512 is a cube
729 is a cube and a square
1000 is a cube
```

## <a name="parameterized-active-patterns"></a>Padrões ativos com parâmetros

Os padrões ativos sempre recebem pelo menos um argumento para o item que está sendo correspondido, mas também podem levar argumentos adicionais; nesse caso, o nome *padrão ativo com parâmetros* se aplica. Argumentos adicionais permitem que um padrão geral seja especializado. Por exemplo, padrões ativos que usam expressões regulares para analisar cadeias de caracteres geralmente incluem a expressão regular como um parâmetro extra, como no código a seguir, que também usa o padrão ativo parcial `Integer` definido no exemplo de código anterior. Neste exemplo, as cadeias de caracteres que usam expressões regulares para vários formatos de data são fornecidas para personalizar o padrão ativo ParseRegex geral. O padrão ativo inteiro é usado para converter as cadeias de caracteres correspondentes em números inteiros que podem ser passados para o Construtor DateTime.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5006.fs)]

A saída do código anterior é a seguinte:

```console
12/22/2008 12:00:00 AM 1/1/2009 12:00:00 AM 1/15/2008 12:00:00 AM 12/28/1995 12:00:00 AM
```

Os padrões ativos não são restritos apenas às expressões de correspondência de padrões, você também pode usá-los em associações Let.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5007.fs)]

A saída do código anterior é a seguinte:

```console
Hello, random citizen!
Hello, George!
```

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
- [Expressões de correspondência](match-expressions.md)
