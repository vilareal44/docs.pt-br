---
title: Matrizes unidimensionais – Guia de Programação em C#
description: Crie uma matriz unidimensional em C# usando o novo operador especificando o tipo de elemento de matriz e o número de elementos.
ms.date: 06/03/2020
helpviewer_keywords:
- single-dimensional arrays [C#]
- arrays [C#], single-dimensional
ms.assetid: 2cec1196-1de0-49d2-baf2-c607c33310e8
ms.openlocfilehash: ada01262d57cbfebc8bfa1a5fee0639a10db5a4b
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474586"
---
# <a name="single-dimensional-arrays-c-programming-guide"></a>Matrizes unidimensionais (Guia de Programação em C#)

Você cria uma matriz unidimensional usando o [novo](../../language-reference/operators/new-operator.md) operador especificando o tipo de elemento de matriz e o número de elementos. O exemplo a seguir declara uma matriz de cinco inteiros:

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="IntDeclaration":::

Essa matriz contém os elementos de `array[0]` a `array[4]`. Os elementos da matriz são inicializados para o valor padrão do tipo de elemento, `0` para inteiros.

As matrizes podem armazenar qualquer tipo de elemento que você especificar, como o exemplo a seguir, que declara uma matriz de cadeias de caracteres:

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="StringDeclaration":::

## <a name="array-initialization"></a>Inicialização de Matriz

Você pode inicializar os elementos de uma matriz ao declarar a matriz. O especificador de comprimento não é necessário porque é inferido pelo número de elementos na lista de inicialização. Por exemplo:

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="IntInitialization":::

O código a seguir mostra uma declaração de uma matriz de cadeia de caracteres em que cada elemento da matriz é inicializado por um nome de um dia:

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="StringInitialization":::
  
Você pode evitar a `new` expressão e o tipo de matriz ao inicializar uma matriz na declaração, conforme mostrado no código a seguir. Isso é chamado de [matriz tipada implicitamente](implicitly-typed-arrays.md):

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="ShorthandInitialization":::

Você pode declarar uma variável de matriz sem criá-la, mas deve usar o `new` operador ao atribuir uma nova matriz a essa variável. Por exemplo:

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="DeclareAllocate":::

## <a name="value-type-and-reference-type-arrays"></a>Matrizes de tipo de valor e de tipo de referência

Considere a seguinte declaração de matriz:  

:::code language="csharp" source="snippets/SingleDimensionArrays.cs" id="FinalInstantiation":::

O resultado dessa instrução depende se `SomeType` é um tipo de valor ou um tipo de referência. Se for um tipo de valor, a instrução criará uma matriz de 10 elementos, cada um com o tipo `SomeType` . Se `SomeType` for um tipo de referência, a instrução criará uma matriz de 10 elementos, cada um deles é inicializado com uma referência nula. Em ambas as instâncias, os elementos são inicializados para o valor padrão para o tipo de elemento. Para obter mais informações sobre tipos de valor e tipos de referência, consulte [tipos de valor](../../language-reference/builtin-types/value-types.md) e [tipos de referência](../../language-reference/keywords/reference-types.md).
  
## <a name="see-also"></a>Consulte também

- <xref:System.Array>
- [matrizes](./index.md)
- [Matrizes multidimensionais](./multidimensional-arrays.md)
- [Matrizes denteadas](./jagged-arrays.md)
