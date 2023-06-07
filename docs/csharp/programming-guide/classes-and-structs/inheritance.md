---
title: Herança – Guia de Programação em C#
description: A herança em C# permite que você crie novas classes que reutilizam, estendem e modificam o comportamento definido em outras classes.
ms.date: 02/07/2020
helpviewer_keywords:
- abstract methods [C#]
- abstract classes [C#]
- inheritance [C#]
- derived classes [C#]
- virtual methods [C#]
- C# language, inheritance
ms.assetid: 81d64ee4-50f9-4d6c-a8dc-257c348d2eea
ms.openlocfilehash: 6b94f58801af188655474f9c7de76b79381e721d
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864249"
---
# <a name="inheritance-c-programming-guide"></a>Herança (Guia de Programação em C#)

A herança, assim como o encapsulamento e o polimorfismo, é uma das três principais características da programação orientada ao objeto. A herança permite que você crie novas classes que reutilizam, estendem e modificam o comportamento definido em outras classes. A classe cujos membros são herdados é chamada *classe base* e a classe que herda esses membros é chamada *classe derivada*. Uma classe derivada pode ter apenas uma classe base direta. No entanto, a herança é transitiva. Se `ClassC` é derivado de, `ClassB` e `ClassB` é derivado de `ClassA` , `ClassC` herda os membros declarados em `ClassB` e `ClassA` .

> [!NOTE]
> Structs não dão suporte a herança, mas podem implementar interfaces. Para obter mais informações, consulte [interfaces](../interfaces/index.md).

Conceitualmente, uma classe derivada é uma especialização da classe base. Por exemplo, se tiver uma classe base `Animal`, você pode ter uma classe derivada chamada `Mammal` e outra classe derivada chamada `Reptile`. Um `Mammal` é um `Animal` e um `Reptile` é um `Animal`, mas cada classe derivada representa especializações diferentes da classe base.

Declarações de interface podem definir uma implementação padrão para seus membros. Essas implementações são herdadas por interfaces derivadas e por classes que implementam essas interfaces. Para obter mais informações sobre métodos de interface padrão, consulte o artigo sobre [interfaces](../../language-reference/keywords/interface.md) na seção referência de idioma.

Quando você define uma classe para derivar de outra classe, a classe derivada obtém implicitamente todos os membros da classe base, exceto por seus construtores e finalizadores. A classe derivada reutiliza o código na classe base sem precisar reimplementá-lo. Você pode adicionar mais membros na classe derivada. A classe derivada estende a funcionalidade da classe base.

A ilustração a seguir mostra uma classe `WorkItem` que representa um item de trabalho em um processo comercial. Como todas as classes, ela deriva de <xref:System.Object?displayProperty=nameWithType> e herda todos os seus métodos. `WorkItem` adiciona cinco membros próprios. Esses membros incluem um construtor, porque os construtores não são herdados. A classe `ChangeRequest` herda de `WorkItem` e representa um tipo específico de item de trabalho. `ChangeRequest` adiciona mais dois membros aos membros que herda de `WorkItem` e de <xref:System.Object>. Ele deve adicionar seu próprio construtor e também adiciona `originalItemID`. A propriedade `originalItemID` permite que a instância `ChangeRequest` seja associada ao `WorkItem` original a que a solicitação de alteração se aplica.

![Diagrama que mostra a herança de classe](./media/inheritance/class-inheritance-diagram.png)

O exemplo a seguir mostra como as relações entre as classes demonstradas na ilustração anterior são expressos em C#. O exemplo também mostra como `WorkItem` substitui o método virtual <xref:System.Object.ToString%2A?displayProperty=nameWithType> e como a classe `ChangeRequest` herda a implementação de `WorkItem` do método. O primeiro bloco define as classes:

[!code-csharp[DefineClasses](~/samples/snippets/csharp/objectoriented/inheritance.cs#Classes)]

Este próximo bloco mostra como usar as classes base e derivadas:

[!code-csharp[UseClasses](~/samples/snippets/csharp/objectoriented/inheritance.cs#UseClasses)]

## <a name="abstract-and-virtual-methods"></a>Métodos abstratos e virtuais

Quando uma classe base declara um método como [`virtual`](../../language-reference/keywords/virtual.md) , uma classe derivada pode [`override`](../../language-reference/keywords/override.md) o método com sua própria implementação. Se uma classe base declarar um membro como [`abstract`](../../language-reference/keywords/abstract.md) , esse método deverá ser substituído em qualquer classe não abstrata que herda diretamente dessa classe. Se uma classe derivada for abstrato, ele herdará membros abstratos sem implementá-los. Membros abstratos e virtuais são a base do polimorfismo, que é a segunda característica principal da programação orientada a objetos. Para obter mais informações, consulte [Polimorfismo](./polymorphism.md).

## <a name="abstract-base-classes"></a>Classes base abstratas

Você poderá declarar uma classe como [abstrata](../../language-reference/keywords/abstract.md) se quiser impedir a instanciação direta usando o operador [new](../../language-reference/operators/new-operator.md). Uma classe abstrata pode ser usada somente se uma nova classe for derivada dela. Uma classe abstrata pode conter uma ou mais assinaturas de método que também são declaradas como abstratas. Essas assinaturas especificam os parâmetros e o valor retornado, mas não têm nenhuma implementação (corpo do método). Uma classe abstrata não precisa conter membros abstratos; no entanto, se uma classe contiver um membro abstrato, a própria classe deverá ser declarada como abstrata. Classes derivadas que não são abstratas em si devem fornecer a implementação para qualquer método abstrato de uma classe base abstrata. Para obter mais informações, consulte [classes abstratas e lacradas e membros de classe](abstract-and-sealed-classes-and-class-members.md).

## <a name="interfaces"></a>Interfaces

Uma *interface* é um tipo de referência que define um conjunto de membros. Todas as classes e estruturas que implementam essa interface devem implementar esse conjunto de membros. Uma interface pode definir uma implementação padrão para qualquer ou todos esses membros. Uma classe pode implementar várias interfaces, mesmo que ela possa derivar de apenas uma classe base direta.

As interfaces são usadas para definir recursos específicos para classes que não necessariamente têm uma relação "é um". Por exemplo, a <xref:System.IEquatable%601?displayProperty=nameWithType> interface pode ser implementada por qualquer classe ou struct para determinar se dois objetos do tipo são equivalentes (no entanto, o tipo define a equivalência). <xref:System.IEquatable%601>Não implica o mesmo tipo de relação "é um" que existe entre uma classe base e uma classe derivada (por exemplo, um `Mammal` é um `Animal` ). Para obter mais informações, consulte [interfaces](../interfaces/index.md).

## <a name="preventing-further-derivation"></a>Impedindo maior derivação  

Uma classe pode impedir que outras classes herdem dela ou de qualquer um de seus membros, declarando-se ou o membro como [`sealed`](../../language-reference/keywords/sealed.md) . Para obter mais informações, consulte [classes abstratas e lacradas e membros de classe](./abstract-and-sealed-classes-and-class-members.md).

## <a name="derived-class-hiding-of-base-class-members"></a>Ocultação de classe derivada de membros de classe base  

Uma classe derivada pode ocultar membros da classe base declarando membros com mesmo nome e assinatura. O [`new`](../../language-reference/keywords/new-modifier.md) modificador pode ser usado para indicar explicitamente que o membro não deve ser uma substituição do membro base. O uso de [`new`](../../language-reference/keywords/new-modifier.md) não é necessário, mas um aviso do compilador será gerado se [`new`](../../language-reference/keywords/new-modifier.md) não for usado. Para obter mais informações, consulte [Controle de versão com as palavras-chave override e new](./versioning-with-the-override-and-new-keywords.md) e [Quando usar as palavras-chave override e new](./knowing-when-to-use-override-and-new-keywords.md).

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Classes e structs](./index.md)
- [class](../../language-reference/keywords/class.md)
