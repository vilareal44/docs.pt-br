---
title: <para> -Guia de programação C#
description: Saiba mais sobre o XML <para> Tags. Essa marca permite que você adicione estrutura ao texto em outra marca, como <summary>, <remarks>ou <returns>.
ms.date: 07/20/2015
f1_keywords:
- <para>
- para
helpviewer_keywords:
- <para> C# XML tag
- para C# XML tag
ms.assetid: c74b8705-29df-40b1-bff5-237492b0e978
ms.openlocfilehash: 146078bcb556b4085724ddcdac561ea868ab0481
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381847"
---
# <a name="para-c-programming-guide"></a>\<para>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<para>content</para>
```

## <a name="parameters"></a>parâmetros

- `content`

  O texto do parágrafo.

## <a name="remarks"></a>Comentários

A `<para>` marca é para uso dentro de uma marca, como [\<summary>](./summary.md) , [\<remarks>](./remarks.md) ou [\<returns>](./returns.md) , e permite que você adicione a estrutura ao texto.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

Consulte [\<summary>](./summary.md) para obter um exemplo de como usar `<para>` .

## <a name="see-also"></a>Veja também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
