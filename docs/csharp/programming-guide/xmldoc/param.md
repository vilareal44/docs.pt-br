---
title: <param> -Guia de programação C#
description: Saiba mais sobre o XML <param> Tags. Essa marca é usada no comentário para uma declaração de método para descrever um dos parâmetros para o método.
ms.date: 07/20/2015
f1_keywords:
- param
- <param>
helpviewer_keywords:
- <param> C# XML tag
- param C# XML tag
ms.assetid: 46d329b1-5b84-4537-9e17-73ca97313e4e
ms.openlocfilehash: a9e3b2e86528afcbe1330788e248f0143efb5c1b
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381548"
---
# <a name="param-c-programming-guide"></a>\<param>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<param name="name">description</param>
```

## <a name="parameters"></a>parâmetros

- `name`

  O nome do parâmetro de um método. Coloque o nome entre aspas duplas (" ").

- `description`

  Uma descrição do parâmetro.

## <a name="remarks"></a>Comentários

A `<param>` marca deve ser usada no comentário para uma declaração de método para descrever um dos parâmetros para o método. Para documentar vários parâmetros, use várias `<param>` marcas.

O texto da `<param>` marca é exibido no IntelliSense, no Pesquisador de objetos e no relatório da Web de comentários de código.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#1)]

## <a name="see-also"></a>Confira também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
