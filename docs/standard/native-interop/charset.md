---
title: Conjuntos de caracteres e marshaling – .NET
description: Saiba como os diferentes valores de um conjunto de caracteres podem alterar a forma como o .NET realiza marshaling dos dados para o código nativo.
ms.date: 01/18/2019
ms.openlocfilehash: 39566593aa38bacfa41b44a8af8cc2dfb294d766
ms.sourcegitcommit: a2c8b19e813a52b91facbb5d7e3c062c7188b457
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85416103"
---
# <a name="charsets-and-marshaling"></a>Conjuntos de caracteres e marshaling

A maneira como valores `char`, objetos `string` e objetos `System.Text.StringBuilder` são empacotados depende do valor do campo `CharSet` em P/Invoke ou na estrutura. Para definir o `CharSet` de um P/Invoke, configure o campo <xref:System.Runtime.InteropServices.DllImportAttribute.CharSet?displayProperty=nameWithType> quando declarar o P/Invoke. Para definir o `CharSet` para um tipo, defina o <xref:System.Runtime.InteropServices.StructLayoutAttribute.CharSet?displayProperty=nameWithType> campo em sua declaração de classe ou struct. Se esses campos do atributo não estiverem definidos, caberá ao compilador de linguagem determinar qual `CharSet` a ser usado. O C# e o Visual Basic usam o conjunto de <xref:System.Runtime.InteropServices.CharSet.Ansi> caracteres por padrão.

A tabela a seguir mostra um mapeamento entre cada conjunto de caracteres e como um caractere ou uma cadeia de caracteres é representado quando o é realizado seu marshaling com esse conjunto de caracteres:

| `CharSet` valor | Windows            | .NET Core 2.2 e anteriores no Unix | .NET Core 3.0 e posteriores e Mono no Unix |
|-----------------|--------------------|-----------------------------------|------------------------------------------|
| `Ansi`          | `char` (a [página de códigos do Windows (ANSI)](/windows/win32/intl/code-pages) padrão do sistema)      | `char` (UTF-8)                    | `char` (UTF-8)                           |
| `Unicode`       | `wchar_t` (UTF-16) | `char16_t` (UTF-16)               | `char16_t` (UTF-16)                      |
| `Auto`          | `wchar_t` (UTF-16) | `char16_t` (UTF-16)               | `char` (UTF-8)                           |

Quando escolher o conjunto de caracteres, saiba qual será a representação esperada por sua representação nativa.
