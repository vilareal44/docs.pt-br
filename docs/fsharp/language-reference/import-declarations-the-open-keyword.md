---
title: 'Declarações de importação: a palavra-chave open'
description: 'Saiba mais sobre as declarações de importação de F # e como elas especificam um módulo ou namespace cujos elementos você pode referenciar sem usar um nome totalmente qualificado.'
ms.date: 08/15/2020
ms.openlocfilehash: 6420df071f86159c44606c2710331d5f587023cc
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557601"
---
# <a name="import-declarations-the-open-keyword"></a>Importar declarações: a `open` palavra-chave

Uma *declaração de importação* especifica um módulo ou namespace cujos elementos você pode referenciar sem usar um nome totalmente qualificado.

## <a name="syntax"></a>Sintaxe

```fsharp
open module-or-namespace-name
```

## <a name="remarks"></a>Comentários

Fazer referência ao código usando o namespace totalmente qualificado ou o caminho do módulo toda vez pode criar código que seja difícil de gravar, ler e manter. Em vez disso, você pode usar a `open` palavra-chave para módulos e namespaces usados com frequência para que, ao fazer referência a um membro desse módulo ou namespace, você possa usar a forma abreviada do nome em vez do nome totalmente qualificado. Essa palavra-chave é semelhante à `using` palavra-chave em C#, `using namespace` em Visual C++ e `Imports` no Visual Basic.

O módulo ou namespace fornecido deve estar no mesmo projeto ou em um projeto ou assembly referenciado. Se não estiver, você poderá adicionar uma referência ao projeto ou usar a `-reference` opção de linha de comando (ou sua abreviação, `-r` ). Para obter mais informações, consulte [Opções do compilador](compiler-options.md).

A declaração de importação torna os nomes disponíveis no código que segue a declaração, até o final do namespace, módulo ou arquivo de circunscrição.

Quando você usa várias declarações de importação, elas devem aparecer em linhas separadas.

O código a seguir mostra o uso da `open` palavra-chave para simplificar o código.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6801.fs)]

O compilador F # não emite um erro ou aviso quando as ambiguidades ocorrem quando o mesmo nome ocorre em mais de um módulo ou namespace aberto. Quando ocorrerem ambiguidades, F # dará preferência ao módulo ou namespace mais recentemente aberto. Por exemplo, no código a seguir, `empty` significa `Seq.empty` , embora `empty` esteja localizado nos `List` `Seq` módulos e.

```fsharp
open List
open Seq
printfn "%A" empty
```

Portanto, tenha cuidado ao abrir módulos ou namespaces, como `List` ou `Seq` que contenham Membros com nomes idênticos; em vez disso, considere usar os nomes qualificados. Você deve evitar qualquer situação na qual o código seja dependente da ordem das declarações de importação.

## <a name="namespaces-that-are-open-by-default"></a>Namespaces que estão abertos por padrão

Alguns namespaces são frequentemente usados no código F # que eles são abertos implicitamente sem a necessidade de uma declaração de importação explícita. A tabela a seguir mostra os namespaces que estão abertos por padrão.

|Namespace|Descrição|
|---------|-----------|
|`FSharp.Core`|Contém definições de tipo F # básicas para tipos internos, como `int` e `float` .|
|`FSharp.Core.Operators`|Contém operações aritméticas básicas, como `+` e `*` .|
|`FSharp.Collections`|Contém classes de coleção imutáveis, como `List` e `Array` .|
|`FSharp.Control`|Contém tipos de construções de controle, como avaliação lenta e fluxos de trabalho assíncronos.|
|`FSharp.Text`|Contém funções para e/s formatada, como a `printf` função.|

## <a name="autoopen-attribute"></a>Atributo AutoOpen

Você pode aplicar o `AutoOpen` atributo a um assembly se desejar abrir automaticamente um namespace ou módulo quando o assembly for referenciado. Você também pode aplicar o `AutoOpen` atributo a um módulo para abrir esse módulo automaticamente quando o namespace ou o módulo pai for aberto. Para obter mais informações, consulte [AutoOpenAttribute](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-autoopenattribute.html).

## <a name="requirequalifiedaccess-attribute"></a>Atributo RequireQualifiedAccess

Alguns módulos, registros ou tipos de União podem especificar o `RequireQualifiedAccess` atributo. Ao referenciar elementos desses módulos, registros ou uniões, você deve usar um nome qualificado, independentemente de você incluir uma declaração de importação. Se você usar esse atributo estrategicamente em tipos que definem nomes comumente usados, você ajudará a evitar colisões de nomes e, portanto, tornará o código mais resiliente às alterações nas bibliotecas. Para obter mais informações, consulte [RequireQualifiedAccessAttribute](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-requirequalifiedaccessattribute.html).

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
- [Namespaces](namespaces.md)
- [Módulos](modules.md)
