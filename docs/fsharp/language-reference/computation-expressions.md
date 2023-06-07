---
title: Expressões de computação
description: 'Saiba como criar uma sintaxe conveniente para escrever computações em F # que podem ser sequenciadas e combinadas usando construções e associações de fluxo de controle.'
ms.date: 11/04/2019
f1_keywords:
- let!_FS
ms.openlocfilehash: 32638e9493fb2c6b7aae30d044a0cda2a97f2178
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87855355"
---
# <a name="computation-expressions"></a>Expressões de computação

As expressões de computação no F # fornecem uma sintaxe conveniente para escrever computações que podem ser sequenciadas e combinadas usando construções e associações de fluxo de controle. Dependendo do tipo de expressão de computação, eles podem ser considerados como uma maneira de expressar monads, monoids, transformadores do Monad e Applicative transmissão functors. No entanto, ao contrário de outras linguagens (como o *-Notation* no Haskell), elas não estão vinculadas a uma única abstração e não dependem de macros ou de outras formas de metaprogramação para realizar uma sintaxe conveniente e sensível ao contexto.

## <a name="overview"></a>Visão geral

As computações podem ter muitas formas. A forma mais comum de computação é a execução de thread único, que é fácil de entender e modificar. No entanto, nem todas as formas de computação são tão simples quanto a execução de thread único. Alguns exemplos incluem:

- Computações não determinísticas
- Computações assíncronas
- Cálculos de efeito
- Computações de geração

Em geral, há cálculos *sensíveis ao contexto* que você deve executar em determinadas partes de um aplicativo. Escrever código sensível ao contexto pode ser desafiador, pois é fácil "vazar" cálculos fora de um determinado contexto sem abstrações para impedir que você faça isso. Muitas vezes, essas abstrações são desafiadoras de escrever por conta própria, ou seja, o F # tem uma maneira generalizada de fazer isso, chamado de **expressões de computação**.

As expressões de computação oferecem uma sintaxe uniforme e um modelo de abstração para codificar cálculos sensíveis ao contexto.

Cada expressão de computação é apoiada por um tipo de *Construtor* . O tipo de construtor define as operações que estão disponíveis para a expressão de computação. Consulte [criando um novo tipo de expressão de computação](computation-expressions.md#creating-a-new-type-of-computation-expression), que mostra como criar uma expressão de computação personalizada.

### <a name="syntax-overview"></a>Visão geral da sintaxe

Todas as expressões de computação têm o seguinte formato:

```fsharp
builder-expr { cexper }
```

em que `builder-expr` é o nome de um tipo de construtor que define a expressão de computação e `cexper` é o corpo da expressão da expressão de computação. Por exemplo, `async` o código de expressão de computação pode ser assim:

```fsharp
let fetchAndDownload url =
    async {
        let! data = downloadData url

        let processedData = processData data

        return processedData
    }
```

Há uma sintaxe adicional e especial disponível em uma expressão de computação, conforme mostrado no exemplo anterior. Os formulários de expressão a seguir são possíveis com expressões de computação:

```fsharp
expr { let! ... }
expr { do! ... }
expr { yield ... }
expr { yield! ... }
expr { return ... }
expr { return! ... }
expr { match! ... }
```

Cada uma dessas palavras-chave e outras palavras-chave F # padrão só estarão disponíveis em uma expressão de computação se elas tiverem sido definidas no tipo de construtor de backup. A única exceção é `match!` , que é, por sua vez, uma simplificação sintática para o uso de `let!` seguido por um padrão de correspondência no resultado.

O tipo de construtor é um objeto que define métodos especiais que regem a maneira como os fragmentos da expressão de computação são combinados; ou seja, seus métodos controlam a forma como a expressão de computação se comporta. Outra maneira de descrever uma classe de construtor é dizer que ela permite que você personalize a operação de muitas construções em F #, como loops e associações.

### `let!`

A `let!` palavra-chave associa o resultado de uma chamada a outra expressão de computação a um nome:

```fsharp
let doThingsAsync url =
    async {
        let! data = getDataAsync url
        ...
    }
```

Se você associar a chamada a uma expressão de computação com `let` , não obterá o resultado da expressão de computação. Em vez disso, você terá associado o valor da chamada não *realizada* a essa expressão de computação. Use `let!` para associar ao resultado.

`let!`é definido pelo `Bind(x, f)` membro no tipo de construtor.

### `do!`

A `do!` palavra-chave é para chamar uma expressão de computação que retorna um `unit` tipo-like (definido pelo `Zero` membro no Construtor):

```fsharp
let doThingsAsync data url =
    async {
        do! submitData data url
        ...
    }
```

Para o [fluxo de trabalho assíncrono](asynchronous-workflows.md), esse tipo é `Async<unit>` . Para outras expressões de computação, é provável que o tipo seja `CExpType<unit>` .

`do!`é definido pelo `Bind(x, f)` membro no tipo de construtor, onde `f` produz um `unit` .

### `yield`

A `yield` palavra-chave é para retornar um valor da expressão de computação para que ela possa ser consumida como um <xref:System.Collections.Generic.IEnumerable%601> :

```fsharp
let squares =
    seq {
        for i in 1..10 do
            yield i * i
    }

for sq in squares do
    printfn "%d" sq
```

Na maioria dos casos, ele pode ser omitido por chamadores. A maneira mais comum de omitir `yield` é com o `->` operador:

```fsharp
let squares =
    seq {
        for i in 1..10 -> i * i
    }

for sq in squares do
    printfn "%d" sq
```

Para expressões mais complexas que podem produzir muitos valores diferentes, e talvez condicionalmente, simplesmente omitir a palavra-chave pode fazer:

```fsharp
let weekdays includeWeekend =
    seq {
        "Monday"
        "Tuesday"
        "Wednesday"
        "Thursday"
        "Friday"
        if includeWeekend then
            "Saturday"
            "Sunday"
    }
```

Assim como acontece com a [palavra-chave yield em C#](../../csharp/language-reference/keywords/yield.md), cada elemento na expressão de computação é devolvido conforme é iterado.

`yield`é definido pelo `Yield(x)` membro no tipo de construtor, em que `x` é o item a ser devolvido.

### `yield!`

A `yield!` palavra-chave é para mesclar uma coleção de valores de uma expressão de computação:

```fsharp
let squares =
    seq {
        for i in 1..3 -> i * i
    }

let cubes =
    seq {
        for i in 1..3 -> i * i * i
    }

let squaresAndCubes =
    seq {
        yield! squares
        yield! cubes
    }

printfn "%A" squaresAndCubes // Prints - 1; 4; 9; 1; 8; 27
```

Quando avaliado, a expressão de computação chamada por `yield!` terá seus itens devolvidos um-por-um, mesclando o resultado.

`yield!`é definido pelo `YieldFrom(x)` membro no tipo de construtor, em que `x` é uma coleção de valores.

Ao contrário `yield` de, `yield!` deve ser especificado explicitamente. Seu comportamento não é implícito em expressões de computação.

### `return`

A `return` palavra-chave encapsula um valor no tipo correspondente à expressão de computação. Além das expressões de computação usando `yield` o, ele é usado para "Concluir" uma expressão de computação:

```fsharp
let req = // 'req' is of type is 'Async<data>'
    async {
        let! data = fetch url
        return data
    }

// 'result' is of type 'data'
let result = Async.RunSynchronously req
```

`return`é definido pelo `Return(x)` membro no tipo de construtor, em que `x` é o item a ser quebrado.

### `return!`

A `return!` palavra-chave percebe o valor de uma expressão de computação e encapsulamentos que resultam no tipo correspondente à expressão de computação:

```fsharp
let req = // 'req' is of type is 'Async<data>'
    async {
        return! fetch url
    }

// 'result' is of type 'data'
let result = Async.RunSynchronously req
```

`return!`é definido pelo `ReturnFrom(x)` membro no tipo de construtor, em que `x` é outra expressão de computação.

### `match!`

A `match!` palavra-chave permite embutir uma chamada para outra expressão de computação e correspondência de padrão em seu resultado:

```fsharp
let doThingsAsync url =
    async {
        match! callService url with
        | Some data -> ...
        | None -> ...
    }
```

Ao chamar uma expressão de computação com o `match!` , ele perceberá o resultado da chamada como `let!` . Isso geralmente é usado ao chamar uma expressão de computação em que o resultado é um [opcional](options.md).

## <a name="built-in-computation-expressions"></a>Expressões de computação internas

A biblioteca principal do F # define três expressões de computação internas: [expressões de sequência](sequences.md), [fluxos de trabalho assíncronos](asynchronous-workflows.md)e [expressões de consulta](query-expressions.md).

## <a name="creating-a-new-type-of-computation-expression"></a>Criando um novo tipo de expressão de computação

Você pode definir as características de suas próprias expressões de computação criando uma classe de construtor e definindo determinados métodos especiais na classe. A classe Builder pode, opcionalmente, definir os métodos conforme listados na tabela a seguir.

A tabela a seguir descreve os métodos que podem ser usados em uma classe do construtor de fluxo de trabalho.

|**Método**|**Assinaturas típicas**|**Descrição**|
|----|----|----|
|`Bind`|`M<'T> * ('T -> M<'U>) -> M<'U>`|Chamado para `let!` e `do!` em expressões de computação.|
|`Delay`|`(unit -> M<'T>) -> M<'T>`|Encapsula uma expressão de computação como uma função.|
|`Return`|`'T -> M<'T>`|Chamado para `return` em expressões de computação.|
|`ReturnFrom`|`M<'T> -> M<'T>`|Chamado para `return!` em expressões de computação.|
|`Run`|`M<'T> -> M<'T>` ou<br /><br />`M<'T> -> 'T`|Executa uma expressão de computação.|
|`Combine`|`M<'T> * M<'T> -> M<'T>` ou<br /><br />`M<unit> * M<'T> -> M<'T>`|Chamado para sequenciamento em expressões de computação.|
|`For`|`seq<'T> * ('T -> M<'U>) -> M<'U>` ou<br /><br />`seq<'T> * ('T -> M<'U>) -> seq<M<'U>>`|Chamado para `for...do` expressões em expressões de computação.|
|`TryFinally`|`M<'T> * (unit -> unit) -> M<'T>`|Chamado para `try...finally` expressões em expressões de computação.|
|`TryWith`|`M<'T> * (exn -> M<'T>) -> M<'T>`|Chamado para `try...with` expressões em expressões de computação.|
|`Using`|`'T * ('T -> M<'U>) -> M<'U> when 'T :> IDisposable`|Chamado para `use` associações em expressões de computação.|
|`While`|`(unit -> bool) * M<'T> -> M<'T>`|Chamado para `while...do` expressões em expressões de computação.|
|`Yield`|`'T -> M<'T>`|Chamado para `yield` expressões em expressões de computação.|
|`YieldFrom`|`M<'T> -> M<'T>`|Chamado para `yield!` expressões em expressões de computação.|
|`Zero`|`unit -> M<'T>`|Chamado para `else` ramificações vazias de `if...then` expressões em expressões de computação.|
|`Quote`|`Quotations.Expr<'T> -> Quotations.Expr<'T>`|Indica que a expressão de computação é passada para o `Run` membro como uma cotação. Ele traduz todas as instâncias de uma computação em uma cotação.|

Muitos dos métodos em uma classe de Construtor usam e retornam uma `M<'T>` construção, que normalmente é um tipo definido separadamente que caracteriza o tipo de cálculo que está sendo combinado, por exemplo, `Async<'T>` para fluxos de trabalho assíncronos e `Seq<'T>` para fluxos de trabalho de sequência. As assinaturas desses métodos permitem que eles sejam combinados e aninhados entre si, para que o objeto de fluxo de trabalho retornado de uma construção possa ser passado para o próximo. O compilador, quando analisa uma expressão de cálculo, converte a expressão em uma série de chamadas de função aninhadas usando os métodos na tabela anterior e o código na expressão de computação.

A expressão aninhada é do seguinte formato:

```fsharp
builder.Run(builder.Delay(fun () -> {| cexpr |}))
```

No código acima, as chamadas para `Run` e `Delay` serão omitidas se não estiverem definidas na classe do construtor de expressões de computação. O corpo da expressão de computação, aqui indicado como `{| cexpr |}` , é traduzido em chamadas que envolvem os métodos da classe Builder pelas traduções descritas na tabela a seguir. A expressão de computação `{| cexpr |}` é definida recursivamente de acordo com essas traduções em que `expr` é uma expressão F # e `cexpr` é uma expressão de computação.

|Expression|Tradução|
|----------|-----------|
|<code>{ let binding in cexpr }</code>|<code>let binding in {&#124; cexpr &#124;}</code>|
|<code>{ let! pattern = expr in cexpr }</code>|<code>builder.Bind(expr, (fun pattern -> {&#124; cexpr &#124;}))</code>|
|<code>{ do! expr in cexpr }</code>|<code>builder.Bind(expr, (fun () -> {&#124; cexpr &#124;}))</code>|
|<code>{ yield expr }</code>|`builder.Yield(expr)`|
|<code>{ yield! expr }</code>|`builder.YieldFrom(expr)`|
|<code>{ return expr }</code>|`builder.Return(expr)`|
|<code>{ return! expr }</code>|`builder.ReturnFrom(expr)`|
|<code>{ use pattern = expr in cexpr }</code>|<code>builder.Using(expr, (fun pattern -> {&#124; cexpr &#124;}))</code>|
|<code>{ use! value = expr in cexpr }</code>|<code>builder.Bind(expr, (fun value -> builder.Using(value, (fun value -> { cexpr }))))</code>|
|<code>{ if expr then cexpr0 &#124;}</code>|<code>if expr then { cexpr0 } else builder.Zero()</code>|
|<code>{ if expr then cexpr0 else cexpr1 &#124;}</code>|<code>if expr then { cexpr0 } else { cexpr1 }</code>|
|<code>{ match expr with &#124; pattern_i -> cexpr_i }</code>|<code>match expr with &#124; pattern_i -> { cexpr_i }</code>|
|<code>{ for pattern in expr do cexpr }</code>|<code>builder.For(enumeration, (fun pattern -> { cexpr }))</code>|
|<code>{ for identifier = expr1 to expr2 do cexpr }</code>|<code>builder.For(enumeration, (fun identifier -> { cexpr }))</code>|
|<code>{ while expr do cexpr }</code>|<code>builder.While(fun () -> expr, builder.Delay({ cexpr }))</code>|
|<code>{ try cexpr with &#124; pattern_i -> expr_i }</code>|<code>builder.TryWith(builder.Delay({ cexpr }), (fun value -> match value with &#124; pattern_i -> expr_i &#124; exn -> reraise exn)))</code>|
|<code>{ try cexpr finally expr }</code>|<code>builder.TryFinally(builder.Delay( { cexpr }), (fun () -> expr))</code>|
|<code>{ cexpr1; cexpr2 }</code>|<code>builder.Combine({ cexpr1 }, { cexpr2 })</code>|
|<code>{ other-expr; cexpr }</code>|<code>expr; { cexpr }</code>|
|<code>{ other-expr }</code>|`expr; builder.Zero()`|

Na tabela anterior, `other-expr` descreve uma expressão que não é listada de outra forma na tabela. Uma classe de construtor não precisa implementar todos os métodos e dar suporte a todas as conversões listadas na tabela anterior. As construções que não estão implementadas não estão disponíveis em expressões de computação desse tipo. Por exemplo, se você não quiser dar suporte à `use` palavra-chave em suas expressões de computação, poderá omitir a definição de `Use` na classe do construtor.

O exemplo de código a seguir mostra uma expressão de computação que encapsula uma computação como uma série de etapas que podem ser avaliadas uma etapa por vez. Um tipo de união discriminada, `OkOrException` , codifica o estado de erro da expressão como avaliado até agora. Esse código demonstra vários padrões típicos que você pode usar em suas expressões de computação, como implementações padronizadas de alguns dos métodos do Builder.

```fsharp
// Computations that can be run step by step
type Eventually<'T> =
    | Done of 'T
    | NotYetDone of (unit -> Eventually<'T>)

module Eventually =
    // The bind for the computations. Append 'func' to the
    // computation.
    let rec bind func expr =
        match expr with
        | Done value -> func value
        | NotYetDone work -> NotYetDone (fun () -> bind func (work()))

    // Return the final value wrapped in the Eventually type.
    let result value = Done value

    type OkOrException<'T> =
        | Ok of 'T
        | Exception of System.Exception

    // The catch for the computations. Stitch try/with throughout
    // the computation, and return the overall result as an OkOrException.
    let rec catch expr =
        match expr with
        | Done value -> result (Ok value)
        | NotYetDone work ->
            NotYetDone (fun () ->
                let res = try Ok(work()) with | exn -> Exception exn
                match res with
                | Ok cont -> catch cont // note, a tailcall
                | Exception exn -> result (Exception exn))

    // The delay operator.
    let delay func = NotYetDone (fun () -> func())

    // The stepping action for the computations.
    let step expr =
        match expr with
        | Done _ -> expr
        | NotYetDone func -> func ()

    // The rest of the operations are boilerplate.
    // The tryFinally operator.
    // This is boilerplate in terms of "result", "catch", and "bind".
    let tryFinally expr compensation =
        catch (expr)
        |> bind (fun res ->
            compensation();
            match res with
            | Ok value -> result value
            | Exception exn -> raise exn)

    // The tryWith operator.
    // This is boilerplate in terms of "result", "catch", and "bind".
    let tryWith exn handler =
        catch exn
        |> bind (function Ok value -> result value | Exception exn -> handler exn)

    // The whileLoop operator.
    // This is boilerplate in terms of "result" and "bind".
    let rec whileLoop pred body =
        if pred() then body |> bind (fun _ -> whileLoop pred body)
        else result ()

    // The sequential composition operator.
    // This is boilerplate in terms of "result" and "bind".
    let combine expr1 expr2 =
        expr1 |> bind (fun () -> expr2)

    // The using operator.
    let using (resource: #System.IDisposable) func =
        tryFinally (func resource) (fun () -> resource.Dispose())

    // The forLoop operator.
    // This is boilerplate in terms of "catch", "result", and "bind".
    let forLoop (collection:seq<_>) func =
        let ie = collection.GetEnumerator()
        tryFinally
            (whileLoop
                (fun () -> ie.MoveNext())
                (delay (fun () -> let value = ie.Current in func value)))
            (fun () -> ie.Dispose())

// The builder class.
type EventuallyBuilder() =
    member x.Bind(comp, func) = Eventually.bind func comp
    member x.Return(value) = Eventually.result value
    member x.ReturnFrom(value) = value
    member x.Combine(expr1, expr2) = Eventually.combine expr1 expr2
    member x.Delay(func) = Eventually.delay func
    member x.Zero() = Eventually.result ()
    member x.TryWith(expr, handler) = Eventually.tryWith expr handler
    member x.TryFinally(expr, compensation) = Eventually.tryFinally expr compensation
    member x.For(coll:seq<_>, func) = Eventually.forLoop coll func
    member x.Using(resource, expr) = Eventually.using resource expr

let eventually = new EventuallyBuilder()

let comp = eventually {
    for x in 1..2 do
        printfn " x = %d" x
    return 3 + 4 }

// Try the remaining lines in F# interactive to see how this
// computation expression works in practice.
let step x = Eventually.step x

// returns "NotYetDone <closure>"
comp |> step

// prints "x = 1"
// returns "NotYetDone <closure>"
comp |> step |> step

// prints "x = 1"
// prints "x = 2"
// returns "Done 7"
comp |> step |> step |> step |> step
```

Uma expressão de computação tem um tipo subjacente, que a expressão retorna. O tipo subjacente pode representar um resultado calculado ou uma computação atrasada que pode ser executada ou pode fornecer uma maneira de iterar por meio de algum tipo de coleção. No exemplo anterior, o tipo subjacente era **eventualmente**. Para uma expressão de sequência, o tipo subjacente é <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> . Para uma expressão de consulta, o tipo subjacente é <xref:System.Linq.IQueryable?displayProperty=nameWithType> . Para um fluxo de trabalho assíncrono, o tipo subjacente é [`Async`](https://msdn.microsoft.com/library/03eb4d12-a01a-4565-a077-5e83f17cf6f7) . O `Async` objeto representa o trabalho a ser executado para calcular o resultado. Por exemplo, você chama [`Async.RunSynchronously`](https://msdn.microsoft.com/library/0a6663a9-50f2-4d38-8bf3-cefd1a51fd6b) para executar uma computação e retornar o resultado.

## <a name="custom-operations"></a>Operações personalizadas

Você pode definir uma operação personalizada em uma expressão de computação e usar uma operação personalizada como um operador em uma expressão de computação. Por exemplo, você pode incluir um operador de consulta em uma expressão de consulta. Ao definir uma operação personalizada, você deve definir o yield e os métodos na expressão de computação. Para definir uma operação personalizada, coloque-a em uma classe de construtor para a expressão de computação e aplique o [`CustomOperationAttribute`](https://msdn.microsoft.com/library/199f3927-79df-484b-ba66-85f58cc49b19) . Esse atributo usa uma cadeia de caracteres como um argumento, que é o nome a ser usado em uma operação personalizada. Esse nome entra no escopo no início da chave de computação da expressão de cálculo. Portanto, você não deve usar identificadores que têm o mesmo nome que uma operação personalizada neste bloco. Por exemplo, evite o uso de identificadores como `all` ou `last` em expressões de consulta.

### <a name="extending-existing-builders-with-new-custom-operations"></a>Estendendo construtores existentes com novas operações personalizadas

Se você já tiver uma classe de construtor, suas operações personalizadas poderão ser estendidas de fora dessa classe do construtor. As extensões devem ser declaradas em módulos. Os namespaces não podem conter membros de extensão, exceto no mesmo arquivo e no mesmo grupo de declarações de namespace em que o tipo é definido.

O exemplo a seguir mostra a extensão da `Microsoft.FSharp.Linq.QueryBuilder` classe existente.

```fsharp
type Microsoft.FSharp.Linq.QueryBuilder with

    [<CustomOperation("existsNot")>]
    member _.ExistsNot (source: QuerySource<'T, 'Q>, predicate) =
        Enumerable.Any (source.Source, Func<_,_>(predicate)) |> not
```

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
- [Fluxos de Trabalho Assíncronos](asynchronous-workflows.md)
- [Sequências](https://msdn.microsoft.com/library/6b773b6b-9c9a-4af8-bd9e-d96585c166db)
- [Expressões de consulta](query-expressions.md)
