---
title: 'Exemplo de expressão regular: Verificação de HREFs'
description: Veja um exemplo de expressões regulares no .NET. O exemplo pesquisa uma cadeia de caracteres de entrada e exibe todos os valores de atributo href e suas localizações.
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
ms.assetid: fae2c15b-7adf-4b15-b118-58eb3906994f
ms.openlocfilehash: 7bcc2a4242bfaed3e3340347a30e97e7e4060794
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85802840"
---
# <a name="regular-expression-example-scanning-for-hrefs"></a>Exemplo de expressão regular: Verificação de HREFs
O exemplo a seguir procura uma cadeia de caracteres de entrada e exibe todos os valores href="…" e suas localizações na cadeia de caracteres.  

[!INCLUDE [regex](../../../includes/regex.md)]

## <a name="the-regex-object"></a>O objeto Regex
 Como o método `DumpHRefs` pode ser chamado várias vezes do código do usuário, ele usa o método `static` (`Shared` no Visual Basic) <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType>. Isso permite que o mecanismo de expressões regulares armazene em cache a expressão regular e evite a sobrecarga de instanciar um novo objeto <xref:System.Text.RegularExpressions.Regex> sempre que o método é chamado. Um objeto <xref:System.Text.RegularExpressions.Match> é usado para iterar por todas as correspondências na cadeia de caracteres.  
  
 [!code-csharp[RegularExpressions.Examples.HREF#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.HREF/cs/example.cs#1)]
 [!code-vb[RegularExpressions.Examples.HREF#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.HREF/vb/example.vb#1)]  
  
 O exemplo a seguir mostra uma chamada para o método `DumpHRefs`.  
  
 [!code-csharp[RegularExpressions.Examples.HREF#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.HREF/cs/example.cs#2)]
 [!code-vb[RegularExpressions.Examples.HREF#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.HREF/vb/example.vb#2)]  
  
 O padrão da expressão regular `href\s*=\s*(?:["'](?<1>[^"']*)["']|(?<1>\S+))` é interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`href`|Corresponder à cadeia de caracteres literal “href”. A correspondência não diferencia maiúsculas de minúsculas.|  
|`\s*`|Corresponder a zero ou mais caracteres de espaço em branco.|  
|`=`|Corresponder ao sinal de igual.|  
|`\s*`|Corresponder a zero ou mais caracteres de espaço em branco.|  
|`(?:\["'\](?<1>\[^"'\]*)["']|(?<1>\S+))`|Corresponde a um dos seguintes itens, sem atribuir o resultado a um grupo capturado:<br /> <ul><li><p>Um sinal de aspas ou apóstrofo, seguido por zero ou mais ocorrências de qualquer caractere que não seja um sinal de aspas ou apóstrofo, seguido por um sinal de aspas ou apóstrofo. O grupo chamado `1` está incluído nesse padrão.</p></li><li><p>Um ou mais caracteres diferentes de espaço em branco. O grupo chamado `1` está incluído nesse padrão.</p></li></ul>|  
|`(?<1>[^"']*)`|Atribuir zero ou mais ocorrências de qualquer caractere diferente de aspas simples ou apóstrofe ao grupo de captura chamado `1`.|  
|`(?<1>\S+)`|Atribuir um ou mais caracteres diferentes de espaço em branco ao grupo de captura chamado `1`.|  
  
## <a name="match-result-class"></a>Classe de resultado de correspondência  
 Os resultados de uma pesquisa são armazenados na classe <xref:System.Text.RegularExpressions.Match>, que fornece acesso a todas as subcadeias de caracteres extraídas pela pesquisa. Também lembra a cadeia de caracteres que está sendo pesquisada e a expressão regular que está sendo usada para poder chamar o método <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> para executar outra pesquisa iniciando onde a última terminou.  
  
## <a name="explicitly-named-captures"></a>Capturas nomeadas explicitamente  
 Em expressões regulares tradicionais, os parênteses de captura são numerados sequencialmente de forma automática. Isso causa dois problemas. Primeiro, se uma expressão regular for modificada pela inserção ou remoção de um conjunto de parênteses, todo o código que se refere às captura numeradas deverá ser reescrito para refletir a nova numeração. Em segundo lugar, como diferentes conjuntos de parênteses geralmente são usados para fornecer duas expressões alternativas para uma correspondência aceitável, pode ser difícil determinar qual das duas expressões realmente retornou um resultado.  
  
 Para resolver esses problemas, a classe <xref:System.Text.RegularExpressions.Regex> dá suporte à sintaxe `(?<name>…)` para capturar uma correspondência em um slot especificado (o slot pode ser nomeado usando uma cadeia de caracteres ou um inteiro; inteiros podem ser recuperados mais rapidamente). Assim, todas as correspondências alternativas para a mesma cadeia de caracteres podem ser direcionadas para o mesmo local. Em caso de conflito, a última correspondência solta em um slot é a correspondência com êxito. (No entanto, está disponível uma lista completa de diversas correspondências para um único slot. Confira a coleção <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> para obter detalhes).  
  
## <a name="see-also"></a>Consulte também

- [Expressões regulares do .NET](regular-expressions.md)
