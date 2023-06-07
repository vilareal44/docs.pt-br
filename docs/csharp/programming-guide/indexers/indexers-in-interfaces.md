---
title: Indexadores em interfaces – Guia de Programação em C#
description: Indexadores podem ser declarados em uma interface em C#. Saiba como os acessadores de indexadores de interface diferem dos acessadores de indexadores de classe.
ms.date: 02/08/2020
helpviewer_keywords:
- indexers [C#], in interfaces
- accessors [C#], indexers
ms.assetid: e16b54bd-4a83-4f52-bd75-65819fca79e8
ms.openlocfilehash: ec77843bdf3181a543bd6c02cfb034b21ded1ae7
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303095"
---
# <a name="indexers-in-interfaces-c-programming-guide"></a>Indexadores em interfaces (Guia de Programação em C#)

Os indexadores podem ser declarados em uma [interface](../../language-reference/keywords/interface.md). Acessadores de indexadores de interface diferem dos acessadores de indexadores de [classe](../../language-reference/keywords/class.md) das seguintes maneiras:

- Os acessadores de interface não usam modificadores.
- Um acessador de interface normalmente não tem um corpo.

A finalidade do acessador é indicar se o indexador é de leitura/gravação, somente leitura ou somente gravação. Você pode fornecer uma implementação para um indexador definido em uma interface, mas isso é raro. Normalmente, os indexadores definem uma API para acessar os campos de dados e os campos de dados não podem ser definidos em uma interface.

Este é um exemplo de um acessador de indexador de interface:

[!code-csharp[DefineInterface](~/samples/snippets/csharp/interfaces/indexers.cs#DefineIndexer)]

A assinatura de um indexador deve ser diferente das assinaturas de todos os outros indexadores declarados na mesma interface.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como implementar indexadores de interface.

[!code-csharp[Implement](~/samples/snippets/csharp/interfaces/indexers.cs#ImplementInterface)]

[!code-csharp[DefineInterface](~/samples/snippets/csharp/interfaces/indexers.cs#ExampleCode)]

No exemplo anterior, é possível usar a implementação de membro de interface explícita usando o nome totalmente qualificado do membro de interface. Por exemplo

```csharp
string IIndexInterface.this[int index]
{
}
```

No entanto, o nome totalmente qualificado só será necessário para evitar ambiguidade quando a classe estiver implementando mais de uma interface com a mesma assinatura do indexador. Por exemplo, se uma classe `Employee` estiver implementando dois interfaces, `ICitizen` e `IEmployee`, e as duas interfaces tiverem a mesma assinatura de indexador, a implementação de membro de interface explícita é necessária. Ou seja, a seguinte declaração de indexador:

```csharp
string IEmployee.this[int index]
{
}
```

implementa o indexador na interface `IEmployee`, enquanto a seguinte declaração:

```csharp
string ICitizen.this[int index]
{
}
```

implementa o indexador na interface `ICitizen`.

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Indexadores](./index.md)
- [Propriedades](../classes-and-structs/properties.md)
- [Interfaces](../interfaces/index.md)
