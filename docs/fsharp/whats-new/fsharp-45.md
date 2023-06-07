---
title: 'O que há de novo no guia F # 4,5-F #'
description: 'Obtenha uma visão geral dos novos recursos disponíveis em F # 4,5.'
ms.date: 11/27/2019
ms.openlocfilehash: 2c978c66a4bf231398508cbc1cbb8839228ea8e9
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202361"
---
# <a name="whats-new-in-f-45"></a>O que há de novo no F # 4,5

O f # 4,5 adiciona vários aprimoramentos à linguagem F #. Muitos desses recursos foram adicionados juntos para permitir que você escreva código eficiente em F # e também garanta que esse código seja seguro. Fazer isso significa adicionar alguns conceitos à linguagem e uma quantidade significativa de análise do compilador ao usar essas construções.

## <a name="get-started"></a>Introdução

O F # 4,5 está disponível em todas as distribuições do .NET Core e ferramentas do Visual Studio. [Introdução ao F #](../get-started/index.md) para saber mais.

## <a name="span-and-byref-like-structs"></a>Estruturas do tipo "span" e "ByRef"

O <xref:System.Span%601> tipo introduzido no .NET Core permite que você represente buffers na memória de maneira fortemente tipada, que agora é permitido em f #, começando com f # 4,5. O exemplo a seguir mostra como você pode usar novamente uma função operando em uma <xref:System.Span%601> com diferentes representações de buffer:

```fsharp
let safeSum (bytes: Span<byte>) =
    let mutable sum = 0
    for i in 0 .. bytes.Length - 1 do
        sum <- sum + int bytes.[i]
    sum

// managed memory
let arrayMemory = Array.zeroCreate<byte>(100)
let arraySpan = new Span<byte>(arrayMemory)

safeSum(arraySpan) |> printfn "res = %d"

// native memory
let nativeMemory = Marshal.AllocHGlobal(100);
let nativeSpan = new Span<byte>(nativeMemory.ToPointer(), 100)

safeSum(nativeSpan) |> printfn "res = %d"
Marshal.FreeHGlobal(nativeMemory)

// stack memory
let mem = NativePtr.stackalloc<byte>(100)
let mem2 = mem |> NativePtr.toVoidPtr
let stackSpan = Span<byte>(mem2, 100)

safeSum(stackSpan) |> printfn "res = %d"
```

Um aspecto importante disso é que o span e outras [estruturas do tipo ByRef](../language-reference/structures.md#byreflike-structs) têm uma análise estática muito rígida executada pelo compilador que restringe seu uso de maneiras que você talvez ache inesperado. Essa é a compensação fundamental entre o desempenho, a expressividade e a segurança introduzidos no F # 4,5.

## <a name="revamped-byrefs"></a>Byrefs remodelados

Antes do F # 4,5, os [byrefs](../language-reference/byrefs.md) em f # eram inseguros e não são seguros para vários aplicativos. Os problemas de correção em relação a byrefs foram resolvidos em F # 4,5 e a mesma análise estática feita para estruturas de span e de ByRef também foi aplicada.

### <a name="inreft-and-outreft"></a>inref<> e outref< ' t>

Para representar a noção de um ponteiro gerenciado somente leitura, somente gravação e leitura/gravação, o F # 4,5 apresenta os `inref<'T>` `outref<'T>` tipos, para representar ponteiros somente leitura e somente gravação, respectivamente. Cada uma tem uma semântica diferente. Por exemplo, você não pode gravar em um `inref<'T>` :

```fsharp
let f (dt: inref<DateTime>) =
    dt <- DateTime.Now // ERROR - cannot write to an inref!
```

Por padrão, a inferência de tipos inferirá ponteiros gerenciados como `inref<'T>` em linha com a natureza imutável do código F #, a menos que algo já tenha sido declarado como mutável. Para tornar algo gravável, você precisará declarar um tipo como `mutable` antes de passar seu endereço para uma função ou um membro que o manipule. Para saber mais, confira [byrefs](../language-reference/byrefs.md).

## <a name="readonly-structs"></a>Structs ReadOnly

A partir do F # 4,5, você pode anotar uma struct com <xref:System.Runtime.CompilerServices.IsReadOnlyAttribute> como tal:

```fsharp
[<IsReadOnly; Struct>]
type S(count1: int, count2: int) =
    member x.Count1 = count1
    member x.Count2 = count2
```

Isso não permite que você declare um membro mutável na estrutura e emita metadados que permitem que o F # e o C# o tratem como ReadOnly quando consumidos de um assembly. Para saber mais, consulte [structs ReadOnly](../language-reference/structures.md#readonly-structs).

## <a name="void-pointers"></a>Ponteiros void

O `voidptr` tipo é adicionado a F # 4,5, assim como as seguintes funções:

* `NativePtr.ofVoidPtr`para converter um ponteiro void em um ponteiro int nativo
* `NativePtr.toVoidPtr`para converter um ponteiro int nativo para um ponteiro void

Isso é útil ao interoperar com um componente nativo que utiliza ponteiros void.

## <a name="the-match-keyword"></a>A palavra-chave `match!`

A `match!` palavra-chave aprimora a correspondência de padrões quando dentro de uma expressão de computação:

```fsharp
// Code that returns an asynchronous option
let checkBananaAsync (s: string) =
    async {
        if s = "banana" then
            return Some s
        else
            return None
    }

// Now you can use 'match!'
let funcWithString (s: string) =
    async {
        match! checkBananaAsync s with
        | Some bananaString -> printfn "It's banana!"
        | None -> printfn "%s" s
}
```

Isso permite que você encurte o código que geralmente envolve a combinação de opções (ou outros tipos) com expressões de computação, como Async. Para saber mais, consulte [correspondência!](../language-reference/computation-expressions.md#match).

## <a name="relaxed-upcasting-requirements-in-array-list-and-sequence-expressions"></a>Requisitos de upcast reduzidos em expressões de matriz, lista e sequência

A combinação de tipos em que um pode herdar de outro dentro de expressões de matriz, lista e sequência exigiu tradicionalmente que você converta qualquer tipo derivado em seu tipo pai com `:>` ou `upcast` . Isso agora é relaxado, demonstrado da seguinte maneira:

```fsharp
let x0 : obj list  = [ "a" ] // ok pre-F# 4.5
let x1 : obj list  = [ "a"; "b" ] // ok pre-F# 4.5
let x2 : obj list  = [ yield "a" :> obj ] // ok pre-F# 4.5

let x3 : obj list  = [ yield "a" ] // Now ok for F# 4.5, and can replace x2
```

## <a name="indentation-relaxation-for-array-and-list-expressions"></a>Relaxamento de recuo para expressões de matriz e lista

Antes do F # 4,5, você precisava recuar excessivamente as expressões de matriz e de lista quando passavam como argumentos para chamadas de método. Isso não é mais necessário:

```fsharp
module NoExcessiveIndenting =
    System.Console.WriteLine(format="{0}", arg = [|
        "hello"
    |])
    System.Console.WriteLine([|
        "hello"
    |])
```
