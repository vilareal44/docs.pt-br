---
title: Substituições em expressões regulares
description: Faça substituições para substituir o texto correspondente usando expressões regulares no .NET. Substituições são elementos de linguagem reconhecidos somente dentro de padrões de substituição.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, substitutions
- replacement patterns
- metacharacters, substitutions
- .NET Framework regular expressions, substitutions
- constructs, substitutions
- substitutions
ms.assetid: d1f52431-1c7d-4dc6-8792-6b988256892e
ms.openlocfilehash: ab2ed6ff87f2d50d0f518ac64188bf8b5c98351c
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768099"
---
# <a name="substitutions-in-regular-expressions"></a>Substituições em expressões regulares
As substituições são elementos de linguagem que são reconhecidos apenas em padrões de substituição. Eles usam um padrão de expressão regular para definir o todo ou parte do texto que substitui o texto correspondente na cadeia de caracteres de entrada. O padrão de substituição pode consistir em uma ou mais substituições junto com caracteres literais. Padrões de substituição são fornecidos para sobrecargas do método <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> que têm um parâmetro `replacement` e para o método <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType>. Os métodos substituem o padrão correspondente pelo padrão que é definido pelo parâmetro `replacement`.  
  
 O .NET Framework define os elementos de substituição listados na tabela a seguir.  
  
|Substituição|Description|  
|------------------|-----------------|  
|$ *number*|Inclui a última subcadeia de caracteres correspondida pelo grupo de captura que é identificado por *number*, no qual *number* é um valor decimal na cadeia de caracteres de substituição. Para obter mais informações, consulte [substituindo um grupo numerado](#substituting-a-numbered-group).|  
|${ *name* }|Inclui a última subcadeia de caracteres correspondida pelo grupo nomeado designado pelo `(?<` *nome* `> )` na cadeia de caracteres de substituição. Para obter mais informações, consulte [substituindo um grupo nomeado](#substituting-a-named-group).|  
|$$|Inclui um único literal “$” na cadeia de caracteres de substituição. Para obter mais informações, consulte [Substituindo um símbolo "$"](#substituting-a--character).|  
|$&|Inclui uma cópia da correspondência inteira na cadeia de caracteres de substituição. Para obter mais informações, consulte [substituindo toda a correspondência](#substituting-the-entire-match).|  
|$\`|Inclui todo o texto da cadeia de caracteres de entrada antes da correspondência na cadeia de caracteres de substituição. Para obter mais informações, consulte [substituindo o texto antes da correspondência](#substituting-the-text-before-the-match).|  
|$'|Inclui todo o texto da cadeia de caracteres de entrada após a correspondência na cadeia de caracteres de substituição. Para obter mais informações, consulte [substituindo o texto após a correspondência](#substituting-the-text-after-the-match).|  
|$+|O inclui o último grupo capturado na cadeia de caracteres de substituição. Para obter mais informações, consulte [substituindo o último grupo capturado](#substituting-the-last-captured-group).|  
|$\_|Inclui a cadeia de entrada inteira na cadeia de caracteres de substituição. Para obter mais informações, consulte [substituindo toda a cadeia de caracteres de entrada](#substituting-the-entire-input-string).|  
  
## <a name="substitution-elements-and-replacement-patterns"></a>Elementos de substituição e padrões de substituição  
 As substituições são as únicas construções especiais reconhecidas em um padrão de substituição. Nenhum dos outros elementos de linguagem de expressões regulares, incluindo caracteres de escape e o ponto final (`.`), que corresponde a qualquer caractere, são aceitos. Da mesma forma, os elementos de linguagem de substituição são reconhecidos apenas nos padrões de substituição e nunca são válidos em padrões de expressões regulares.  
  
 O único caractere que pode aparecer em um padrão de expressão regular ou em uma substituição é o caractere `$`, embora ele tenha um significado diferente em cada contexto. Em um padrão de expressão regular, `$` é uma âncora que corresponde ao final da cadeia de caracteres. Em um padrão de substituição, `$` indica o início de uma substituição.  
  
> [!NOTE]
> Para uma funcionalidade semelhante a um padrão de substituição dentro de uma expressão regular, use um backreference. Para obter mais informações sobre referências anteriores, consulte [construções de referência anterior](backreference-constructs-in-regular-expressions.md).  

## <a name="substituting-a-numbered-group"></a>Substituindo um grupo numerado  
 O `$` elemento de idioma *Number* inclui a última subcadeia de caracteres correspondida pelo grupo de captura de *número* na cadeia de caracteres de substituição, em que *Number* é o índice do grupo de captura. Por exemplo, o padrão de substituição `$1` indica que a subcadeia de caracteres correspondente deve ser substituída pelo primeiro grupo capturado. Para saber mais sobre os grupos de captura numerados, consulte [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).  
  
 Todos os dígitos a seguir `$` são interpretados como pertencentes ao grupo de *números* . Se essa não for sua intenção, você poderá substituir um grupo nomeado. Por exemplo, você pode usar a cadeia de caracteres de substituição `${1}1` em vez de `$11` para definir a cadeia de caracteres de substituição como o valor do primeiro grupo capturado junto com o número “1 ". Para obter mais informações, consulte [substituindo um grupo nomeado](#substituting-a-named-group).  
  
 A captura de grupos que não são atribuídos explicitamente a nomes usando a sintaxe de `(?<` *nome* `>)` são numeradas da esquerda para a direita, começando em um. Os grupos nomeados também são numerados da esquerda para a direita, começando em um maior que o índice do último grupo não nomeado. Por exemplo, na expressão regular `(\w)(?<digit>\d)`, o índice do grupo nomeado `digit` é 2.  
  
 Se *number* não especificar um grupo de captura válido definido no padrão de expressão regular, `$`*number* será interpretado como uma sequência de caracteres literal que será usada para substituir cada correspondência.  
  
 O exemplo a seguir usa a substituição `$`*número* para remover o símbolo de moeda de um valor decimal. Ele remove os símbolos de moeda localizados no início ou término de um valor monetário e reconhece os dois separadores decimais mais comuns (“.” e “, ").  
  
 [!code-csharp[Conceptual.RegEx.Language.Substitutions#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.substitutions/cs/numberedgroup1.cs#1)]
 [!code-vb[Conceptual.RegEx.Language.Substitutions#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.substitutions/vb/numberedgroup1.vb#1)]  
  
 O padrão de expressão regular `\p{Sc}*(\s?\d+[.,]?\d*)\p{Sc}*` é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\p{Sc}*`|Corresponde a zero ou mais caracteres de símbolos de moedas.|  
|`\s?`|Corresponder a zero ou a um caractere de espaço em branco.|  
|`\d+`|Corresponde a um ou mais dígitos decimais.|  
|`[.,]?`|Corresponde a zero ou um ponto ou vírgula.|  
|`\d*`|Corresponde a zero ou mais dígitos decimais.|  
|`(\s?\d+[.,]?\d*)`|Corresponde a um espaço em branco seguido por um ou mais dígitos decimais, seguidos por zero ou um ponto ou uma vírgula, seguidos por zero ou mais dígitos decimais. Este é o primeiro grupo de captura. Como o padrão de substituição é `$1`, a chamada ao método <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> substitui a subcadeia de caracteres inteira correspondente a esse grupo capturado.|  

## <a name="substituting-a-named-group"></a>Substituindo um grupo nomeado  
 O `${` elemento *Name* `}` Language substitui a última subcadeia de caracteres correspondida pelo *nome* capturando o grupo, em que *Name* é o nome de um grupo de captura definido pelo `(?<` elemento *Name* `>)` Language. Para saber mais sobre os grupos de captura nomeados, consulte [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).  
  
 Se o *nome* não especificar um grupo de captura nomeado válido definido no padrão de expressão regular, mas consistir em dígitos, o `${` *nome* `}` será interpretado como um grupo numerado.  
  
 Se *name* não especificar um grupo de captura nomeado válido nem um grupo de captura numerado válido, definido no padrão da expressão regular, `${`*name*`}` será interpretado como uma sequência de caracteres literal que será usada para substituir cada correspondência.  
  
 O exemplo a seguir usa a substituição de `${` *nome* `}` para remover o símbolo de moeda de um valor decimal. Ele remove os símbolos de moeda localizados no início ou término de um valor monetário e reconhece os dois separadores decimais mais comuns (“.” e “, ").  
  
 [!code-csharp[Conceptual.RegEx.Language.Substitutions#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.substitutions/cs/namedgroup1.cs#2)]
 [!code-vb[Conceptual.RegEx.Language.Substitutions#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.substitutions/vb/namedgroup1.vb#2)]  
  
 O padrão de expressão regular `\p{Sc}*(?<amount>\s?\d[.,]?\d*)\p{Sc}*` é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\p{Sc}*`|Corresponde a zero ou mais caracteres de símbolos de moedas.|  
|`\s?`|Corresponder a zero ou a um caractere de espaço em branco.|  
|`\d+`|Corresponde a um ou mais dígitos decimais.|  
|`[.,]?`|Corresponde a zero ou um ponto ou vírgula.|  
|`\d*`|Corresponde a zero ou mais dígitos decimais.|  
|`(?<amount>\s?\d[.,]?\d*)`|Corresponde a um espaço em branco seguido por um ou mais dígitos decimais, seguidos por zero ou um ponto ou uma vírgula, seguidos por zero ou mais dígitos decimais. Este é o grupo de captura chamado `amount`. Como o padrão de substituição é `${amount}`, a chamada ao método <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> substitui a subcadeia de caracteres inteira correspondente a esse grupo capturado.|  

## <a name="substituting-a--character"></a>Substituindo um caractere “$”  
 A substituição de `$$` insere um caractere literal “$” na cadeia de caracteres substituída.  
  
 O exemplo a seguir usa o objeto <xref:System.Globalization.NumberFormatInfo> para determinar o símbolo de moeda atual da cultura e seu posicionamento em uma cadeia de caracteres de moeda. Ele então cria um padrão de expressão regular e um padrão de substituição dinamicamente. Se o exemplo é executado em um computador cuja cultura atual é en-US, ele gera o padrão de expressão regular `\b(\d+)(\.(\d+))?` e o padrão de substituição `$$ $1$2`. O padrão de substituição substitui o texto correspondente por um símbolo de moeda e um espaço seguido pelo primeiro e segundo grupos capturados.  
  
 [!code-csharp[Conceptual.Regex.Language.Substitutions#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.substitutions/cs/dollarsign1.cs#8)]
 [!code-vb[Conceptual.Regex.Language.Substitutions#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.substitutions/vb/dollarsign1.vb#8)]  
  
 O padrão de expressão regular `\b(\d+)(\.(\d+))?` é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Inicia a correspondência no começo de um limite de palavra.|  
|`(\d+)`|Corresponde a um ou mais dígitos decimais. Este é o primeiro grupo de captura.|  
|`\.`|Faz a correspondência a um período (o separador decimal).|  
|`(\d+)`|Corresponde a um ou mais dígitos decimais. Este é o terceiro grupo de captura.|  
|`(\.(\d+))?`|Faz a correspondência de zero ou uma ocorrência de um período seguido por um ou mais dígitos decimais. Este é o segundo grupo de captura.|  

## <a name="substituting-the-entire-match"></a>Substituindo a correspondência inteira  
 A substituição `$&` inclui a correspondência inteira na cadeia de caracteres de substituição. Muitas vezes, ela é usada para adicionar uma subcadeia de caracteres ao início ou fim da cadeia de caracteres correspondente. Por exemplo, o padrão de substituição `($&)` adiciona parênteses no início e no final de cada correspondência. Se não houver correspondência, a substituição `$&` não terá efeito.  
  
 O exemplo a seguir usa a substituição `$&` para adicionar aspas no início e no final de títulos de livros armazenados em uma matriz de cadeia de caracteres.  
  
 [!code-csharp[Conceptual.RegEx.Language.Substitutions#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.substitutions/cs/entirematch1.cs#3)]
 [!code-vb[Conceptual.RegEx.Language.Substitutions#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.substitutions/vb/entirematch1.vb#3)]  
  
 O padrão de expressão regular `^(\w+\s?)+$` é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`^`|Começa a correspondência no início da cadeia de caracteres de entrada.|  
|`(\w+\s?)+`|Corresponde ao padrão de um ou mais caracteres de palavra seguidos por zero ou um espaço em branco uma ou mais vezes.|  
|`$`|Corresponder ao final da cadeia de caracteres de entrada.|  
  
 O padrão de substituição `"$&"` adiciona aspas literais no início e no final de cada correspondência.  

## <a name="substituting-the-text-before-the-match"></a>Substituindo texto antes da correspondência  
 A substituição ``$` `` substitui a cadeia de caracteres correspondida pela cadeia de caracteres de entrada inteira antes da correspondência. Ou seja, ela duplica a cadeia de caracteres de entrada até a correspondência e remove o texto correspondido. Qualquer texto após o texto correspondido permanece inalterado na cadeia de caracteres de resultado. Se houver várias correspondências em uma cadeia de caracteres de entrada, o texto de substituição será derivado da cadeia de caracteres de entrada original, em vez da cadeia de caracteres em que o texto foi substituído por correspondências anteriores. \(O exemplo fornece uma ilustração. \) Se não houver correspondência, a ``$` `` substituição não terá nenhum efeito.  
  
 O exemplo a seguir usa o padrão de expressão regular `\d+` para corresponder a uma sequência de um ou mais dígitos decimais na cadeia de caracteres de entrada. A cadeia de caracteres de substituição ``$` `` substitui esses dígitos pelo texto que precede a correspondência.  
  
 [!code-csharp[Conceptual.Regex.Language.Substitutions#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.substitutions/cs/before1.cs#4)]
 [!code-vb[Conceptual.Regex.Language.Substitutions#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.substitutions/vb/before1.vb#4)]  
  
 Neste exemplo, a cadeia de caracteres de entrada `"aa1bb2cc3dd4ee5"` contém cinco correspondências. A tabela a seguir ilustra como a substituição ``$` `` faz com que o mecanismo de expressão regular substitua cada correspondência na cadeia de caracteres de entrada. O texto inserido é mostrado em negrito na coluna de resultados.  
  
|Corresponder a|Posição|Cadeia de caracteres antes da correspondência|Cadeia de caracteres de resultado|  
|-----------|--------------|-------------------------|-------------------|  
|1|2|aa|aa**aa**bb2cc3dd4ee5|  
|2|5|aa1bb|aaaabb**aa1bb**cc3dd4ee5|  
|3|8|aa1bb2cc|aaaabbaa1bbcc**aa1bb2cc**dd4ee5|  
|4|11|aa1bb2cc3dd|aaaabbaa1bbccaa1bb2ccdd**aa1bb2cc3dd**ee5|  
|5|14|aa1bb2cc3dd4ee|aaaabbaa1bbccaa1bb2ccddaa1bb2cc3ddee**aa1bb2cc3dd4ee**|

## <a name="substituting-the-text-after-the-match"></a>Substituindo texto após a correspondência  
 A substituição `$'` substitui a cadeia de caracteres correspondida pela cadeia de caracteres de entrada inteira após a correspondência. Ou seja, ela duplica a cadeia de caracteres de entrada após a correspondência e remove o texto correspondido. Qualquer texto antes do texto correspondido permanece inalterado na cadeia de caracteres de resultado. Se não houver correspondência, a substituição `$'` não terá efeito.  
  
 O exemplo a seguir usa o padrão de expressão regular `\d+` para corresponder a uma sequência de um ou mais dígitos decimais na cadeia de caracteres de entrada. A cadeia de caracteres de substituição `$'` substitui esses dígitos pelo texto após a correspondência.  
  
 [!code-csharp[Conceptual.Regex.Language.Substitutions#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.substitutions/cs/after1.cs#5)]
 [!code-vb[Conceptual.Regex.Language.Substitutions#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.substitutions/vb/after1.vb#5)]  
  
 Neste exemplo, a cadeia de caracteres de entrada `"aa1bb2cc3dd4ee5"` contém cinco correspondências. A tabela a seguir ilustra como a substituição `$'` faz com que o mecanismo de expressão regular substitua cada correspondência na cadeia de caracteres de entrada. O texto inserido é mostrado em negrito na coluna de resultados.  
  
|Corresponder a|Posição|Cadeia de caracteres após a correspondência|Cadeia de caracteres de resultado|  
|-----------|--------------|------------------------|-------------------|  
|1|2|bb2cc3dd4ee5|aa**bb2cc3dd4ee5**bb2cc3dd4ee5|  
|2|5|cc3dd4ee5|aabb2cc3dd4ee5bb**cc3dd4ee5**cc3dd4ee5|  
|3|8|dd4ee5|aabb2cc3dd4ee5bbcc3dd4ee5cc**dd4ee5**dd4ee5|  
|4|11|ee5|aabb2cc3dd4ee5bbcc3dd4ee5ccdd4ee5dd**ee5**ee5|  
|5|14|<xref:System.String.Empty?displayProperty=nameWithType>|aabb2cc3dd4ee5bbcc3dd4ee5ccdd4ee5ddee5ee|  

## <a name="substituting-the-last-captured-group"></a>Substituindo o último grupo capturado  
 A substituição `$+` substitui a cadeia de caracteres correspondida pelo último grupo capturado. Se não houver nenhum grupo capturado ou se o valor do grupo capturado por último for <xref:System.String.Empty?displayProperty=nameWithType>, a substituição `$+` não terá efeito.  
  
 O exemplo a seguir identifica palavras duplicadas em uma cadeia de caracteres e usa a substituição `$+` para substituí-los por uma única ocorrência da palavra. A opção <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> é usada para garantir que as palavras que diferem apenas em termos de letras maiúsculas e minúsculas, mas que de outra forma são idênticas, sejam consideradas duplicadas.  
  
 [!code-csharp[Conceptual.Regex.Language.Substitutions#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.substitutions/cs/lastmatch1.cs#6)]
 [!code-vb[Conceptual.Regex.Language.Substitutions#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.substitutions/vb/lastmatch1.vb#6)]  
  
 O padrão de expressão regular `\b(\w+)\s\1\b` é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`(\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. Este é o primeiro grupo de captura.|  
|`\s`|Corresponde a um caractere de espaço em branco.|  
|`\1`|Corresponde ao primeiro grupo capturado.|  
|`\b`|Termina a correspondência em um limite de palavra.|  

## <a name="substituting-the-entire-input-string"></a>Substituindo a cadeia de caracteres de entrada inteira  
 A substituição `$_` substitui a cadeia de caracteres correspondida pela cadeia de caracteres de entrada inteira. Ou seja, ela remove o texto correspondido e o substitui pela cadeia de caracteres inteira, inclusive o texto correspondido.  
  
 O exemplo a seguir corresponde a um ou mais dígitos decimais na cadeia de caracteres de entrada. Ele usa a substituição `$_` para substituí-los pela cadeia de caracteres de entrada inteira.  
  
 [!code-csharp[Conceptual.Regex.Language.Substitutions#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.substitutions/cs/entire1.cs#7)]
 [!code-vb[Conceptual.Regex.Language.Substitutions#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.substitutions/vb/entire1.vb#7)]  
  
 Neste exemplo, a cadeia de caracteres de entrada `"ABC123DEF456"` contém duas correspondências. A tabela a seguir ilustra como a substituição `$_` faz com que o mecanismo de expressão regular substitua cada correspondência na cadeia de caracteres de entrada. O texto inserido é mostrado em negrito na coluna de resultados.  
  
|Corresponder a|Posição|Corresponder a|Cadeia de caracteres de resultado|  
|-----------|--------------|-----------|-------------------|  
|1|3|123|ABC**ABC123DEF456**DEF456|  
|2|5|456|ABCABC123DEF456DEF**ABC123DEF456**|  
  
## <a name="see-also"></a>Veja também

- [Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)
