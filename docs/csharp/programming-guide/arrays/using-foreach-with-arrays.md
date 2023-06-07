---
title: Usar foreach com matrizes – Guia de Programação em C#
description: A instrução foreach em C# itera através dos elementos de uma matriz. Para matrizes unidimensionais, foreach processa elementos em ordem de índice crescente.
ms.date: 05/23/2018
helpviewer_keywords:
- arrays [C#], foreach
- foreach statement [C#], using with arrays
ms.assetid: 5f2da2a9-1f56-4de5-94cc-e07f4f7a0244
ms.openlocfilehash: d924a3ef3351cbb30b809a1542f35314ee721852
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474534"
---
# <a name="using-foreach-with-arrays-c-programming-guide"></a>Usar foreach com matrizes (Guia de Programação em C#)

Essa instrução [foreach](../../language-reference/keywords/foreach-in.md) fornece uma maneira simples e limpa de iterar através dos elementos de uma matriz.

Em matrizes unidimensionais, a instrução `foreach` processa elementos em ordem crescente de índice, começando com o índice 0 e terminando com índice `Length - 1`:

 [!code-csharp[csProgGuideArrays#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#28)]

Em matrizes multidimensionais, os elementos são percorridos de modo a que os índices da dimensão mais à direita sejam aumentados primeiro e, em seguida, da próxima dimensão à esquerda, e assim por diante seguindo para a esquerda:

 [!code-csharp[csProgGuideArrays#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#29)]

No entanto, com matrizes multidimensionais, usar um loop aninhado [for](../../language-reference/keywords/for.md) oferece mais controle sobre a ordem na qual processar os elementos da matriz.

## <a name="see-also"></a>Consulte também

- <xref:System.Array>
- [Guia de programação C#](../index.md)
- [matrizes](index.md)
- [Matrizes unidimensionais](single-dimensional-arrays.md)
- [Matrizes multidimensionais](multidimensional-arrays.md)
- [Matrizes denteadas](jagged-arrays.md)
