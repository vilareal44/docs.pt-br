---
title: Implementação de interface explícita – Guia de Programação em C#
description: Uma classe pode implementar interfaces que contêm um membro com a mesma assinatura em C#. A implementação explícita cria um membro de classe específico a uma interface.
ms.date: 01/24/2020
helpviewer_keywords:
- explicit interfaces [C#]
- interfaces [C#], explicit
ms.assetid: 181c901f-0d4c-4f29-97fc-895079617bf2
ms.openlocfilehash: a6ec328c08d1da84a11431d9400a094df8c72223
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303081"
---
# <a name="explicit-interface-implementation-c-programming-guide"></a>Implementação de interface explícita (Guia de Programação em C#)

Caso uma [classe](../../language-reference/keywords/class.md) implemente duas interfaces que contêm um membro com a mesma assinatura, logo, implementar esse membro na classe fará com que as duas interfaces usem tal membro como sua implementação. No exemplo a seguir, todas as chamadas para `Paint` invocam o mesmo método. Este primeiro exemplo define os tipos:

[!code-csharp[DefineSimpleTypes](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#DefineTypes)]

O exemplo a seguir chama os métodos:

[!code-csharp[DefineSimpleTypes](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallMethods)]

Quando dois membros de interface não executam a mesma função, ele leva a uma implementação incorreta de uma ou de ambas as interfaces. É possível implementar um membro de interface explicitamente, criando um membro de classe que é chamado apenas por meio da interface e é específico para essa interface. Nomeie o membro da classe com o nome da interface e um ponto final. Por exemplo:

[!code-csharp[DefineExplicitImplementation](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#ExplicitImplementation)]

O membro da classe `IControl.Paint` está disponível somente por meio da interface `IControl` e `ISurface.Paint` está disponível apenas por meio do `ISurface`. Ambas as implementações de método são separadas e nenhuma está disponível diretamente na classe. Por exemplo:

[!code-csharp[CallExplicitImplementation](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallExplicitImplementation)]

A implementação explícita também é usada para resolver casos em que duas interfaces declaram Membros diferentes do mesmo nome, como uma propriedade e um método. Para implementar ambas as interfaces, uma classe deve usar a implementação explícita para a propriedade `P` ou o método `P` , ou ambos, para evitar um erro do compilador. Por exemplo:

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#NameCollision)]

A partir do [C# 8,0](../../whats-new/csharp-8.md#default-interface-methods), você pode definir uma implementação para membros declarados em uma interface. Se uma classe herdar uma implementação de método de uma interface, esse método só poderá ser acessado por meio de uma referência do tipo de interface. O membro herdado não aparece como parte da interface pública. O exemplo a seguir define uma implementação padrão para um método de interface:

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#DefaultImplementation)]

O exemplo a seguir invoca a implementação padrão:

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallDefaultImplementation)]

Qualquer classe que implemente a `IControl` interface pode substituir o `Paint` método padrão, seja como um método público ou como uma implementação de interface explícita.

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Classes e structs](../classes-and-structs/index.md)
- [Interfaces](./index.md)
- [Herança](../classes-and-structs/inheritance.md)
