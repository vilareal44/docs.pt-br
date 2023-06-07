---
title: Sequências
description: 'Saiba como usar sequências F #, quando você tem uma coleção de dados grande e ordenada, mas não espera necessariamente usar todos os elementos.'
ms.date: 08/13/2020
ms.openlocfilehash: c84e0aebcc79a595c0ae3b9075648fb629bdd65c
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559031"
---
# <a name="sequences"></a>Sequências

Uma *sequência* é uma série lógica de elementos de um tipo. As sequências são particularmente úteis quando você tem uma coleção de dados grande e ordenada, mas não espera necessariamente usar todos os elementos. Os elementos de sequência individuais são calculados somente conforme necessário, portanto, uma sequência pode fornecer um melhor desempenho do que uma lista em situações nas quais nem todos os elementos são usados. As sequências são representadas pelo `seq<'T>` tipo, que é um alias para <xref:System.Collections.Generic.IEnumerable%601> . Portanto, qualquer tipo de .NET que implemente <xref:System.Collections.Generic.IEnumerable%601> a interface pode ser usado como uma sequência. O [módulo Seq](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html) fornece suporte para manipulações que envolvem sequências.

## <a name="sequence-expressions"></a>Expressões de sequência

Uma *expressão de sequência* é uma expressão que é avaliada como uma sequência. Expressões de sequência podem usar vários formulários. A forma mais simples especifica um intervalo. Por exemplo, `seq { 1 .. 5 }` cria uma sequência que contém cinco elementos, incluindo os pontos de extremidade 1 e 5. Você também pode especificar um incremento (ou decrementar) entre dois pontos duplos. Por exemplo, o código a seguir cria a sequência de múltiplos de 10.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1502.fs)]

Expressões de sequência são constituídas de expressões F # que produzem valores da sequência. Você também pode gerar valores programaticamente:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1503.fs)]

O exemplo anterior usa o `->` operador, que permite que você especifique uma expressão cujo valor se tornará uma parte da sequência. Você só poderá usar `->` se todas as partes do código que o segue retornarem um valor.

Como alternativa, você pode especificar a `do` palavra-chave, com um opcional a `yield` seguir:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1504.fs)]

O código a seguir gera uma lista de pares de coordenadas junto com um índice em uma matriz que representa a grade. Observe que a primeira `for` expressão requer que um `do` seja especificado.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1505.fs)]

Uma `if` expressão usada em uma sequência é um filtro. Por exemplo, para gerar uma sequência de apenas números primos, supondo que você tenha uma função `isprime` do tipo `int -> bool` , construa a sequência da seguinte maneira.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1506.fs)]

Conforme mencionado anteriormente, `do` é necessário aqui porque não há `else` ramificação com o `if` . Se você tentar usar o `->` , receberá um erro dizendo que nem todas as ramificações retornarão um valor.

## <a name="the-yield-keyword"></a>A palavra-chave `yield!`

Às vezes, você pode desejar incluir uma sequência de elementos em outra sequência. Para incluir uma sequência em outra sequência, você precisará usar a `yield!` palavra-chave:

```fsharp
// Repeats '1 2 3 4 5' ten times
seq {
    for _ in 1..10 do
        yield! seq { 1; 2; 3; 4; 5}
}
```

Outra maneira de pensar `yield!` é que ele nivela uma sequência interna e, em seguida, inclui isso na sequência que a contém.

Quando `yield!` é usado em uma expressão, todos os outros valores únicos devem usar a `yield` palavra-chave:

```fsharp
// Combine repeated values with their values
seq {
    for x in 1..10 do
        yield x
        yield! seq { for i in 1..x -> i}
}
```

O exemplo anterior produzirá o valor de, `x` além de todos os valores de `1` para `x` cada `x` .

## <a name="examples"></a>Exemplos

O primeiro exemplo usa uma expressão de sequência que contém uma iteração, um filtro e um yield para gerar uma matriz. Esse código imprime uma sequência de números primos entre 1 e 100 no console.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1507.fs)]

O exemplo a seguir cria uma tabela de multiplicação que consiste em tuplas de três elementos, cada um consistindo de dois fatores e do produto:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1508.fs)]

O exemplo a seguir demonstra o uso de `yield!` para combinar Sequências individuais em uma única sequência final. Nesse caso, as sequências para cada subárvore em uma árvore binária são concatenadas em uma função recursiva para produzir a sequência final.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1509.fs)]

## <a name="using-sequences"></a>Usando sequências

As sequências dão suporte a muitas das mesmas funções que as [listas](lists.md). As sequências também dão suporte a operações como agrupamento e contagem usando funções de geração de chaves. As sequências também dão suporte a funções mais diversificadas para extração de subsequências.

Muitos tipos de dados, como listas, matrizes, conjuntos e mapas são implicitamente sequências porque são coleções enumeráveis. Uma função que usa uma sequência como um argumento funciona com qualquer um dos tipos de dados comuns de F #, além de qualquer tipo de dados .NET que implemente `System.Collections.Generic.IEnumerable<'T>` . Compare isso com uma função que usa uma lista como um argumento, que só pode receber listas. O tipo `seq<'T>` é uma abreviação de tipo para `IEnumerable<'T>` . Isso significa que qualquer tipo que implementa o genérico `System.Collections.Generic.IEnumerable<'T>` , que inclui matrizes, listas, conjuntos e mapas em F #, e também a maioria dos tipos de coleção .net, é compatível com o `seq` tipo e pode ser usado sempre que uma sequência é esperada.

## <a name="module-functions"></a>Funções do módulo

O [módulo Seq](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html) no [namespace FSharp. Collections](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections.html) contém funções para trabalhar com sequências. Essas funções funcionam com listas, matrizes, mapas e conjuntos também, porque todos esses tipos são enumeráveis e, portanto, podem ser tratados como sequências.

## <a name="creating-sequences"></a>Criando sequências

Você pode criar sequências usando expressões de sequência, conforme descrito anteriormente, ou usando determinadas funções.

Você pode criar uma sequência vazia usando [Seq. Empty](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#empty)ou pode criar uma sequência de apenas um elemento especificado usando [Seq. Singleton](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#singleton).

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet9.fs)]

Você pode usar [Seq.init](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#init) para criar uma sequência para a qual os elementos são criados usando uma função que você fornece. Você também fornece um tamanho para a sequência. Essa função é exatamente como [List.init](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#init), exceto que os elementos não são criados até que você itere pela sequência. O código a seguir ilustra o uso de `Seq.init` .

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet10.fs)]

A saída é

```console
0 10 20 30 40
```

Usando a função [Seq. ofArray](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#ofArray) e [seq. ofList&#60;&#62;](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#ofList), você pode criar sequências de matrizes e listas. No entanto, você também pode converter matrizes e listas em sequências usando um operador cast. As duas técnicas são mostradas no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet11.fs)]

Usando [Seq. Cast](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#cast), você pode criar uma sequência de uma coleção de tipo fraco, como aquelas definidas em `System.Collections` . Essas coleções tipadas de forma fraca têm o tipo de elemento `System.Object` e são enumeradas usando o tipo não genérico `System.Collections.Generic.IEnumerable&#96;1` . O código a seguir ilustra o uso de `Seq.cast` para converter um `System.Collections.ArrayList` em uma sequência.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet12.fs)]

Você pode definir sequências infinitas usando a função [Seq.initInfinite](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#initInfinite) . Para tal sequência, você fornece uma função que gera cada elemento do índice do elemento. Sequências infinitas são possíveis devido à avaliação lenta; os elementos são criados conforme necessário chamando a função que você especificar. O exemplo de código a seguir produz uma sequência infinita de números de ponto flutuante, neste caso, a série alternada de recíprocos de quadrados de inteiros sucessivos.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet13.fs)]

[Seq. desdobrar](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#unfold) gera uma sequência de uma função de computação que usa um estado e a transforma para produzir cada elemento subsequente na sequência. O estado é apenas um valor que é usado para computar cada elemento e pode ser alterado conforme cada elemento é computado. O segundo argumento para `Seq.unfold` é o valor inicial usado para iniciar a sequência. `Seq.unfold` usa um tipo de opção para o estado, que permite que você finalize a sequência retornando o `None` valor. O código a seguir mostra dois exemplos de sequências `seq1` e `fib` , que são gerados por uma `unfold` operação. A primeira, `seq1` , é apenas uma sequência simples com números de até 20. O segundo, `fib` ,, usa `unfold` para calcular a sequência Fibonacci. Como cada elemento na sequência Fibonacci é a soma dos dois números de Fibonacci anteriores, o valor de estado é uma tupla que consiste nos dois números anteriores na sequência. O valor inicial é `(1,1)` , os dois primeiros números na sequência.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet14.fs)]

A saída é da seguinte maneira:

```console
The sequence seq1 contains numbers from 0 to 20.

0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20

The sequence fib contains Fibonacci numbers.

2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597
```

O código a seguir é um exemplo que usa muitas das funções de módulo de sequência descritas aqui para gerar e calcular os valores de sequências infinitas. O código pode levar alguns minutos para ser executado.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet15.fs)]

## <a name="searching-and-finding-elements"></a>Pesquisando e Localizando elementos

As sequências dão suporte à funcionalidade disponível com listas: [Seq. Exists](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#exists), [Seq. exists2](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#exists), [Seq. Find](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#find), [Seq. FindIndex](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#findIndex), [Seq. escolha](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#pick), [Seq. tryFind](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#tryFind)e [Seq. tryFindIndex](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#tryFindIndex). As versões dessas funções que estão disponíveis para sequências avaliam a sequência somente até o elemento que está sendo pesquisado. Para obter exemplos, consulte [listas](lists.md).

## <a name="obtaining-subsequences"></a>Obtendo subsequências

[Seq. Filter](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#filter) e [Seq. Choose](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#choose) são como as funções correspondentes que estão disponíveis para listas, exceto que a filtragem e a escolha não ocorrem até que os elementos de sequência sejam avaliados.

[Seq. Truncate](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#truncate) cria uma sequência de outra sequência, mas limita a sequência a um número especificado de elementos. [Seq. Take](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#take) cria uma nova sequência que contém apenas um número especificado de elementos desde o início de uma sequência. Se houver menos elementos na sequência do que você especificar para executar, `Seq.take` o lançará um `System.InvalidOperationException` . A diferença entre `Seq.take` e `Seq.truncate` é que o não `Seq.truncate` produzirá um erro se o número de elementos for menor que o número especificado.

O código a seguir mostra o comportamento e as diferenças entre `Seq.truncate` e `Seq.take` .

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet16.fs)]

A saída, antes da ocorrência do erro, é a seguinte.

```console
1 4 9 16 25
1 4 9 16 25 36 49 64 81 100
1 4 9 16 25
1 4 9 16 25 36 49 64 81 100
```

Usando [Seq. TakeWhile](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#takeWhile), você pode especificar uma função de predicado (uma função booliana) e criar uma sequência de outra sequência composta desses elementos da sequência original para a qual o predicado é `true` , mas parar antes do primeiro elemento para o qual o predicado retorna `false` . [Seq. Skip](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#skip) retorna uma sequência que ignora um número especificado dos primeiros elementos de outra sequência e retorna os elementos restantes. [Seq. SkipWhile](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#skipWhile) retorna uma sequência que ignora os primeiros elementos de outra sequência, desde que o predicado retorne `true` e, em seguida, retorne os elementos restantes, começando com o primeiro elemento para o qual o predicado retorna `false` .

O exemplo de código a seguir ilustra o comportamento e as diferenças entre `Seq.takeWhile` , `Seq.skip` e `Seq.skipWhile` .

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet17.fs)]

A saída é a seguinte.

```console
1 4 9
36 49 64 81 100
16 25 36 49 64 81 100
```

## <a name="transforming-sequences"></a>Transformando sequências

[Seq. emparelha](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#pairwise) cria uma nova sequência na qual os elementos sucessivos da sequência de entrada são agrupados em tuplas.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet18.fs)]

O [Seq. Window](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#windowed) é como `Seq.pairwise` , exceto que, em vez de produzir uma sequência de tuplas, ele produz uma sequência de matrizes que contêm cópias de elementos adjacentes (uma *janela*) da sequência. Você especifica o número de elementos adjacentes que deseja em cada matriz.

O exemplo de código a seguir demonstra o uso de `Seq.windowed`. Nesse caso, o número de elementos na janela é 3. O exemplo usa `printSeq` , que é definido no exemplo de código anterior.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet180.fs)]

A saída é a seguinte.

Sequência inicial:

```console
1.0 1.5 2.0 1.5 1.0 1.5

Windows of length 3:
[|1.0; 1.5; 2.0|] [|1.5; 2.0; 1.5|] [|2.0; 1.5; 1.0|] [|1.5; 1.0; 1.5|]

Moving average:
1.5 1.666666667 1.5 1.333333333
```

## <a name="operations-with-multiple-sequences"></a>Operações com várias sequências

[Seq.zip](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#zip) e [Seq.zip3](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#zip3) demoram duas ou três sequências e produzem uma sequência de tuplas. Essas funções são como as funções correspondentes disponíveis para [listas](lists.md). Não há nenhuma funcionalidade correspondente para separar uma sequência em duas ou mais sequências. Se você precisar dessa funcionalidade para uma sequência, converta a sequência em uma lista e use [list. Unzip](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#unzip).

## <a name="sorting-comparing-and-grouping"></a>Classificando, comparando e agrupando

As funções de classificação com suporte para listas também funcionam com sequências. Isso inclui [Seq. Sort](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#sort) e [Seq. sortBy](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#sortBy). Essas funções iteram por toda a sequência.

Você compara duas sequências usando a função [Seq. compareWith](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#compareWith) . A função compara os elementos sucessivos por vez e para quando encontra o primeiro par desigual. Quaisquer elementos adicionais não contribuem para a comparação.

O código a seguir demonstra o uso de `Seq.compareWith`.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet19.fs)]

No código anterior, apenas o primeiro elemento é computado e examinado, e o resultado é-1.

[Seq. countBy](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#countBy) usa uma função que gera um valor chamado *chave* para cada elemento. Uma chave é gerada para cada elemento chamando essa função em cada elemento. `Seq.countBy` em seguida, retorna uma sequência que contém os valores de chave e uma contagem do número de elementos que geraram cada valor da chave.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet201.fs)]

A saída é a seguinte.

```console
(1, 34) (2, 33) (0, 33)
```

A saída anterior mostra que havia 34 elementos da sequência original que produziram os valores de chave 1, 33 que produziram a chave 2 e 33 valores que produziram a chave 0.

Você pode agrupar elementos de uma sequência chamando [Seq. GroupBy](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#groupBy). `Seq.groupBy` usa uma sequência e uma função que gera uma chave de um elemento. A função é executada em cada elemento da sequência. `Seq.groupBy` Retorna uma sequência de tuplas, em que o primeiro elemento de cada tupla é a chave e a segunda é uma sequência de elementos que produzem essa chave.

O exemplo de código a seguir mostra o uso de `Seq.groupBy` para particionar a sequência de números de 1 a 100 em três grupos que têm os valores de chave distintos 0, 1 e 2.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet202.fs)]

A saída é a seguinte.

```console
(1, seq [1; 4; 7; 10; ...]) (2, seq [2; 5; 8; 11; ...]) (0, seq [3; 6; 9; 12; ...])
```

Você pode criar uma sequência que elimina elementos duplicados chamando [Seq. Distinct](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#distinct). Ou você pode usar [Seq. distinctBy](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#distinctBy), que usa uma função de geração de chaves a ser chamada em cada elemento. A sequência resultante contém elementos da sequência original que têm chaves exclusivas; os elementos posteriores que produzem uma chave duplicada para um elemento anterior são descartados.

O exemplo de código a seguir ilustra o uso do objeto `Seq.distinct`. `Seq.distinct` é demonstrado pela geração de sequências que representam números binários e, em seguida, mostrando que os únicos elementos distintos são 0 e 1.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet22.fs)]

O código a seguir demonstra `Seq.distinctBy` iniciando com uma sequência que contém números negativos e positivos e usando a função de valor absoluto como a função de geração de chaves. A sequência resultante não tem todos os números positivos que correspondem aos números negativos na sequência, pois os números negativos aparecem anteriormente na sequência e, portanto, são selecionados em vez dos números positivos que têm o mesmo valor absoluto ou chave.

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet23.fs)]

## <a name="readonly-and-cached-sequences"></a>Sequências ReadOnly e armazenadas em cache

[Seq. ReadOnly](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#readonly) cria uma cópia somente leitura de uma sequência. `Seq.readonly` é útil quando você tem uma coleção de leitura/gravação, como uma matriz, e não deseja modificar a coleção original. Essa função pode ser usada para preservar o encapsulamento de dados. No exemplo de código a seguir, um tipo que contém uma matriz é criado. Uma propriedade expõe a matriz, mas em vez de retornar uma matriz, ela retorna uma sequência que é criada a partir da matriz usando `Seq.readonly` .

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet24.fs)]

[Seq. cache](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#cache) cria uma versão armazenada de uma sequência. Use `Seq.cache` para evitar a reavaliação de uma sequência, ou quando você tiver vários threads que usam uma sequência, mas você deve certificar-se de que cada elemento seja acionado apenas uma vez. Quando você tem uma sequência que está sendo usada por vários threads, você pode ter um thread que enumera e computa os valores para a sequência original, e os threads restantes podem usar a sequência armazenada em cache.

## <a name="performing-computations-on-sequences"></a>Executando cálculos em sequências

Operações aritméticas simples são como as de listas, como [Seq. Average](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#average), [Seq. Sum](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#sum), [Seq. averageBy](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#averageBy), [Seq. sumBy](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#sumBy)e assim por diante.

[Seq. fold](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#fold), [Seq. reduz](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#reduce)e [Seq. scan](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#scan) são como as funções correspondentes que estão disponíveis para listas. As sequências dão suporte a um subconjunto das variações completas dessas funções que listam o suporte. Para obter mais informações e exemplos, consulte [listas](lists.md).

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
- [Tipos F#](fsharp-types.md)
