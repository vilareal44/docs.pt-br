---
title: <c>-Guia de programação C#
description: Saiba mais sobre a <c> marca XML. Esta marca marca um texto de linha única em uma descrição como código, enquanto <code>indicates multiple lines.
ms.date: 07/20/2015
f1_keywords:
- c
- <c>
helpviewer_keywords:
- text, marking as code [C#]
- code, marking text as [C#]
- c C# XML tag
- <c> C# XML tag
ms.assetid: aad5b16e-a29e-445e-bd0d-eea0b138d7b2
ms.openlocfilehash: 78e59e1df4b096782e0a97b6d12c21c843a1cb21
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87382016"
---
# <a name="c-c-programming-guide"></a>\<c>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<c>text</c>
```

## <a name="parameters"></a>parâmetros

- `text`

  O texto que você deseja indicar como código.

## <a name="remarks"></a>Comentários

A `<c>` marca fornece uma maneira de indicar que o texto dentro de uma descrição deve ser marcado como código. Use [\<code>](./code.md) para indicar várias linhas como código.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#2)]
  
## <a name="see-also"></a>Confira também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
