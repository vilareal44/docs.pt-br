---
title: <typeparam> -Guia de programação C#
description: Saiba mais sobre o XML <typeparam> Tags. Essa marca é usada no comentário para um tipo genérico ou declaração de método para descrever um parâmetro de tipo.
ms.date: 07/20/2015
f1_keywords:
- typeparam
helpviewer_keywords:
- <typeparam> C# XML tag
- typeparam C# XML tag
ms.assetid: 9b99d400-e911-4e55-99c6-64367c96aa4f
ms.openlocfilehash: 5e5333e384e8c77b500f74ab7c6038146df6e2c0
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87380781"
---
# <a name="typeparam-c-programming-guide"></a>\<typeparam>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<typeparam name="name">description</typeparam>
```

## <a name="parameters"></a>parâmetros

- `name`

  O nome do parâmetro de tipo. Coloque o nome entre aspas duplas (" ").

- `description`

  Uma descrição do parâmetro de tipo.

## <a name="remarks"></a>Comentários

A marca `<typeparam>` deve ser usada no comentário para um tipo genérico ou para uma declaração de método descrever um dos parâmetros de tipo. Adicione uma marca para cada parâmetro de tipo do tipo ou do método genérico.

Para obter mais informações, consulte [Genéricos](../generics/index.md).

O texto da marca `<typeparam>` será exibido no IntelliSense, o relatório Web de comentários sobre código da [Janela do Pesquisador de Objetos](/visualstudio/ide/viewing-the-structure-of-code#BKMK_ObjectBrowser).

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#13)]

## <a name="see-also"></a>Confira também

- [Referência de C#](../../language-reference/index.md)
- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
