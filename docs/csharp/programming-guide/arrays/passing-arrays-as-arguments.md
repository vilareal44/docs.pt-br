---
title: Como passar matrizes argumentos – Guia de Programação em C#
description: As matrizes em C# podem ser passadas como argumentos para parâmetros de método. Como matrizes são tipos de referência, o método pode alterar o valor dos elementos.
ms.date: 07/05/2018
helpviewer_keywords:
- arrays [C#], passing as arguments
ms.assetid: f3a0971e-c87c-4a1f-8262-bc0a3b712772
ms.openlocfilehash: 68f174421e56e2cf082fe670f93c4f6627d7c17b
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474625"
---
# <a name="passing-arrays-as-arguments-c-programming-guide"></a>Passando matrizes como argumentos (Guia de Programação em C#)

As matrizes podem ser passadas como argumentos para parâmetros de método. Como matrizes são tipos de referência, o método pode alterar o valor dos elementos.

## <a name="passing-single-dimensional-arrays-as-arguments"></a>Passando matrizes unidimensionais como argumentos

É possível passar uma matriz unidimensional inicializada para um método. Por exemplo, a instrução a seguir envia uma matriz a um método de impressão.

[!code-csharp[csProgGuideArrays#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#34)]

O código a seguir mostra uma implementação parcial do método de impressão.

[!code-csharp[csProgGuideArrays#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#33)]

É possível inicializar e passar uma nova matriz em uma etapa, conforme mostrado no exemplo a seguir.

[!code-csharp[CsProgGuideArrays#35](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#35)]

### <a name="example"></a>Exemplo

No exemplo a seguir, uma matriz de cadeia de caracteres é inicializada e passada como um argumento para um método `DisplayArray` para cadeias de caracteres. O método exibe os elementos da matriz. Em seguida, o método `ChangeArray` inverte os elementos da matriz, e o método `ChangeArrayElements` modifica os três primeiros elementos da matriz. Depois que cada método retorna, o método `DisplayArray` mostra que passar uma matriz por valor não impede alterações nos elementos da matriz.

[!code-csharp[csProgGuideArrays#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/ArrayExample.cs)]

## <a name="passing-multidimensional-arrays-as-arguments"></a>Passando matrizes multidimensionais como argumentos

Você passa uma matriz multidimensional inicializada para um método da mesma forma que você passa uma matriz unidimensional.

[!code-csharp[csProgGuideArrays#41](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#41)]

O código a seguir mostra uma declaração parcial de um método de impressão que aceita uma matriz bidimensional como seu argumento.

[!code-csharp[csProgGuideArrays#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#36)]

É possível inicializar e passar uma nova matriz em uma única etapa, conforme é mostrado no exemplo a seguir:

[!code-csharp[csProgGuideArrays#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#32)]

### <a name="example"></a>Exemplo

No exemplo a seguir, uma matriz bidimensional de inteiros é inicializada e passada para o método `Print2DArray`. O método exibe os elementos da matriz.

[!code-csharp[csProgGuideArrays#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#31)]

## <a name="see-also"></a>Consulte também

- [Guia de programação C#](../index.md)
- [matrizes](index.md)
- [Matrizes unidimensionais](single-dimensional-arrays.md)
- [Matrizes multidimensionais](multidimensional-arrays.md)
- [Matrizes denteadas](jagged-arrays.md)
