---
title: <example> -Guia de programação C#
description: Saiba mais sobre o XML <example> Tags. Essa marca permite que você especifique um exemplo de como usar um método ou outro membro da biblioteca.
ms.date: 07/20/2015
f1_keywords:
- <example>
- example
helpviewer_keywords:
- <example> C# XML tag
- example C# XML tag
ms.assetid: 32d6e73b-2554-4abb-83ee-a1e321334fd2
ms.openlocfilehash: dd529e8f2a54cf9086d0d8c555dd1adb70b99126
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381522"
---
# <a name="example-c-programming-guide"></a>\<example>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<example>description</example>
```

## <a name="parameters"></a>parâmetros

- `description`

  Uma descrição do exemplo de código.

## <a name="remarks"></a>Comentários

A `<example>` marca permite especificar um exemplo de como usar um método ou outro membro da biblioteca. Isso geralmente envolve o uso da [\<code>](./code.md) marca.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#3)]

## <a name="see-also"></a>Confira também

- [Guia de programação C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
