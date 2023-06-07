---
title: Genéricos – Guia de Programação em C#
description: Saiba mais sobre os genéricos. Os tipos genéricos maximizam a reutilização de código, a segurança de tipo e o desempenho, e são comumente usados para criar classes de coleção.
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, generics
- generics [C#]
ms.assetid: 75ea8509-a4ea-4e7a-a2b3-cf72482e9282
ms.openlocfilehash: beef9c20e3ac62505bc7a4584b404637935de1dc
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87299390"
---
# <a name="generics-c-programming-guide"></a>Genéricos (Guia de Programação em C#)

Os genéricos apresentam o conceito de parâmetros de tipo para .NET, o que possibilita criar classes e métodos que adiam a especificação de um ou mais tipos até que a classe ou o método seja declarado e instanciado pelo código do cliente. Por exemplo, usando um parâmetro de tipo genérico `T` , você pode escrever uma única classe que outro código de cliente pode usar sem incorrer no custo ou risco de conversões de tempo de execução ou operações boxing, conforme mostrado aqui:

[!code-csharp[csProgGuideGenerics#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#1)]

Classes e métodos genéricos combinam reusabilidade, segurança de tipo e eficiência de forma que suas contrapartes não genéricas não possam. Os genéricos são usados com mais frequência com coleções e com os métodos que operam nelas. O <xref:System.Collections.Generic> namespace contém várias classes de coleção baseadas em genéricos. As coleções não genéricas, como <xref:System.Collections.ArrayList> não são recomendadas e são mantidas para fins de compatibilidade. Para saber mais, confira [Genéricos no .NET](../../../standard/generics/index.md).

É claro que você também pode criar tipos e métodos genéricos personalizados para fornecer suas próprias soluções e padrões de design generalizados que sejam fortemente tipados e eficientes. O exemplo de código a seguir mostra uma classe de lista vinculada genérica simples para fins de demonstração. (Na maioria dos casos, você deve usar a <xref:System.Collections.Generic.List%601> classe fornecida pelo .net em vez de criar a sua própria.) O parâmetro de tipo `T` é usado em vários locais em que um tipo concreto normalmente seria usado para indicar o tipo do item armazenado na lista. Ele é usado das seguintes maneiras:

- Como o tipo de um parâmetro de método no método `AddHead`.
- Como o tipo de retorno da propriedade `Data` na classe `Node` aninhada.
- Como o tipo de `data` do membro particular na classe aninhada.

 Observe que `T` está disponível para a classe aninhada `Node` . Quando `GenericList<T>` é instanciada com um tipo concreto, por exemplo como um `GenericList<int>`, cada ocorrência de `T` será substituída por `int`.

[!code-csharp[csProgGuideGenerics#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#2)]

O exemplo de código a seguir mostra como o código cliente usa a classe `GenericList<T>` genérica para criar uma lista de inteiros. Ao simplesmente alterar o argumento de tipo, o código a seguir poderia facilmente ser modificado para criar listas de cadeias de caracteres ou qualquer outro tipo personalizado:

[!code-csharp[csProgGuideGenerics#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#3)]

## <a name="generics-overview"></a>Visão geral dos genéricos

- Use tipos genéricos para maximizar a reutilização de código, o desempenho e a segurança de tipo.
- O uso mais comum de genéricos é para criar classes de coleção.
- A biblioteca de classes .NET contém várias classes de coleção genéricas no <xref:System.Collections.Generic> namespace. Eles devem ser usados sempre que possível, em vez de classes como <xref:System.Collections.ArrayList> no namespace <xref:System.Collections>.
- Você pode criar suas próprias interfaces, classes, métodos, eventos e delegados genéricos.
- Classes genéricas podem ser restringidas para habilitar o acesso aos métodos em tipos de dados específicos.
- Informações sobre os tipos que são usados em um tipo de dados genérico podem ser obtidas no tempo de execução por meio de reflexão.

## <a name="related-sections"></a>Seções relacionadas

- [Parâmetros de tipo genérico](generic-type-parameters.md)
- [Restrições a parâmetros de tipo](constraints-on-type-parameters.md)
- [Classes genéricas](generic-classes.md)
- [Interfaces genéricas](generic-interfaces.md)
- [Métodos genéricos](generic-methods.md)
- [Delegados genéricos](generic-delegates.md)
- [Diferenças entre modelos C++ e genéricos C#](differences-between-cpp-templates-and-csharp-generics.md)
- [Genéricos e reflexão](generics-and-reflection.md)
- [Genéricos em tempo de execução](generics-in-the-run-time.md)

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, consulte a [especificação da linguagem C#](~/_csharplang/spec/types.md#constructed-types).

## <a name="see-also"></a>Veja também

- <xref:System.Collections.Generic>
- [Guia de programação C#](../index.md)
- [Types](../types/index.md)
- [\<typeparam>](../xmldoc/typeparam.md)
- [\<typeparamref>](../xmldoc/typeparamref.md)
- [Generics in .NET (Genéricos no .NET)](../../../standard/generics/index.md)
