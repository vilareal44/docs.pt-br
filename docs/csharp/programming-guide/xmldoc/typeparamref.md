---
title: <typeparamref>-Guia de programação C#
description: Saiba mais sobre a <typeparamref> marca XML. Essa marca permite que os consumidores do arquivo de documentação formatem a palavra de alguma forma distinta, por exemplo, em itálico.
ms.date: 07/20/2015
f1_keywords:
- typeparamref
helpviewer_keywords:
- typeparamref C# XML tag
- <typeparamref> C# XML tag
ms.assetid: 6d8ffc58-12c5-4688-8db6-833a7ded5886
ms.openlocfilehash: a39e896f1242452c7bcc94faa1e7ef3086ae2149
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87380716"
---
# <a name="typeparamref-c-programming-guide"></a>\<typeparamref>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<typeparamref name="name"/>
```

## <a name="parameters"></a>parâmetros

- `name`

  O nome do parâmetro de tipo. Coloque o nome entre aspas duplas (" ").

## <a name="remarks"></a>Comentários

Para obter mais informações sobre parâmetros de tipo em tipos e métodos genéricos, consulte [Genéricos](../generics/index.md).

Use essa marca para habilitar os consumidores do arquivo de documentação a formatar a palavra de alguma forma distinta, por exemplo, em itálico.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#13)]

## <a name="see-also"></a>Confira também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
