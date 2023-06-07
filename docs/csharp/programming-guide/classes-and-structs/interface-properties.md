---
title: Propriedades de interface – Guia de Programação em C#
description: As propriedades podem ser declaradas em uma interface em C#. Este exemplo declara um acessador de propriedade de interface.
ms.date: 01/31/2020
helpviewer_keywords:
- properties [C#], on interfaces
- interfaces [C#], properties
ms.assetid: 6503e9ed-33d7-44ec-b4c1-cc16c084b795
ms.openlocfilehash: 920381ae804a6a968bfd667171269377f3d7e448
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864963"
---
# <a name="interface-properties-c-programming-guide"></a>Propriedades de interface (Guia de Programação em C#)

As propriedades podem ser declaradas em uma [interface](../../language-reference/keywords/interface.md). O exemplo a seguir declara um acessador de propriedade de interface:

[!code-csharp[DeclareProperties](~/samples/snippets/csharp/interfaces/properties.cs#DeclareInterfaceProperties)]

Normalmente, as propriedades de interface não têm um corpo. Os acessadores indicam se a propriedade é de leitura/gravação, somente leitura ou somente gravação. Ao contrário de classes e structs, a declaração dos acessadores sem um corpo não declara uma [propriedade implementada automaticamente](auto-implemented-properties.md). A partir do C# 8,0, uma interface pode definir uma implementação padrão para membros, incluindo propriedades. Definir uma implementação padrão para uma propriedade em uma interface é raro porque as interfaces não podem definir campos de dados de instância.

## <a name="example"></a>Exemplo

Neste exemplo, a interface `IEmployee` tem uma propriedade de leitura/gravação, `Name` e uma propriedade somente leitura, `Counter`. A classe `Employee` implementa a interface `IEmployee` e usa essas duas propriedades. O programa lê o nome de um novo funcionário e o número atual de funcionários e exibe o nome do funcionário e o número do funcionário computado.

Seria possível usar o nome totalmente qualificado da propriedade, que referencia a interface na qual o membro é declarado. Por exemplo:

[!code-csharp[ExplicitProperties](~/samples/snippets/csharp/interfaces/properties.cs#ExplicitImplementation)]

O exemplo anterior demonstra a [implementação de interface explícita](../interfaces/explicit-interface-implementation.md). Por exemplo, se a classe `Employee` estiver implementando duas interfaces `ICitizen` e `IEmployee` e as duas interfaces tiverem a propriedade `Name`, será necessária a implementação explícita de membro da interface. Ou seja, a seguinte declaração de propriedade:

[!code-csharp[ExplicitProperties](~/samples/snippets/csharp/interfaces/properties.cs#ExplicitImplementation)]

implementa a propriedade `Name` na interface `IEmployee`, enquanto a seguinte declaração:

[!code-csharp[ExplicitProperties](~/samples/snippets/csharp/interfaces/properties.cs#CitizenImplementation)]

implementa a propriedade `Name` na interface `ICitizen`.

[!code-csharp[Example](~/samples/snippets/csharp/interfaces/properties.cs#PropertyExample)]
[!code-csharp[Example](~/samples/snippets/csharp/interfaces/properties.cs#UseProperty)]

**`210 Hazem Abolrous`**

## <a name="sample-output"></a>Saída de exemplo

```console
Enter number of employees: 210
Enter the name of the new employee: Hazem Abolrous
The employee information:
Employee number: 211
Employee name: Hazem Abolrous
```

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Propriedades](./properties.md)
- [Usando propriedades](./using-properties.md)
- [Comparação entre propriedades e indexadores](../indexers/comparison-between-properties-and-indexers.md)
- [Indexadores](../indexers/index.md)
- [Interfaces](../interfaces/index.md)
