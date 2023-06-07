---
title: Como Pesquisar cadeias de caracteres (guia C#)
description: Saiba mais sobre duas estratégias para Pesquisar texto em cadeias de caracteres em C#. Os métodos da classe String pesquisam um texto específico. Expressões regulares pesquisam por padrões no texto.
ms.date: 02/21/2018
helpviewer_keywords:
- searching strings [C#]
- strings [C#], searching with String methods
- strings [C#], searching with regular expressions
ms.assetid: fb1d9a6d-598d-4a35-bd5f-b86012edcb2b
ms.openlocfilehash: 17bf6e080542242d30791b70ffbf00b05f03a7b0
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86473988"
---
# <a name="how-to-search-strings"></a>Como Pesquisar cadeias de caracteres

Você pode usar duas estratégias principais para pesquisar texto em cadeias de caracteres. Os métodos da classe <xref:System.String> pesquisam por um texto específico. Expressões regulares pesquisam por padrões no texto.

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

O tipo [string](../language-reference/builtin-types/reference-types.md#the-string-type), que é um alias para a classe <xref:System.String?displayProperty=nameWithType>, fornece uma série de métodos úteis para pesquisar o conteúdo de uma cadeia de caracteres. Entre elas estão <xref:System.String.Contains%2A>, <xref:System.String.StartsWith%2A>, <xref:System.String.EndsWith%2A>, <xref:System.String.IndexOf%2A> e <xref:System.String.LastIndexOf%2A>. A classe <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> fornece um vocabulário avançado para pesquisar por padrões de texto. Neste artigo, você aprenderá essas técnicas e como escolher o melhor método para suas necessidades.

## <a name="does-a-string-contain-text"></a>Uma cadeia de caracteres contém texto?

Os <xref:System.String.Contains%2A?displayProperty=nameWithType> <xref:System.String.StartsWith%2A?displayProperty=nameWithType> métodos, e <xref:System.String.EndsWith%2A?displayProperty=nameWithType> pesquisam uma cadeia de caracteres para um texto específico. O exemplo a seguir mostra cada um desses métodos e uma variação que usa uma pesquisa que não diferencia maiúsculas de minúsculas:

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs" id="Snippet1":::

O exemplo anterior demonstra um ponto importante para usar esses métodos. As pesquisas de texto **diferenciam maiúsculas e minúsculas** por padrão. Use o <xref:System.StringComparison.CurrentCultureIgnoreCase?displayProperty=nameWithType> valor de enumeração para especificar uma pesquisa que não diferencia maiúsculas de minúsculas.

## <a name="where-does-the-sought-text-occur-in-a-string"></a>Em que local de uma cadeia de caracteres o texto procurado ocorre?

Os métodos <xref:System.String.IndexOf%2A> e <xref:System.String.LastIndexOf%2A> também pesquisam texto em cadeias de caracteres. Esses métodos retornam o local do texto que está sendo procurado. Se o texto não for encontrado, elas retornarão `-1`. O exemplo a seguir mostra uma pesquisa para a primeira e a última ocorrência da palavra "métodos" e exibe o texto entre elas.

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs" id="Snippet2":::

## <a name="finding-specific-text-using-regular-expressions"></a>Localizar texto específico usando expressões regulares

A classe <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> pode ser usada para pesquisar cadeias de caracteres. Essas pesquisas podem variar em complexidade, de padrões de texto simples até os complicados.

O exemplo de código a seguir procura a palavra "the" ou "their" em uma oração, sem diferenciar maiúsculas e minúsculas. O método estático <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> realiza a pesquisa. Você fornece a ele a cadeia de caracteres a pesquisar e um padrão de pesquisa. Nesse caso, um terceiro argumento especifica que a pesquisa não diferencia maiúsculas de minúsculas. Para obter mais informações, consulte <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType>.

O padrão de pesquisa descreve o texto pelo qual procurar. A tabela a seguir descreve cada elemento desse padrão de pesquisa. (A tabela a seguir usa o único `\` , que deve ter escape como `\\` em uma cadeia de caracteres C#).

| Padrão  | Significado                          |
|----------|----------------------------------|
| `the`    | corresponder ao texto "the"             |
| `(eir)?` | corresponder a 0 ou 1 ocorrência de "eir" |
| `\s`     | corresponder a um caractere de espaço em branco    |

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs" id="Snippet3":::

> [!TIP]
> Os métodos `string` são geralmente melhores opções quando você está procurando por uma cadeia de caracteres exata. As expressões regulares são melhores quando você está procurando por algum padrão em uma cadeia de caracteres de origem.

## <a name="does-a-string-follow-a-pattern"></a>Uma cadeia de caracteres segue um padrão?

O código a seguir usa expressões regulares para validar o formato de cada cadeia de caracteres em uma matriz. A validação requer que cada cadeia de caracteres tenha a forma de um número de telefone no qual os três grupos de dígitos são separados por traços, os dois primeiros grupos contêm três dígitos e o terceiro grupo contém quatro dígitos. O padrão de pesquisa usa a expressão regular `^\\d{3}-\\d{3}-\\d{4}$`. Para obter mais informações, consulte [Linguagem de expressões regulares – referência rápida](../../standard/base-types/regular-expression-language-quick-reference.md).

| Padrão | Significado                             |
|---------|-------------------------------------|
| `^`     | corresponde ao início da cadeia de caracteres |
| `\d{3}` | corresponde a exatamente 3 caracteres de dígitos  |
| `-`     | corresponde ao caractere '-'           |
| `\d{3}` | corresponde a exatamente 3 caracteres de dígitos  |
| `-`     | corresponde ao caractere '-'           |
| `\d{4}` | corresponde a exatamente 4 caracteres de dígitos  |
| `$`     | corresponde ao final da cadeia de caracteres       |

:::code language="csharp" interactive="try-dotnet-method" source="../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs" id="Snippet4":::

Este padrão de pesquisa único corresponde a várias cadeias de caracteres válidas. Expressões regulares são melhores para pesquisar por ou validar mediante um padrão, em vez de uma única cadeia de caracteres de texto.

## <a name="see-also"></a>Consulte também

- [Guia de programação em C#](../programming-guide/index.md)
- [Cadeias de caracteres](../programming-guide/strings/index.md)
- [LINQ e cadeias de caracteres](../programming-guide/concepts/linq/linq-and-strings.md)
- <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType>
- [.NET Framework expressões regulares](../../standard/base-types/regular-expressions.md)
- [Linguagem de expressão regular-referência rápida](../../standard/base-types/regular-expression-language-quick-reference.md)
- [Práticas recomendadas para usar cadeias de caracteres no .NET](../../standard/base-types/best-practices-strings.md)
