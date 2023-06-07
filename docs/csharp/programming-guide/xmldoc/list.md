---
title: <list> -Guia de programação C#
description: Saiba mais sobre o XML <list> Tags. Essa marca é usada para criar tabelas e definições com marcadores ou numeradas usando blocos ' item '.
ms.date: 07/20/2015
f1_keywords:
- list
- <list>
helpviewer_keywords:
- list C# XML tag
- listheader C# XML tag
- <listheader> C# XML tag
- item C# XML tag
- <item> C# XML tag
- <list> C# XML tag
ms.assetid: c9620b1b-c2e6-43f1-ab88-8ab47308ffec
ms.openlocfilehash: 361c2e6f343554a9b8519c3b2e41219b209e682d
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381860"
---
# <a name="list-c-programming-guide"></a>\<list>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<list type="bullet|number|table">
    <listheader>
        <term>term</term>
        <description>description</description>
    </listheader>
    <item>
        <term>term</term>
        <description>description</description>
    </item>
</list>
```

## <a name="parameters"></a>parâmetros

- `term`

  Um termo a se definir, que será definido em `description`.

- `description`

  Um item em uma lista com marcadores ou numerada ou uma definição de um `term`.
  
## <a name="remarks"></a>Comentários

O `<listheader>` bloco é usado para definir a linha de cabeçalho de uma tabela ou lista de definições. Ao definir uma tabela, é necessário fornecer uma entrada para o termo no título.

Cada item na lista é especificado com um `<item>` bloco. Ao criar uma lista de definições, é necessário especificar `term` e `description`. No entanto, para uma tabela, lista com marcadores ou lista numerada, será necessário fornecer apenas uma entrada para `description`.

Uma lista ou tabela pode ter tantos `<item>` blocos quantos forem necessários.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#6)]

## <a name="see-also"></a>Confira também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
