---
title: Diretrizes de formatação de código do F#
description: 'Aprenda as diretrizes para formatar o código F #.'
ms.date: 11/04/2019
ms.openlocfilehash: fe8da6070e1c92bb5205e9cb408b8ac75372b061
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558303"
---
# <a name="f-code-formatting-guidelines"></a>Diretrizes de formatação de código do F#

Este artigo oferece diretrizes sobre como formatar seu código para que o código F # seja:

* Mais legível
* De acordo com as convenções aplicadas pelas ferramentas de formatação no Visual Studio e em outros editores
* Semelhante a outro código online

Essas diretrizes se baseiam em [um guia abrangente para as convenções de formatação F #](https://github.com/dungpa/fantomas/blob/master/docs/FormattingConventions.md) por [Anh-Dung Phan](https://github.com/dungpa).

## <a name="general-rules-for-indentation"></a>Regras gerais para recuo

O F # usa um espaço em branco significativo por padrão. As diretrizes a seguir destinam-se a fornecer orientações sobre como manipular alguns desafios que isso pode impor.

### <a name="using-spaces"></a>Usando espaços

Quando o recuo é necessário, você deve usar espaços, não tabulações. Pelo menos um espaço é necessário. Sua organização pode criar padrões de codificação para especificar o número de espaços a serem usados para recuo; dois, três ou quatro espaços de recuo em cada nível em que o recuo ocorre é típico.

**Recomendamos quatro espaços por recuo.**

Dito isso, o recuo de programas é uma questão subjetiva. As variações estão OK, mas a primeira regra que você deve seguir é a *consistência do recuo*. Escolha um estilo de recuo em geral aceito e use-o sistematicamente em toda a base de código.

## <a name="formatting-white-space"></a>Formatando espaço em branco

F # diferencia espaço em branco. Embora a maioria das semânticas de espaço em branco seja coberta por um recuo adequado, há algumas outras coisas a serem consideradas.

### <a name="formatting-operators-in-arithmetic-expressions"></a>Formatando operadores em expressões aritméticas

Sempre use espaço em branco em relação a expressões aritméticas binárias:

```fsharp
let subtractThenAdd x = x - 1 + 3
```

Os operadores unários `-` sempre devem ser seguidos imediatamente pelo valor que estão negando:

```fsharp
// OK
let negate x = -x

// Bad
let negateBad x = - x
```

Adicionar um caractere de espaço em branco após o `-` operador pode levar à confusão para outras pessoas.

Em resumo, é importante sempre:

* Operadores binários surround com espaço em branco
* Nunca ter espaço em branco à direita após um operador unário

A diretriz de operador aritmético binária é especialmente importante. Falha ao envolver um `-` operador binário, quando combinado com determinadas opções de formatação, pode levar à interpretação dele como um unário `-` .

### <a name="surround-a-custom-operator-definition-with-white-space"></a>Cerca de uma definição de operador personalizada com espaço em branco

Sempre use o espaço em branco para envolver uma definição de operador:

```fsharp
// OK
let ( !> ) x f = f x

// Bad
let (!>) x f = f x
```

Para qualquer operador personalizado que comece com `*` e que tenha mais de um caractere, você precisará adicionar um espaço em branco ao início da definição para evitar uma ambiguidade do compilador. Por isso, é recomendável que você simplesmente coloque as definições de todos os operadores com um único caractere de espaço em branco.

### <a name="surround-function-parameter-arrows-with-white-space"></a>Setas de parâmetro de função surround com espaço em branco

Ao definir a assinatura de uma função, use espaço em branco em volta do `->` símbolo:

```fsharp
// OK
type MyFun = int -> int -> string

// Bad
type MyFunBad = int->int->string
```

### <a name="surround-function-arguments-with-white-space"></a>Argumentos da função surround com espaço em branco

Ao definir uma função, use o espaço em branco em volta de cada argumento.

```fsharp
// OK
let myFun (a: decimal) b c = a + b + c

// Bad
let myFunBad (a:decimal)(b)c = a + b + c
```

### <a name="place-parameters-on-a-new-line-for-long-definitions"></a>Coloque os parâmetros em uma nova linha para definições longas

Se você tiver uma definição de função muito longa, coloque os parâmetros em novas linhas e recue-os para corresponder ao nível de recuo do parâmetro subsequente.

```fsharp
module M =
    let LongFunctionWithLotsOfParameters(aVeryLongParam: AVeryLongTypeThatYouNeedToUse)
                                        (aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse)
                                        (aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse)
                                        =
        // ... the body of the method follows
```

Isso também se aplica a membros, construtores e parâmetros usando tuplas:

```fsharp
type TM() =
    member _.LongMethodWithLotsOfParameters(aVeryLongParam: AVeryLongTypeThatYouNeedToUse,
                                            aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse,
                                            aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse) =
        // ... the body of the method

type TC(aVeryLongCtorParam: AVeryLongTypeThatYouNeedToUse,
        aSecondVeryLongCtorParam: AVeryLongTypeThatYouNeedToUse,
        aThirdVeryLongCtorParam: AVeryLongTypeThatYouNeedToUse) =
    // ... the body of the class follows
```

Se os parâmetros forem currified ou houver uma anotação explícita de tipo de retorno, é preferível posicionar o `=` caractere em uma nova linha:

```fsharp
type C() =
    member _.LongMethodWithLotsOfParameters(aVeryLongParam: AVeryLongTypeThatYouNeedToUse,
                                            aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse,
                                            aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse)
                                            : AReturnType =
        // ... the body of the method
    member _.LongMethodWithLotsOfCurrifiedParams(aVeryLongParam: AVeryLongTypeThatYouNeedToUse)
                                                (aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse)
                                                (aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse)
                                                =
        // ... the body of the method
```

Essa é uma maneira de evitar linhas muito longas (caso o tipo de retorno possa ter um nome longo) e ter menos danos na linha ao adicionar parâmetros.

### <a name="type-annotations"></a>Anotações de tipo

#### <a name="right-pad-function-argument-type-annotations"></a>Anotações do tipo de argumento da função de preenchimento direto

Ao definir argumentos com anotações de tipo, use o espaço em branco após o `:` símbolo:

```fsharp
// OK
let complexFunction (a: int) (b: int) c = a + b + c

// Bad
let complexFunctionBad (a :int) (b :int) (c:int) = a + b + c
```

#### <a name="surround-return-type-annotations-with-white-space"></a>Anotações de tipo de retorno surround com espaço em branco

Em uma anotação de tipo de valor ou função que permite associação (tipo de retorno no caso de uma função), use espaço em branco antes e depois do `:` símbolo:

```fsharp
// OK
let expensiveToCompute : int = 0 // Type annotation for let-bound value
let myFun (a: decimal) b c : decimal = a + b + c // Type annotation for the return type of a function
// Bad
let expensiveToComputeBad1:int = 1
let expensiveToComputeBad2 :int = 2
let myFunBad (a: decimal) b c:decimal = a + b + c
```

## <a name="formatting-blank-lines"></a>Formatando linhas em branco

* Separe as definições de função e classe de nível superior com duas linhas em branco.
* As definições de método dentro de uma classe são separadas por uma única linha em branco.
* Linhas em branco extras podem ser usadas (com moderação) para separar grupos de funções relacionadas. As linhas em branco podem ser omitidas entre uma porção de uma linha única relacionada (por exemplo, um conjunto de implementações fictícias).
* Use linhas em branco em funções, com moderação, para indicar seções lógicas.

## <a name="formatting-comments"></a>Formatando comentários

Geralmente, prefira vários comentários de barra dupla sobre comentários de bloco no estilo ML.

```fsharp
// Prefer this style of comments when you want
// to express written ideas on multiple lines.

(*
    ML-style comments are fine, but not a .NET-ism.
    They are useful when needing to modify multi-line comments, though.
*)
```

Os comentários embutidos devem colocar a primeira letra em maiúscula.

```fsharp
let f x = x + 1 // Increment by one.
```

## <a name="naming-conventions"></a>Convenções de nomenclatura

### <a name="use-camelcase-for-class-bound-expression-bound-and-pattern-bound-values-and-functions"></a>Usar camelCase para valores e funções associados à classe, associados à expressão e ao padrão

É comum e aceito o estilo F # para usar camelCase para todos os nomes associados como variáveis locais ou em correspondências de padrão e definições de função.

```fsharp
// OK
let addIAndJ i j = i + j

// Bad
let addIAndJ I J = I+J

// Bad
let AddIAndJ i j = i + j
```

As funções associadas localmente nas classes também devem usar camelCase.

```fsharp
type MyClass() =

    let doSomething () =

    let firstResult = ...

    let secondResult = ...

    member x.Result = doSomething()
```

### <a name="use-camelcase-for-module-bound-public-functions"></a>Usar camelCase para funções públicas associadas a módulo

Quando uma função associada a um módulo faz parte de uma API pública, ela deve usar camelCase:

```fsharp
module MyAPI =
    let publicFunctionOne param1 param2 param2 = ...

    let publicFunctionTwo param1 param2 param3 = ...
```

### <a name="use-camelcase-for-internal-and-private-module-bound-values-and-functions"></a>Usar camelCase para valores e funções associados ao módulo interno e privado

Use camelCase para valores associados ao módulo privado, incluindo o seguinte:

* Funções ad hoc em scripts

* Valores que constituem a implementação interna de um módulo ou tipo

```fsharp
let emailMyBossTheLatestResults =
    ...
```

### <a name="use-camelcase-for-parameters"></a>Usar camelCase para parâmetros

Todos os parâmetros devem usar camelCase de acordo com as convenções de nomenclatura do .NET.

```fsharp
module MyModule =
    let myFunction paramOne paramTwo = ...

type MyClass() =
    member this.MyMethod(paramOne, paramTwo) = ...
```

### <a name="use-pascalcase-for-modules"></a>Usar PascalCase para módulos

Todos os módulos (nível superior, interno, privado, aninhado) devem usar PascalCase.

```fsharp
module MyTopLevelModule

module Helpers =
    module private SuperHelpers =
        ...

    ...
```

### <a name="use-pascalcase-for-type-declarations-members-and-labels"></a>Usar PascalCase para declarações de tipo, membros e rótulos

Classes, interfaces, structs, enumerações, delegados, registros e uniões discriminadas devem ser nomeadas com PascalCase. Membros dentro de tipos e rótulos para registros e uniões discriminadas também devem usar PascalCase.

```fsharp
type IMyInterface =
    abstract Something: int

type MyClass() =
    member this.MyMethod(x, y) = x + y

type MyRecord = { IntVal: int; StringVal: string }

type SchoolPerson =
    | Professor
    | Student
    | Advisor
    | Administrator
```

### <a name="use-pascalcase-for-constructs-intrinsic-to-net"></a>Usar PascalCase para construções intrínsecas ao .NET

Namespaces, exceções, eventos e projeto/ `.dll` nomes também devem usar PascalCase. Isso não apenas faz com que o consumo de outras linguagens .NET pareça mais natural para os consumidores, também é consistente com as convenções de nomenclatura do .NET que você provavelmente encontrará.

### <a name="avoid-underscores-in-names"></a>Evitar sublinhados em nomes

Historicamente, algumas bibliotecas F # usaram sublinhados em nomes. No entanto, isso não é mais amplamente aceito, parcialmente porque ele conflita com as convenções de nomenclatura do .NET. Dito isso, alguns programadores de F # usam sublinhados intensamente, parcialmente por motivos históricos, e a tolerância e o respeito são importantes. No entanto, lembre-se de que o estilo geralmente é desgosto por outros que têm a opção de usá-lo.

Uma exceção inclui a interoperação com componentes nativos, onde os sublinhados são comuns.

### <a name="use-standard-f-operators"></a>Usar operadores F # padrão

Os operadores a seguir são definidos na biblioteca padrão F # e devem ser usados em vez de definir equivalentes. O uso desses operadores é recomendado, pois tende a tornar o código mais legível e idiomática. Os desenvolvedores com uma experiência em OCaml ou outra linguagem de programação funcional podem estar acostumados a linguagens diferentes. A lista a seguir resume os operadores F # recomendados.

```fsharp
x |> f // Forward pipeline
f >> g // Forward composition
x |> ignore // Discard away a value
x + y // Overloaded addition (including string concatenation)
x - y // Overloaded subtraction
x * y // Overloaded multiplication
x / y // Overloaded division
x % y // Overloaded modulus
x && y // Lazy/short-cut "and"
x || y // Lazy/short-cut "or"
x <<< y // Bitwise left shift
x >>> y // Bitwise right shift
x ||| y // Bitwise or, also for working with “flags” enumeration
x &&& y // Bitwise and, also for working with “flags” enumeration
x ^^^ y // Bitwise xor, also for working with “flags” enumeration
```

### <a name="use-prefix-syntax-for-generics-foot-in-preference-to-postfix-syntax-t-foo"></a>Use a sintaxe de prefixo para genéricos ( `Foo<T>` ) em preferência à sintaxe de sufixo ( `T Foo` )

F # herda o estilo de am do sufixo de nomenclatura de tipos genéricos (por exemplo, `int list` ), bem como o prefixo do estilo .net (por exemplo, `list<int>` ). Prefira o estilo .NET, exceto para cinco tipos específicos:

1. Para listas F #, use o formulário sufixo: `int list` em vez de `list<int>` .
2. Para opções de F #, use o formulário de sufixo: `int option` em vez de `option<int>` .
3. Para opções de valor F #, use o formulário sufixo: `int voption` em vez de `voption<int>` .
4. Para matrizes F #, use o nome sintático `int[]` em vez de `int array` ou `array<int>` .
5. Para células de referência, use `int ref` em vez de `ref<int>` ou `Ref<int>` .

Para todos os outros tipos, use o formulário de prefixo.

## <a name="formatting-tuples"></a>Formatando tuplas

Uma instanciação de tupla deve estar entre parênteses e as vírgulas de delimitação dentro dela devem ser seguidas por um único espaço, por exemplo: `(1, 2)` , `(x, y, z)` .

Normalmente, é aceito para omitir parênteses em correspondência de padrões de tuplas:

```fsharp
let (x, y) = z // Destructuring
let x, y = z // OK

// OK
match x, y with
| 1, _ -> 0
| x, 1 -> 0
| x, y -> 1
```

Ele também é aceito para omitir parênteses se a tupla for o valor de retorno de uma função:

```fsharp
// OK
let update model msg =
    match msg with
    | 1 -> model + 1, []
    | _ -> model, [ msg ]
```

Em resumo, prefira instanciações de tupla entre parênteses, mas ao usar tuplas para correspondência de padrões ou um valor de retorno, ele é considerado bem para evitar parênteses.

## <a name="formatting-discriminated-union-declarations"></a>Formatando declarações de União discriminadas

Recuar `|` na definição de tipo por quatro espaços:

```fsharp
// OK
type Volume =
    | Liter of float
    | FluidOunce of float
    | ImperialPint of float

// Not OK
type Volume =
| Liter of float
| USPint of float
| ImperialPint of float
```

## <a name="formatting-discriminated-unions"></a>Formatando uniões discriminadas

As uniões com instâncias discriminadas que se dividem em várias linhas devem dar aos dados contidos um novo escopo com recuo:

```fsharp
let tree1 =
    BinaryNode
        (BinaryNode(BinaryValue 1, BinaryValue 2),
         BinaryNode(BinaryValue 3, BinaryValue 4))
```

O parêntese de fechamento também pode estar em uma nova linha:

```fsharp
let tree1 =
    BinaryNode(
        BinaryNode(BinaryValue 1, BinaryValue 2),
        BinaryNode(BinaryValue 3, BinaryValue 4)
    )
```

## <a name="formatting-record-declarations"></a>Formatando declarações de registro

Recue `{` na definição de tipo por quatro espaços e inicie a lista de campos na mesma linha:

```fsharp
// OK
type PostalAddress =
    { Address: string
      City: string
      Zip: string }
    member x.ZipAndCity = sprintf "%s %s" x.Zip x.City

// Not OK
type PostalAddress =
  { Address: string
    City: string
    Zip: string }
    member x.ZipAndCity = sprintf "%s %s" x.Zip x.City

// Unusual in F#
type PostalAddress =
    {
        Address: string
        City: string
        Zip: string
    }
```

Colocar o token de abertura em uma nova linha e o token de fechamento em uma nova linha é preferível se você estiver declarando implementações de interface ou membros no registro:

```fsharp
// Declaring additional members on PostalAddress
type PostalAddress =
    {
        Address: string
        City: string
        Zip: string
    }
    member x.ZipAndCity = sprintf "%s %s" x.Zip x.City

type MyRecord =
    {
        SomeField: int
    }
    interface IMyInterface
```

## <a name="formatting-records"></a>Formatando registros

Registros curtos podem ser gravados em uma linha:

```fsharp
let point = { X = 1.0; Y = 0.0 }
```

Os registros que são mais longos devem usar linhas novas para rótulos:

```fsharp
let rainbow =
    { Boss = "Jeffrey"
      Lackeys = ["Zippy"; "George"; "Bungle"] }
```

Colocar o token de abertura em uma nova linha, o conteúdo tabulado em um escopo e o token de fechamento em uma nova linha será preferível se você:

* Movendo registros em código com escopos de recuo diferentes
* Canalizando-os para uma função

```fsharp
let rainbow =
    {
        Boss1 = "Jeffrey"
        Boss2 = "Jeffrey"
        Boss3 = "Jeffrey"
        Boss4 = "Jeffrey"
        Boss5 = "Jeffrey"
        Boss6 = "Jeffrey"
        Boss7 = "Jeffrey"
        Boss8 = "Jeffrey"
        Lackeys = ["Zippy"; "George"; "Bungle"]
    }

type MyRecord =
    {
        SomeField: int
    }
    interface IMyInterface

let foo a =
    a
    |> Option.map (fun x ->
        {
            MyField = x
        })
```

As mesmas regras se aplicam a elementos de lista e matriz.

## <a name="formatting-copy-and-update-record-expressions"></a>Formatando expressões de registro de copiar e atualizar

Uma expressão de registro Copy-and-Update ainda é um registro, portanto, diretrizes semelhantes se aplicam.

As expressões curtas podem caber em uma linha:

```fsharp
let point2 = { point with X = 1; Y = 2 }
```

As expressões mais longas devem usar novas linhas:

```fsharp
let rainbow2 =
    { rainbow with
        Boss = "Jeffrey"
        Lackeys = [ "Zippy"; "George"; "Bungle" ] }
```

E, assim como nas diretrizes de registro, talvez você queira dedicar linhas separadas para as chaves e recuar um escopo para a direita com a expressão. Em alguns casos especiais, como encapsular um valor com um opcional sem parênteses, talvez seja necessário manter uma chave em uma linha:

```fsharp
type S = { F1: int; F2: string }
type State = { F:  S option }

let state = { F = Some { F1 = 1; F2 = "Hello" } }
let newState =
    {
        state with
            F = Some {
                    F1 = 0
                    F2 = ""
                }
    }
```

## <a name="formatting-lists-and-arrays"></a>Formatando listas e matrizes

Escreva `x :: l` com espaços em volta do `::` operador ( `::` é um operador de infixo, portanto, circundado por espaços).

Lista e matrizes declaradas em uma única linha devem ter um espaço após o colchete de abertura e antes do colchete de fechamento:

```fsharp
let xs = [ 1; 2; 3 ]
let ys = [| 1; 2; 3; |]
```

Sempre use pelo menos um espaço entre dois operadores de chaves distintas. Por exemplo, deixe um espaço entre um `[` e um `{` .

```fsharp
// OK
[ { IngredientName = "Green beans"; Quantity = 250 }
  { IngredientName = "Pine nuts"; Quantity = 250 }
  { IngredientName = "Feta cheese"; Quantity = 250 }
  { IngredientName = "Olive oil"; Quantity = 10 }
  { IngredientName = "Lemon"; Quantity = 1 } ]

// Not OK
[{ IngredientName = "Green beans"; Quantity = 250 }
 { IngredientName = "Pine nuts"; Quantity = 250 }
 { IngredientName = "Feta cheese"; Quantity = 250 }
 { IngredientName = "Olive oil"; Quantity = 10 }
 { IngredientName = "Lemon"; Quantity = 1 }]
```

A mesma diretriz se aplica a listas ou matrizes de tuplas.

Listas e matrizes que dividem em várias linhas seguem uma regra semelhante à medida que os registros fazem:

```fsharp
let pascalsTriangle =
    [|
        [| 1 |]
        [| 1; 1 |]
        [| 1; 2; 1 |]
        [| 1; 3; 3; 1 |]
        [| 1; 4; 6; 4; 1 |]
        [| 1; 5; 10; 10; 5; 1 |]
        [| 1; 6; 15; 20; 15; 6; 1 |]
        [| 1; 7; 21; 35; 35; 21; 7; 1 |]
        [| 1; 8; 28; 56; 70; 56; 28; 8; 1 |]
    |]
```

E, assim como acontece com os registros, a declaração dos colchetes de abertura e fechamento em sua própria linha fará com que a movimentação de código e a tubulação em funções sejam mais fáceis.

Ao gerar matrizes e listas programaticamente, prefira `->` `do ... yield` quando um valor for sempre gerado:

```fsharp
// Preferred
let squares = [ for x in 1..10 -> x * x ]

// Not preferred
let squares' = [ for x in 1..10 do yield x * x ]
```

Versões mais antigas da linguagem F # exigia especificar `yield` em situações em que os dados podem ser gerados condicionalmente ou pode haver expressões consecutivas a serem avaliadas. Prefira omitir essas `yield` palavras-chave, a menos que você precise compilar com uma versão de linguagem F # mais antiga:

```fsharp
// Preferred
let daysOfWeek includeWeekend =
    [
        "Monday"
        "Tuesday"
        "Wednesday"
        "Thursday"
        "Friday"
        if includeWeekend then
            "Saturday"
            "Sunday"
    ]

// Not preferred
let daysOfWeek' includeWeekend =
    [
        yield "Monday"
        yield "Tuesday"
        yield "Wednesday"
        yield "Thursday"
        yield "Friday"
        if includeWeekend then
            yield "Saturday"
            yield "Sunday"
    ]
```

Em alguns casos, o `do...yield` pode ajudar na legibilidade. Esses casos, embora subjetivo, devem ser levados em consideração.

## <a name="formatting-if-expressions"></a>Formatando expressões If

O recuo de condicionais depende dos tamanhos das expressões que as compõem. Se `cond` `e1` e `e2` forem curtos, basta escrevê-los em uma linha:

```fsharp
if cond then e1 else e2
```

Se `cond` `e1` ou `e2` for maior, mas não uma linha múltipla:

```fsharp
if cond
then e1
else e2
```

Se qualquer uma das expressões for de várias linhas:

```fsharp
if cond then
    e1
else
    e2
```

Várias condicionais com `elif` e `else` são recuadas no mesmo escopo que o `if` :

```fsharp
if cond1 then e1
elif cond2 then e2
elif cond3 then e3
else e4
```

### <a name="pattern-matching-constructs"></a>Constructos de correspondência de padrões

Use uma `|` cláusula for each de uma correspondência sem recuo. Se a expressão for curta, você poderá considerar o uso de uma única linha se cada subexpressão também for simples.

```fsharp
// OK
match l with
| { him = x; her = "Posh" } :: tail -> x
| _ :: tail -> findDavid tail
| [] -> failwith "Couldn't find David"

// Not OK
match l with
    | { him = x; her = "Posh" } :: tail -> x
    | _ :: tail -> findDavid tail
    | [] -> failwith "Couldn't find David"
```

Se a expressão à direita da seta de correspondência de padrões for muito grande, mova-a para a linha a seguir, recuada uma etapa do `match` / `|` .

```fsharp
match lam with
| Var v -> 1
| Abs(x, body) ->
    1 + sizeLambda body
| App(lam1, lam2) ->
    sizeLambda lam1 + sizeLambda lam2

```

A correspondência de padrões de funções anônimas, começando pelo `function` , geralmente não deve ser recuada muito longe. Por exemplo, o recuo de um escopo da seguinte maneira é bom:

```fsharp
lambdaList
|> List.map (function
    | Abs(x, body) -> 1 + sizeLambda 0 body
    | App(lam1, lam2) -> sizeLambda (sizeLambda 0 lam1) lam2
    | Var v -> 1)
```

Correspondência de padrões em funções definidas por `let` ou `let rec` devem ser recuadas quatro espaços após o início de `let` , mesmo que a `function` palavra-chave seja usada:

```fsharp
let rec sizeLambda acc = function
    | Abs(x, body) -> sizeLambda (succ acc) body
    | App(lam1, lam2) -> sizeLambda (sizeLambda acc lam1) lam2
    | Var v -> succ acc
```

Não recomendamos o alinhamento de setas.

## <a name="formatting-trywith-expressions"></a>Formatando expressões try/com

A correspondência de padrões no tipo de exceção deve ser recuada no mesmo nível de `with` .

```fsharp
try
    if System.DateTime.Now.Second % 3 = 0 then
        raise (new System.Exception())
    else
        raise (new System.ApplicationException())
with
| :? System.ApplicationException ->
    printfn "A second that was not a multiple of 3"
| _ ->
    printfn "A second that was a multiple of 3"
```

## <a name="formatting-function-parameter-application"></a>Formatando o aplicativo de parâmetros de função

Em geral, a maioria dos aplicativos de parâmetro de função é feita na mesma linha.

Se você quiser aplicar parâmetros a uma função em uma nova linha, recue-os por um escopo.

```fsharp
// OK
sprintf "\t%s - %i\n\r"
     x.IngredientName x.Quantity

// OK
sprintf
     "\t%s - %i\n\r"
     x.IngredientName x.Quantity

// OK
let printVolumes x =
    printf "Volume in liters = %f, in us pints = %f, in imperial = %f"
        (convertVolumeToLiter x)
        (convertVolumeUSPint x)
        (convertVolumeImperialPint x)
```

As mesmas diretrizes se aplicam a expressões lambda como argumentos de função. Se o corpo de uma expressão lambda, o corpo pode ter outra linha, recuada por um escopo

```fsharp
let printListWithOffset a list1 =
    List.iter
        (fun elem -> printfn "%d" (a + elem))
        list1

// OK if lambda body is long enough
let printListWithOffset a list1 =
    List.iter
        (fun elem ->
            printfn "%d" (a + elem))
        list1
```

No entanto, se o corpo de uma expressão lambda for maior que uma linha, considere a possibilidade de separá-la em uma função separada em vez de ter uma construção de várias linhas aplicada como um único argumento a uma função.

### <a name="formatting-infix-operators"></a>Formatando operadores infixos

Separe os operadores por espaços. As exceções óbvias a essa regra são os `!` `.` operadores e.

As expressões de infixos estão OK para serem organizadas na mesma coluna:

```fsharp
acc +
(sprintf "\t%s - %i\n\r"
     x.IngredientName x.Quantity)

let function1 arg1 arg2 arg3 arg4 =
    arg1 + arg2 +
    arg3 + arg4
```

### <a name="formatting-pipeline-operators"></a>Formatando operadores de pipeline

`|>`Os operadores de pipeline devem ficar abaixo das expressões em que operam.

```fsharp
// Preferred approach
let methods2 =
    System.AppDomain.CurrentDomain.GetAssemblies()
    |> List.ofArray
    |> List.map (fun assm -> assm.GetTypes())
    |> Array.concat
    |> List.ofArray
    |> List.map (fun t -> t.GetMethods())
    |> Array.concat

// Not OK
let methods2 = System.AppDomain.CurrentDomain.GetAssemblies()
            |> List.ofArray
            |> List.map (fun assm -> assm.GetTypes())
            |> Array.concat
            |> List.ofArray
            |> List.map (fun t -> t.GetMethods())
            |> Array.concat
```

### <a name="formatting-modules"></a>Módulos de formatação

O código em um módulo local deve ser recuado em relação ao módulo, mas o código em um módulo de nível superior não deve ser recuado. Os elementos do namespace não precisam ser recuados.

```fsharp
// A is a top-level module.
module A

let function1 a b = a - b * b
```

```fsharp
// A1 and A2 are local modules.
module A1 =
    let function1 a b = a * a + b * b

module A2 =
    let function2 a b = a * a - b * b
```

### <a name="formatting-object-expressions-and-interfaces"></a>Formatando expressões e interfaces de objeto

As expressões e as interfaces de objeto devem ser alinhadas da mesma maneira com o `member` recuo após quatro espaços.

```fsharp
let comparer =
    { new IComparer<string> with
          member x.Compare(s1, s2) =
              let rev (s: String) =
                  new String (Array.rev (s.ToCharArray()))
              let reversed = rev s1
              reversed.CompareTo (rev s2) }
```

### <a name="formatting-white-space-in-expressions"></a>Formatando espaço em branco em expressões

Evite espaço em branco incorreto em expressões F #.

```fsharp
// OK
spam (ham.[1])

// Not OK
spam ( ham.[ 1 ] )
```

Os argumentos nomeados também não devem ter espaço em torno de `=` :

```fsharp
// OK
let makeStreamReader x = new System.IO.StreamReader(path=x)

// Not OK
let makeStreamReader x = new System.IO.StreamReader(path = x)
```

## <a name="formatting-attributes"></a>Atributos de formatação

Os [atributos](../language-reference/attributes.md) são colocados acima de um constructo:

```fsharp
[<SomeAttribute>]
type MyClass() = ...

[<RequireQualifiedAccess>]
module M =
    let f x = x

[<Struct>]
type MyRecord =
    { Label1: int
      Label2: string }
```

### <a name="formatting-attributes-on-parameters"></a>Formatando atributos em parâmetros

Os atributos também podem ser colocados em parâmetros. Nesse caso, coloque na mesma linha que o parâmetro e antes do nome:

```fsharp
// Defines a class that takes an optional value as input defaulting to false.
type C() =
    member _.M([<Optional; DefaultParameterValue(false)>] doSomething: bool)
```

### <a name="formatting-multiple-attributes"></a>Formatando vários atributos

Quando vários atributos são aplicados a uma construção que não é um parâmetro, eles devem ser colocados de forma que haja um atributo por linha:

```fsharp
[<Struct>]
[<IsByRefLike>]
type MyRecord =
    { Label1: int
      Label2: string }
```

Quando aplicado a um parâmetro, eles devem estar na mesma linha e separados por um `;` separador.

## <a name="formatting-literals"></a>Literais de formatação

Os [literais F #](../language-reference/literals.md) que usam o `Literal` atributo devem posicionar o atributo em sua própria linha e usar a nomenclatura PascalCase:

```fsharp
[<Literal>]
let Path = __SOURCE_DIRECTORY__ + "/" + __SOURCE_FILE__

[<Literal>]
let MyUrl = "www.mywebsitethatiamworkingwith.com"
```

Evite colocar o atributo na mesma linha que o valor.
