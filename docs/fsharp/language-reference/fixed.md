---
title: A palavra-chave Fixed
description: 'Saiba como você pode "fixar" um local na pilha para evitar a coleta com a palavra-chave "fixed" do F #.'
ms.date: 08/15/2020
ms.openlocfilehash: 786ffd706c243fc83f8fb3afc2201d2a34536372
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559174"
---
# <a name="the-fixed-keyword"></a>A palavra-chave Fixed

A `fixed` palavra-chave permite que você "fixe" um local na pilha para impedir que ele seja coletado ou movido durante a coleta de lixo.  Ele é usado para cenários de programação de baixo nível.

## <a name="syntax"></a>Sintaxe

```fsharp
use ptr = fixed expression
```

## <a name="remarks"></a>Comentários

Isso estende a sintaxe de expressões para permitir a extração de um ponteiro e a vinculação a um nome que é impedido de ser coletado ou movido durante a coleta de lixo.  

Um ponteiro de uma expressão é corrigido por meio da `fixed` palavra-chave é associado a um identificador por meio da `use` palavra-chave.  A semântica disso é semelhante ao gerenciamento de recursos por meio da `use` palavra-chave.  O ponteiro é fixo enquanto está no escopo e, quando está fora do escopo, ele não é mais fixo.  `fixed` Não pode ser usado fora do contexto de uma `use` associação.  Você deve associar o ponteiro a um nome com `use` .

O uso de `fixed` deve ocorrer dentro de uma expressão em uma função ou um método.  Ele não pode ser usado em um escopo de nível de script ou de módulo.

Como todo o código de ponteiro, esse é um recurso não seguro e emitirá um aviso quando usado.

## <a name="example"></a>Exemplo

```fsharp
open Microsoft.FSharp.NativeInterop

type Point = { mutable X: int; mutable Y: int}

let squareWithPointer (p: nativeptr<int>) =
    // Dereference the pointer at the 0th address.
    let mutable value = NativePtr.get p 0

    // Perform some work
    value <- value * value

    // Set the value in the pointer at the 0th address.
    NativePtr.set p 0 value

let pnt = { X = 1; Y = 2 }
printfn "pnt before - X: %d Y: %d" pnt.X pnt.Y // prints 1 and 2

// Note that the use of 'fixed' is inside a function.
// You cannot fix a pointer at a script-level or module-level scope.
let doPointerWork() =
    use ptr = fixed &pnt.Y

    // Square the Y value
    squareWithPointer ptr
    printfn "pnt after - X: %d Y: %d" pnt.X pnt.Y // prints 1 and 4

doPointerWork()
```

## <a name="see-also"></a>Confira também

- [Módulo NativePtr](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-nativeinterop-nativeptrmodule.html)
