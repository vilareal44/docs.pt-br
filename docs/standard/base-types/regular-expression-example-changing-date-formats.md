---
title: 'Exemplo de expressão regular: Alterando formatos de data'
ms.date: 06/30/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- searching with regular expressions, examples
- parsing text with regular expressions, examples
- regular expressions, examples
- .NET Framework regular expressions, examples
- regular expressions [.NET Framework], examples
- pattern-matching with regular expressions, examples
ms.assetid: 5fcc75a5-09d7-45ae-a4c0-9ad6085ac83d
ms.openlocfilehash: 3b657ac6c88a1ee846f7f1d2156a18fd34621808
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803944"
---
# <a name="regular-expression-example-changing-date-formats"></a>Exemplo de expressão regular: Alterando formatos de data
O exemplo de código a seguir usa o <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> método para substituir datas que têm a forma *mm* / *DD* / *AA* com datas que têm a forma *DD* - *mm* - *AA*.  

[!INCLUDE [regex](../../../includes/regex.md)]

## <a name="example"></a>Exemplo  
 [!code-csharp[RegularExpressions.Examples.ChangeDateFormats#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.ChangeDateFormats/cs/Example_ChangeDateFormats1.cs#1)]
 [!code-vb[RegularExpressions.Examples.ChangeDateFormats#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.ChangeDateFormats/vb/Example_ChangeDateFormats1.vb#1)]  
  
 O código a seguir mostra como o método `MDYToDMY` pode ser chamado em um aplicativo.  
  
 [!code-csharp[RegularExpressions.Examples.ChangeDateFormats#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.ChangeDateFormats/cs/Example_ChangeDateFormats1.cs#2)]
 [!code-vb[RegularExpressions.Examples.ChangeDateFormats#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.ChangeDateFormats/vb/Example_ChangeDateFormats1.vb#2)]  
  
## <a name="comments"></a>Comentários  
 O padrão de expressão regular `\b(?<month>\d{1,2})/(?<day>\d{1,2})/(?<year>\d{2,4})\b` é interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`(?<month>\d{1,2})`|Corresponder a um ou dois dígitos decimais. Este é o grupo capturado do `month`.|  
|`/`|Corresponde à barra.|  
|`(?<day>\d{1,2})`|Corresponder a um ou dois dígitos decimais. Este é o grupo capturado do `day`.|  
|`/`|Corresponde à barra.|  
|`(?<year>\d{2,4})`|Corresponder de dois a quatro dígitos decimais. Este é o grupo capturado do `year`.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
  
 O padrão `${day}-${month}-${year}` define a cadeia de caracteres de substituição conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`$(day)`|Adicionar a cadeia de caracteres capturada pelo grupo de captura `day`.|  
|`-`|Adicionar um hífen.|  
|`$(month)`|Adicionar a cadeia de caracteres capturada pelo grupo de captura `month`.|  
|`-`|Adicionar um hífen.|  
|`$(year)`|Adicionar a cadeia de caracteres capturada pelo grupo de captura `year`.|  
  
## <a name="see-also"></a>Consulte também

- [Expressões regulares do .NET](regular-expressions.md)
