---
title: <returns> -Guia de programação C#
description: Saiba mais sobre o XML <returns> Tags. Essa marca é usada no comentário para uma declaração de método para descrever o valor de retorno.
ms.date: 07/20/2015
f1_keywords:
- returns
- <returns>
helpviewer_keywords:
- <returns> C# XML tag
- returns C# XML tag
ms.assetid: bb2d9958-62fc-47c7-9511-6311171f119f
ms.openlocfilehash: e461d46df619a351048ae7942e59847d39e490f9
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381392"
---
# <a name="returns-c-programming-guide"></a>\<returns>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<returns>description</returns>
```

## <a name="parameters"></a>parâmetros

- `description`

  Uma descrição do valor retornado.

## <a name="remarks"></a>Comentários

A `<returns>` marca deve ser usada no comentário para uma declaração de método para descrever o valor de retorno.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#10)]

## <a name="see-also"></a>Confira também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
