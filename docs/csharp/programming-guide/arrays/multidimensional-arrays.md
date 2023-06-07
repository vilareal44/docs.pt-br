---
title: Matrizes multidimensionais – Guia de Programação em C#
description: As matrizes em C# podem ter mais de uma dimensão. Esta declaração de exemplo cria uma matriz bidimensional de quatro linhas e duas colunas.
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [C#], multidimensional
- multidimensional arrays [C#]
ms.assetid: 020ce02e-7dff-4273-8e53-bf0b33747232
ms.openlocfilehash: a2f204c0866672b317569fbc620aa8af60829ffd
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475002"
---
# <a name="multidimensional-arrays-c-programming-guide"></a>Matrizes multidimensionais (Guia de Programação em C#)

As matrizes podem ter mais de uma dimensão. Por exemplo, a declaração a seguir cria uma matriz bidimensional de quatro linhas e duas colunas.  
  
 [!code-csharp[csProgGuideArrays#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#11)]  
  
 A declaração a seguir cria uma matriz de três dimensões, 4, 2 e 3.  
  
 [!code-csharp[csProgGuideArrays#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#12)]  
  
## <a name="array-initialization"></a>Inicialização de Matriz

 É possível inicializar a matriz na declaração, conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[csProgGuideArrays#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#13)]  
  
 Você também pode inicializar a matriz sem especificar a classificação.  
  
 [!code-csharp[csProgGuideArrays#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#14)]  
  
 Caso você escolha declarar uma variável de matriz sem inicialização, será necessário usar o operador `new` ao atribuir uma matriz a essa variável. O uso de `new` é mostrado no exemplo a seguir.  
  
 [!code-csharp[csProgGuideArrays#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#15)]  
  
 O exemplo a seguir atribui um valor a um elemento de matriz específico.  
  
 [!code-csharp[csProgGuideArrays#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#16)]  
  
 Da mesma forma, o exemplo a seguir obtém o valor de um elemento de matriz específico e o atribui à variável `elementValue`.  
  
 [!code-csharp[csProgGuideArrays#42](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#42)]  
  
 O exemplo de código a seguir inicializa os elementos da matriz com valores padrão (exceto em matrizes denteadas).  
  
 [!code-csharp[csProgGuideArrays#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#17)]  
  
## <a name="see-also"></a>Consulte também

- [Guia de programação C#](../index.md)
- [matrizes](./index.md)
- [Matrizes unidimensionais](./single-dimensional-arrays.md)
- [Matrizes denteadas](./jagged-arrays.md)
