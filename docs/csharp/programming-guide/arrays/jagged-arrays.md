---
title: Matrizes denteadas – Guia de Programação em C#
description: Uma matriz denteada em C# é uma matriz cujos elementos são matrizes de dimensões e tamanhos diferentes. Saiba como declarar, inicializar e acessar matrizes denteadas.
ms.date: 07/20/2015
helpviewer_keywords:
- jagged arrays [C#]
- arrays [C#], jagged
ms.assetid: 537c65a6-0e0a-4a00-a2b8-086f38519c70
ms.openlocfilehash: 40da9fbda34aef4e69ebf2ae20485e883b79f871
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474677"
---
# <a name="jagged-arrays-c-programming-guide"></a>Matrizes denteadas (Guia de Programação em C#)

Uma matriz denteada é uma matriz cujos elementos são matrizes. Os elementos de uma matriz denteada podem ter diferentes dimensões e tamanhos. Às vezes, uma matriz denteada é chamada de uma "matriz de matrizes." Os exemplos a seguir mostram como declarar, inicializar e acessar matrizes denteadas.  
  
 A seguir, há uma declaração de uma matriz unidimensional que tem três elementos, cada um do qual é uma matriz de dimensão única de inteiros:  
  
 [!code-csharp[csProgGuideArrays#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#19)]  
  
 Antes de usar `jaggedArray`, seus elementos devem ser inicializados. É possível inicializar os elementos dessa forma:  
  
 [!code-csharp[csProgGuideArrays#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#20)]  
  
 Cada um dos elementos é uma matriz unidimensional de inteiros. O primeiro elemento é uma matriz de 5 inteiros, o segundo é uma matriz de 4 inteiros e o terceiro é uma matriz de 2 inteiros.  
  
 Também é possível usar os inicializadores para preencher os elementos matriz com valores, caso em que não é necessário o tamanho da matriz. Por exemplo:  
  
 [!code-csharp[csProgGuideArrays#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#21)]  
  
 Também é possível inicializar a matriz mediante uma declaração como esta:  
  
 [!code-csharp[csProgGuideArrays#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#22)]  
  
 É possível usar a seguinte forma abreviada. Observe que não é possível omitir o operador `new` da inicialização de elementos, porque não há nenhuma inicialização padrão para os elementos:  
  
 [!code-csharp[csProgGuideArrays#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#23)]  
  
 Uma matriz denteada é uma matriz de matrizes e, portanto, seus elementos são tipos de referência e são inicializados para `null`.  
  
 É possível acessar elementos de matrizes individuais como estes exemplos:  
  
 [!code-csharp[csProgGuideArrays#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#24)]  
  
 É possível misturar matrizes denteadas e multidimensionais. A seguir, há uma declaração e inicialização de uma matriz denteada unidimensional que contém três elementos de matriz bidimensional de tamanhos diferentes. Para obter mais informações sobre matrizes bidimensionais, consulte [Matrizes Multidimensionais](./multidimensional-arrays.md).  
  
 [!code-csharp[csProgGuideArrays#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#25)]  
  
 É possível acessar elementos individuais conforme mostrado neste exemplo, que exibe o valor do elemento `[1,0]` da primeira matriz (valor `5`):  
  
 [!code-csharp[csProgGuideArrays#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#26)]  
  
 O método `Length` retorna inúmeros conjuntos contidos na matriz denteada. Por exemplo, supondo que você tenha declarado a matriz anterior, esta linha:  
  
 [!code-csharp[csProgGuideArrays#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#27)]  
  
 retorna um valor de 3.  
  
## <a name="example"></a>Exemplo

 Este exemplo cria uma matriz cujos elementos são matrizes. Cada um dos elementos da matriz tem um tamanho diferente.  
  
 [!code-csharp[csProgGuideArrays#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#18)]  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Array>
- [Guia de programação C#](../index.md)
- [matrizes](./index.md)
- [Matrizes unidimensionais](./single-dimensional-arrays.md)
- [Matrizes multidimensionais](./multidimensional-arrays.md)
