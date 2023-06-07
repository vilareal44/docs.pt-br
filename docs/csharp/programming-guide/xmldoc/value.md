---
title: <value> -Guia de programação C#
description: Saiba mais sobre o XML <value> Tags. Essa marca permite descrever o valor que uma propriedade representa.
ms.date: 07/20/2015
f1_keywords:
- <value>
helpviewer_keywords:
- <value> C# XML tag
- value C# XML tag
ms.assetid: 08dbadaf-9ab6-43d9-9493-98e43bed199a
ms.openlocfilehash: d8294b477d7067653c71d1ec2047a85a0bfe6d02
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87380768"
---
# <a name="value-c-programming-guide"></a>\<value>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<value>property-description</value>
```

## <a name="parameters"></a>parâmetros

- `property-description`

  Uma descrição da propriedade.

## <a name="remarks"></a>Comentários

A `<value>` marca permite descrever o valor que uma propriedade representa. Quando você adiciona uma propriedade por meio do assistente de código no ambiente de desenvolvimento .NET do Visual Studio, ela adiciona uma [\<summary>](./summary.md) marca para a nova propriedade. Em seguida, você deve adicionar manualmente uma `<value>` marca para descrever o valor que a propriedade representa.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#14)]

## <a name="see-also"></a>Confira também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
