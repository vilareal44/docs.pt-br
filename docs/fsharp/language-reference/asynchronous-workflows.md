---
title: Fluxos de trabalho assíncronos
description: 'Saiba mais sobre o suporte na linguagem de programação F # para executar computações de forma assíncrona, que são executadas sem bloquear a execução de outro trabalho.'
ms.date: 08/15/2020
ms.openlocfilehash: ac727fc630f13db01da964131ab39dc242a12cd1
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557705"
---
# <a name="asynchronous-workflows"></a>Fluxos de trabalho assíncronos

Este artigo descreve o suporte em F # para executar cálculos de forma assíncrona, ou seja, sem bloquear a execução de outro trabalho. Por exemplo, as Computações assíncronas podem ser usadas para gravar aplicativos que têm interfaces do usuário que permanecem responsivas aos usuários enquanto o aplicativo executa outro trabalho.

## <a name="syntax"></a>Sintaxe

```fsharp
async { expression }
```

## <a name="remarks"></a>Comentários

Na sintaxe anterior, a computação representada por `expression` é configurada para ser executada de forma assíncrona, ou seja, sem bloquear o thread de computação atual quando operações de suspensão assíncronas, e/s e outras operações assíncronas são executadas. As Computações assíncronas geralmente são iniciadas em um thread em segundo plano enquanto a execução continua no thread atual. O tipo da expressão é `Async<'T>` , em que `'T` é o tipo retornado pela expressão quando a `return` palavra-chave é usada. O código nessa expressão é conhecido como um *bloco assíncrono*ou um *bloco assíncrono*.

Há várias maneiras de programar de forma assíncrona, e a [`Async`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html) classe fornece métodos que dão suporte a vários cenários. A abordagem geral é criar `Async` objetos que representem a computação ou cálculos que você deseja executar de forma assíncrona e, em seguida, iniciar esses cálculos usando uma das funções de disparo. As várias funções de disparo fornecem diferentes maneiras de executar Computações assíncronas e qual delas você usa depende se você deseja usar o thread atual, um thread em segundo plano ou um .NET Framework objeto de tarefa e se há funções de continuação que devem ser executadas quando o cálculo é concluído. Por exemplo, para iniciar uma computação assíncrona no thread atual, você pode usar [`Async.StartImmediate`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html#StartImmediate) . Quando você inicia uma computação assíncrona do thread da interface do usuário, não bloqueia o loop de evento principal que processa ações do usuário, como pressionamentos de teclas e atividade do mouse, para que seu aplicativo permaneça responsivo.

## <a name="asynchronous-binding-by-using-let"></a>Associação assíncrona usando Let!

Em um fluxo de trabalho assíncrono, algumas expressões e operações são síncronas e algumas são computações mais longas que são projetadas para retornar um resultado de forma assíncrona. Ao chamar um método de forma assíncrona, em vez de uma `let` Associação comum, você usa `let!` . O efeito do `let!` é permitir que a execução continue em outros cálculos ou threads à medida que o cálculo está sendo executado. Após o lado direito da `let!` Associação retornar, o restante do fluxo de trabalho assíncrono retomará a execução.

O código a seguir mostra a diferença entre `let` e `let!` . A linha de código que o usa `let` cria apenas uma computação assíncrona como um objeto que você pode executar mais tarde usando, por exemplo, `Async.StartImmediate` ou [`Async.RunSynchronously`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html#RunSynchronously) . A linha de código que o usa `let!` inicia a computação e, em seguida, o thread é suspenso até que o resultado esteja disponível, no ponto em que a execução continua.

```fsharp
// let just stores the result as an asynchronous operation.
let (result1 : Async<byte[]>) = stream.AsyncRead(bufferSize)
// let! completes the asynchronous operation and returns the data.
let! (result2 : byte[])  = stream.AsyncRead(bufferSize)
```

Além disso `let!` , você pode usar o `use!` para executar associações assíncronas. A diferença entre `let!` e `use!` é a mesma que a diferença entre `let` e `use` . Para `use!` , o objeto é Descartado no fechamento do escopo atual. Observe que, na versão atual da linguagem F #, o não `use!` permite que um valor seja inicializado como nulo, embora `use` o faça.

## <a name="asynchronous-primitives"></a>Primitivos assíncronos

Um método que executa uma única tarefa assíncrona e retorna o resultado é chamado de *primitivo assíncrono*, e eles são projetados especificamente para uso com o `let!` . Vários primitivos assíncronos são definidos na biblioteca principal do F #. Dois métodos desse tipo para aplicativos Web são definidos no módulo [`FSharp.Control.WebExtensions`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-webextensions.html) : [`WebRequest.AsyncGetResponse`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-webextensions.html#AsyncGetResponse) e [`WebClient.AsyncDownloadString`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-webextensions.html#AsyncDownloadString) . Os dois primitivos baixam dados de uma página da Web, dada uma URL. `AsyncGetResponse` produz um `System.Net.WebResponse` objeto e `AsyncDownloadString` produz uma cadeia de caracteres que representa o HTML de uma página da Web.

Vários primitivos para operações de e/s assíncronas estão incluídos no [`FSharp.Control.CommonExtensions`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-commonextensions.html) módulo. Esses métodos de extensão da `System.IO.Stream` classe são [`Stream.AsyncRead`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-commonextensions.html#AsyncRead) e [`Stream.AsyncWrite`](hhttps://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-commonextensions.html#AsyncWrite) .

Você também pode escrever seus próprios primitivos assíncronos definindo uma função cujo corpo completo é colocado em um bloco assíncrono.

Para usar métodos assíncronos no .NET Framework que são criados para outros modelos assíncronos com o modelo de programação de F # assíncrono, você cria uma função que retorna um objeto F # `Async` . A biblioteca F # tem funções que facilitam essa tarefa.

Um exemplo de uso de fluxos de trabalho assíncronos está incluído aqui; Há muitos outros na documentação para os métodos da [classe Async](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html).

Este exemplo mostra como usar fluxos de trabalho assíncronos para executar cálculos em paralelo.

No exemplo de código a seguir, uma função `fetchAsync` Obtém o texto HTML retornado de uma solicitação da Web. A `fetchAsync` função contém um bloco de código assíncrono. Quando uma associação é feita ao resultado de um primitivo assíncrono, nesse caso [`AsyncDownloadString`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-webextensions.html#AsyncDownloadString) , `let!` é usado em vez de `let` .

Você usa a função [`Async.RunSynchronously`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html#RunSynchronously) para executar uma operação assíncrona e aguardar seu resultado. Por exemplo, você pode executar várias operações assíncronas em paralelo usando a [`Async.Parallel`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html#Parallel) função junto com a `Async.RunSynchronously` função. A `Async.Parallel` função usa uma lista dos `Async` objetos, configura o código para cada objeto de `Async` tarefa a ser executado em paralelo e retorna um `Async` objeto que representa a computação paralela. Assim como para uma única operação, você chama `Async.RunSynchronously` para iniciar a execução.

A `runAll` função inicia três fluxos de trabalho assíncronos em paralelo e aguarda até que todos tenham sido concluídos.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet8003.fs)]

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
- [Expressões de computação](computation-expressions.md)
- [Classe Control. Async](https://msdn.microsoft.com/visualfsharpdocs/conceptual/control.async-class-%5bfsharp%5d)
