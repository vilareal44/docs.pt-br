---
title: Matrizes como objetos – Guia de Programação em C#
description: As matrizes em C# são objetos da matriz de tipo base abstrata. Você pode usar as propriedades e outros membros de classe da matriz, como a propriedade Length.
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [C#], as objects
ms.assetid: f76d4403-bd0a-42a0-9bc8-694c55b2c926
ms.openlocfilehash: 984def3ef74b07d7099fae6cae6d6f8ce7e03e12
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474716"
---
# <a name="arrays-as-objects-c-programming-guide"></a>Matrizes como objetos (Guia de Programação em C#)

No C#, as matrizes são objetos e não apenas regiões endereçáveis de memória contígua, como no C e no C++. <xref:System.Array> é o tipo base abstrato de todos os tipos de matriz. Você pode usar as propriedades e outros membros da classe que o <xref:System.Array> tem. Um exemplo disso é usar a <xref:System.Array.Length%2A> propriedade para obter o comprimento de uma matriz. O código a seguir atribui o comprimento da matriz `numbers`, que é `5`, a uma variável denominada `lengthOfNumbers`:

[!code-csharp[csProgGuideArrays#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#3)]

A classe <xref:System.Array> fornece vários outros métodos e propriedades úteis para classificar, pesquisar e copiar matrizes.

## <a name="example"></a>Exemplo

Este exemplo usa a propriedade <xref:System.Array.Rank%2A> para exibir a quantidade de dimensões de uma matriz.

[!code-csharp[csProgGuideArrays#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#2)]

## <a name="see-also"></a>Consulte também

- [Guia de programação C#](../index.md)
- [matrizes](./index.md)
- [Matrizes unidimensionais](./single-dimensional-arrays.md)
- [Matrizes multidimensionais](./multidimensional-arrays.md)
- [Matrizes denteadas](./jagged-arrays.md)
