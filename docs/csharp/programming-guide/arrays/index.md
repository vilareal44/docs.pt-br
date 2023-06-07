---
title: Matrizes – Guia de Programação em C#
description: Armazene várias variáveis do mesmo tipo em uma estrutura de dados de matriz em C#. Declare uma matriz especificando um tipo ou especifique um objeto para armazenar qualquer tipo.
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [C#]
- C# language, arrays
ms.assetid: bb79bdde-e570-4c30-adb0-1dd5759ae041
ms.openlocfilehash: e302ff2e4c2488c4899c4eb99a666d2d322119ce
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474729"
---
# <a name="arrays-c-programming-guide"></a>Matrizes (Guia de Programação em C#)

Você pode armazenar diversas variáveis do mesmo tipo em uma estrutura de dados de matriz. Você pode declarar uma matriz especificando o tipo de seus elementos. Se você quiser que a matriz armazene elementos de qualquer tipo, você pode especificar `object` como seu tipo. No sistema de tipos unificado do C#, todos os tipos, predefinidos e definidos pelo usuário, tipos de referência e tipos de valor, herdam direta ou indiretamente de <xref:System.Object>.

```csharp
type[] arrayName;
```

## <a name="example"></a>Exemplo

O exemplo a seguir cria matrizes unidimensionais, multidimensionais e denteadas:

[!code-csharp[csProgGuideArrays#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideArrays/CS/Arrays.cs#1)]

## <a name="array-overview"></a>Visão geral da matriz

Uma matriz tem as seguintes propriedades:

- Uma matriz pode ser [Unidimensional](single-dimensional-arrays.md), [Multidimensional](multidimensional-arrays.md) ou [Denteada](jagged-arrays.md).
- O número de dimensões e o tamanho de cada dimensão são estabelecidos quando a instância de matriz é criada. Esses valores não podem ser alterados durante o ciclo de vida da instância.
- Os valores padrão dos elementos de matriz numérica são definidos como zero, e os elementos de referência são definidos como null.
- Uma matriz denteada é uma matriz de matrizes e, portanto, seus elementos são tipos de referência e são inicializados para `null`.
- As matrizes são indexadas por zero: uma matriz com elementos `n` é indexada de `0` para `n-1`.
- Os elementos de matriz podem ser de qualquer tipo, inclusive um tipo de matriz.
- Os tipos de matriz são [tipos de referência](../../language-reference/keywords/reference-types.md) derivados do tipo base abstrato <xref:System.Array>. Como esse tipo implementa <xref:System.Collections.IEnumerable> e <xref:System.Collections.Generic.IEnumerable%601>, você pode usar a iteração [foreach](../../language-reference/keywords/foreach-in.md) em todas as matrizes em c#.

## <a name="related-sections"></a>Seções relacionadas

- [Matrizes como Objetos](arrays-as-objects.md)
- [Usar foreach com matrizes](using-foreach-with-arrays.md)
- [Passar matrizes como argumentos](passing-arrays-as-arguments.md)

## <a name="c-language-specification"></a>especificação da linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Consulte também

- [Guia de programação C#](../index.md)
- [Coleções](../concepts/collections.md)
