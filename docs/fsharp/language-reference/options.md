---
title: Opções
description: 'Saiba como usar tipos de opção F # quando um valor real pode não existir para um valor ou variável nomeada.'
ms.date: 08/13/2020
ms.openlocfilehash: 0618590c10f6ecac51a23483ca0ab6cd66f2df4f
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557562"
---
# <a name="options"></a>Opções

O tipo de opção em F # é usado quando um valor real pode não existir para um valor ou uma variável nomeada. Uma opção tem um tipo subjacente e pode conter um valor desse tipo, ou pode não ter um valor.

## <a name="remarks"></a>Comentários

O código a seguir ilustra uma função que gera um tipo de opção.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1404.fs)]

Como você pode ver, se a entrada `a` for maior que 0, `Some(a)` será gerada.  Caso contrário, `None` será gerado.

O valor `None` é usado quando uma opção não tem um valor real. Caso contrário, a expressão `Some( ... )` fornecerá a opção um valor. Os valores `Some` e `None` são úteis na correspondência de padrões, como na função a seguir `exists` , que retorna `true` se a opção tem um valor e se não tem `false` .

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1401.fs)]

## <a name="using-options"></a>Usando opções

As opções são usadas normalmente quando uma pesquisa não retorna um resultado correspondente, conforme mostrado no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1403.fs)]

No código anterior, uma lista é pesquisada recursivamente. A função `tryFindMatch` usa uma função de predicado `pred` que retorna um valor booliano e uma lista a ser pesquisada. Se um elemento que satisfizer o predicado for encontrado, a recursão terminará e a função retornará o valor como uma opção na expressão `Some(head)` . A recursão termina quando a lista vazia é correspondida. Nesse ponto, o valor `head` não foi encontrado e `None` é retornado.

Muitas funções de biblioteca F # que pesquisam uma coleção de um valor que pode ou não existir retornam o `option` tipo. Por convenção, essas funções começam com o `try` prefixo, por exemplo, [`Seq.tryFindIndex`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-seqmodule.html#tryFindIndex) .

As opções também podem ser úteis quando um valor pode não existir, por exemplo, se for possível que uma exceção seja lançada quando você tentar construir um valor. O exemplo de código a seguir ilustra isso.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1402.fs)]

A `openFile` função no exemplo anterior tem tipo `string -> File option` , pois ela retornará um `File` objeto se o arquivo for aberto com êxito e `None` se ocorrer uma exceção. Dependendo da situação, talvez não seja uma opção de design apropriada para capturar uma exceção em vez de permitir que ela se propague.

Além disso, ainda é possível passar `null` ou um valor que seja nulo para o `Some` caso de uma opção. Isso geralmente é evitado e normalmente está em programação F # rotineira, mas é possível devido à natureza dos tipos de referência no .NET.

## <a name="option-properties-and-methods"></a>Métodos e propriedades de opção

O tipo de opção oferece suporte às propriedades e métodos a seguir.

|Propriedade ou método|Type|Descrição|
|------------------|----|-----------|
|[Nenhuma](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-fsharpoption-1.html#None)|`'T option`|Uma propriedade estática que permite que você crie um valor de opção com o `None` valor.|
|[IsNone](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-fsharpoption-1.html#IsNone)|`bool`|Retorna `true` se a opção tem o `None` valor.|
|[Issome](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-fsharpoption-1.html#IsSome)|`bool`|Retorna `true` se a opção tem um valor que não é `None` .|
|[Qualquer](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-fsharpoption-1.html#Some)|`'T option`|Um membro estático que cria uma opção que tem um valor que não é `None` .|
|[Valor](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-fsharpoption-1.html#Value)|`'T`|Retorna o valor subjacente ou gera um `System.NullReferenceException` se o valor for `None` .|

## <a name="option-module"></a>Módulo de opção

Há um módulo, [opção](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html), que contém funções úteis que executam operações em opções. Algumas funções repetim a funcionalidade das propriedades, mas são úteis em contextos onde uma função é necessária. [Option. issome](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#isSome) e [Option. IsNone](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#isNone) são ambas funções de módulo que testam se uma opção contém um valor. [Opção. Get](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#get) Obtém o valor, se houver um. Se não houver nenhum valor, ele será acionado `System.ArgumentException` .

A função [Option. bind](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#bind) executa uma função no valor, se houver um valor. A função deve usar exatamente um argumento e seu tipo de parâmetro deve ser o tipo de opção. O valor de retorno da função é outro tipo de opção.

O módulo de opção também inclui funções que correspondem às funções que estão disponíveis para listas, matrizes, sequências e outros tipos de coleção. Essas funções incluem [`Option.map`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#map) ,,,,, [`Option.iter`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#iter) [`Option.forall`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#forall) [`Option.exists`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#exists) [`Option.foldBack`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#foldBack) [`Option.fold`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#fold) e [`Option.count`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#count) . Essas funções permitem que as opções sejam usadas como uma coleção de zero ou um elemento. Para obter mais informações e exemplos, consulte a discussão sobre funções de coleção em [listas](lists.md).

## <a name="converting-to-other-types"></a>Convertendo para outros tipos

As opções podem ser convertidas em listas ou matrizes. Quando uma opção é convertida em qualquer uma dessas estruturas de dados, a estrutura de dados resultante tem zero ou um elemento. Para converter uma opção em uma matriz, use [`Option.toArray`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#toArray) . Para converter uma opção em uma lista, use [`Option.toList`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-optionmodule.html#toList) .

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
- [Tipos F#](fsharp-types.md)
