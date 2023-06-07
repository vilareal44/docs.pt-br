---
title: Construtores diversos em expressões regulares
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- constructs, miscellaneous
- .NET Framework regular expressions, miscellaneous constructs
- regular expressions, miscellaneous constructs
ms.assetid: 7d10d11f-680f-4721-b047-fb136316b4cd
ms.openlocfilehash: 8ca888074aa757a1bfba786a7bec5928b75b1da2
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290403"
---
# <a name="miscellaneous-constructs-in-regular-expressions"></a>Construtores diversos em expressões regulares
As expressões regulares em .NET incluem três constructos diversos de linguagem. Um deles permite habilitar ou desabilitar opções específicas de correspondência no meio de um padrão de expressão regular. Os dois restantes permitem incluir comentários em uma expressão regular.  
  
## <a name="inline-options"></a>Opções embutidas  
 É possível definir ou desabilitar opções específicas de correspondência de padrão para parte de uma expressão regular mediante uso da sintaxe  
  
`(?imnsx-imnsx)`  
  
 Você lista as opções que deseja habilitar após o ponto de interrogação e as opções que deseja desabilitar após o sinal de subtração. A tabela a seguir descreve cada opção. Para obter mais informações sobre cada opção, consulte [Opções de expressão regular](regular-expression-options.md).  
  
|Opção|Description|  
|------------|-----------------|  
|`i`|Correspondência sem diferenciação entre maiúsculas e minúsculas.|  
|`m`|Modo multilinha.|  
|`n`|Apenas capturas explícitas. (Parênteses não funcionam como grupos de captura.)|  
|`s`|Modo de linha única.|  
|`x`|Ignorar espaço em branco sem escape e permitir comentários no modo x.|  
  
 Qualquer alteração nas opções de expressões regulares definida pelo constructo `(?imnsx-imnsx)` permanece em vigor até o fim do grupo delimitador.  
  
> [!NOTE]
> A `(?imnsx-imnsx:` construção de agrupamento de *subexpressão* `)` fornece uma funcionalidade idêntica para uma subexpressão. Para saber mais, confira [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).  
  
 O exemplo a seguir usa as opções `i`, `n` e `x` para desabilitar a diferenciação entre maiúsculas e minúsculas e habilitar e capturas explícitas, bem como ignorar o espaço em branco no padrão de expressão regular no meio de uma expressão regular.  
  
 [!code-csharp[RegularExpressions.Language.Miscellaneous#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.miscellaneous/cs/miscellaneous1.cs#1)]
 [!code-vb[RegularExpressions.Language.Miscellaneous#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.miscellaneous/vb/miscellaneous1.vb#1)]  
  
 O exemplo define duas expressões regulares. A primeira, `\b(D\w+)\s(d\w+)\b`, corresponde a duas palavras consecutivas que começam com um “D” maiúsculo e um “d” minúsculo. A segunda expressão regular, `\b(D\w+)(?ixn) \s (d\w+) \b`, usa opções embutidas para modificar esse padrão, conforme descrito na tabela a seguir. Uma comparação dos resultados confirma o efeito do constructo `(?ixn)`.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`(D\w+)`|Corresponder a um “D” maiúsculo seguido por um ou mais caracteres de palavra. Este é o primeiro grupo de captura.|  
|`(?ixn)`|Desse ponto em diante, faça comparações sem distinção de maiúsculas e minúsculas, faça apenas capturas explícitas e ignore o espaço em branco no padrão de expressão regular.|  
|`\s`|Corresponde a um caractere de espaço em branco.|  
|`(d\w+)`|Corresponder a um “d” maiúsculo ou minúsculo seguido por um ou mais caracteres de palavra. Este grupo não foi capturado porque a opção `n` (captura explícita) estava habilitada.|  
|`\b`|Corresponder a um limite de palavra.|  
  
## <a name="inline-comment"></a>Comentário embutido  
 A `(?#` construção de *Comentário* `)` permite incluir um comentário embutido em uma expressão regular. O mecanismo de expressões regulares não usa nenhuma parte do comentário na correspondência de padrão, apesar de o comentário estar incluído na cadeia de caracteres que é retornada pelo método <xref:System.Text.RegularExpressions.Regex.ToString%2A?displayProperty=nameWithType>. O comentário é encerrado no primeiro caractere de fechar parênteses.  
  
 O exemplo a seguir repete o primeiro padrão de expressão regular do exemplo na seção anterior. Ele adiciona dois comentários embutidos na expressão regular para indicar se a comparação diferencia maiúsculas de minúsculas. O padrão de expressão regular, `\b((?# case-sensitive comparison)D\w+)\s(?ixn)((?#case-insensitive comparison)d\w+)\b`, é definido da seguinte forma.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`(?# case-sensitive comparison)`|Um comentário. Não afeta o comportamento de correspondência.|  
|`(D\w+)`|Corresponder a um “D” maiúsculo seguido por um ou mais caracteres de palavra. Este é o primeiro grupo de captura.|  
|`\s`|Corresponde a um caractere de espaço em branco.|  
|`(?ixn)`|Desse ponto em diante, faça comparações sem distinção de maiúsculas e minúsculas, faça apenas capturas explícitas e ignore o espaço em branco no padrão de expressão regular.|  
|`(?#case-insensitive comparison)`|Um comentário. Não afeta o comportamento de correspondência.|  
|`(d\w+)`|Corresponder a um “d” maiúsculo ou minúsculo seguido por um ou mais caracteres de palavra. Este é o segundo grupo de captura.|  
|`\b`|Corresponder a um limite de palavra.|  
  
 [!code-csharp[RegularExpressions.Language.Miscellaneous#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.miscellaneous/cs/miscellaneous2.cs#2)]
 [!code-vb[RegularExpressions.Language.Miscellaneous#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.miscellaneous/vb/miscellaneous2.vb#2)]  
  
## <a name="end-of-line-comment"></a>Comentário de final de linha  
 Um sinal numérico (`#`) marca um comentário do modo x, que começa com o caractere # sem escape no final do padrão de expressão regular e continua até o final da linha. Para usar este constructo, você deve habilitar a opção `x` (por meio de opções embutidas) ou fornecer o valor <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> ao parâmetro `option` ao instanciar o objeto <xref:System.Text.RegularExpressions.Regex> ou chamar um método estático <xref:System.Text.RegularExpressions.Regex>.  
  
 O exemplo a seguir ilustra o constructo do comentário de final de linha. Ele determina se uma cadeia de caracteres é uma cadeia de caracteres de formato de composição que inclui pelo menos um item de formato. A tabela a seguir descreve os constructos no padrão de expressão regular:  
  
 `\{\d+(,-*\d+)*(\:\w{1,4}?)*\}(?x) # Looks for a composite format item.`  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\{`|Corresponder a uma chave de abertura.|  
|`\d+`|Corresponde a um ou mais dígitos decimais.|  
|`(,-*\d+)*`|Corresponder a zero ou a uma ocorrência de vírgula, seguida por um sinal de subtração opcional, seguido por um ou mais dígitos decimais.|  
|`(\:\w{1,4}?)*`|Corresponder a zero ou a uma ocorrência de dois-pontos, seguido de um a quatro caracteres em branco, mas o mínimo possível.|  
|`\}`|Corresponder a uma chave de fechamento.|  
|`(?x)`|Habilitar a opção de ignorar espaço em branco no padrão para o comentário de final de linha ser reconhecido.|  
|`# Looks for a composite format item.`|Um comentário de final de linha.|  
  
 [!code-csharp[RegularExpressions.Language.Miscellaneous#3](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.miscellaneous/cs/miscellaneous3.cs#3)]
 [!code-vb[RegularExpressions.Language.Miscellaneous#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.miscellaneous/vb/miscellaneous3.vb#3)]  
  
 Observe que, em vez de fornecer o constructo `(?x)` na expressão regular, o comentário pode também poderia ter sido reconhecido chamando o método <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> e passando-o para o valor de enumeração <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType>.  
  
## <a name="see-also"></a>Veja também

- [Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)
