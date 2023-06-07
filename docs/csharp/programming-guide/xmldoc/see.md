---
title: <see>-Guia de programação C#
description: Saiba mais sobre a <see> marca XML. Essa marca permite especificar um link de dentro do texto, por exemplo, usando um atributo cref.
ms.date: 07/20/2015
f1_keywords:
- <see>
- see
helpviewer_keywords:
- cref [C#], <see> tag
- <see> C# XML tag
- cross-references [C#]
- see C# XML tag
ms.assetid: 0200de01-7e2f-45c4-9094-829d61236383
ms.openlocfilehash: 1cc4982d1ebe9d6896404218a6d200b10cc6503f
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381925"
---
# <a name="see-c-programming-guide"></a>\<see>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<see cref="member"/>
```

## <a name="parameters"></a>parâmetros

- cref = " `member` "

  Uma referência a um membro ou campo disponível para ser chamado do ambiente de compilação atual. O compilador verifica se o elemento de código fornecido existe e passa `member` para o nome de elemento no XML de saída. Coloque *member* entre aspas duplas (“ ”).

## <a name="remarks"></a>Comentários

A `<see>` marca permite especificar um link de dentro do texto. Use [\<seealso>](./seealso.md) para indicar que o texto deve ser colocado em uma seção ver também. Use o [atributo cref](./cref-attribute.md) para criar hyperlinks internos para páginas de documentação para elementos de código. Além disso, ``href`` é um atributo válido que funcionará como um hiperlink.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

O exemplo a seguir mostra uma `<see>` marca em uma seção de resumo.

[!code-csharp[csProgGuideDocComments#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#12)]

## <a name="see-also"></a>Veja também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
