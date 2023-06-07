---
title: 'Como: extrair um protocolo e um número da porta de uma URL'
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
ms.assetid: ab7f62b3-6d2c-4efb-8ac6-28600df5fd5c
ms.openlocfilehash: c1d45dbcb2916af86d645d7813594f2b278bb7c2
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803866"
---
# <a name="how-to-extract-a-protocol-and-port-number-from-a-url"></a>Como: extrair um protocolo e um número da porta de uma URL
O exemplo a seguir extrai um protocolo e um número da porta de uma URL.  

[!INCLUDE [regex](../../../includes/regex.md)]

## <a name="example"></a>Exemplo  
 O exemplo usa o método <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> para retornar o protocolo seguido por dois-pontos e pelo número da porta.  
  
 [!code-csharp[RegularExpressions.Examples.Protocol#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.Protocol/cs/Example.cs#1)]
 [!code-vb[RegularExpressions.Examples.Protocol#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.Protocol/vb/Example.vb#1)]  
  
 O padrão de expressão regular `^(?<proto>\w+)://[^/]+?(?<port>:\d+)?/` pode ser interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`^`|Comece a correspondência no início da cadeia de caracteres.|  
|`(?<proto>\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. Nomeie esse grupo como `proto`.|  
|`://`|Fazer a correspondência de um sinal de dois-pontos seguido por duas barras "/".|  
|`[^/]+?`|Fazer a correspondência de uma ou mais ocorrências (mas o menor número possível) de qualquer caractere que não seja uma barra "/".|  
|`(?<port>:\d+)?`|Fazer a correspondência de zero ou uma ocorrência de um sinal de dois-pontos seguido por um ou mais caracteres de dígito. Nomeie esse grupo como `port`.|  
|`/`|Fazer a correspondência de uma barra "/".|  
  
 O método <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> expande a sequência de substituição `${proto}${port}`, que concatena o valor dos dois grupos nomeados capturados no padrão de expressão regular. Essa é uma alternativa conveniente para concatenar explicitamente as cadeias de caracteres recuperadas do objeto da coleção retornado pela propriedade <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType>.  
  
 O exemplo usa o método <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> com duas substituições, `${proto}` e `${port}`, para incluir os grupos capturados na cadeia de caracteres de saída. Você pode recuperar os grupos capturados do objeto <xref:System.Text.RegularExpressions.GroupCollection> da correspondência em vez disso, conforme mostra o código a seguir.  
  
 [!code-csharp[RegularExpressions.Examples.Protocol#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.Protocol/cs/example2.cs#2)]
 [!code-vb[RegularExpressions.Examples.Protocol#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.Protocol/vb/example2.vb#2)]  
  
## <a name="see-also"></a>Consulte também

- [Expressões regulares do .NET](regular-expressions.md)
