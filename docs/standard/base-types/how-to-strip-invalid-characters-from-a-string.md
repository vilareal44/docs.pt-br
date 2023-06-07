---
title: 'Como: retirar caracteres inválidos de uma cadeia de caracteres'
description: Leia um exemplo que mostra como remover caracteres potencialmente prejudiciais de uma cadeia de caracteres usando o método estático Regex. Replace.
ms.date: 06/30/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, examples
- cleaning input
- user input, examples
- .NET Framework regular expressions, examples
- regular expressions [.NET Framework], examples
- Regex.Replace method
- stripping invalid characters
- Replace method
- validating user input
ms.assetid: b4319c8a-9032-4129-a9d5-6f6fc28e7f32
ms.openlocfilehash: 5e0cd423df7fce03cdefb3da7bc192f3045e8f9c
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803983"
---
# <a name="how-to-strip-invalid-characters-from-a-string"></a>Como: retirar caracteres inválidos de uma cadeia de caracteres
O exemplo a seguir usa o método <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> estático para retirar caracteres inválidos de uma cadeia de caracteres.  

[!INCLUDE [regex](../../../includes/regex.md)]

## <a name="example"></a>Exemplo  
 Você pode usar o método `CleanInput` definido neste exemplo para retirar caracteres potencialmente prejudiciais que tenham sido inseridos em um campo de texto que aceita entrada do usuário. Nesse caso, o `CleanInput` remove todos os caracteres não alfanuméricos, exceto pontos (.), arrobas (@) e hifens (-) e retorna a cadeia de caracteres restante. No entanto, você pode modificar o padrão da expressão regular para que ela elimine qualquer caractere que não deve ser incluído em uma cadeia de caracteres de entrada.  
  
 [!code-csharp[RegularExpressions.Examples.StripChars#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.StripChars/cs/Example.cs#1)]
 [!code-vb[RegularExpressions.Examples.StripChars#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.StripChars/vb/Example.vb#1)]  
  
 O padrão da expressão regular `[^\w\.@-]` corresponde a qualquer caractere que não seja um caractere de palavra, um ponto, um símbolo de @ ou um hífen. Um caractere de palavra é qualquer letra, dígito decimal ou conector de pontuação, como um sublinhado. Qualquer caractere que corresponde a esse padrão é substituído pelo <xref:System.String.Empty?displayProperty=nameWithType>, que é a cadeia de caracteres definida pelo padrão de substituição. Para permitir caracteres adicionais na entrada do usuário, adicione esses caracteres à classe de caractere no padrão de expressão regular. Por exemplo, o padrão de expressão regular `[^\w\.@-\\%]` também permite um símbolo percentual e uma barra invertida em uma cadeia de caracteres de entrada.  
  
## <a name="see-also"></a>Consulte também

- [Expressões regulares do .NET](regular-expressions.md)
