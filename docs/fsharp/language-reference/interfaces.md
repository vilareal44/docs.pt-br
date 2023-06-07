---
title: Interfaces
description: 'Saiba como as interfaces F # especificam conjuntos de membros relacionados que outras classes implementam.'
ms.date: 08/15/2020
ms.openlocfilehash: 36272b52fcff83e8e8a54ccc4e6ecd1252a91819
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558121"
---
# <a name="interfaces"></a>Interfaces

*Interfaces* especificam conjuntos de membros relacionados que outras classes implementam.

## <a name="syntax"></a>Sintaxe

```fsharp
// Interface declaration:
[ attributes ]
type [accessibility-modifier] interface-name =
    [ interface ]     [ inherit base-interface-name ...]
    abstract member1 : [ argument-types1 -> ] return-type1
    abstract member2 : [ argument-types2 -> ] return-type2
    ...
[ end ]

// Implementing, inside a class type definition:
interface interface-name with
    member self-identifier.member1argument-list = method-body1
    member self-identifier.member2argument-list = method-body2

// Implementing, by using an object expression:
[ attributes ]
let class-name (argument-list) =
    { new interface-name with
        member self-identifier.member1argument-list = method-body1
        member self-identifier.member2argument-list = method-body2
        [ base-interface-definitions ]
    }
    member-list
```

## <a name="remarks"></a>Comentários

Declarações de interface assemelham-se a declarações de classe, exceto que nenhum membro é implementado. Em vez disso, todos os membros são abstratos, conforme indicado pela palavra-chave `abstract` . Você não fornece um corpo de método para métodos abstratos. No entanto, você pode fornecer uma implementação padrão, incluindo também uma definição separada do membro como um método junto com a `default` palavra-chave. Isso é equivalente a criar um método virtual em uma classe base em outras linguagens .NET. Tal método virtual pode ser substituído em classes que implementam a interface.

A acessibilidade padrão para interfaces é `public` .

Opcionalmente, você pode atribuir um nome a cada parâmetro de método usando a sintaxe F # normal:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet24032.fs)]

No exemplo acima `ISprintable` , o `Print` método tem um único parâmetro do tipo `string` com o nome `format` .

Há duas maneiras de implementar interfaces: usando expressões de objeto e usando tipos de classe. Em ambos os casos, o tipo de classe ou expressão de objeto fornece corpos de método para métodos abstratos da interface. As implementações são específicas para cada tipo que implementa a interface. Portanto, os métodos de interface em diferentes tipos podem ser diferentes uns dos outros.

As palavras-chave `interface` e `end` , que marcam o início e o final da definição, são opcionais quando você usa a sintaxe leve. Se você não usar essas palavras-chave, o compilador tentará inferir se o tipo é uma classe ou uma interface analisando as construções que você usa. Se você definir um membro ou usar outra sintaxe de classe, o tipo será interpretado como uma classe.

O estilo de codificação .NET é iniciar todas as interfaces com um capital `I` .

Você pode especificar vários parâmetros de duas maneiras: F #-Style e. Estilo de rede. Ambas serão compiladas da mesma forma para os consumidores do .NET, mas o estilo F # forçará os chamadores F # a usar o aplicativo de parâmetro de estilo F # e o. O estilo NET forçará os chamadores F # a usar o aplicativo de argumento de tupla.

```fsharp
type INumeric1 =
    abstract Add: x: int -> y: int -> int

type INumeric2 =
    abstract Add: x: int * y: int -> int
```

## <a name="implementing-interfaces-by-using-class-types"></a>Implementando interfaces usando tipos de classe

Você pode implementar uma ou mais interfaces em um tipo de classe usando a `interface` palavra-chave, o nome da interface e a `with` palavra-chave, seguida pelas definições de membro da interface, conforme mostrado no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2801.fs)]

Implementações de interface são herdadas, portanto, quaisquer classes derivadas não precisam reimplementá-las.

## <a name="calling-interface-methods"></a>Chamando métodos de interface

Métodos de interface podem ser chamados somente por meio da interface, não por meio de qualquer objeto do tipo que implementa a interface. Portanto, talvez você precise fazer a conversão para o tipo de interface usando o `:>` operador ou o `upcast` operador para chamar esses métodos.

Para chamar o método de interface quando você tem um objeto do tipo `SomeClass` , você deve fazer o Convert do objeto para o tipo de interface, conforme mostrado no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2802.fs)]

Uma alternativa é declarar um método no objeto que faz a conversão e chama o método de interface, como no exemplo a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2803.fs)]

## <a name="implementing-interfaces-by-using-object-expressions"></a>Implementando interfaces usando expressões de objeto

As expressões de objeto fornecem uma maneira curta de implementar uma interface. Eles são úteis quando você não precisa criar um tipo nomeado e só deseja um objeto que dê suporte aos métodos de interface, sem nenhum método adicional. Uma expressão de objeto é ilustrada no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2804.fs)]

## <a name="interface-inheritance"></a>Herança de interface

As interfaces podem herdar de uma ou mais interfaces base.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2805.fs)]

## <a name="see-also"></a>Confira também

- [Referência de linguagem F #](index.md)
- [Expressões de objeto](object-expressions.md)
- [Classes](classes.md)
