---
title: <remarks> -Guia de programação C#
description: Saiba mais sobre o XML <remarks> Tags. Essa marca é usada para adicionar informações sobre um tipo, complementando as informações especificadas com <summary>.
ms.date: 07/20/2015
f1_keywords:
- remarks
- <remarks>
helpviewer_keywords:
- remarks C# XML tag
- <remarks> C# XML tag
ms.assetid: f8641391-31f3-4735-af7a-c502a5b6a251
ms.openlocfilehash: d38905d100e24158e7a1412f6be9f01a7ced2382
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381496"
---
# <a name="remarks-c-programming-guide"></a>\<remarks>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<remarks>description</remarks>
```

## <a name="parameters"></a>parâmetros

- `Description`

  Uma descrição do membro.

## <a name="remarks"></a>Comentários

A `<remarks>` marca é usada para adicionar informações sobre um tipo, complementando as informações especificadas com [\<summary>](./summary.md) . Essas informações são exibidas na janela do Pesquisador de Objetos.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#9)]

## <a name="see-also"></a>Confira também

- [Guia de programação C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
