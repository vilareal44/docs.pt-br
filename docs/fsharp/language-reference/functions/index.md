---
title: Funções
description: 'Saiba mais sobre o Functions em F # e como o F # dá suporte a construções de programação funcional comuns.'
ms.date: 05/16/2016
ms.openlocfilehash: e49183e0634dee1750757abadbfe9e9c824f51a8
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596468"
---
# <a name="functions"></a>Funções

As funções são a unidade fundamental de execução do programa em qualquer linguagem de programação. Como em outras linguagens, uma função do F# tem um nome, pode ter parâmetros e receber argumentos, e tem um corpo. O F# também oferece suporte a construções de programação funcional como tratamento de funções como valores, uso de funções sem nome em expressões, composição de funções para formar novas funções, funções via currying e a definição implícita de funções por meio da aplicação parcial dos argumentos da função.

Defina funções usando a palavra-chave `let`, ou, se a função for recursiva, a combinação de palavras-chave `let rec`.

## <a name="syntax"></a>Sintaxe

```fsharp
// Non-recursive function definition.
let [inline] function-name parameter-list [ : return-type ] = function-body
// Recursive function definition.
let rec function-name parameter-list = recursive-function-body
```

## <a name="remarks"></a>Comentários

*function-name* é um identificador que representa a função. *parameter-list* é formado por parâmetros sucessivos separados por espaços. Você pode especificar um tipo explícito para cada parâmetro, conforme descrito na seção Parâmetros. Se você não especificar um tipo de argumento específico, o compilador tentará inferir o tipo do corpo da função. *function-body* é formado por uma expressão. A expressão que constitui o corpo da função normalmente é uma expressão composta formada por diversas expressões que culminam em uma expressão final, que é o valor de retorno. *return-type* é um sinal de dois-pontos seguido por um tipo, e é opcional. Se você não especificar explicitamente o tipo do valor de retorno, o compilador determinará o tipo de retorno da expressão final.

Uma definição de função simples é semelhante à seguinte:

```fsharp
let f x = x + 1
```

No exemplo anterior, o nome da função é `f`, o argumento é `x`, que tem o tipo `int`, o corpo da função é `x + 1` e o valor de retorno é do tipo `int`.

As funções podem ser marcadas como `inline`. Para saber mais sobre `inline`, veja [Funções embutidas](inline-functions.md).

## <a name="scope"></a>Escopo

Em qualquer nível de escopo diferente do escopo do módulo, não é errado reutilizar um nome de função ou valor. Se você reutilizar um nome, o nome declarado posteriormente cobrirá o nome declarado anteriormente. No entanto, no escopo de nível superior em um módulo, os nomes devem ser exclusivos. Por exemplo, o código a seguir gera um erro quando é exibido no escopo do módulo, mas não quando aparece dentro de uma função:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet101.fs)]

Mas o código a seguir é aceitável em qualquer nível do escopo:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet102.fs)]

### <a name="parameters"></a>Parâmetros

Os nomes de parâmetros são listados após o nome da função. Você pode especificar um tipo para um parâmetro, conforme mostra o exemplo a seguir:

```fsharp
let f (x : int) = x + 1
```

Se você especificar um tipo, ele virá após o nome do parâmetro e será separado do nome por dois-pontos. Se você omitir o tipo do parâmetro, o tipo de parâmetro será inferido pelo compilador. Por exemplo, na definição de função a seguir, o argumento `x` é inferido como sendo do tipo `int`, pois 1 é do tipo `int`.

```fsharp
let f x = x + 1
```

No entanto, o compilador tentará tornar a função a mais genérica possível. Por exemplo, observe o código a seguir:

```fsharp
let f x = (x, x)
```

A função cria uma tupla de um argumento de qualquer tipo. Como o tipo não foi especificado, a função pode ser usada com qualquer tipo de argumento. Para saber mais, veja [Generalização automática](../generics/automatic-generalization.md).

## <a name="function-bodies"></a>Corpos de Função

O corpo de uma função pode conter definições de funções e variáveis locais. Essas variáveis e funções estão no escopo no corpo da função atual, mas não fora dela. Quando a opção de sintaxe leve está habilitada, é necessário usar o recuo para indicar que uma definição está em um corpo de função, conforme mostra o exemplo a seguir:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet103.fs)]

Para saber mais, veja [Diretrizes de formatação de código](../../style-guide/formatting.md) e [Sintaxe detalhada](../verbose-syntax.md).

## <a name="return-values"></a>Valores de retorno

O compilador usa a expressão final em um corpo de função para determinar o valor de retorno e o tipo. O compilador pode inferir o tipo da expressão final a partir das expressões anteriores. Na função `cylinderVolume`, mostrada na seção anterior, o tipo de `pi` é determinado pelo tipo do literal `3.14159` como `float`. O compilador usa o tipo de `pi` para determinar o tipo da expressão `h * pi * r * r` como `float`. Portanto, o tipo de retorno geral da função é `float`.

Para especificar explicitamente o valor de retorno, escreva o código da seguinte maneira:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet105.fs)]

À medida que o código é escrito acima, o compilador aplica **float** à função inteira; se você também pretende aplicá-lo aos tipos de parâmetro, use o código a seguir:

```fsharp
let cylinderVolume (radius : float) (length : float) : float
```

## <a name="calling-a-function"></a>Chamando uma Função

Você pode chamar funções especificando o nome da função seguido por um espaço e por quaisquer argumentos, separados por espaços. Por exemplo, para chamar a função **cylinderVolume** e atribuir o resultado para o valor **vol**, escreva o código a seguir:

```fsharp
let vol = cylinderVolume 2.0 3.0
```

## <a name="partial-application-of-arguments"></a>Aplicativo de Argumentos Parcial

Se você fornecer uma quantidade inferior ao número especificado de argumentos, criará uma nova função que esperará os argumentos restantes. Esse método de manipulação de argumentos é conhecido como *currying*, e é uma característica de linguagens de programação funcionais como F#. Por exemplo, vamos supor que você esteja trabalhando com dois tamanhos de pipe: uma com um raio de **2.0** e outro com um raio de **3.0**. Você poderia criar funções que determinam o volume do pipe da seguinte maneira:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet106.fs)]

Em seguida, você forneceria o argumento adicional, conforme o necessário, para várias durações de pipe dos dois tamanhos diferentes:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet107.fs)]

## <a name="recursive-functions"></a>Funções Recursivas

As *funções recursivas* são funções que chamam a si próprias. Eles exigem que você especifique a palavra-chave **rec** logo após a palavra-chave **let**. Invoque a função recursiva de dentro do corpo da função, exatamente como você invocaria qualquer chamada de função. A função recursiva a seguir computa o número *n*<sup>ésimo</sup> Fibonacci. A sequência numérica de Fibonacci é conhecida desde a antiguidade e é uma sequência na qual cada número sucessivo é a soma dos dois números anteriores na sequência.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet108.fs)]

Algumas funções recursivas podem estourar a pilha do programa ou executar de maneira ineficiente se não forem escritas com cuidado e com conhecimento de técnicas especiais, como o uso de acumuladores e continuações.

## <a name="function-values"></a>Valores de Função

Em F#, todas as funções são consideradas valores; na verdade, elas são conhecidas como *valores de função*. Como as funções são valores, elas podem ser usadas como argumentos de outras funções ou em outros contextos nos quais os valores são usados. Veja a seguir um exemplo de uma função que usa um valor de função como argumento:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet109.fs)]

Especifique o tipo de valor de uma função usando o token `->`. À esquerda desse token está o tipo do argumento, e à direita está o valor de retorno. No exemplo anterior, `apply1` é uma função que use uma função `transform` como um argumento, em que `transform` é uma função que usa um inteiro e retorna outro inteiro. O código a seguir mostra como usar `apply1`:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet110.fs)]

O valor de `result` será 101 após a execução do código anterior.

Vários argumentos são separados por tokens `->` sucessivos, conforme mostra o exemplo a seguir:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet111.fs)]

O resultado é 200.

## <a name="lambda-expressions"></a>Expressões lambda

Uma *expressão lambda* é uma função sem nome. Nos exemplos anteriores, em vez de definir as funções nomeadas **increment** e **mul**, use expressões lambda, da seguinte maneira:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet112.fs)]

Defina expressões lambda usando a palavra-chave `fun`. Uma expressão lambda é semelhante a uma definição de função, com exceção de que em vez do token `=`, o token `->` é usado para separar a lista de argumentos do corpo da função. Assim como em uma definição de função normal, os tipos de argumento podem ser inferidos ou explicitamente especificados, e o tipo de retorno da expressão lambda é inferido do tipo da última expressão no corpo. Para saber mais, veja [Expressões lambda: a palavra-chave `fun`](lambda-expressions-the-fun-keyword.md).

## <a name="function-composition-and-pipelining"></a>Composição de Função e Pipelining

As funções em F# podem ser compostas de outras funções. A composição de duas funções **função1** e **função2** é outra função que representa a aplicação da **função1** seguida pela aplicação da **função2**:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet113.fs)]

O resultado é 202.

O pipelining permite o encadeamento de chamadas de função como operações sucessivas. O pipelining funciona da seguinte maneira:

```fsharp
let result = 100 |> function1 |> function2
```

O resultado é 202 novamente.

Os operadores de composição usam duas funções e retornam uma função; por outro lado, os operadores de pipeline usam uma função e um argumento e retornam um valor. O exemplo de código a seguir mostra a diferença entre os operadores de pipeline e de composição, mostrando as diferenças no uso e nas assinaturas da função.

```fsharp
// Function composition and pipeline operators compared.

let addOne x = x + 1
let timesTwo x = 2 * x

// Composition operator
// ( >> ) : ('T1 -> 'T2) -> ('T2 -> 'T3) -> 'T1 -> 'T3
let Compose2 = addOne >> timesTwo

// Backward composition operator
// ( << ) : ('T2 -> 'T3) -> ('T1 -> 'T2) -> 'T1 -> 'T3
let Compose1 = addOne << timesTwo

// Result is 5
let result1 = Compose1 2

// Result is 6
let result2 = Compose2 2

// Pipelining
// Pipeline operator
// ( |> ) : 'T1 -> ('T1 -> 'U) -> 'U
let Pipeline2 x = addOne x |> timesTwo

// Backward pipeline operator
// ( <| ) : ('T -> 'U) -> 'T -> 'U
let Pipeline1 x = addOne <| timesTwo x

// Result is 5
let result3 = Pipeline1 2

// Result is 6
let result4 = Pipeline2 2
```

## <a name="overloading-functions"></a>Sobrecarregando Funções

Você pode sobrecarregar métodos de um tipo, mas não funções. Para saber mais, veja [Métodos](../members/methods.md).

## <a name="see-also"></a>Consulte também

- [Valores](../values/index.md)
- [Referência de linguagem F #](../index.md)
