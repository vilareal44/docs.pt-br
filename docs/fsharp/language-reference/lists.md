---
title: Listas
description: 'Saiba mais sobre as listas F #, uma série ordenada e imutável de elementos do mesmo tipo.'
ms.date: 08/13/2020
ms.openlocfilehash: 16d7195039d25cf63630f5cc3be6563b1bf45c44
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559161"
---
# <a name="lists"></a>Listas

Uma lista em F# é uma série imutável, ordenada, de elementos do mesmo tipo. Para executar operações básicas em listas, use as funções no [módulo de lista](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html).

## <a name="creating-and-initializing-lists"></a>Criando e inicializando listas

Você pode definir uma lista ao relacionar explicitamente os elementos, separados por ponto e vírgula e entre colchetes, como mostra a linha de código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1301.fs)]

Você também pode colocar quebras de linha entre os elementos, caso em que as vírgulas são opcionais. A última sintaxe pode resultar em um código mais legível quando as expressões de inicialização de elemento são mais longas ou quando você quer incluir um comentário para cada elemento.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet13011.fs)]

Normalmente, todos os elementos da lista devem ser do mesmo tipo. Uma exceção existe quando uma lista em que os elementos são especificados para ser um tipo de base pode ter elementos do tipo derivado. Dessa forma, o que segue é aceitável, porque tanto `Button` quanto `CheckBox` derivam de `Control`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet13012.fs)]

Você também pode definir os elementos da lista, usando um intervalo indicado por inteiros separados pelo operador de intervalo (`..`), como mostrado no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1302.fs)]

Uma lista vazia é especificada por um par de colchetes sem nada entre eles.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1304.fs)]

Você também pode usar uma expressão de sequência para criar uma lista. Consulte [expressões de sequência](sequences.md#sequence-expressions) para obter mais informações. Por exemplo, o código a seguir cria uma lista dos quadrados de números inteiros de 1 a 10.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1303.fs)]

## <a name="operators-for-working-with-lists"></a>Operadores para trabalhar com listas

Você pode anexar elementos a uma lista usando o operador (cons) `::`. Se `list1` for `[2; 3; 4]`, o seguinte código criará `list2` como `[100; 2; 3; 4]`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1305.fs)]

Você pode concatenar listas que possuem tipos compatíveis usando o operador `@`, como no código a seguir. Se `list1` for `[2; 3; 4]` e `list2` for `[100; 2; 3; 4]`, esse código criará `list3` como `[2; 3; 4; 100; 2; 3; 4]`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1306.fs)]

As funções para executar operações em listas estão disponíveis no [módulo lista](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html).

Como as listas em F# são imutáveis, quaisquer operações de modificação geram novas listas em vez de modificar as listas existentes.

As listas em F # são implementadas como listas vinculadas individualmente, o que significa que as operações que acessam apenas o cabeçalho da lista são O (1) e O acesso ao elemento é O (*n*).

## <a name="properties"></a>Propriedades

O tipo de lista oferece suporte às seguintes propriedades:

|Propriedade|Type|Descrição|
|--------|----|-----------|
|[Principal](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-list-1.html#Head)|`'T`|O primeiro elemento.|
|[Empty (vazio)](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-list-1.html#Empty)|`'T list`|A propriedade estática que retorna uma lista vazia do tipo apropriado.|
|[IsEmpty](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-list-1.html#IsEmpty)|`bool`|`true` se a lista não tiver elementos.|
|[Item](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-list-1.html#Item)|`'T`|O elemento no índice especificado (com base em zero).|
|[Comprimento](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-list-1.html#Length)|`int`|O número de elementos.|
|[Tail](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-list-1.html#Tail)|`'T list`|A lista sem o primeiro elemento.|

A seguir, alguns exemplos de como usar essas propriedades.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1307.fs)]

## <a name="using-lists"></a>Usando as listas

A programação com listas permite executar operações complexas com uma pequena quantidade de código. Esta seção descreve operações comuns em listas que são importantes para programação funcional.

### <a name="recursion-with-lists"></a>Recursão com listas

As listas são particularmente adequadas para técnicas de programação recursivas. Considere uma operação que deve ser executada em cada elemento de uma lista. Você pode fazer isso de forma recursiva, operando no cabeçalho da lista e passando para a parte final da lista, que é uma lista menor que consiste na lista original, sem o primeiro elemento, e volte novamente ao próximo nível de recursão.

Para escrever uma função recursiva, você usa o operador cons (`::`) em correspondência de padrões, o que permite separar o cabeçalho de uma lista da parte final da lista.

O exemplo de código a seguir mostra como usar a correspondência de padrões para implementar uma função recursiva que executa operações em uma lista.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet13071.fs)]

O código anterior funciona bem para pequenas listas, mas para listas maiores, pode estourar a pilha. O código a seguir melhora este código usando um argumento acumulador, uma técnica padrão para trabalhar com funções recursivas. O uso do argumento acumulador torna a parte final da função recursiva, o que economiza espaço em pilha.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet13072.fs)]

A função `RemoveAllMultiples` é uma função recursiva que usa duas listas. A primeira lista contém os números cujos múltiplos serão removidos e a segunda lista é a lista a partir da qual os números serão removidos. O código no exemplo a seguir usa essa função recursiva para eliminar todos os números não primos de uma lista, deixando uma lista de números primos como o resultado.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1308.fs)]

A saída é da seguinte maneira:

```console
Primes Up To 100:
[2; 3; 5; 7; 11; 13; 17; 19; 23; 29; 31; 37; 41; 43; 47; 53; 59; 61; 67; 71; 73; 79; 83; 89; 97]
```

## <a name="module-functions"></a>Funções do módulo

O [módulo lista](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html) fornece funções que acessam os elementos de uma lista. O elemento cabeçalho é o mais rápido e fácil de acessar. Use o [cabeçalho](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-list-1.html#Head) da propriedade ou a lista de funções do módulo [. Head](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#head). Você pode acessar a parte final de uma lista usando a propriedade [tail](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-list-1.html#Tail) ou a função [list. tail](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#tail) . Para localizar um elemento por índice, use a função [list. nth](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#nth) . `List.nth` percorre a lista. Portanto, é O (*n*). Se seu código usa `List.nth` com frequência, considere o uso de uma matriz em vez de uma lista. O acesso de elemento em matrizes é O(1).

### <a name="boolean-operations-on-lists"></a>Operações boolianas em listas

A função [list. IsEmpty](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#isEmpty) determina se uma lista tem elementos.

A função [list. Exists](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#exists) aplica um teste booliano aos elementos de uma lista e retorna `true` se algum elemento satisfizer o teste. [List. exists2](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#exists2) é semelhante, mas opera em pares sucessivos de elementos em duas listas.

O código a seguir demonstra o uso de `List.exists`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet1.fs)]

A saída é da seguinte maneira:

```console
For list [0; 1; 2; 3], contains zero is true
```

O exemplo a seguir demonstra o uso de `List.exists2`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet2.fs)]

A saída é da seguinte maneira:

```console
Lists [1; 2; 3; 4; 5] and [5; 4; 3; 2; 1] have at least one equal element at the same position.
```

Você pode usar [list. ForAll](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#forall) se desejar testar se todos os elementos de uma lista atendem a uma condição.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet3.fs)]

A saída é da seguinte maneira:

```console
true
false
```

Da mesma forma, [list. forall2](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#forall2) determina se todos os elementos nas posições correspondentes em duas listas atendem a uma expressão booliana que envolve cada par de elementos.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet4.fs)]

A saída é da seguinte maneira:

```console
true
false
```

### <a name="sort-operations-on-lists"></a>Operações de classificação em listas

As listas de classificação [list. Sort](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#sort), [list. sortBy](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#sortBy)e [list. sortWith](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#sortWith) functions. A função de classificação determina qual dessas três funções usar. `List.sort` usa uma comparação genérica padrão. A comparação genérica utiliza operadores globais com base na função de comparação genérica para comparar valores. Funciona de forma eficiente com uma grande variedade de tipos de elementos, tais como tipos numéricos simples, tuplas, registros, uniões discriminadas, listas, matrizes e qualquer tipo que implementa `System.IComparable`. Para os tipos que implementam `System.IComparable`, a comparação genérica usa a função `System.IComparable.CompareTo()`. A comparação genérica também funciona com cadeias de caracteres, mas usa uma ordem de classificação independente de cultura. A comparação genérica não deve ser usada em tipos sem suporte, como os tipos de função. Além disso, o desempenho da comparação genérica padrão é melhor para tipos estruturados pequenos; para tipos estruturados maiores que precisam ser comparados e classificados com frequência, considere implementar `System.IComparable` e fornecer uma implementação eficiente do método `System.IComparable.CompareTo()`.

`List.sortBy` usa uma função que retorna um valor usado como critério de classificação e `List.sortWith` assume uma função de comparação como argumento. Essas duas últimas funções são úteis quando você está trabalhando com tipos que não suportam comparação ou quando a comparação requer mais semântica de comparação complexa, como é o caso das cadeias de caracteres com reconhecimento de cultura.

O exemplo a seguir demonstra o uso de `List.sort`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet5.fs)]

A saída é da seguinte maneira:

```console
[-2; 1; 4; 5; 8]
```

O exemplo a seguir demonstra o uso de `List.sortBy`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet6.fs)]

A saída é da seguinte maneira:

```console
[1; -2; 4; 5; 8]
```

O exemplo a seguir demonstra o uso de `List.sortWith`. Neste exemplo, a função de comparação personalizada `compareWidgets` é usada para comparar um primeiro campo de um tipo personalizado e depois outro, quando os valores do primeiro campo são iguais.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet7.fs)]

A saída é da seguinte maneira:

```console
[{ID = 92;
Rev = 1;}; {ID = 92;
Rev = 1;}; {ID = 100;
Rev = 2;}; {ID = 100;
Rev = 5;}; {ID = 110;
Rev = 1;}]
```

### <a name="search-operations-on-lists"></a>Operações de pesquisa em listas

Várias operações de pesquisa têm suporte para listas. O mais simples, [list. Find](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#find), permite que você encontre o primeiro elemento que corresponde a uma determinada condição.

O exemplo de código a seguir demonstra o uso de `List.find` para encontrar o primeiro número divisível por 5 em uma lista.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet8.fs)]

O resultado é 5.

Se os elementos devem ser transformados primeiro, chame [list. escolha](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#pick), que usa uma função que retorna uma opção e procura o primeiro valor de opção que é `Some(x)` . Em vez de retornar o elemento, `List.pick` retorna o resultado `x`. Se nenhum elemento correspondente for encontrado, `List.pick` lançará `System.Collections.Generic.KeyNotFoundException`. O código a seguir demonstra o uso de `List.pick`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet9.fs)]

A saída é da seguinte maneira:

```console
"b"
```

Outro grupo de operações de pesquisa, [list. tryFind](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#tryFind) e funções relacionadas, retornam um valor de opção. A função `List.tryFind` retorna o primeiro elemento de uma lista que satisfaz a uma condição se tal elemento existir, mas retorna o valor da opção `None` se o elemento não existir. A lista de variação [. tryFindIndex](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#tryFindIndex) retorna o índice do elemento, se um for encontrado, em vez do próprio elemento. Essas funções são ilustradas no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet10.fs)]

A saída é da seguinte maneira:

```console
The first even value is 22.
The first even value is at position 8.
```

### <a name="arithmetic-operations-on-lists"></a>Operações aritméticas em listas

Operações aritméticas comuns, como Sum e Average, são criadas no [módulo List](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html). Para trabalhar com [list. Sum](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#sum), o tipo de elemento de lista deve dar suporte ao `+` operador e ter um valor zero. Todos os tipos aritméticos internos satisfazem a essas condições. Para trabalhar com [list. Average](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#average), o tipo de elemento deve dar suporte à divisão sem pendência, o que exclui tipos integrais, mas permite tipos de ponto flutuante. As funções [list. sumBy](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#sumBy) e [list. averageBy](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#averageBy) usam uma função como um parâmetro e os resultados dessa função são usados para calcular os valores para a soma ou a média.

O código a seguir demonstra o uso de `List.sum`, `List.sumBy` e `List.average`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet11.fs)]

A saída é `1.000000`.

O código a seguir demonstra o uso de `List.averageBy`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet12.fs)]

A saída é `5.5`.

### <a name="lists-and-tuples"></a>Listas e tuplas

As listas que contêm tuplas podem ser manipuladas pelas funções de compactação e descompactação. Essas funções combinam duas listas de valores únicos em uma lista de tuplas ou separam uma lista de tuplas em duas listas de valores únicos. A função [List.zip](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#zip) mais simples usa duas listas de elementos únicos e produz uma única lista de pares de tupla. Outra versão, [List.zip3](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#zip3), usa três listas de elementos únicos e produz uma única lista de tuplas que têm três elementos. O exemplo de código a seguir demonstra o uso de `List.zip`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet13.fs)]

A saída é da seguinte maneira:

```console
[(1, -1); (2, -2); (3; -3)]
```

O exemplo de código a seguir demonstra o uso de `List.zip3`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet14.fs)]

A saída é da seguinte maneira:

```console
[(1, -1, 0); (2, -2, 0); (3, -3, 0)]
```

As versões de descompactação correspondentes, [list. Unzip](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#unzip) e [list. unzip3](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#unzip3), tomam listas de tuplas e listas de retorno em uma tupla, em que a primeira lista contém todos os elementos que foram primeiro em cada tupla, e a segunda lista contém o segundo elemento de cada tupla e assim por diante.

O exemplo de código a seguir demonstra o uso de [list. Unzip](https://msdn.microsoft.com/library/639db80c-41b5-45bb-a6b4-1eaa04d61d21).

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet15.fs)]

A saída é da seguinte maneira:

```console
([1; 3], [2; 4])
[1; 3] [2; 4]
```

O exemplo de código a seguir demonstra o uso de [list. unzip3](https://msdn.microsoft.com/library/43078c77-32ec-4342-85b3-c31ccf984db4).

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet16.fs)]

A saída é da seguinte maneira:

```console
([1; 4], [2; 5], [3; 6])
```

### <a name="operating-on-list-elements"></a>Operando em elementos de lista

F# oferece suporte a uma variedade de operações em elementos de lista. A mais simples é [list. iter](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#iter), que permite chamar uma função em todos os elementos de uma lista. As variações incluem [list. iter2](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#iter2), que permite que você execute uma operação em elementos de duas listas, [list. iteri](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#iteri), que é como, `List.iter` exceto que o índice de cada elemento é passado como um argumento para a função que é chamada para cada elemento e [list. iteri2](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#iteri2), que é uma combinação da funcionalidade do `List.iter2` e do `List.iteri` . O exemplo de código a seguir ilustra essas funções.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet17.fs)]

A saída é da seguinte maneira:

```console
List.iter: element is 1
List.iter: element is 2
List.iter: element is 3
List.iteri: element 0 is 1
List.iteri: element 1 is 2
List.iteri: element 2 is 3
List.iter2: elements are 1 4
List.iter2: elements are 2 5
List.iter2: elements are 3 6
List.iteri2: element 0 of list1 is 1; element 0 of list2 is 4
List.iteri2: element 1 of list1 is 2; element 1 of list2 is 5
List.iteri2: element 2 of list1 is 3; element 2 of list2 is 6
```

Outra função usada com frequência que transforma os elementos da lista é [list. map](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#map), que permite que você aplique uma função a cada elemento de uma lista e coloque todos os resultados em uma nova lista. [List. map2](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#map2) e [list. map3](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#map3) são variações que usam várias listas. Você também pode usar [list. MAPI](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#mapi) e [list. mapi2](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#mapi2), se, além do elemento, a função precisar passar o índice de cada elemento. A única diferença entre `List.mapi2` e `List.mapi` é que `List.mapi2` funciona com as duas listas. O exemplo a seguir ilustra [list. map](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#map).

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet18.fs)]

A saída é da seguinte maneira:

```console
[2; 3; 4]
```

O exemplo a seguir mostra o uso de `List.map2`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet19.fs)]

A saída é da seguinte maneira:

```console
[5; 7; 9]
```

O exemplo a seguir mostra o uso de `List.map3`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet20.fs)]

A saída é da seguinte maneira:

```console
[7; 10; 13]
```

O exemplo a seguir mostra o uso de `List.mapi`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet21.fs)]

A saída é da seguinte maneira:

```console
[1; 3; 5]
```

O exemplo a seguir mostra o uso de `List.mapi2`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet22.fs)]

A saída é da seguinte maneira:

```console
[0; 7; 18]
```

[List. Collect](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#collect) é como `List.map` , exceto que cada elemento produz uma lista e todas essas listas são concatenadas em uma lista final. No código a seguir, cada elemento da lista gera três números. Eles são todos coletados em uma única lista.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet23.fs)]

A saída é da seguinte maneira:

```console
[1; 2; 3; 2; 4; 6; 3; 6; 9]
```

Você também pode usar [list. Filter](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#filter), que usa uma condição booliana e produz uma nova lista que consiste apenas em elementos que atendem à condição fornecida.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet24.fs)]

A lista resultante é `[2; 4; 6]`.

Uma combinação de mapear e filtrar, [lista. escolha](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#choose) permite que você transforme e selecione elementos ao mesmo tempo. `List.choose` aplica uma função que retorna uma opção para cada elemento de uma lista e retorna uma nova lista de resultados para os elementos quando a função retorna o valor de opção `Some`.

O código a seguir demonstra o uso de `List.choose` para selecionar palavras em maiúsculas a partir de uma lista de palavras.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet25.fs)]

A saída é da seguinte maneira:

```console
["Rome's"; "Bob's"]
```

### <a name="operating-on-multiple-lists"></a>Operando em várias listas

As listas podem ser reunidas. Para unir duas listas em uma, use [list. Append](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#append). Para unir mais de duas listas, use [list. Concat](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#concat).

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet26.fs)]

### <a name="fold-and-scan-operations"></a>Operações de dobra e digitalização

Algumas operações de lista envolvem interdependências entre todos os elementos da lista. As operações de dobra e verificação são como `List.iter` e, `List.map` em que você invoca uma função em cada elemento, mas essas operações fornecem um parâmetro adicional chamado de *acumulador* que transporta informações por meio da computação.

Use `List.fold` para executar um cálculo em uma lista.

O exemplo de código a seguir demonstra o uso de [list. Dobre](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#fold) para executar várias operações.

A lista é percorrida; o acumulador `acc` é um valor passado conforme ocorre o progresso do cálculo. O primeiro argumento usa o acumulador e o elemento da lista, retornando o resultado intermediário do cálculo para esse elemento da lista. O segundo argumento é o valor inicial do acumulador.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet27.fs)]

As versões dessas funções que têm um dígito no nome da função operam em mais de uma lista. Por exemplo, [list. fold2](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#fold2) executa cálculos em duas listas.

O exemplo a seguir demonstra o uso de `List.fold2`.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet28.fs)]

`List.fold` e [list. scan](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#scan) é diferente em que `List.fold` retorna o valor final do parâmetro extra, mas `List.scan` retorna a lista dos valores intermediários (juntamente com o valor final) do parâmetro extra.

Cada uma dessas funções inclui uma variação inversa, por exemplo, [list. foldBack](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#foldBack), que difere na ordem em que a lista é atravessada e a ordem dos argumentos. Além disso, `List.fold` e `List.foldBack` têm variações, [list. fold2](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#fold2) e [list. foldBack2](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#foldBack2), que usam duas listas de comprimento igual. A função executada em cada elemento pode usar elementos correspondentes das duas listas para realizar alguma ação. Os tipos de elementos das duas listas podem ser diferentes, como no exemplo a seguir, em que uma lista contém valores de transação de uma conta bancária e a outra lista contém o tipo de transação: depósito ou retirada.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet29.fs)]

Para um cálculo como somatória, `List.fold` e `List.foldBack` têm o mesmo efeito, porque o resultado não depende da ordem de passagem. No exemplo a seguir, `List.foldBack` é usado para adicionar os elementos em uma lista.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet30.fs)]

O exemplo a seguir retorna ao exemplo da conta bancária. Desta vez, um novo tipo de transação é adicionado: cálculo de juros. O saldo final agora depende da ordem das transações.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet34.fs)]

A lista de funções [. reduzir](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#reduce) é de certa forma `List.fold` e `List.scan` , exceto que, em vez de passar em um acumulador separado, `List.reduce` usa uma função que usa dois argumentos do tipo de elemento em vez de apenas um, e um desses argumentos atua como o acumulador, o que significa que ele armazena o resultado intermediário da computação. `List.reduce` começa a operar nos dois primeiros elementos da lista e, em seguida, usa o resultado da operação juntamente com o próximo elemento. Como não há um acumulador separado que tem o seu próprio tipo, `List.reduce` pode ser usado no lugar de `List.fold` somente quando o acumulador e o tipo de elemento têm o mesmo tipo. O código a seguir demonstra o uso de `List.reduce`. `List.reduce` lançará uma exceção se a lista fornecida não tiver elementos.

No código a seguir, a primeira chamada para a expressão lambda recebeu os argumentos 2 e 4, retornando 6 e a chamada seguinte recebeu os argumentos 6 e 10, de modo que o resultado é 16.

[!code-fsharp[Main](~/samples/snippets/fsharp/lists/snippet33.fs)]

### <a name="converting-between-lists-and-other-collection-types"></a>Convertendo entre listas e outros tipos de coleção

O módulo `List` fornece funções para converter de e para sequências e matrizes. Para converter de ou para uma sequência, use [list. toSeq](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#toSeq) ou [list. ofSeq](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#ofSeq). Para converter de ou para uma matriz, use [list. toArray](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#toArray) ou [list. ofArray](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#ofArray).

### <a name="additional-operations"></a>Operações adicionais

Para obter informações sobre operações adicionais em listas, consulte o [módulo lista](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html)de tópicos de referência de biblioteca.

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
- [Tipos F#](fsharp-types.md)
- [Sequências](sequences.md)
- [matrizes](arrays.md)
- [Opções](options.md)
