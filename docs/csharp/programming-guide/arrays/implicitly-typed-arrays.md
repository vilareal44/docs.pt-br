---
title: Matrizes de tipo implícito – Guia de Programação em C#
description: O tipo de uma matriz de tipo implícito em C# é inferido a partir dos elementos no inicializador de matriz. Use matrizes de tipo implícito em expressões de consulta.
ms.date: 07/20/2015
helpviewer_keywords:
- arrays [C#], implicitly-typed
- implicitly-typed arrays [C#]
- C# language, implicitly typed arrays
ms.assetid: e05be95c-6732-403d-ae42-b35f057cbbea
ms.openlocfilehash: 1f14f68207dfb79c92eaa01ac2a8ffaa08facc03
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474703"
---
# <a name="implicitly-typed-arrays-c-programming-guide"></a>Matrizes de tipo implícito (Guia de Programação em C#)

É possível criar uma matriz de tipo implícito na qual o tipo da instância da matriz é inferido com base nos elementos especificados no inicializador de matriz. As regras para qualquer variável de tipo implícito também se aplicam a matrizes de tipo implícito. Para obter mais informações, consulte [Variáveis locais de tipo implícito](../classes-and-structs/implicitly-typed-local-variables.md).

Geralmente, as matrizes de tipo implícito são usadas em expressões de consulta juntamente com objetos e tipos anônimos e inicializadores de coleção.

Os exemplos a seguir mostram como criar uma matriz de tipo implícito:

[!code-csharp[csProgGuideLINQ#37](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#37)]

No exemplo anterior, observe que, com as matrizes de tipo implícito, não são usados colchetes do lado esquerdo da instrução de inicialização. Observe também que as matrizes denteadas são inicializadas usando `new []` assim como matrizes unidimensionais.

## <a name="implicitly-typed-arrays-in-object-initializers"></a>Matrizes de tipo implícito em Inicializadores de objeto

Ao criar um tipo anônimo que contém uma matriz, ela deve ser de tipo implícito no inicializador de objeto do tipo. No exemplo a seguir, `contacts` é uma matriz de tipo implícito de tipos anônimos, cada um contém uma matriz denominada `PhoneNumbers`. Observe que a palavra-chave `var` não é usada dentro dos inicializadores de objeto.

[!code-csharp[csProgGuideLINQ#38](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#38)]

## <a name="see-also"></a>Consulte também

- [Guia de programação C#](../index.md)
- [Variáveis Locais Tipadas Implicitamente](../classes-and-structs/implicitly-typed-local-variables.md)
- [matrizes](./index.md)
- [Tipos anônimos](../classes-and-structs/anonymous-types.md)
- [Inicializadores de objeto e coleção](../classes-and-structs/object-and-collection-initializers.md)
- [var](../../language-reference/keywords/var.md)
- [LINQ em C#](../../linq/index.md)
