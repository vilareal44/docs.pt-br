---
title: Convenções de codificação do F#
description: 'Aprenda as diretrizes gerais e os idiomas ao escrever o código F #.'
ms.date: 01/15/2020
ms.openlocfilehash: 47e9183ce22689a050878cf10d7a9bcf3b929ec6
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84143520"
---
# <a name="f-coding-conventions"></a>Convenções de codificação do F#

As convenções a seguir são formuladas de experiência de trabalho com grandes bases de código F #. Os [cinco princípios de um bom código F #](index.md#five-principles-of-good-f-code) são a base de cada recomendação. Eles estão relacionados às [diretrizes de design de componente f #](component-design-guidelines.md), mas são aplicáveis a qualquer código F #, não apenas a componentes como bibliotecas.

## <a name="organizing-code"></a>Organizando o código

O F # apresenta duas maneiras principais de organizar código: módulos e namespaces. Eles são semelhantes, mas têm as seguintes diferenças:

* Os namespaces são compilados como namespaces .NET. Os módulos são compilados como classes estáticas.
* Os namespaces são sempre de nível superior. Os módulos podem ser de nível superior e aninhados dentro de outros módulos.
* Os namespaces podem abranger vários arquivos. Os módulos não podem.
* Os módulos podem ser decorados com `[<RequireQualifiedAccess>]` e `[<AutoOpen>]` .

As diretrizes a seguir ajudarão você a usá-las para organizar seu código.

### <a name="prefer-namespaces-at-the-top-level"></a>Preferir namespaces no nível superior

Para qualquer código de consumo publicamente, os namespaces são preferenciais aos módulos no nível superior. Como eles são compilados como namespaces do .NET, eles são consumíveis do C# sem nenhum problema.

```fsharp
// Good!
namespace MyCode

type MyClass() =
    ...
```

O uso de um módulo de nível superior pode não aparecer diferente quando chamado apenas de F #, mas para consumidores em C#, os chamadores podem ficar surpresos ao se qualificar `MyClass` com o `MyCode` módulo.

```fsharp
// Bad!
module MyCode

type MyClass() =
    ...
```

### <a name="carefully-apply-autoopen"></a>Aplicar cuidadosamente`[<AutoOpen>]`

A `[<AutoOpen>]` construção pode poluir o escopo do que está disponível para os chamadores, e a resposta para onde algo vem é "mágica". Isso não é algo bom. Uma exceção a essa regra é a biblioteca principal do F # (embora esse fato também seja um pouco controverso).

No entanto, é uma conveniência se você tiver a funcionalidade auxiliar para uma API pública que você deseja organizar separadamente dessa API pública.

```fsharp
module MyAPI =
    [<AutoOpen>]
    module private Helpers =
        let helper1 x y z =
            ...

    let myFunction1 x =
        let y = ...
        let z = ...

        helper1 x y z
```

Isso permite que você separe de forma limpa os detalhes de implementação da API pública de uma função sem precisar qualificar totalmente um auxiliar cada vez que chamá-lo.

Além disso, a exposição de métodos de extensão e construtores de expressão no nível de namespace pode ser claramente expressa com `[<AutoOpen>]` .

### <a name="use-requirequalifiedaccess-whenever-names-could-conflict-or-you-feel-it-helps-with-readability"></a>Use `[<RequireQualifiedAccess>]` sempre que os nomes puderem entrar em conflito ou se você achar que ele ajuda na legibilidade

A adição do `[<RequireQualifiedAccess>]` atributo a um módulo indica que o módulo pode não estar aberto e que as referências aos elementos do módulo exigem acesso qualificado explícito. Por exemplo, o `Microsoft.FSharp.Collections.List` módulo tem esse atributo.

Isso é útil quando as funções e os valores no módulo têm nomes que provavelmente estão em conflito com nomes em outros módulos. Exigir acesso qualificado pode aumentar muito a manutenção e o evolucionabilidade de longo prazo de uma biblioteca.

```fsharp
[<RequireQualifiedAccess>]
module StringTokenization =
    let parse s = ...

...

let s = getAString()
let parsed = StringTokenization.parse s // Must qualify to use 'parse'
```

### <a name="sort-open-statements-topologically"></a>Instruções de classificação `open` topologicamente

Em F #, a ordem das declarações é importante, incluindo com `open` instruções. Isso é diferente do C#, em que o efeito de `using` e `using static` é independente da ordenação dessas instruções em um arquivo.

Em F #, os elementos abertos em um escopo podem sombrear outros já presentes. Isso significa que `open` as instruções de reordenação podem alterar o significado do código. Como resultado, qualquer classificação arbitrária de todas as `open` instruções (por exemplo, alfanuméricamente) não é recomendada, última você gera um comportamento diferente que você pode esperar.

Em vez disso, recomendamos que você os classifique [topologicamente](https://en.wikipedia.org/wiki/Topological_sorting); ou seja, ordene suas `open` instruções na ordem em que as _camadas_ do seu sistema estão definidas. A classificação alfanumérica em diferentes camadas de topológica também pode ser considerada.

Por exemplo, aqui está a classificação topológica para o arquivo de API pública do serviço de compilador F #:

```fsharp
namespace Microsoft.FSharp.Compiler.SourceCodeServices

open System
open System.Collections.Generic
open System.Collections.Concurrent
open System.Diagnostics
open System.IO
open System.Reflection
open System.Text

open Microsoft.FSharp.Compiler
open Microsoft.FSharp.Compiler.AbstractIL
open Microsoft.FSharp.Compiler.AbstractIL.Diagnostics
open Microsoft.FSharp.Compiler.AbstractIL.IL
open Microsoft.FSharp.Compiler.AbstractIL.ILBinaryReader
open Microsoft.FSharp.Compiler.AbstractIL.Internal
open Microsoft.FSharp.Compiler.AbstractIL.Internal.Library

open Microsoft.FSharp.Compiler.AccessibilityLogic
open Microsoft.FSharp.Compiler.Ast
open Microsoft.FSharp.Compiler.CompileOps
open Microsoft.FSharp.Compiler.CompileOptions
open Microsoft.FSharp.Compiler.Driver
open Microsoft.FSharp.Compiler.ErrorLogger
open Microsoft.FSharp.Compiler.Infos
open Microsoft.FSharp.Compiler.InfoReader
open Microsoft.FSharp.Compiler.Lexhelp
open Microsoft.FSharp.Compiler.Layout
open Microsoft.FSharp.Compiler.Lib
open Microsoft.FSharp.Compiler.NameResolution
open Microsoft.FSharp.Compiler.PrettyNaming
open Microsoft.FSharp.Compiler.Parser
open Microsoft.FSharp.Compiler.Range
open Microsoft.FSharp.Compiler.Tast
open Microsoft.FSharp.Compiler.Tastops
open Microsoft.FSharp.Compiler.TcGlobals
open Microsoft.FSharp.Compiler.TypeChecker
open Microsoft.FSharp.Compiler.SourceCodeServices.SymbolHelpers

open Internal.Utilities
open Internal.Utilities.Collections
```

Observe que uma quebra de linha separa as camadas topológica, com cada camada sendo classificada alfanuméricamente depois. Isso organiza corretamente o código sem sombrear valores de forma acidental.

## <a name="use-classes-to-contain-values-that-have-side-effects"></a>Usar classes para conter valores que têm efeitos colaterais

Há muitas ocasiões em que a inicialização de um valor pode ter efeitos colaterais, como instanciar um contexto para um banco de dados ou outro recurso remoto. É tentador inicializar essas coisas em um módulo e usá-las em funções subsequentes:

```fsharp
// This is bad!
module MyApi =
    let dep1 = File.ReadAllText "/Users/{your name}/connectionstring.txt"
    let dep2 = Environment.GetEnvironmentVariable "DEP_2"

    let private r = Random()
    let dep3() = r.Next() // Problematic if multiple threads use this

    let function1 arg = doStuffWith dep1 dep2 dep3 arg
    let function2 arg = doSutffWith dep1 dep2 dep3 arg
```

Geralmente, essa é uma má ideia por alguns motivos:

Primeiro, a configuração do aplicativo é enviada por push para a base de código com `dep1` e `dep2` . Isso é difícil de ser mantido em bases de código maiores.

Segundo, os dados inicializados estaticamente não devem incluir valores que não sejam thread-safe se o seu componente usar vários threads. Isso é claramente violado pelo `dep3` .

Por fim, a inicialização do módulo é compilada em um construtor estático para toda a unidade de compilação. Se ocorrer algum erro na inicialização de valor de Let associado nesse módulo, ele será manifestado como `TypeInitializationException` sendo armazenado em cache durante todo o tempo de vida do aplicativo. Isso pode ser difícil de diagnosticar. Normalmente, há uma exceção interna com a qual você pode tentar se deparar, mas se não houver, não há nenhum informando qual é a causa raiz.

Em vez disso, basta usar uma classe simples para manter as dependências:

```fsharp
type MyParametricApi(dep1, dep2, dep3) =
    member _.Function1 arg1 = doStuffWith dep1 dep2 dep3 arg1
    member _.Function2 arg2 = doStuffWith dep1 dep2 dep3 arg2
```

Isso permite o seguinte:

1. Enviar por push qualquer estado dependente fora da própria API.
2. Agora, a configuração pode ser feita fora da API.
3. Erros na inicialização de valores dependentes provavelmente não serão manifestados como um `TypeInitializationException` .
4. Agora, a API é mais fácil de testar.

## <a name="error-management"></a>Gerenciamento de erros

O gerenciamento de erros em sistemas grandes é um esforço complexo e nuance, e não há nenhum marcador prata para garantir que seus sistemas sejam tolerantes a falhas e se comporte bem. As diretrizes a seguir devem oferecer orientações para navegar nesse espaço difícil.

### <a name="represent-error-cases-and-illegal-state-in-types-intrinsic-to-your-domain"></a>Representar casos de erro e estado ilegal em tipos intrínsecos ao seu domínio

Com [uniões discriminadas](../language-reference/discriminated-unions.md), o F # oferece a capacidade de representar o estado do programa com falha em seu sistema de tipos. Por exemplo:

```fsharp
type MoneyWithdrawalResult =
    | Success of amount:decimal
    | InsufficientFunds of balance:decimal
    | CardExpired of DateTime
    | UndisclosedFailure
```

Nesse caso, há três maneiras conhecidas de retirar dinheiro de uma conta bancária. Cada caso de erro é representado no tipo e, portanto, pode ser tratado com segurança em todo o programa.

```fsharp
let handleWithdrawal amount =
    let w = withdrawMoney amount
    match w with
    | Success am -> printfn "Successfully withdrew %f" am
    | InsufficientFunds balance -> printfn "Failed: balance is %f" balance
    | CardExpired expiredDate -> printfn "Failed: card expired on %O" expiredDate
    | UndisclosedFailure -> printfn "Failed: unknown"
```

Em geral, se você puder modelar as diferentes maneiras pelas quais algo pode **falhar** em seu domínio, o código de tratamento de erros não será mais tratado como algo com o qual você deve lidar, além do fluxo de programa normal. Ele é simplesmente parte do fluxo normal do programa e não é considerado **excepcional**. Há dois benefícios principais para isso:

1. É mais fácil de manter à medida que seu domínio muda ao longo do tempo.
2. Casos de erro são mais fáceis de testar unidade.

### <a name="use-exceptions-when-errors-cannot-be-represented-with-types"></a>Usar exceções quando erros não puderem ser representados com tipos

Nem todos os erros podem ser representados em um domínio com problema. Esses tipos de falhas são *excepcionais* por natureza, portanto, a capacidade de gerar e capturar exceções em F #.

Primeiro, é recomendável que você leia as [diretrizes de design de exceção](../../standard/design-guidelines/exceptions.md). Elas também são aplicáveis ao F #.

As principais construções disponíveis em F # para fins de geração de exceções devem ser consideradas na seguinte ordem de preferência:

| Função | Sintaxe | Finalidade |
|----------|--------|---------|
| `nullArg` | `nullArg "argumentName"` | Gera um `System.ArgumentNullException` com o nome do argumento especificado. |
| `invalidArg` | `invalidArg "argumentName" "message"` | Gera um `System.ArgumentException` com um nome de argumento e uma mensagem especificados. |
| `invalidOp` | `invalidOp "message"` | Gera um `System.InvalidOperationException` com a mensagem especificada. |
|`raise`| `raise (ExceptionType("message"))` | Mecanismo de finalidade geral para lançar exceções. |
| `failwith` | `failwith "message"` | Gera um `System.Exception` com a mensagem especificada. |
| `failwithf` | `failwithf "format string" argForFormatString` | Gera um `System.Exception` com uma mensagem determinada pela cadeia de caracteres de formato e suas entradas. |

Use `nullArg` `invalidArg` e `invalidOp` como o mecanismo para gerar `ArgumentNullException` `ArgumentException` e `InvalidOperationException` quando apropriado.

As `failwith` `failwithf` funções e geralmente devem ser evitadas porque geram o `Exception` tipo base, não uma exceção específica. De acordo com as [diretrizes de design de exceção](../../standard/design-guidelines/exceptions.md), você deseja gerar exceções mais específicas quando puder.

### <a name="using-exception-handling-syntax"></a>Usando a sintaxe de tratamento de exceção

O F # dá suporte a padrões de exceção por meio da `try...with` sintaxe:

```fsharp
try
    tryGetFileContents()
with
| :? System.IO.FileNotFoundException as e -> // Do something with it here
| :? System.Security.SecurityException as e -> // Do something with it here
```

A reconciliação da funcionalidade a ser executada diante de uma exceção com correspondência de padrões pode ser um pouco complicada se você quiser manter a limpeza do código. Uma forma de lidar com isso é usar [padrões ativos](../language-reference/active-patterns.md) como um meio de agrupar a funcionalidade em torno de um caso de erro com uma exceção em si. Por exemplo, você pode estar consumindo uma API que, quando ela gera uma exceção, inclui informações valiosas nos metadados de exceção. Desencapsular um valor útil no corpo da exceção capturada dentro do padrão ativo e retornar esse valor pode ser útil em algumas situações.

### <a name="do-not-use-monadic-error-handling-to-replace-exceptions"></a>Não use o tratamento de erros monadic para substituir exceções

As exceções são vistas como um pouco da na programação funcional. Na verdade, as exceções violam a pureza, portanto, é seguro considerá-las não muito funcionais. No entanto, isso ignora a realidade de onde o código deve ser executado e que podem ocorrer erros de tempo de execução. Em geral, escreva o código na suposição de que a maioria das coisas não é pura nem total, para minimizar surpresas desagradáveis.

É importante considerar os seguintes pontos fortes/aspectos principais de exceções em relação à sua relevância e à sua adequação no tempo de execução do .NET e no ecossistema entre linguagens como um todo:

1. Eles contêm informações detalhadas de diagnóstico, o que é muito útil ao depurar um problema.
2. Eles são bem compreendidos pelo tempo de execução e por outras linguagens .NET.
3. Eles podem reduzir um timbre significativo em comparação com o código que sai do seu caminho para *evitar* exceções implementando algum subconjunto de sua semântica em uma base ad hoc.

Esse terceiro ponto é crítico. Para operações complexas não triviais, a falha ao usar exceções pode resultar em lidar com estruturas como esta:

```fsharp
Result<Result<MyType, string>, string list>
```

Que pode facilmente levar a um código frágil como correspondência de padrões em erros de "tipo de cadeia de caracteres":

```fsharp
let result = doStuff()
match result with
| Ok r -> ...
| Error e ->
    if e.Contains "Error string 1" then ...
    elif e.Contains "Error string 2" then ...
    else ... // Who knows?
```

Além disso, pode ser tentador assimilar qualquer exceção no desejo de uma função "simples" que retorna um tipo "interessante":

```fsharp
// This is bad!
let tryReadAllText (path : string) =
    try System.IO.File.ReadAllText path |> Some
    with _ -> None
```

Infelizmente, `tryReadAllText` o pode lançar várias exceções com base na infinidade de coisas que podem ocorrer em um sistema de arquivos, e esse código descarta todas as informações sobre o que pode realmente estar errado em seu ambiente. Se você substituir esse código por um tipo de resultado, você voltará à análise da mensagem de erro "tipo de cadeia de caracteres":

```fsharp
// This is bad!
let tryReadAllText (path : string) =
    try System.IO.File.ReadAllText path |> Ok
    with e -> Error e.Message

let r = tryReadAllText "path-to-file"
match r with
| Ok text -> ...
| Error e ->
    if e.Contains "uh oh, here we go again..." then ...
    else ...
```

E colocar o objeto de exceção em si no `Error` Construtor apenas força você a lidar corretamente com o tipo de exceção no site de chamada em vez de na função. Fazer isso efetivamente cria exceções marcadas, que são notoriamente unfun para lidar como um chamador de uma API.

Uma boa alternativa para os exemplos acima é capturar exceções *específicas* e retornar um valor significativo no contexto dessa exceção. Se você modificar a `tryReadAllText` função da seguinte maneira, o `None` terá mais significado:

```fsharp
let tryReadAllTextIfPresent (path : string) =
    try System.IO.File.ReadAllText path |> Some
    with :? FileNotFoundException -> None
```

Em vez de funcionar como um problema, essa função agora tratará corretamente o caso quando um arquivo não foi encontrado e atribuirá esse significado a um retorno. Esse valor de retorno pode mapear para esse caso de erro, sem descartar nenhuma informação contextual ou forçar os chamadores a lidar com um caso que pode não ser relevante nesse ponto no código.

Tipos como `Result<'Success, 'Error>` são apropriados para operações básicas em que não estão aninhados, e tipos opcionais F # são perfeitos para representar quando algo poderia retornar *algo* ou *nada*. Eles não são uma substituição de exceções, porém, e não devem ser usados em uma tentativa de substituir exceções. Em vez disso, eles devem ser aplicados criteriosamente para abordar aspectos específicos da política de gerenciamento de erros e exceções de formas direcionadas.

## <a name="partial-application-and-point-free-programming"></a>Aplicativo parcial e programação sem ponto

O F # dá suporte ao aplicativo parcial e, portanto, várias maneiras de programar em um estilo sem ponto. Isso pode ser benéfico para reutilização de código dentro de um módulo ou a implementação de algo, mas não é algo a ser exposto publicamente. Em geral, a programação sem ponto não é uma questão em si própria e pode adicionar uma barreira cognitiva significativa para pessoas que não estão imersos no estilo.

### <a name="do-not-use-partial-application-and-currying-in-public-apis"></a>Não use aplicativos parciais e currying em APIs públicas

Com pouca exceção, o uso de aplicativos parciais em APIs públicas pode ser confuso para os consumidores. Normalmente, `let` os valores associados no código F # são **valores**, e não **valores de função**. Misturar valores e valores de função pode resultar na economia de um pequeno número de linhas de código no Exchange para uma grande sobrecarga de cognitiva, especialmente se combinada com operadores como `>>` para compor funções.

### <a name="consider-the-tooling-implications-for-point-free-programming"></a>Considere as implicações de ferramentas para a programação sem ponto

As funções na forma curried não rotulam seus argumentos. Isso tem implicações de ferramentas. Considere as duas funções a seguir:

```fsharp
let func name age =
    printfn "My name is %s and I am %d years old!" name age

let funcWithApplication =
    printfn "My name is %s and I am %d years old!"
```

Ambos são funções válidas, mas `funcWithApplication` é uma função na forma curried. Ao passar o mouse sobre seus tipos em um editor, você verá:

```fsharp
val func : name:string -> age:int -> unit

val funcWithApplication : (string -> int -> unit)
```

No local de chamada, as dicas de ferramenta em ferramentas como o Visual Studio não fornecerão informações significativas sobre o que os `string` tipos de entrada e de `int` fato representam.

Se você encontrar um código sem ponto como o `funcWithApplication` que é de consumo público, é recomendável fazer uma η completa para que as ferramentas possam escolher nomes significativos para os argumentos.

Além disso, a depuração de código sem ponto pode ser desafiadora, se não impossível. As ferramentas de depuração dependem de valores associados aos nomes (por exemplo, `let` associações) para que você possa inspecionar valores intermediários no meio da execução. Quando seu código não tem valores a serem inspecionados, não há nada a ser depurado. No futuro, as ferramentas de depuração podem evoluir para resumir esses valores com base em caminhos executados anteriormente, mas não é uma boa ideia envolver suas opções em *potencial* funcionalidade de depuração.

### <a name="consider-partial-application-as-a-technique-to-reduce-internal-boilerplate"></a>Considere o aplicativo parcial como uma técnica para reduzir o timbre interno

Em contraste com o ponto anterior, o aplicativo parcial é uma excelente ferramenta para reduzir o texto clichê dentro de um aplicativo ou os detalhes internos de uma API. Pode ser útil para o teste de unidade da implementação de APIs mais complicadas, em que o clichê é muitas vezes um problema para lidar. Por exemplo, o código a seguir mostra como você pode realizar o que a maioria das estruturas fictícias fornece sem tirar uma dependência externa de tal estrutura e ter que aprender uma API do bespoke relacionada.

Por exemplo, considere a topografia de solução a seguir:

```
MySolution.sln
|_/ImplementationLogic.fsproj
|_/ImplementationLogic.Tests.fsproj
|_/API.fsproj
```

`ImplementationLogic.fsproj`pode expor código como:

```fsharp
module Transactions =
    let doTransaction txnContext txnType balance =
        ...

type Transactor(ctx, currentBalance) =
    member _.ExecuteTransaction(txnType) =
        Transactions.doTransaction ctx txtType currentBalance
        ...
```

O teste `Transactions.doTransaction` de unidade no `ImplementationLogic.Tests.fsproj` é fácil:

```fsharp
namespace TransactionsTestingUtil

open Transactions

module TransactionsTestable =
    let getTestableTransactionRoutine mockContext = Transactions.doTransaction mockContext
```

A aplicação parcial `doTransaction` com um objeto de contexto fictício permite chamar a função em todos os testes de unidade sem a necessidade de construir um contexto fictício a cada vez:

```fsharp
namespace TransactionTests

open Xunit
open TransactionTypes
open TransactionsTestingUtil
open TransactionsTestingUtil.TransactionsTestable

let testableContext =
    { new ITransactionContext with
        member _.TheFirstMember() = ...
        member _.TheSecondMember() = ... }

let transactionRoutine = getTestableTransactionRoutine testableContext

[<Fact>]
let ``Test withdrawal transaction with 0.0 for balance``() =
    let expected = ...
    let actual = transactionRoutine TransactionType.Withdraw 0.0
    Assert.Equal(expected, actual)
```

Essa técnica não deve ser aplicada universalmente a toda a base de código, mas é uma boa maneira de reduzir o texto clichê para os internos complicados e os testes de unidade desses elementos internos.

## <a name="access-control"></a>Controle de acesso

O F # tem várias opções para o [controle de acesso](../language-reference/access-control.md), herdadas do que está disponível no tempo de execução do .net. Eles não são apenas utilizáveis para tipos – você também pode usá-los para funções.

* Prefira não `public` tipos e membros até que você precise que eles sejam publicamente consumíveis. Isso também minimiza o que os consumidores desejam.
* Busque para manter toda a funcionalidade auxiliar `private` .
* Considere o uso de `[<AutoOpen>]` em um módulo particular de funções auxiliares se eles se tornarem numerosos.

## <a name="type-inference-and-generics"></a>Inferência de tipos e genéricos

A inferência de tipos pode evitar que você digite muita clichê. E a generalização automática no compilador de F # pode ajudá-lo a escrever código mais genérico com quase nenhum esforço extra de sua parte. No entanto, esses recursos não são universalmente bons.

* Considere rotular nomes de argumentos com tipos explícitos em APIs públicas e não confie na inferência de tipos para isso.

    O motivo disso é que **você** deve estar no controle da forma de sua API, não do compilador. Embora o compilador possa fazer um bom trabalho em tipos informativos, é possível que a forma da sua API seja alterada se os internos dos quais ele depende tiverem tipos alterados. Isso pode ser o que você deseja, mas certamente resultará em uma alteração de API de interrupção com a qual os consumidores de downstream terão que lidar. Em vez disso, se você controlar explicitamente a forma de sua API pública, poderá controlar essas alterações significativas. Em termos de DDD, isso pode ser considerado uma camada anticorrupção.

* Considere fornecer um nome significativo para seus argumentos genéricos.

    A menos que você esteja escrevendo um código verdadeiramente genérico que não seja específico de um determinado domínio, um nome significativo pode ajudar outros programadores a compreender o domínio no qual estão trabalhando. Por exemplo, um parâmetro de tipo chamado `'Document` no contexto de interação com um banco de dados de documento torna mais claro que os tipos de documento genéricos podem ser aceitos pela função ou pelo membro com o qual você está trabalhando.

* Considere nomear parâmetros de tipo genérico com PascalCase.

    Essa é a maneira geral de fazer coisas no .NET, portanto, é recomendável que você use PascalCase em vez de snake_case ou camelCase.

Por fim, a generalização automática nem sempre é uma benefício para pessoas que são novas para a F # ou uma grande base de código. Há uma sobrecarga cognitiva no uso de componentes genéricos. Além disso, se as funções generalizadas automaticamente não forem usadas com tipos de entrada diferentes (independentemente de se destinarem a serem usadas como tal), não há nenhum benefício real para eles serem genéricos nesse ponto no tempo. Sempre considere se o código que você está escrevendo realmente se beneficiará de ser genérico.

## <a name="performance"></a>Desempenho

### <a name="prefer-structs-for-small-data-types"></a>Preferir structs para tipos de dados pequenos

O uso de structs (também chamados de tipos de valor) geralmente pode resultar em um melhor desempenho para algum código, pois normalmente evita a alocação de objetos. No entanto, structs nem sempre são um botão "ir mais rápido": se o tamanho dos dados em uma struct exceder 16 bytes, a cópia dos dados poderá resultar em mais tempo de CPU do que usar um tipo de referência.

Para determinar se você deve usar uma estrutura, considere as seguintes condições:

- Se o tamanho de seus dados for de 16 bytes ou menor.
- Se for provável que você tenha muitos desses tipos de dados residentes na memória em um programa em execução.

Se a primeira condição se aplicar, você geralmente deve usar uma struct. Se ambos se aplicarem, você quase sempre deve usar uma struct. Pode haver alguns casos em que as condições anteriores se aplicam, mas usar uma struct não é melhor ou pior do que usar um tipo de referência, mas é provável que seja raro. É importante sempre medir ao fazer alterações como essa, no entanto, e não operar na suposição ou na intuição.

#### <a name="prefer-struct-tuples-when-grouping-small-value-types"></a>Preferir tuplas de struct ao agrupar tipos de valor pequeno

Considere as duas funções a seguir:

```fsharp
let rec runWithTuple t offset times =
    let offsetValues x y z offset =
        (x + offset, y + offset, z + offset)

    if times <= 0 then
        t
    else
        let (x, y, z) = t
        let r = offsetValues x y z offset
        runWithTuple r offset (times - 1)

let rec runWithStructTuple t offset times =
    let offsetValues x y z offset =
        struct(x + offset, y + offset, z + offset)

    if times <= 0 then
        t
    else
        let struct(x, y, z) = t
        let r = offsetValues x y z offset
        runWithStructTuple r offset (times - 1)
```

Ao avaliar essas funções com uma ferramenta de parâmetro de comparação estatística como [BenchmarkDotNet](https://benchmarkdotnet.org/), você descobrirá que a `runWithStructTuple` função que usa tuplas de struct é executada em 40% mais rápida e não aloca memória.

No entanto, esses resultados não serão sempre o caso em seu próprio código. Se você marcar uma função como `inline` , o código que usa tuplas de referência poderá obter algumas otimizações adicionais ou o código que alocaria poderia simplesmente ser otimizado para fora. Você sempre deve medir os resultados sempre que o desempenho estiver preocupado e nunca operar com base na suposição ou na intuição.

#### <a name="prefer-struct-records-when-the-data-type-is-small"></a>Prefira registros de struct quando o tipo de dados for pequeno

A regra geral descrita anteriormente também contém os [tipos de registro F #](../language-reference/records.md). Considere os seguintes tipos de dados e funções que os processam:

```fsharp
type Point = { X: float; Y: float; Z: float }

[<Struct>]
type SPoint = { X: float; Y: float; Z: float }

let rec processPoint (p: Point) offset times =
    let inline offsetValues (p: Point) offset =
        { p with X = p.X + offset; Y = p.Y + offset; Z = p.Z + offset }

    if times <= 0 then
        p
    else
        let r = offsetValues p offset
        processPoint r offset (times - 1)

let rec processStructPoint (p: SPoint) offset times =
    let inline offsetValues (p: SPoint) offset =
        { p with X = p.X + offset; Y = p.Y + offset; Z = p.Z + offset }

    if times <= 0 then
        p
    else
        let r = offsetValues p offset
        processStructPoint r offset (times - 1)
```

Isso é semelhante ao código de tupla anterior, mas desta vez o exemplo usa registros e uma função interna embutida.

Ao avaliar essas funções com uma ferramenta de benchmark estatística como [BenchmarkDotNet](https://benchmarkdotnet.org/), você verá que o `processStructPoint` executa quase 60% mais rápido e aloca nada no heap gerenciado.

#### <a name="prefer-struct-discriminated-unions-when-the-data-type-is-small"></a>Preferir uniões discriminadas de struct quando o tipo de dados for pequeno

As observações anteriores sobre o desempenho com tuplas e registros de struct também são mantidas para [un# uniões discriminadas](../language-reference/discriminated-unions.md). Considere o código a seguir:

```fsharp
    type Name = Name of string

    [<Struct>]
    type SName = SName of string

    let reverseName (Name s) =
        s.ToCharArray()
        |> Array.rev
        |> string
        |> Name

    let structReverseName (SName s) =
        s.ToCharArray()
        |> Array.rev
        |> string
        |> SName
```

É comum definir uniões discriminadas de um único caso como essa para modelagem de domínio. Ao avaliar essas funções com uma ferramenta de parâmetro de comparação estatística como [BenchmarkDotNet](https://benchmarkdotnet.org/), você descobrirá que `structReverseName` é executado cerca de 25% mais rápido do que `reverseName` para cadeias de caracteres pequenas. Para cadeias de caracteres grandes, ambos executam sobre o mesmo. Portanto, nesse caso, é sempre preferível usar uma struct. Como mencionado anteriormente, sempre meça e não opere em suposições ou intuições.

Embora o exemplo anterior mostre que uma união discriminada de struct gerou um desempenho melhor, é comum ter uniões discriminadas maiores ao modelar um domínio. Tipos de dados maiores como esse também podem não ser executados, se forem structos, dependendo das operações neles, já que mais cópias poderiam ser envolvidas.

### <a name="functional-programming-and-mutation"></a>Programação funcional e mutação

Valores F # são imutáveis por padrão, o que permite que você evite determinadas classes de bugs (especialmente aqueles que envolvem simultaneidade e paralelismo). No entanto, em determinados casos, para obter eficiência ideal (ou até mesmo razoável) de tempo de execução ou alocações de memória, um intervalo de trabalho pode ser melhor implementado usando a mutação in-loco do estado. Isso é possível em uma base opcional com o F # com a `mutable` palavra-chave.

O uso do `mutable` em F # pode sentir a probabilidade de estar em conflito com a pureza funcional. Isso é compreensível, mas a pureza funcional em todos os lugares pode estar em conflito com as metas de desempenho. Um comprometimento é encapsular a mutação de forma que os chamadores não precisem se preocupar com o que acontece quando chamam uma função. Isso permite que você escreva uma interface funcional em uma implementação baseada em mutação para código de desempenho crítico.

#### <a name="wrap-mutable-code-in-immutable-interfaces"></a>Encapsular código mutável em interfaces imutáveis

Com a transparência referencial como meta, é essencial escrever um código que não exponha a Underbelly mutável de funções de desempenho crítico. Por exemplo, o código a seguir implementa a `Array.contains` função na biblioteca principal de F #:

```fsharp
[<CompiledName("Contains")>]
let inline contains value (array:'T[]) =
    checkNonNull "array" array
    let mutable state = false
    let mutable i = 0
    while not state && i < array.Length do
        state <- value = array.[i]
        i <- i + 1
    state
```

Chamar essa função várias vezes não altera a matriz subjacente, nem requer que você mantenha qualquer estado mutável ao consumi-la. Ele é transparevelmente transparente, embora quase todas as linhas de código dentro dele usem mutação.

#### <a name="consider-encapsulating-mutable-data-in-classes"></a>Considere o encapsulamento de dados mutáveis em classes

O exemplo anterior usava uma única função para encapsular operações usando dados mutáveis. Isso nem sempre é suficiente para conjuntos de dados mais complexos. Considere os seguintes conjuntos de funções:

```fsharp
open System.Collections.Generic

let addToClosureTable (key, value) (t: Dictionary<_,_>) =
    if not (t.ContainsKey(key)) then
        t.Add(key, value)
    else
        t.[key] <- value

let closureTableCount (t: Dictionary<_,_>) = t.Count

let closureTableContains (key, value) (t: Dictionary<_, HashSet<_>>) =
    match t.TryGetValue(key) with
    | (true, v) -> v.Equals(value)
    | (false, _) -> false
```

Esse código é de alto desempenho, mas expõe a estrutura de dados baseada em mutação que os chamadores são responsáveis por manter. Isso pode ser encapsulado dentro de uma classe sem Membros subjacentes que podem ser alterados:

```fsharp
open System.Collections.Generic

/// The results of computing the LALR(1) closure of an LR(0) kernel
type Closure1Table() =
    let t = Dictionary<Item0, HashSet<TerminalIndex>>()

    member _.Add(key, value) =
        if not (t.ContainsKey(key)) then
            t.Add(key, value)
        else
            t.[key] <- value

    member _.Count = t.Count

    member _.Contains(key, value) =
        match t.TryGetValue(key) with
        | (true, v) -> v.Equals(value)
        | (false, _) -> false
```

`Closure1Table`encapsula a estrutura de dados baseada em mutação subjacente, impedindo que os chamadores mantenham a estrutura de dados subjacente. As classes são uma maneira eficiente de encapsular dados e rotinas com base em mutação sem expor os detalhes aos chamadores.

#### <a name="prefer-let-mutable-to-reference-cells"></a>Preferir `let mutable` fazer referência a células

As células de referência são uma maneira de representar a referência a um valor em vez do próprio valor. Embora eles possam ser usados para código de desempenho crítico, eles não são recomendados. Considere o exemplo a seguir:

```fsharp
let kernels =
    let acc = ref Set.empty

    processWorkList startKernels (fun kernel ->
        if not ((!acc).Contains(kernel)) then
            acc := (!acc).Add(kernel)
        ...)

    !acc |> Seq.toList
```

O uso de uma célula de referência agora "polui" todos os códigos subsequentes com a falta de desreferenciar e rereferenciar os dados subjacentes. Em vez disso, considere `let mutable` :

```fsharp
let kernels =
    let mutable acc = Set.empty

    processWorkList startKernels (fun kernel ->
        if not (acc.Contains(kernel)) then
            acc <- acc.Add(kernel)
        ...)

    acc |> Seq.toList
```

Além do ponto único de mutação no meio da expressão lambda, todos os demais códigos que o tocam `acc` podem fazer isso de uma maneira que não seja diferente do uso de um `let` valor imutável de limite normal. Isso facilitará a alteração com o passar do tempo.

## <a name="object-programming"></a>Programação de objeto

O F # tem suporte total para objetos e conceitos orientados a objeto (OO). Embora muitos conceitos de OO sejam poderosos e úteis, nem todos eles são ideais para uso. As listas a seguir oferecem orientação sobre as categorias de recursos do OO em um alto nível.

**Considere o uso desses recursos em muitas situações:**

* Notação de ponto ( `x.Length` )
* Membros da instância
* Construtores implícitos
* Membros estáticos
* Notação do indexador ( `arr.[x]` )
* Argumentos nomeados e opcionais
* Interfaces e implementações de interface

**Não alcance esses recursos primeiro, mas os aplica criteriosamente quando é conveniente resolver um problema:**

* Sobrecarga de método
* Dados mutáveis encapsulados
* Operadores em tipos
* Propriedades automáticas
* Implementando `IDisposable` e`IEnumerable`
* Extensões de tipo
* Eventos
* Estruturas
* Delegados
* Enumerações

**Geralmente, evite esses recursos, a menos que você precise usá-los:**

* Hierarquias de tipos baseadas em herança e herança de implementação
* Nulos e`Unchecked.defaultof<_>`

### <a name="prefer-composition-over-inheritance"></a>Preferir composição sobre herança

A [composição sobre herança](https://en.wikipedia.org/wiki/Composition_over_inheritance) é um idioma muito positivo que o bom código F # pode aderir. O princípio fundamental é que você não deve expor uma classe base e forçar chamadores a herdar dessa classe base para obter a funcionalidade.

### <a name="use-object-expressions-to-implement-interfaces-if-you-dont-need-a-class"></a>Use expressões de objeto para implementar interfaces se você não precisar de uma classe

As [expressões de objeto](../language-reference/object-expressions.md) permitem que você implemente interfaces dinamicamente, associando a interface implementada a um valor sem precisar fazer isso dentro de uma classe. Isso é conveniente, especialmente se você _só_ precisa implementar a interface e não precisa de uma classe completa.

Por exemplo, aqui está o código que é executado no [Ionide](https://ionide.io/) para fornecer uma ação de correção de código se você adicionou um símbolo para o qual você não tem uma `open` instrução:

```fsharp
    let private createProvider () =
        { new CodeActionProvider with
            member this.provideCodeActions(doc, range, context, ct) =
                let diagnostics = context.diagnostics
                let diagnostic = diagnostics |> Seq.tryFind (fun d -> d.message.Contains "Unused open statement")
                let res =
                    match diagnostic with
                    | None -> [||]
                    | Some d ->
                        let line = doc.lineAt d.range.start.line
                        let cmd = createEmpty<Command>
                        cmd.title <- "Remove unused open"
                        cmd.command <- "fsharp.unusedOpenFix"
                        cmd.arguments <- Some ([| doc |> unbox; line.range |> unbox; |] |> ResizeArray)
                        [|cmd |]
                res
                |> ResizeArray
                |> U2.Case1
        }
```

Como não há necessidade de uma classe ao interagir com a API de Visual Studio Code, as expressões de objeto são uma ferramenta ideal para isso. Eles também são valiosos para testes de unidade, quando você deseja stub de uma interface com rotinas de teste de maneira ad hoc.

## <a name="consider-type-abbreviations-to-shorten-signatures"></a>Considere abreviações de tipo para diminuir as assinaturas

As [abreviações de tipo](../language-reference/type-abbreviations.md) são uma maneira conveniente de atribuir um rótulo a outro tipo, como uma assinatura de função ou um tipo mais complexo. Por exemplo, o alias a seguir atribui um rótulo para o que é necessário para definir uma computação com [CNTK](https://docs.microsoft.com/cognitive-toolkit/), uma biblioteca de aprendizado profundo:

```fsharp
open CNTK

// DeviceDescriptor, Variable, and Function all come from CNTK
type Computation = DeviceDescriptor -> Variable -> Function
```

O `Computation` nome é uma maneira conveniente de denotar qualquer função que corresponda à assinatura que está sendo alias. Usar abreviações de tipo como essa é conveniente e permite um código mais sucinto.

### <a name="avoid-using-type-abbreviations-to-represent-your-domain"></a>Evite usar abreviações de tipo para representar seu domínio

Embora as abreviações de tipo sejam convenientes para dar um nome às assinaturas de função, elas podem ser confusas ao abreviar outros tipos. Considere esta abreviação:

```fsharp
// Does not actually abstract integers.
type BufferSize = int
```

Isso pode ser confuso de várias maneiras:

* `BufferSize`Não é uma abstração; Ele é apenas outro nome para um inteiro.
* Se `BufferSize` é exposto em uma API pública, pode facilmente ser interpretado erroneamente para significar mais do que apenas `int` . Geralmente, os tipos de domínio têm vários atributos e não são tipos primitivos como `int` . Essa abreviação viola essa suposição.
* A capitalização de `BufferSize` (PascalCase) implica que esse tipo contém mais dados.
* Esse alias não oferece maior clareza em comparação com o fornecimento de um argumento nomeado para uma função.
* A abreviação não será manifestada no IL compilado; é apenas um inteiro e esse alias é uma construção de tempo de compilação.

```fsharp
module Networking =
    ...
    let send data (bufferSize: int) = ...
```

Em resumo, a armadilha com abreviações de tipo é que elas **não** são abstrações sobre os tipos que estão abreviando. No exemplo anterior, `BufferSize` é apenas um dos `int` bastidores, sem dados adicionais, nem quaisquer benefícios do sistema de tipos além do que `int` já tem.

Uma abordagem alternativa para usar abreviações de tipo para representar um domínio é usar uniões discriminadas de um único caso. O exemplo anterior pode ser modelado da seguinte maneira:

```fsharp
type BufferSize = BufferSize of int
```

Se você escrever um código que opere em termos de `BufferSize` e seu valor subjacente, você precisará construir um em vez de passar em qualquer inteiro arbitrário:

```fsharp
module Networking =
    ...
    let send data (BufferSize size) =
    ...
```

Isso reduz a probabilidade de passar erroneamente um inteiro arbitrário para a `send` função, porque o chamador deve construir um `BufferSize` tipo para encapsular um valor antes de chamar a função.
