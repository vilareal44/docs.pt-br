---
title: Matrizes
description: 'Saiba como criar e usar matrizes na linguagem de programação F #.'
ms.date: 08/13/2020
ms.openlocfilehash: 37f781ccd2c7bc2ca2c7b93bda53bbb3ea93b504
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608507"
---
# <a name="arrays"></a>Matrizes

As matrizes são coleções de tamanho fixo, com base em zero e mutáveis de elementos de dados consecutivos que são do mesmo tipo.

## <a name="create-arrays"></a>Criar matrizes

Você pode criar matrizes de várias maneiras. Você pode criar uma pequena matriz listando valores consecutivos entre `[|` e `|]` e separados por ponto e vírgula, conforme mostrado nos exemplos a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet1.fs)]

Você também pode colocar cada elemento em uma linha separada; nesse caso, o separador de ponto-e-vírgula é opcional.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet2.fs)]

O tipo dos elementos da matriz é inferido a partir dos literais usados e deve ser consistente. O código a seguir causa um erro porque 1,0 é um float e 2 e 3 são inteiros.

```fsharp
// Causes an error.
// let array2 = [| 1.0; 2; 3 |]
```

Você também pode usar expressões de sequência para criar matrizes. Veja a seguir um exemplo que cria uma matriz de quadrados de inteiros de 1 a 10.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet3.fs)]

Para criar uma matriz na qual todos os elementos são inicializados como zero, use `Array.zeroCreate` .

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet4.fs)]

## <a name="access-elements"></a>Elementos de acesso

Você pode acessar elementos de matriz usando um operador de ponto ( `.` ) e colchetes ( `[` e `]` ).

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet5.fs)]

Os índices de matriz começam em 0.

Você também pode acessar elementos de matriz usando a notação de fatia, que permite especificar um subintervalo da matriz. Seguem exemplos de notação de fatia.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet51.fs)]

Quando a notação de fatia é usada, uma nova cópia da matriz é criada.

## <a name="array-types-and-modules"></a>Módulos e tipos de matriz

O tipo de todas as matrizes F # é o tipo de .NET Framework <xref:System.Array?displayProperty=nameWithType> . Portanto, as matrizes F # oferecem suporte a todas as funcionalidades disponíveis no <xref:System.Array?displayProperty=nameWithType> .

O [ `Array` módulo](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html) dá suporte a operações em matrizes unidimensionais. Os módulos `Array2D` , `Array3D` e `Array4D` contêm funções que dão suporte a operações em matrizes de duas, três e quatro dimensões, respectivamente. Você pode criar matrizes de classificação maior que quatro usando <xref:System.Array?displayProperty=nameWithType> .

### <a name="simple-functions"></a>Funções simples

[`Array.get`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#get) Obtém um elemento. [`Array.length`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#length) fornece o comprimento de uma matriz. [`Array.set`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#set) define um elemento para um valor especificado. O exemplo de código a seguir ilustra o uso dessas funções.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet9.fs)]

A saída é a seguinte.

```console
0 1 2 3 4 5 6 7 8 9
```

### <a name="functions-that-create-arrays"></a>Funções que criam matrizes

Várias funções criam matrizes sem a necessidade de uma matriz existente. [`Array.empty`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#empty) Cria uma nova matriz que não contém nenhum elemento. [`Array.create`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#create) Cria uma matriz de um tamanho especificado e define todos os elementos para os valores fornecidos. [`Array.init`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#init) Cria uma matriz, dada uma dimensão e uma função para gerar os elementos. [`Array.zeroCreate`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#zeroCreate) Cria uma matriz na qual todos os elementos são inicializados com o valor zero para o tipo da matriz. O código a seguir demonstra essas funções.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet91.fs)]

A saída é a seguinte.

```console
Length of empty array: 0
Area of floats set to 5.0: [|5.0; 5.0; 5.0; 5.0; 5.0; 5.0; 5.0; 5.0; 5.0; 5.0|]
Array of squares: [|0; 1; 4; 9; 16; 25; 36; 49; 64; 81|]
```

[`Array.copy`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#copy) Cria uma nova matriz que contém elementos que são copiados de uma matriz existente. Observe que a cópia é uma cópia superficial, o que significa que se o tipo de elemento for um tipo de referência, somente a referência será copiada, não o objeto subjacente. O exemplo de código a seguir ilustra isso.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet11.fs)]

A saída do código anterior é a seguinte:

```console
[|Test1; Test2; |]
[|; Test2; |]
```

A cadeia de caracteres `Test1` aparece apenas na primeira matriz porque a operação de criação de um novo elemento substitui a referência em `firstArray` , mas não afeta a referência original a uma cadeia de caracteres vazia que ainda está presente no `secondArray` . A cadeia de caracteres `Test2` aparece em ambas as matrizes porque a `Insert` operação no <xref:System.Text.StringBuilder?displayProperty=nameWithType> tipo afeta o <xref:System.Text.StringBuilder?displayProperty=nameWithType> objeto subjacente, que é referenciado em ambas as matrizes.

[`Array.sub`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#sub) gera uma nova matriz de um subintervalo de uma matriz. Você especifica o subintervalo fornecendo o índice inicial e o comprimento. O código a seguir demonstra o uso de `Array.sub`.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet12.fs)]

A saída mostra que a submatriz começa no elemento 5 e contém 10 elementos.

```console
[|5; 6; 7; 8; 9; 10; 11; 12; 13; 14|]
```

[`Array.append`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#append) Cria uma nova matriz combinando duas matrizes existentes.

O código a seguir demonstra o **array. Append**.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet13.fs)]

A saída do código anterior é a seguinte.

```console
[|1; 2; 3; 4; 5; 6|]
```

[`Array.choose`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#choose) seleciona os elementos de uma matriz para incluir em uma nova matriz. O código a seguir demonstra `Array.choose` . Observe que o tipo de elemento da matriz não precisa corresponder ao tipo de valor retornado no tipo de opção. Neste exemplo, o tipo de elemento é `int` e a opção é o resultado de uma função polinomial, `elem*elem - 1` , como um número de ponto flutuante.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet14.fs)]

A saída do código anterior é a seguinte.

```console
[|3.0; 15.0; 35.0; 63.0; 99.0|]
```

[`Array.collect`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#collect) executa uma função especificada em cada elemento da matriz de uma matriz existente e, em seguida, coleta os elementos gerados pela função e os combina em uma nova matriz. O código a seguir demonstra `Array.collect` .

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet15.fs)]

A saída do código anterior é a seguinte.

```console
[|0; 1; 0; 1; 2; 3; 4; 5; 0; 1; 2; 3; 4; 5; 6; 7; 8; 9; 10|]
```

[`Array.concat`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#concat) usa uma sequência de matrizes e as combina em uma única matriz. O código a seguir demonstra `Array.concat` .

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet16.fs)]

A saída do código anterior é a seguinte.

```console
[|(1, 1, 1); (1, 2, 2); (1, 3, 3); (2, 1, 2); (2, 2, 4); (2, 3, 6); (3, 1, 3);
(3, 2, 6); (3, 3, 9)|]
```

[`Array.filter`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#filter) usa uma função de condição booliana e gera uma nova matriz que contém somente os elementos da matriz de entrada para a qual a condição é verdadeira. O código a seguir demonstra `Array.filter` .

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet17.fs)]

A saída do código anterior é a seguinte.

```console
[|2; 4; 6; 8; 10|]
```

[`Array.rev`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#rev) gera uma nova matriz revertendo a ordem de uma matriz existente. O código a seguir demonstra `Array.rev` .

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet18.fs)]

A saída do código anterior é a seguinte.

```console
"Hello world!"
```

Você pode combinar facilmente funções no módulo de matriz que transformam matrizes usando o operador de pipeline ( `|>` ), conforme mostrado no exemplo a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet19.fs)]

A saída é

```console
[|100; 36; 16; 4|]
```

### <a name="multidimensional-arrays"></a>Matrizes multidimensionais

Uma matriz multidimensional pode ser criada, mas não há nenhuma sintaxe para gravar um literal de matriz multidimensional. Use o operador [`array2D`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-array2dmodule.html) para criar uma matriz de uma sequência de sequências de elementos de matriz. As sequências podem ser literais de matriz ou lista. Por exemplo, o código a seguir cria uma matriz bidimensional.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet20.fs)]

Você também pode usar a função [`Array2D.init`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-array2dmodule.html#init) para inicializar matrizes de duas dimensões e funções semelhantes estão disponíveis para matrizes de três e quatro dimensões. Essas funções usam uma função que é usada para criar os elementos. Para criar uma matriz bidimensional que contém elementos definidos como um valor inicial em vez de especificar uma função, use a [`Array2D.create`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-array2dmodule.html#create) função, que também está disponível para matrizes de até quatro dimensões. O exemplo de código a seguir mostra primeiro como criar uma matriz de matrizes que contêm os elementos desejados e, em seguida, usa `Array2D.init` para gerar a matriz bidimensional desejada.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet21.fs)]

A indexação de matriz e a sintaxe de divisão têm suporte para matrizes até a classificação 4. Quando você especifica um índice em várias dimensões, usa vírgulas para separar os índices, conforme ilustrado no exemplo de código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet22.fs)]

O tipo de uma matriz bidimensional é escrito como `<type>[,]` (por exemplo, `int[,]` `double[,]` ), e o tipo de uma matriz tridimensional é escrito como `<type>[,,]` e assim por diante para matrizes de dimensões superiores.

Somente um subconjunto das funções disponíveis para matrizes unidimensionais também está disponível para matrizes multidimensionais.

### <a name="array-slicing-and-multidimensional-arrays"></a>Fatias de matrizes e matrizes multidimensionais

Em uma matriz bidimensional (uma matriz), você pode extrair uma submatriz especificando intervalos e usando um caractere curinga ( `*` ) para especificar linhas ou colunas inteiras.

```fsharp
// Get rows 1 to N from an NxM matrix (returns a matrix):
matrix.[1.., *]

// Get rows 1 to 3 from a matrix (returns a matrix):
matrix.[1..3, *]

// Get columns 1 to 3 from a matrix (returns a matrix):
matrix.[*, 1..3]

// Get a 3x3 submatrix:
matrix.[1..3, 1..3]
```

Você pode decompor uma matriz multidimensional em subconjuntos da mesma dimensão ou menor. Por exemplo, você pode obter um vetor de uma matriz especificando uma única linha ou coluna.

```fsharp
// Get row 3 from a matrix as a vector:
matrix.[3, *]

// Get column 3 from a matrix as a vector:
matrix.[*, 3]
```

Você pode usar essa sintaxe de divisão para tipos que implementam os operadores de acesso do elemento e os métodos sobrecarregados `GetSlice` . Por exemplo, o código a seguir cria um tipo de matriz que encapsula a matriz 2D F #, implementa uma propriedade item para fornecer suporte para indexação de matriz e implementa três versões do `GetSlice` . Se você puder usar esse código como um modelo para os tipos de matriz, poderá usar todas as operações de divisão que esta seção descreve.

```fsharp
type Matrix<'T>(N: int, M: int) =
    let internalArray = Array2D.zeroCreate<'T> N M

    member this.Item
        with get(a: int, b: int) = internalArray.[a, b]
        and set(a: int, b: int) (value:'T) = internalArray.[a, b] <- value

    member this.GetSlice(rowStart: int option, rowFinish : int option, colStart: int option, colFinish : int option) =
        let rowStart =
            match rowStart with
            | Some(v) -> v
            | None -> 0
        let rowFinish =
            match rowFinish with
            | Some(v) -> v
            | None -> internalArray.GetLength(0) - 1
        let colStart =
            match colStart with
            | Some(v) -> v
            | None -> 0
        let colFinish =
            match colFinish with
            | Some(v) -> v
            | None -> internalArray.GetLength(1) - 1
        internalArray.[rowStart..rowFinish, colStart..colFinish]

    member this.GetSlice(row: int, colStart: int option, colFinish: int option) =
        let colStart =
            match colStart with
            | Some(v) -> v
            | None -> 0
        let colFinish =
            match colFinish with
            | Some(v) -> v
            | None -> internalArray.GetLength(1) - 1
        internalArray.[row, colStart..colFinish]

    member this.GetSlice(rowStart: int option, rowFinish: int option, col: int) =
        let rowStart =
            match rowStart with
            | Some(v) -> v
            | None -> 0
        let rowFinish =
            match rowFinish with
            | Some(v) -> v
            | None -> internalArray.GetLength(0) - 1
        internalArray.[rowStart..rowFinish, col]

module test =
    let generateTestMatrix x y =
        let matrix = new Matrix<float>(3, 3)
        for i in 0..2 do
            for j in 0..2 do
                matrix.[i, j] <- float(i) * x - float(j) * y
        matrix

    let test1 = generateTestMatrix 2.3 1.1
    let submatrix = test1.[0..1, 0..1]
    printfn "%A" submatrix

    let firstRow = test1.[0,*]
    let secondRow = test1.[1,*]
    let firstCol = test1.[*,0]
    printfn "%A" firstCol
```

### <a name="boolean-functions-on-arrays"></a>Funções booleanas em matrizes

As funções [`Array.exists`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#exists) e os [`Array.exists2`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#exists2) elementos de teste em uma ou duas matrizes, respectivamente. Essas funções usam uma função de teste e retornam `true` se há um elemento (ou par de elementos para `Array.exists2` ) que satisfaça a condição.

O código a seguir demonstra o uso de `Array.exists` e `Array.exists2` . Nesses exemplos, novas funções são criadas aplicando-se apenas um dos argumentos, nesses casos, o argumento da função.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet23.fs)]

A saída do código anterior é a seguinte.

```console
true
false
false
true
```

Da mesma forma, a função [`Array.forall`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#forall) testa uma matriz para determinar se cada elemento atende a uma condição booliana. A variação [`Array.forall2`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#forall2) faz a mesma coisa usando uma função booleana que envolve elementos de duas matrizes de comprimento igual. O código a seguir ilustra o uso dessas funções.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet24.fs)]

A saída desses exemplos é a seguinte.

```console
false
true
true
false
```

### <a name="search-arrays"></a>Pesquisar matrizes

[`Array.find`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#find) usa uma função booliana e retorna o primeiro elemento para o qual a função retorna `true` , ou gera um <xref:System.Collections.Generic.KeyNotFoundException?displayProperty=nameWithType> elemento If nenhum que satisfaça a condição é encontrado. [`Array.findIndex`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#findIndex) é como `Array.find` , exceto pelo fato de que ele retorna o índice do elemento em vez do próprio elemento.

O código a seguir usa `Array.find` e `Array.findIndex` para localizar um número que seja um cubo perfeito e um quadrado perfeitos.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet25.fs)]

A saída é a seguinte.

```console
The first element that is both a square and a cube is 64 and its index is 62.
```

[`Array.tryFind`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#tryFind) é como `Array.find` , exceto que seu resultado é um tipo de opção e retorna `None` se nenhum elemento for encontrado. `Array.tryFind` deve ser usado em vez de `Array.find` quando você não sabe se um elemento correspondente está na matriz. Da mesma forma, [`Array.tryFindIndex`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#tryFindIndex) é como, `Array.findIndex` exceto que o tipo de opção é o valor de retorno. Se nenhum elemento for encontrado, a opção será `None` .

O código a seguir demonstra o uso de `Array.tryFind`. Esse código depende do código anterior.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet26.fs)]

A saída é a seguinte.

```console
Found an element: 1
Found an element: 729
Failed to find a matching element.
```

Use [`Array.tryPick`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#tryPick) quando precisar transformar um elemento, além de localizá-lo. O resultado é o primeiro elemento para o qual a função retorna o elemento transformado como um valor de opção ou `None` se nenhum elemento desse tipo é encontrado.

O código a seguir demonstra o uso de `Array.tryPick`. Nesse caso, em vez de uma expressão lambda, várias funções auxiliares locais são definidas para simplificar o código.

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet27.fs)]

A saída é a seguinte.

```console
Found an element 1 with square root 1 and cube root 1.
Found an element 64 with square root 8 and cube root 4.
Found an element 729 with square root 27 and cube root 9.
Found an element 4096 with square root 64 and cube root 16.
Did not find an element that is both a perfect square and a perfect cube.
```

### <a name="perform-computations-on-arrays"></a>Executar cálculos em matrizes

A [`Array.average`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#average) função retorna a média de cada elemento em uma matriz. Ele é limitado a tipos de elementos que dão suporte à divisão exata por um inteiro, que inclui tipos de ponto flutuante, mas não tipos integrais. A [`Array.averageBy`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#averageBy) função retorna a média dos resultados da chamada de uma função em cada elemento. Para uma matriz de tipo integral, você pode usar `Array.averageBy` e fazer com que a função Converta cada elemento em um tipo de ponto flutuante para a computação.

Use [`Array.max`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#max) ou [`Array.min`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#min) para obter o elemento máximo ou mínimo, se o tipo de elemento oferecer suporte a ele. Da mesma forma, [`Array.maxBy`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#maxBy) e [`Array.minBy`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#minBy) permite que uma função seja executada primeiro, talvez para transformar em um tipo que dê suporte à comparação.

[`Array.sum`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#sum) Adiciona os elementos de uma matriz e [`Array.sumBy`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#sumBy) chama uma função em cada elemento e adiciona os resultados juntos.

Para executar uma função em cada elemento em uma matriz sem armazenar os valores de retorno, use [`Array.iter`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#iter) . Para uma função que envolva duas matrizes de comprimento igual, use [`Array.iter2`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#iter2) . Se você também precisar manter uma matriz dos resultados da função, use [`Array.map`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#map) ou [`Array.map2`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#map2) , que opera em duas matrizes por vez.

As variações [`Array.iteri`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#iteri) e [`Array.iteri2`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#iteri2) permitem que o índice do elemento esteja envolvido na computação; o mesmo é verdadeiro para [`Array.mapi`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#mapi) e [`Array.mapi2`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#mapi2) .

As funções [`Array.fold`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#fold) , [`Array.foldBack`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#foldBack) , [`Array.reduce`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#reduce) , [`Array.reduceBack`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#reduceBack) , [`Array.scan`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#scan) e [`Array.scanBack`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#scanBack) executam algoritmos que envolvem todos os elementos de uma matriz. Da mesma forma, as variações [`Array.fold2`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#fold2) e [`Array.foldBack2`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#foldBack2) executam cálculos em duas matrizes.

Essas funções para executar computações correspondem às funções de mesmo nome no módulo de [lista](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html). Para obter exemplos de uso, consulte [listas](lists.md).

### <a name="modify-arrays"></a>Modificar matrizes

[`Array.set`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#set) define um elemento para um valor especificado. [`Array.fill`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#fill) define um intervalo de elementos em uma matriz para um valor especificado. O código a seguir fornece um exemplo de `Array.fill` .

[!code-fsharp[Main](~/samples/snippets/fsharp/arrays/snippet28.fs)]

A saída é a seguinte.

```console
[|1; 2; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 0; 23; 24; 25|]
```

Você pode usar [`Array.blit`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#blit) para copiar uma subseção de uma matriz para outra matriz.

### <a name="convert-to-and-from-other-types"></a>Converter de e para outros tipos

[`Array.ofList`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#ofList) Cria uma matriz de uma lista. [`Array.ofSeq`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#ofSeq) Cria uma matriz de uma sequência. [`Array.toList`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#toList) e [`Array.toSeq`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#toSeq) converter para esses outros tipos de coleção do tipo de matriz.

### <a name="sort-arrays"></a>Classificar matrizes

Use [`Array.sort`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#sort) para classificar uma matriz usando a função de comparação genérica. Use [`Array.sortBy`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#sortBy) para especificar uma função que gera um valor, chamado de *chave*, para classificar usando a função de comparação genérica na chave. Use [`Array.sortWith`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#sortWith) se você quiser fornecer uma função de comparação personalizada. `Array.sort`, `Array.sortBy` e `Array.sortWith` todos retornam a matriz classificada como uma nova matriz. As variações [`Array.sortInPlace`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#sortInPlace) , [`Array.sortInPlaceBy`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#sortInPlaceBy) e [`Array.sortInPlaceWith`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#sortInPlaceWith) modificam a matriz existente em vez de retornar uma nova.

### <a name="arrays-and-tuples"></a>Matrizes e tuplas

As funções [`Array.zip`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#zip) e [`Array.unzip`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#unzip) convertem matrizes de pares de tupla para tuplas de matrizes e vice-versa. [`Array.zip3`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#zip3) e [`Array.unzip3`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#unzip3) são semelhantes, exceto que funcionam com tuplas de três elementos ou tuplas de três matrizes.

## <a name="parallel-computations-on-arrays"></a>Cálculos paralelos em matrizes

O módulo [`Array.Parallel`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule-parallel.html) contém funções para executar cálculos paralelos em matrizes. Este módulo não está disponível em aplicativos que visam versões do .NET Framework antes da versão 4.

## <a name="see-also"></a>Veja também

- [Referência de linguagem F #](index.md)
- [Tipos F#](fsharp-types.md)
