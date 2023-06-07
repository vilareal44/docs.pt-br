---
title: <seealso> -Guia de programação C#
description: Saiba como usar o XML <seealso> Tags. Essa marca permite especificar o texto que você pode querer que apareça em uma seção ' Ver também '.
ms.date: 07/20/2015
f1_keywords:
- cref
- <seealso>
- seealso
helpviewer_keywords:
- cref [C#], see also
- seealso C# XML tag
- cref [C#]
- cross-references [C#], tags
- <seealso> C# XML tag
ms.assetid: 8e157f3f-f220-4fcf-9010-88905b080b18
ms.openlocfilehash: e8618ef352a4fa7f0fd4c88733c6568b0e12e376
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87380638"
---
# <a name="seealso-c-programming-guide"></a>\<seealso>(Guia de programação C#)

## <a name="syntax"></a>Sintaxe

```xml
<seealso cref="member"/>
```

## <a name="parameters"></a>parâmetros

- cref = " `member`"

  Uma referência a um membro ou campo disponível para ser chamado do ambiente de compilação atual. O compilador verifica se o elemento de código fornecido existe e passa `member` para o nome de elemento no XML de saída.`member` deve ser exibido entre aspas duplas (" ").

  Para obter informações sobre como criar uma referência cref para um tipo genérico, consulte [atributo cref](./cref-attribute.md).

## <a name="remarks"></a>Comentários

A `<seealso>` marca permite especificar o texto que você pode querer que apareça em uma seção ver também. Use [\<see>](./see.md) para especificar um link de dentro do texto.

Compile com [-Doc](../../language-reference/compiler-options/doc-compiler-option.md) para processar comentários de documentação em um arquivo.

## <a name="example"></a>Exemplo

Consulte [\<summary>](./summary.md) para obter um exemplo de como usar \<seealso> .

## <a name="see-also"></a>Confira também

- [Guia de programação em C#](../index.md)
- [Marcas recomendadas para comentários de documentação](./recommended-tags-for-documentation-comments.md)
