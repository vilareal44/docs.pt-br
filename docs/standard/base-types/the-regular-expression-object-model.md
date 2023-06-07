---
title: O modelo de objeto de expressão regular
description: Examine o modelo de objeto de expressão regular no .NET. Trabalhe com o mecanismo de expressões regulares, & objetos & coleções relacionadas à correspondência, ao agrupamento & a captura.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- searching with regular expressions, backreferences
- Regex class
- Match class
- pattern-matching with regular expressions, backreferences
- .NET Framework regular expressions, classes
- CaptureCollection class
- Group class
- characters [.NET Framework], backreferences
- substrings
- .NET Framework regular expressions, backreferences
- searching with regular expressions, classes
- backreferences
- Capture class
- repeating groups of characters
- MatchCollection class
- parsing text with regular expressions, backreferences
- regular expressions [.NET Framework]
- characters [.NET Framework], regular expressions
- classes [.NET Framework], regular expression
- regular expressions [.NET Framework], classes
- characters [.NET Framework], metacharacters
- metacharacters, regular expression classes
- metacharacters, backreferences
- parsing text with regular expressions, classes
- regular expressions [.NET Framework], backreferences
- strings [.NET Framework], regular expressions
- pattern-matching with regular expressions, classes
- GroupCollection class
ms.assetid: 49a21470-64ca-4b5a-a889-8e24e3c0af7e
ms.openlocfilehash: 43672b85ecb64a15179881ec23c7fadd13d64868
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768047"
---
# <a name="the-regular-expression-object-model"></a>O modelo de objeto de expressão regular
<a name="introduction"></a> Este tópico descreve o modelo do objeto usado ao trabalhar com expressões regulares do .NET. Ele contém as seções a seguir:  
  
- [O mecanismo de expressão regular](#Engine)  
  
- [Os objetos MatchCollection e Match](#Match_and_MCollection)  
  
- [A coleção de grupos](#GroupCollection)  
  
- [O grupo capturado](#the_captured_group)  
  
- [A coleção de captura](#CaptureCollection)  
  
- [A captura individual](#the_individual_capture)  
  
<a name="Engine"></a>
## <a name="the-regular-expression-engine"></a>O mecanismo da expressão regular  
 O mecanismo de expressões regulares no .NET é representada pela classe <xref:System.Text.RegularExpressions.Regex>. O mecanismo de expressões regulares é responsável por analisar e compilar uma expressão regular e realizar operações que correspondem ao padrão da expressão regular com uma cadeia de caracteres de entrada. O mecanismo é o componente central no modelo do objeto da expressão regular do .NET.  
  
 Você pode usar o mecanismo de expressões regulares de duas maneiras:  
  
- Ao calcular os métodos estáticos da classe <xref:System.Text.RegularExpressions.Regex>. Os parâmetros do método incluem a cadeia de caracteres de entrada e o padrão da expressão regular. O mecanismo de expressões regulares armazena em cache expressões regulares que são usadas em chamadas de método estático; por isso, chamadas repetidas a métodos de expressão regular estáticos que usam a mesma expressão regular oferecem um desempenho relativamente bom.  
  
- Ao instanciar um objeto <xref:System.Text.RegularExpressions.Regex>, transmitindo uma expressão regular ao construtor da classe. Nesse caso, o objeto <xref:System.Text.RegularExpressions.Regex> é imutável (somente leitura) e representa um mecanismo de expressão regular que está ligado rigorosamente a uma única expressão regular. Como as expressões regulares usadas por instâncias <xref:System.Text.RegularExpressions.Regex> não são armazenadas em cache, você não deve instanciar um objeto <xref:System.Text.RegularExpressions.Regex> várias vezes com a mesma expressão regular.  
  
 Você pode chamar os métodos da classe <xref:System.Text.RegularExpressions.Regex> para realizar as seguintes operações:  
  
- Determinar se uma cadeia de caracteres corresponde a um padrão de expressão regular.  
  
- Extrair uma única correspondência ou a primeira correspondência.  
  
- Extrair todas as correspondências.  
  
- Substituir uma subcadeia de caracteres correspondida.  
  
- Dividir uma cadeia única em uma matriz de cadeias de caracteres.  
  
 Essas operações são descritas nas seções a seguir.  
  
### <a name="matching-a-regular-expression-pattern"></a>Correspondendo a um padrão de expressão regular  
 O método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> retorna `true` se a cadeia corresponder ao padrão, ou `false` se não corresponder. Geralmente, o método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A> é usado para validar a entrada da cadeia de caracteres. Por exemplo, o código a seguir assegura que uma cadeia de caracteres corresponda a um número do cadastro de pessoas físicas válido nos Estados Unidos.  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/validate1.cs#1)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/validate1.vb#1)]  
  
 O padrão da expressão regular `^\d{3}-\d{2}-\d{4}$` é interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`^`|Corresponder ao início da cadeia de caracteres de entrada.|  
|`\d{3}`|Corresponder a três dígitos decimais.|  
|`-`|Corresponder a um hífen.|  
|`\d{2}`|Corresponde a dois dígitos decimais.|  
|`-`|Corresponder a um hífen.|  
|`\d{4}`|Corresponder a quatro dígitos decimais.|  
|`$`|Corresponder ao final da cadeia de caracteres de entrada.|  
  
### <a name="extracting-a-single-match-or-the-first-match"></a>Extraindo uma única correspondência ou a primeira correspondência  
 O método <xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType> retorna um objeto <xref:System.Text.RegularExpressions.Match> que contém informações sobre a primeira subcadeia de caracteres que corresponde a um padrão de expressão regular. Se a propriedade `Match.Success` retornar `true`, indicando que uma correspondência foi localizada, você pode recuperar informações sobre correspondências subsequentes chamando o método <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType>. Essas chamadas de método podem continuar até que a propriedade `Match.Success` retorne `false`. Por exemplo, o código a seguir usa o método <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%29?displayProperty=nameWithType> para localizar a primeira ocorrência de uma palavra duplicada em uma cadeia de caracteres. Em seguida, ele chama o método <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> para encontrar ocorrências adicionais. O exemplo examina a propriedade `Match.Success` após cada chamada de método para determinar se a correspondência atual foi bem-sucedida e se uma chamada para o método <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> deve seguir.  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/match1.cs#2)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/match1.vb#2)]  
  
 O padrão da expressão regular `\b(\w+)\W+(\1)\b` é interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começa a correspondência em um limite de palavra.|  
|`(\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. Este é o primeiro grupo de captura.|  
|`\W+`|Estabeleça a correspondência com um ou mais caracteres que não compõem palavras.|  
|`(\1)`|Corresponder à primeira cadeia capturada. Este é o segundo grupo de captura.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
  
### <a name="extracting-all-matches"></a>Extraindo todas as correspondências  
 O método <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> retorna um objeto <xref:System.Text.RegularExpressions.MatchCollection> que contém informações sobre todas as correspondências que o mecanismo de expressão regular localizou na cadeia de caracteres de entrada. Por exemplo, o exemplo anterior poderia ser regravado para chamar o método <xref:System.Text.RegularExpressions.Regex.Matches%2A> ao invés dos métodos <xref:System.Text.RegularExpressions.Regex.Match%2A> e <xref:System.Text.RegularExpressions.Match.NextMatch%2A>.  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/matches1.cs#3)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/matches1.vb#3)]  
  
### <a name="replacing-a-matched-substring"></a>Substituindo uma subcadeia de caracteres correspondida  
 O método <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> substitui cada subcadeia de caracteres que corresponde ao padrão da expressão regular com uma cadeia de caracteres ou padrão de expressão regular especificado, e retorna a cadeia de caracteres de entrada inteira com as substituições. Por exemplo, o código a seguir adiciona um símbolo de moeda dos Estados Unidos antes de um número decimal em uma cadeia de caracteres.  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/replace1.cs#4)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/replace1.vb#4)]  
  
 O padrão da expressão regular `\b\d+\.\d{2}\b` é interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`\d+`|Corresponde a um ou mais dígitos decimais.|  
|`\.`|Corresponde a um ponto final.|  
|`\d{2}`|Corresponde a dois dígitos decimais.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
  
 O padrão de substituição `$$$&` é interpretado conforme a tabela a seguir.  
  
|Padrão|Cadeia de caracteres de substituição|  
|-------------|------------------------|  
|`$$`|O caractere de cifrão ($).|  
|`$&`|A subcadeia de caracteres correspondida inteira.|  
  
### <a name="splitting-a-single-string-into-an-array-of-strings"></a>Dividindo uma cadeia de caracteres única em uma matriz de cadeias de caracteres  
 O método <xref:System.Text.RegularExpressions.Regex.Split%2A?displayProperty=nameWithType> divide a cadeia de caracteres de entrada nas posições definidas por uma correspondência de expressão regular. Por exemplo, o código a seguir coloca os itens em uma lista numerada em uma matriz de cadeia de caracteres.  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/split1.cs#5)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/split1.vb#5)]  
  
 O padrão da expressão regular `\b\d{1,2}\.\s` é interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`\d{1,2}`|Corresponder a um ou dois dígitos decimais.|  
|`\.`|Corresponde a um ponto final.|  
|`\s`|Corresponde a um caractere de espaço em branco.|  
  
<a name="Match_and_MCollection"></a>
## <a name="the-matchcollection-and-match-objects"></a>A coleção de correspondência e os objetos de correspondência  
 Métodos regex retornam dois objetos que fazem parte do modelos de objeto de expressão regular: os objetos <xref:System.Text.RegularExpressions.MatchCollection> e <xref:System.Text.RegularExpressions.Match>.  
  
<a name="the_match_collection"></a>
### <a name="the-match-collection"></a>A coleção de correspondência  
 O método <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> retorna um objeto <xref:System.Text.RegularExpressions.MatchCollection> que contém objetos <xref:System.Text.RegularExpressions.Match> que representam todas as correspondências localizadas pelo mecanismo de expressão regular na ordem em que elas ocorrem na cadeia de caracteres de entrada. Se não houver correspondências, o método retorna um objeto <xref:System.Text.RegularExpressions.MatchCollection> sem membros. A propriedade <xref:System.Text.RegularExpressions.MatchCollection.Item%2A?displayProperty=nameWithType> permite que você acesse membros individuais da coleção por índice, de zero a um valor uma vez menor do que o valor da propriedade <xref:System.Text.RegularExpressions.MatchCollection.Count%2A?displayProperty=nameWithType>. <xref:System.Text.RegularExpressions.MatchCollection.Item%2A> é o indexador da coleção (no C#) e a propriedade padrão (no Visual Basic).  
  
 Por padrão, a chamada para o método <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> usa a avaliação lenta para preencher o objeto <xref:System.Text.RegularExpressions.MatchCollection>. O acesso a propriedades que requerem uma coleção completamente preenchida, como as propriedades <xref:System.Text.RegularExpressions.MatchCollection.Count%2A?displayProperty=nameWithType> e <xref:System.Text.RegularExpressions.MatchCollection.Item%2A?displayProperty=nameWithType>, pode envolver uma penalização de desempenho. Como resultado, recomendamos que você acesse a coleção usando o objeto <xref:System.Collections.IEnumerator> retornado pelo método <xref:System.Text.RegularExpressions.MatchCollection.GetEnumerator%2A?displayProperty=nameWithType>. Linguagens individuais oferecem constructos, como `For Each` no Visual Basic e `foreach` no C#, que encapsulam a interface <xref:System.Collections.IEnumerator> da coleção.  
  
 O exemplo a seguir usa o método <xref:System.Text.RegularExpressions.Regex.Matches%28System.String%29?displayProperty=nameWithType> para preencher um objeto <xref:System.Text.RegularExpressions.MatchCollection> com todas as correspondências localizadas em uma cadeia de caracteres de entrada. O exemplo enumera a coleção, copia as correspondências para uma matriz de cadeia de caracteres e registra as posições dos caracteres em uma matriz de inteiros.  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/matchcollection1.cs#6)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/matchcollection1.vb#6)]  
  
<a name="the_match"></a>
### <a name="the-match"></a>A correspondência  
 A classe <xref:System.Text.RegularExpressions.Match> representa o resultado de uma única correspondência de expressão regular. É possível acessar objetos <xref:System.Text.RegularExpressions.Match> de duas formas:  
  
- Recuperando-os do objeto <xref:System.Text.RegularExpressions.MatchCollection> que é retornado pelo método <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType>. Para recuperar objetos <xref:System.Text.RegularExpressions.Match> individuais, itere a coleção usando um constructo `foreach` (no C#) ou `For Each`...`Next` (no Visual Basic) ou use a propriedade <xref:System.Text.RegularExpressions.MatchCollection.Item%2A?displayProperty=nameWithType> para recuperar um objeto <xref:System.Text.RegularExpressions.Match> específico por índice ou por nome. Também é possível recuperar objetos <xref:System.Text.RegularExpressions.Match> individuais da coleção iterando a coleção por índice, de zero a um valor uma vez menor do que o número de objetos na coleção de dados. Entretanto, esse método não utiliza a avaliação lenta, pois ele acessa a propriedade <xref:System.Text.RegularExpressions.MatchCollection.Count%2A?displayProperty=nameWithType>.  
  
     O exemplo a seguir recupera objetos <xref:System.Text.RegularExpressions.Match> individuais de um objeto <xref:System.Text.RegularExpressions.MatchCollection> iterando a coleção usando o constructo `foreach` ou `For Each`...`Next`. A expressão regular simplesmente corresponde a cadeia de caracteres “abc” na cadeia de caracteres de entrada.  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/match2.cs#7)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/match2.vb#7)]  
  
- Ao chamar o método <xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType>, que retorna um objeto <xref:System.Text.RegularExpressions.Match> que representa a primeira correspondência em uma cadeia de caracteres ou parte de uma cadeia. É possível determinar se a correspondência foi localizada recuperando o valor da propriedade `Match.Success`. Para recuperar objetos <xref:System.Text.RegularExpressions.Match> que representam correspondências subsequentes, chame o método <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> repetidamente, até que a propriedade `Success` do objeto <xref:System.Text.RegularExpressions.Match> retornado seja `false`.  
  
     O exemplo a seguir usa os métodos <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%29?displayProperty=nameWithType> e <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> para corresponder a cadeia de caracteres "abc" na cadeia de caracteres de entrada.  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/match3.cs#8)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/match3.vb#8)]  
  
 Duas propriedades da classe <xref:System.Text.RegularExpressions.Match> retornam objetos de coleção:  
  
- A propriedade <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> retorna um objeto <xref:System.Text.RegularExpressions.GroupCollection> que contém informações sobre as subcadeias de caracteres que correspondem grupos de captura no padrão de expressão regular.  
  
- A propriedade `Match.Captures` retorna um objeto <xref:System.Text.RegularExpressions.CaptureCollection> de uso limitado. A coleção não é preenchida para um objeto <xref:System.Text.RegularExpressions.Match> cuja propriedade `Success` é `false`. Caso contrário, ela contém um único objeto <xref:System.Text.RegularExpressions.Capture> que possui as mesmas informações do objeto <xref:System.Text.RegularExpressions.Match>.  
  
 Para obter mais informações sobre esses objetos, consulte as seções [A coleção de Grupo](#GroupCollection) e [A coleção Capture](#CaptureCollection) mais adiante neste tópico.  
  
 Duas propriedades adicionais da classe <xref:System.Text.RegularExpressions.Match> oferecem informações sobre a correspondência. A propriedade `Match.Value` retorna a subcadeia na cadeia de caracteres de entrada que corresponde ao padrão de expressão regular. A propriedade `Match.Index` retorna a posição inicial baseada em zero da cadeia correspondida na cadeia de caracteres de entrada.  
  
 A classe <xref:System.Text.RegularExpressions.Match> também tem dois métodos de correspondência de padrões:  
  
- O método <xref:System.Text.RegularExpressions.Match.NextMatch%2A?displayProperty=nameWithType> localiza a correspondência após a correspondência representada pelo objeto <xref:System.Text.RegularExpressions.Match> atual e retorna um objeto <xref:System.Text.RegularExpressions.Match> que representa essa correspondência.  
  
- O método <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> realiza uma operação de substituição especificada na cadeia de caracteres correspondida e retorna o resultado.  
  
 O exemplo a seguir usa o método <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> para inserir um símbolo de $ e um espaço antes de cada número que inclua dois dígitos fracionais.  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/result1.cs#9)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/result1.vb#9)]  
  
 O padrão de expressão regular `\b\d+(,\d{3})*\.\d{2}\b` é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`\d+`|Corresponde a um ou mais dígitos decimais.|  
|`(,\d{3})*`|Corresponder a zero ou mais ocorrências de uma vírgula seguida por três dígitos decimais.|  
|`\.`|Corresponder ao caractere de vírgula decimal.|  
|`\d{2}`|Corresponde a dois dígitos decimais.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
  
 O padrão de substituição `$$ $&` indica que a subcadeia de caracteres correspondida deve ser substituída por um símbolo de cifrão ($) (o padrão `$$`), um espaço e o valor da correspondência (o padrão `$&`).  
  
 [Voltar ao início](#introduction)  
  
<a name="GroupCollection"></a>
## <a name="the-group-collection"></a>A coleção de grupo  
 A propriedade <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> retorna um objeto <xref:System.Text.RegularExpressions.GroupCollection> que contém <xref:System.Text.RegularExpressions.Group> objetos que representam grupos capturados em uma única correspondência. O primeiro objeto <xref:System.Text.RegularExpressions.Group> na coleção (no índice 0) representa a correspondência inteira. Cada objeto que se segue representa os resultados de um único grupo de captura.  
  
 É possível recuperar objetos <xref:System.Text.RegularExpressions.Group> individuais na coleção usando a propriedade <xref:System.Text.RegularExpressions.GroupCollection.Item%2A?displayProperty=nameWithType>. É possível recuperar grupos não nomeados por sua posição ordinal na coleção e recuperar grupos nomeados por nome ou posição ordinal. Capturas não nomeadas aparecem primeiro na coleção e são indexadas da esquerda para a direita na ordem na qual aparecem no padrão de expressão regular. Capturas nomeadas são indexadas após as capturas não nomeadas, da esquerda para a direita, na ordem em que aparecem no padrão de expressão regular. Para determinar quais grupos numerados estão disponíveis na coleção retornada para um método específico de correspondência de expressão regular, você pode chamar o método da instância <xref:System.Text.RegularExpressions.Regex.GetGroupNumbers%2A?displayProperty=nameWithType>. Para determinar quais grupos nomeados estão disponíveis na coleção, você pode chamar o método da instância <xref:System.Text.RegularExpressions.Regex.GetGroupNames%2A?displayProperty=nameWithType>. Os dois métodos são especificamente úteis em rotinas gerais que analisam as correspondências encontradas por qualquer expressão regular.  
  
 A propriedade <xref:System.Text.RegularExpressions.GroupCollection.Item%2A?displayProperty=nameWithType> é o indexador da coleção no C# e a propriedade padrão do objeto da coleção no Visual Basic. Isso significa que objetos <xref:System.Text.RegularExpressions.Group> individuais podem ser acessados por índice (ou por nome, no caso de grupos nomeados) como a seguir:  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/groupsyntax1.cs#13)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/groupsyntax1.vb#13)]  
  
 O exemplo a seguir define uma expressão regular que usa constructos de agrupamento para capturar o mês, o dia e o ano de uma data.  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/groupcollection1.cs#10)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/groupcollection1.vb#10)]  
  
 O padrão de expressão regular `\b(\w+)\s(\d{1,2}),\s(\d{4})\b` é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`(\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. Este é o primeiro grupo de captura.|  
|`\s`|Corresponde a um caractere de espaço em branco.|  
|`(\d{1,2})`|Corresponder a um ou dois dígitos decimais. Este é o segundo grupo de captura.|  
|`,`|Corresponde a uma vírgula.|  
|`\s`|Corresponde a um caractere de espaço em branco.|  
|`(\d{4})`|Corresponder a quatro dígitos decimais. Este é o terceiro grupo de captura.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
  
 [Voltar ao início](#introduction)  
  
<a name="the_captured_group"></a>
## <a name="the-captured-group"></a>O grupo capturado  
 A classe <xref:System.Text.RegularExpressions.Group> representa o resultado de um único grupo de captura. Objetos de grupo que representam os grupos de captura definidos em uma expressão regular são retornados pela propriedade <xref:System.Text.RegularExpressions.GroupCollection.Item%2A> do objeto <xref:System.Text.RegularExpressions.GroupCollection> retornado pela propriedade <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType>. A propriedade <xref:System.Text.RegularExpressions.GroupCollection.Item%2A> é o indexador (no C#) e a propriedade padrão (no Visual Basic) da classe <xref:System.Text.RegularExpressions.Group>. Você também pode recuperar membros individuais iterando a coleção usando o constructo `foreach` ou `For Each`. Para ver um exemplo, consulte a seção anterior.  
  
 O exemplo a seguir usa constructos de agrupamento aninhados para capturar subcadeias de caracteres em grupos. O padrão de expressão regular `(a(b))c` corresponde à cadeia de caracteres “abc”. Ele atribui a subcadeia “ab” ao primeiro grupo de captura e a subcadeia de caracteres “b” ao segundo grupo de captura.  
  
 [!code-csharp[RegularExpressions.Classes#6](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Classes/cs/Example.cs#6)]
 [!code-vb[RegularExpressions.Classes#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Classes/vb/Example.vb#6)]  
  
 O exemplo a seguir usa constructos de agrupamento nomeados para capturar subcadeias de caracteres de uma cadeia que contém dados no formato “DATANAME:VALUE”, que a expressão regular divide usando dois pontos (:).  
  
 [!code-csharp[RegularExpressions.Classes#8](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Classes/cs/Example.cs#8)]
 [!code-vb[RegularExpressions.Classes#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Classes/vb/Example.vb#8)]  
  
 O padrão de expressão regular `^(?<name>\w+):(?<value>\w+)` é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`^`|Começar a correspondência no início da cadeia de caracteres de entrada.|  
|`(?<name>\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. O nome deste grupo de captura é `name`.|  
|`:`|Corresponder a dois pontos.|  
|`(?<value>\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. O nome deste grupo de captura é `value`.|  
  
 As propriedades da classe <xref:System.Text.RegularExpressions.Group> fornecem informações sobre o grupo capturado: a propriedade `Group.Value` contém a subcadeia de caracteres capturada, a propriedade `Group.Index` indica a posição inicial do grupo capturado no texto de entrada, a propriedade `Group.Length` contém o comprimento do texto capturado e a propriedade `Group.Success` indica se uma subcadeia de caracteres corresponde ao padrão definido pelo grupo de captura.  
  
 Aplicar quantificadores a um grupo (para obter mais informações, confira [Quantificadores](quantifiers-in-regular-expressions.md)) modifica a relação de uma captura por grupo de captura de duas formas:  
  
- Se o quantificador `*` ou `*?` (que especifica zero ou mais correspondências) for aplicado a um grupo, um grupo de captura pode não ter uma correspondência na cadeia de caracteres de entrada. Quando não há texto capturado, as propriedades do objeto <xref:System.Text.RegularExpressions.Group> são definidas como mostrado na tabela a seguir.  
  
    |Propriedade do grupo|Valor|  
    |--------------------|-----------|  
    |`Success`|`false`|  
    |`Value`|<xref:System.String.Empty?displayProperty=nameWithType>|  
    |`Length`|0|  
  
     O exemplo a seguir ilustra esse cenário. No padrão de expressão regular `aaa(bbb)*ccc`, o primeiro grupo de captura (a subcadeia de caracteres “bbb”) pode ser correspondido a zero ou mais vezes. Como a cadeia de caracteres de entrada “aaaccc” corresponde ao padrão, o grupo de captura não tem correspondência.  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/nocapture1.cs#11)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/nocapture1.vb#11)]  
  
- Os quantificadores podem corresponder a diversas ocorrências de um padrão que é definido por um grupo de captura. Nesse caso, as propriedades `Value` e `Length` de um objeto <xref:System.Text.RegularExpressions.Group> contêm informações somente sobre a última subcadeia de caracteres capturada. Por exemplo, a seguinte expressão regular corresponde a uma única sentença que termina com um ponto. Ela utiliza dois constructos de agrupamento: o primeiro captura palavras individuais, juntamente com um caractere de espaço em branco; o segundo captura palavras individuais. Como a saída do exemplo mostra, embora a expressão regular tenha sucesso ao capturar uma sequência inteira, o segundo grupo de captura só captura a última palavra.  
  
     [!code-csharp[Conceptual.RegularExpressions.ObjectModel#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/lastcapture1.cs#12)]
     [!code-vb[Conceptual.RegularExpressions.ObjectModel#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/lastcapture1.vb#12)]  
  
 [Voltar ao início](#introduction)  
  
<a name="CaptureCollection"></a>
## <a name="the-capture-collection"></a>A coleção de captura  
 O objeto <xref:System.Text.RegularExpressions.Group> contém informações somente sobre a última captura. No entanto, o conjunto inteiro de capturas realizadas por um grupo de captura ainda estará disponível do objeto <xref:System.Text.RegularExpressions.CaptureCollection> que é retornado pela propriedade <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType>. Cada membro da coleção é um objeto <xref:System.Text.RegularExpressions.Capture> que representa uma captura realizada por esse grupo de captura, na ordem na qual elas foram capturadas (e, portanto, na ordem em que as cadeias capturadas foram correspondidas da esquerda para a direita na cadeia de caracteres de entrada). Você pode recuperar objetos <xref:System.Text.RegularExpressions.Capture> individuais na coleção de duas formas:  
  
- Ao iterar pela coleção usando um constructo como `foreach` (no C#) ou `For Each` (no Visual Basic).  
  
- Ao usar a propriedade <xref:System.Text.RegularExpressions.CaptureCollection.Item%2A?displayProperty=nameWithType> para recuperar um objeto específico por índice. A propriedade <xref:System.Text.RegularExpressions.CaptureCollection.Item%2A> é a propriedade padrão (no Visual Basic) ou o indexador (no C#) do objeto <xref:System.Text.RegularExpressions.CaptureCollection>.  
  
 Se um quantificador não for aplicado a um grupo de captura, o objeto <xref:System.Text.RegularExpressions.CaptureCollection> conterá um único objeto <xref:System.Text.RegularExpressions.Capture> de pouco interesse, pois ele fornece informações sobre a mesma correspondência que seu objeto <xref:System.Text.RegularExpressions.Group>. Se um quantificador for aplicado a um grupo de captura, o objeto <xref:System.Text.RegularExpressions.CaptureCollection> conterá todas as capturas realizadas pelo grupo de captura, e o último membro da coleção representará a mesma captura do objeto <xref:System.Text.RegularExpressions.Group>.  
  
 Por exemplo, se você usar o padrão de expressão regular `((a(b))c)+` (em que o quantificador + especifica uma ou mais correspondências) para capturar correspondências da cadeia de caracteres "abcabcabc", o objeto <xref:System.Text.RegularExpressions.CaptureCollection> para cada objeto <xref:System.Text.RegularExpressions.Group> conterá três membros.  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/capturecollection1.cs#14)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/capturecollection1.vb#14)]  
  
 O exemplo a seguir usa a expressão regular `(Abc)+` para localizar uma ou mais execuções consecutivas da cadeia de caracteres “Abc” na cadeia “XYZAbcAbcAbcXYZAbcAb”. O exemplo ilustra o uso da propriedade <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> para retornar vários grupos de subcadeias de caracteres capturadas.  
  
 [!code-csharp[RegularExpressions.Classes#5](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Classes/cs/Example.cs#5)]
 [!code-vb[RegularExpressions.Classes#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Classes/vb/Example.vb#5)]  
  
 [Voltar ao início](#introduction)  
  
<a name="the_individual_capture"></a>
## <a name="the-individual-capture"></a>A captura individual  
 A classe <xref:System.Text.RegularExpressions.Capture> contém os resultados de uma única captura de subexpressão. A propriedade <xref:System.Text.RegularExpressions.Capture.Value%2A?displayProperty=nameWithType> contém o texto correspondido, e a propriedade <xref:System.Text.RegularExpressions.Capture.Index%2A?displayProperty=nameWithType> indica a posição baseada em zero na cadeia de caracteres de entrada na qual a subcadeia de caracteres correspondida começa.  
  
 O exemplo a seguir analisa uma cadeia de caracteres de entrada para a temperatura das cidades escolhidas. Uma vírgula (“,”) é usada para separar uma cidade e sua temperatura, enquanto um ponto e vírgula (“;”) é usado para separar os dados de cada cidade. A cadeia de caracteres de entrada inteira representa uma única correspondência. No padrão de expressão regular `((\w+(\s\w+)*),(\d+);)+`, usado para analisar a cadeia de caracteres, o nome da cidade é atribuído ao segundo grupo de captura, enquanto a temperatura é atribuída ao quarto grupo de captura.  
  
 [!code-csharp[Conceptual.RegularExpressions.ObjectModel#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/cs/capture1.cs#16)]
 [!code-vb[Conceptual.RegularExpressions.ObjectModel#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.objectmodel/vb/capture1.vb#16)]  
  
 A expressão regular é definida como mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\w+`|Fazer a correspondência a um ou mais caracteres de palavra.|  
|`(\s\w+)*`|Corresponder a zero ou mais ocorrências de um caractere de espaço em branco seguido por um ou mais caracteres de palavra. Este padrão corresponde a nomes de cidade com várias palavras. Este é o terceiro grupo de captura.|  
|`(\w+(\s\w+)*)`|Corresponder a um ou mais caracteres de palavra seguido por zero ou mais ocorrências de um caractere de espaço em branco e um ou mais caracteres de palavra. Este é o segundo grupo de captura.|  
|`,`|Corresponde a uma vírgula.|  
|`(\d+)`|Corresponder a um ou mais dígitos. Este é o quarto grupo de captura.|  
|`;`|Corresponder a um ponto e vírgula.|  
|`((\w+(\s\w+)*),(\d+);)+`|Corresponder ao padrão de uma palavra seguida por qualquer palavra adicional seguida por uma vírgula, um ou mais dígitos e um ponto e vírgula, uma ou mais vezes. Este é o primeiro grupo de captura.|  
  
## <a name="see-also"></a>Veja também

- <xref:System.Text.RegularExpressions>
- [Expressões regulares do .NET](regular-expressions.md)
- [Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)
