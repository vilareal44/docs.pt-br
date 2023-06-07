---
title: Expressões regulares do .NET
description: Use expressões regulares para localizar padrões de caracteres específicos, validar texto, trabalhar com subcadeias de texto, & adicionar cadeias de caracteres extraídas a uma coleção no .NET.
ms.date: 06/30/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- pattern-matching with regular expressions, about pattern-matching
- substrings
- searching with regular expressions, about regular expressions
- pattern-matching with regular expressions
- searching with regular expressions
- parsing text with regular expressions
- regular expressions [.NET Framework], about regular expressions
- regular expressions [.NET Framework]
- .NET Framework regular expressions, about
- characters [.NET Framework], regular expressions
- parsing text with regular expressions, overview
- .NET Framework regular expressions
- strings [.NET Framework], regular expressions
ms.assetid: 521b3f6d-f869-42e1-93e5-158c54a6895d
ms.openlocfilehash: f57199c2ddf6569020554e74b6e70801844da641
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85802891"
---
# <a name="net-regular-expressions"></a>Expressões regulares do .NET

Expressões regulares oferecem um método poderoso, flexível e eficiente de processamento de texto. A notação extensiva de correspondência de padrões de expressões regulares permite que você analise rapidamente grandes quantidades de texto para:

- Localizar padrões de caracteres específicos.
- Valide o texto para garantir que ele corresponda a um padrão predefinido (como um endereço de email).
- Extraia, edite, substitua ou exclua subcadeias de texto.
- Adicione cadeias de caracteres extraídas a uma coleção para gerar um relatório.

Para vários aplicativos que lidam com cadeias de caracteres ou que analisam grandes blocos de texto, as expressões regulares são uma ferramenta indispensável.  
  
## <a name="how-regular-expressions-work"></a>Como as expressões regulares funcionam

 A parte mais importante do processamento de texto com expressões regulares é o mecanismo de expressão regular, que é representado pelo objeto <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> no .NET. No mínimo, o processamento de texto usando expressões regulares exige que o mecanismo de expressões regulares seja fornecido com os dois itens informativos a seguir:  
  
- O padrão de expressão regular a ser identificado no texto.  
  
     No .NET, padrões de expressão regular são definidos por uma sintaxe ou linguagem especial, compatível com expressões regulares Perl 5 e inclui alguns recursos adicionais, como correspondência da direita para a esquerda. Para obter mais informações, consulte [Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md).  
  
- O texto a ser analisado para o padrão de expressão regular.  
  
Os métodos da classe <xref:System.Text.RegularExpressions.Regex> permitem que você realize as seguintes operações:  
  
- Determinar se o padrão de expressão regular ocorre no texto de entrada chamando o método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType>. Para obter um exemplo que usa o método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A> para validar texto, confira [Como verificar se cadeias de caracteres estão em um formato de email válido](how-to-verify-that-strings-are-in-valid-email-format.md).  
  
- Recuperar uma ou todas as ocorrências de texto que corresponde ao padrão de expressão regular chamando o método <xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType> ou <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType>. O primeiro método retorna um objeto <xref:System.Text.RegularExpressions.Match?displayProperty=nameWithType> que oferece informações sobre o texto correspondente. O último retorna um objeto <xref:System.Text.RegularExpressions.MatchCollection> que contém um objeto <xref:System.Text.RegularExpressions.Match?displayProperty=nameWithType> para cada correspondência encontrada no texto analisado.  
  
- Substitua o texto que corresponde ao padrão da expressão regular chamando o método <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType>. Para ver exemplos que usam o método <xref:System.Text.RegularExpressions.Regex.Replace%2A> para alterar formatos de data e remover caracteres inválidos de uma cadeia de caracteres, confira [Como retirar caracteres inválidos de uma cadeia de caracteres](how-to-strip-invalid-characters-from-a-string.md) e [Exemplo: alterando formatos de data](regular-expression-example-changing-date-formats.md).  
  
Para obter uma visão geral do modelo de objeto de expressão regular, consulte [O modelo de objeto de expressão regular](the-regular-expression-object-model.md).  
  
Para saber mais sobre a linguagem de expressão regular, confira [Linguagem de expressão regular - referência rápida](regular-expression-language-quick-reference.md) ou faça download e imprima um dos seguintes folhetos:  
  
- [Referência rápida no formato Word (.docx)](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.docx)  
- [Referência rápida no formato PDF (.pdf)](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.pdf)  
  
## <a name="regular-expression-examples"></a>Exemplos de expressões regulares

A classe <xref:System.String> inclui uma série de métodos de pesquisa de cadeia de caracteres e substituição que podem ser usados quando você quer localizar cadeias de caracteres literais em uma cadeia de caracteres maior. Expressões regulares são mais úteis quando você quer localizar uma dentre diversas subcadeias de caracteres em uma cadeia de caracteres maior ou quando quer identificar padrões em uma cadeia de caracteres, como mostrado nos exemplos a seguir.

[!INCLUDE [regex](../../../includes/regex.md)]

> [!TIP]
> O namespace <xref:System.Web.RegularExpressions> contém vários objetos de expressão regular que implementam padrões predefinidos de expressão regular para analisar cadeias de caracteres de documentos HTML, XML e ASP.NET. Por exemplo, a classe <xref:System.Web.RegularExpressions.TagRegex> identifica marcas de início em uma cadeia de caracteres e a classe <xref:System.Web.RegularExpressions.CommentRegex> identifica comentários ASP.NET em uma cadeia de caracteres.

### <a name="example-1-replace-substrings"></a>Exemplo 1: substituir subcadeias de caracteres  

 Vamos presumir que uma lista de endereçamento contém nomes que, às vezes, incluem um pronome de tratamento (Sr., Srs., Srta. ou Sra.) junto com o nome e o sobrenome. Se não quiser incluir os pronomes de tratamento ao gerar etiquetas para envelopes na lista, é possível usar uma expressão regular para removê-los, como mostrado no exemplo a seguir.  
  
 [!code-csharp[Conceptual.Regex#2](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex/cs/example1.cs#2)]
 [!code-vb[Conceptual.Regex#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex/vb/example1.vb#2)]  
  
 O padrão de expressão regular `(Mr\.? |Mrs\.? |Miss |Ms\.? )` corresponde a qualquer ocorrência de “Sr”, “Sr.”, “Sra”, “Sra.”, “Srta” ou “Srta.”. A chamada para o método <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> substitui a cadeia de caracteres correspondida por <xref:System.String.Empty?displayProperty=nameWithType>; em outras palavras, ela a remove da cadeia de caracteres original.  
  
### <a name="example-2-identify-duplicated-words"></a>Exemplo 2: identificar palavras duplicadas  

 Duplicar palavras acidentalmente é um erro comum que escritores cometem. Uma expressão regular pode ser usada para identificar palavras duplicadas, como mostrado no exemplo a seguir.  
  
 [!code-csharp[Conceptual.Regex#3](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex/cs/example2.cs#3)]
 [!code-vb[Conceptual.Regex#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex/vb/example2.vb#3)]  
  
 O padrão de expressão regular `\b(\w+?)\s\1\b` pode ser interpretado da seguinte maneira:  
  
> [!div class="mx-tdCol2BreakAll"]
> |Padrão|Interpretação|  
> |-|-|
> |`\b`|Iniciar em um limite de palavra.|  
> |`(\w+?)`|Corresponder a um ou mais caracteres de palavra, mas o menor número de caracteres possível. Juntos, formam um grupo que pode ser chamado de `\1`.|  
> |`\s`|Corresponde a um caractere de espaço em branco.|  
> |`\1`|Corresponder à subcadeia de caracteres que é igual ao grupo denominado `\1`.|  
> |`\b`|Corresponder a um limite de palavra.|  
  
 O método <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> é chamado com opções de expressão regular definidas como <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType>. Portanto, a operação de correspondência não faz distinção entre maiúsculas e minúsculas e o exemplo identifica a subcadeia de caracteres “This this” como uma duplicação.  
  
 A cadeia de caracteres de entrada inclui a subcadeia de caracteres "esta? This”. No entanto, devido à pontuação no meio, ela não é identificada como uma duplicação.  
  
### <a name="example-3-dynamically-build-a-culture-sensitive-regular-expression"></a>Exemplo 3: criar dinamicamente uma expressão regular sensível à cultura  

 O exemplo a seguir mostra a força das expressões regulares combinada à flexibilidade oferecida pelos recursos de globalização do .NET. Ele utiliza o objeto <xref:System.Globalization.NumberFormatInfo> para determinar o formato de valores de moeda na cultura atual do sistema. Então, essa informação é usada para construir dinamicamente uma expressão regular que extrai valores de moeda do texto. Para cada correspondência, ele extrai o subgrupo que contém somente a subcadeia de caracteres, converte-a para um valor <xref:System.Decimal> e calcula uma soma acumulada.  
  
 [!code-csharp[Conceptual.Regex#1](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex/cs/example.cs#1)]
 [!code-vb[Conceptual.Regex#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex/vb/example.vb#1)]  
  
 Em um computador cuja cultura atual seja Inglês – Estados Unidos (en-US), o exemplo compila dinamicamente a expressão regular `\$\s*[-+]?([0-9]{0,3}(,[0-9]{3})*(\.[0-9]+)?)`. Esse padrão de expressão regular pode ser interpretado da seguinte maneira:  

> [!div class="mx-tdCol2BreakAll"]
> |Padrão|Interpretação|  
> |-|-|  
> |`\$`|Procure uma única ocorrência do símbolo de cifrão (`$`) na cadeia de caracteres de entrada. A cadeia de caracteres do padrão de expressão regular inclui uma barra invertida para indicar que o símbolo de cifrão deve ser interpretado literalmente ao invés de como uma âncora de expressão regular. (O `$` símbolo sozinho indicaria que o mecanismo de expressões regulares deve tentar iniciar sua correspondência no final de uma cadeia de caracteres.) Para garantir que o símbolo de moeda da cultura atual não seja interpretado erroneamente como um símbolo de expressão regular, o exemplo chama o <xref:System.Text.RegularExpressions.Regex.Escape%2A?displayProperty=nameWithType> método para escapar o caractere.|  
> |`\s*`|Procure zero ou mais ocorrências de um caractere de espaço em branco.|  
> |`[-+]?`|Procure zero ou uma ocorrência de um sinal de positivo ou negativo.|  
> |`([0-9]{0,3}(,[0-9]{3})*(\.[0-9]+)?)`|Os parênteses externos ao redor da expressão a definem como um grupo de captura ou uma subexpressão. Se uma correspondência for localizada, informações sobre essa parte da cadeia de caracteres correspondente podem ser recuperadas do segundo objeto <xref:System.Text.RegularExpressions.Group> no objeto <xref:System.Text.RegularExpressions.GroupCollection> retornado pela propriedade <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType>. (O primeiro elemento na coleção representa a correspondência inteira.)|  
> |`[0-9]{0,3}`|Procure de zero a três ocorrências dos dígitos decimais de 0 a 9.|  
> |`(,[0-9]{3})*`|Procure zero ou mais ocorrências de um separador de grupo seguido por três dígitos decimais.|  
> |`\.`|Procure uma única ocorrência do separador decimal.|  
> |`[0-9]+`|Procure por um ou mais dígitos decimais.|  
> |`(\.[0-9]+)?`|Procure zero ou uma ocorrência de um separador decimal seguido por pelo menos um dígito decimal.|  
  
 Se cada um desses subpadrões for encontrado na cadeia de caracteres de entrada, a correspondência será bem-sucedida, e um objeto <xref:System.Text.RegularExpressions.Match> que contém informações sobre a correspondência será adicionado ao objeto <xref:System.Text.RegularExpressions.MatchCollection>.  
  
## <a name="related-topics"></a>Tópicos relacionados  
  
|Title|Descrição|  
|-----------|-----------------|  
|[Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)|Oferece informações sobre o conjunto de caracteres, operadores e constructos que você pode usar para definir expressões regulares.|  
|[O modelo de objeto de expressão regular](the-regular-expression-object-model.md)|Oferece informações e exemplos de código que ilustram como usar as classes de expressão regular.|  
|[Detalhes do comportamento de expressões regulares](details-of-regular-expression-behavior.md)|Oferece informações sobre os recursos e o comportamento das expressões regulares do .NET.|
|[Usar expressões regulares no Visual Studio](/visualstudio/ide/using-regular-expressions-in-visual-studio)||
  
## <a name="reference"></a>Referência  

- <xref:System.Text.RegularExpressions?displayProperty=nameWithType>  
- <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType>  
- [Expressões regulares - referência rápida (fazer download no formato Word)](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.docx)  
- [Expressões regulares - referência rápida (fazer download no formato PDF)](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.pdf)
