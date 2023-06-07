---
title: <exception>-Guia de programação C#
description: Saiba mais sobre a <exception> marca XML. Essa marca permite que você especifique quais exceções podem ser geradas e pode ser aplicada a métodos, propriedades, eventos e indexadores.
ms.date: 07/20/2015
f1_keywords:
- exception
- <exception>
helpviewer_keywords:
- <exception> C# XML tag
- exception C# XML tag
ms.assetid: dd73aac5-3c74-4fcf-9498-f11bff3a2f3c
ms.openlocfilehash: 22a28f3fe6de5a0db9aea0f1fd7963d4e6fcee05
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381730"
---
# <a name="exception-c-programming-guide"></a>\<exception>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<exception cref="member">description</exception>
```

## <a name="parameters"></a>parâmetros

- cref = " `member`"

  Uma referência a uma exceção que está disponível no ambiente de compilação atual. O compilador verifica se a exceção apresentada existe e move o `member` para o nome de elemento canônico no XML de saída. `member` deve ser exibido entre aspas duplas (" ").

  Confira mais informações sobre como formatar `member` para fazer referência a um tipo genérico em [Como processar o Arquivo XML](processing-the-xml-file.md).

- `description`

  Uma descrição da exceção.

## <a name="remarks"></a>Comentários

A `<exception>` marca permite especificar quais exceções podem ser geradas. Essa marca pode ser aplicada às definições de métodos, propriedades, eventos e indexadores.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

Para obter mais informações sobre o tratamento de exceção, consulte [Exceções e tratamento de exceção](../exceptions/index.md).

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideDocComments#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#4)]

## <a name="see-also"></a>Confira também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](recommended-tags-for-documentation-comments.md)
