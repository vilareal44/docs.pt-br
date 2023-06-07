---
title: <paramref>-Guia de programação C#
description: Saiba mais sobre a <paramref> marca XML. Essa marca fornece uma maneira de indicar que uma palavra no código é um parâmetro.
ms.date: 07/20/2015
f1_keywords:
- paramref
- <paramref>
helpviewer_keywords:
- <paramref> C# XML tag
- paramref C# XML tag
ms.assetid: 756c24c1-f591-40e8-a838-559761539b0b
ms.openlocfilehash: 133f43abfaf349806404d6d37fb472e3145c51b7
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381834"
---
# <a name="paramref-c-programming-guide"></a>\<paramref>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<paramref name="name"/>
```

## <a name="parameters"></a>parâmetros

- `name`

  O nome do parâmetro ao qual você deseja se referir. Coloque o nome entre aspas duplas (" ").

## <a name="remarks"></a>Comentários

A `<paramref>` marca fornece uma maneira de indicar que uma palavra nos comentários do código, por exemplo, em um `<summary>` `<remarks>` bloco ou se refere a um parâmetro. O arquivo XML pode ser processado para formatar essa palavra de alguma forma distinta, como com uma fonte em negrito ou itálico.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#7)]

## <a name="see-also"></a>Confira também

- [Guia de programação C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
