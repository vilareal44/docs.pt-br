---
title: <permission> -Guia de programação C#
description: Saiba mais sobre o XML <permission> Tags. Essa marca permite documentar o acesso de um membro, enquanto a classe PermissionSet permite que você especifique o acesso a um membro.
ms.date: 07/20/2015
f1_keywords:
- permission
- <permission>
helpviewer_keywords:
- <permission> C# XML tag
- permission C# XML tag
ms.assetid: 769e93fe-8404-443f-bf99-577aa42b6a49
ms.openlocfilehash: 38c87505b8b2973875e474ffd296dc02b7fb9de6
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381717"
---
# <a name="permission-c-programming-guide"></a>\<permission>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<permission cref="member">description</permission>
```

## <a name="parameters"></a>parâmetros

- cref = " `member`"

  Uma referência a um membro ou campo disponível para ser chamado do ambiente de compilação atual. O compilador verifica se o elemento de código fornecido existe e converte `member` no nome de elemento canônico no XML de saída. *member* deve ser exibido entre aspas duplas (" ").

  Para obter informações sobre como criar uma referência cref para um tipo genérico, consulte [atributo cref](./cref-attribute.md).

- `description`

  Uma descrição do acesso ao membro.

## <a name="remarks"></a>Comentários

A `<permission>` marca permite documentar o acesso de um membro. A classe <xref:System.Security.PermissionSet> permite que você especifique o acesso a um membro.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#8)]

## <a name="see-also"></a>Confira também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
