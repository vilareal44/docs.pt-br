---
title: Opções de expressões regulares
description: Saiba como usar as opções de expressão regular no .NET, como correspondência que não diferencia maiúsculas de minúsculas, modo multilinha e modo da direita para a esquerda.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, options
- constructs, options
- .NET Framework regular expressions, options
- inline option constructs
- options parameter
ms.assetid: c82dc689-7e82-4767-a18d-cd24ce5f05e9
ms.openlocfilehash: 268e05c2212539b030ccc3c7195f618bb3afa707
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662869"
---
# <a name="regular-expression-options"></a>Opções de expressões regulares

Por padrão, a comparação de uma cadeia de caracteres de entrada com quaisquer caracteres literais em um padrão de expressão regular diferencia maiúsculas e minúsculas; o espaço em branco em um padrão de expressão regular é interpretado como caracteres de espaço em branco literais e os grupos de captura em uma expressão regular são nomeados implícita e explicitamente. É possível modificar esses e vários outros aspectos do comportamento de expressão regular especificando opções de expressões regulares. Essas opções, que estão listadas na tabela a seguir, podem ser incluídas embutidas como parte do padrão de expressão regular, ou podem ser fornecidas a um construtor de classe <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> ou método de correspondência padrão estático como um valor de enumeração <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType>.

|Membro de RegexOptions|Caractere embutido|Efeito|
|-------------------------|----------------------|------------|
|<xref:System.Text.RegularExpressions.RegexOptions.None>|Não disponível|Use o comportamento padrão. Para obter mais informações, consulte [opções padrão](#default-options).|
|<xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase>|`i`|Use correspondência sem diferenciação de maiúsculas e minúsculas. Para obter mais informações, consulte correspondência que não [diferencia maiúsculas de minúsculas](#case-insensitive-matching).|
|<xref:System.Text.RegularExpressions.RegexOptions.Multiline>|`m`|Use o modo multilinha, em que `^` e `$` correspondem com o início e o fim de cada linha (em vez do início e o fim da cadeia de caracteres de entrada). Para obter mais informações, consulte [modo multilinha](#multiline-mode).|
|<xref:System.Text.RegularExpressions.RegexOptions.Singleline>|`s`|Use o modo de linha única, em que o ponto (.) corresponde com todos os caracteres (em vez de todos os caracteres, exceto `\n`). Para obter mais informações, consulte [modo de linha única](#single-line-mode).|
|<xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture>|`n`|Não capture grupos sem nome. As únicas capturas válidas são grupos explicitamente nomeados ou numerados da `(?<` *name* `>` *subexpressão*Name do formulário `)` . Para obter mais informações, consulte [somente capturas explícitas](#explicit-captures-only).|
|<xref:System.Text.RegularExpressions.RegexOptions.Compiled>|Não disponível|Compile a expressão regular para um assembly. Para obter mais informações, consulte [expressões regulares compiladas](#compiled-regular-expressions).|
|<xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace>|`x`|Exclua um espaço em branco sem escape do padrão e habilite comentários após uma tecla de cerquilha (`#`). Para obter mais informações, confira [Ignorar espaço em branco](#ignore-white-space).|
|<xref:System.Text.RegularExpressions.RegexOptions.RightToLeft>|Não disponível|Altera a direção da pesquisa. A pesquisa se move da direita para a esquerda, em vez de da esquerda para a direita. Para obter mais informações, consulte [modo da direita para a esquerda](#right-to-left-mode).|
|<xref:System.Text.RegularExpressions.RegexOptions.ECMAScript>|Não disponível|Habilite o comportamento compatível com ECMAScript para a expressão. Para obter mais informações, consulte [comportamento de correspondência ECMAScript](#ecmascript-matching-behavior).|
|<xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant>|Não disponível|Ignorar diferenças culturais no idioma. Para obter mais informações, consulte [comparação usando a cultura invariável](#comparison-using-the-invariant-culture).|

## <a name="specifying-the-options"></a>Especificando as opções

É possível especificar opções para expressões regulares de uma destas três maneiras:

- No parâmetro `options` de um construtor de classe <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> ou método de correspondência padrão (`Shared` no Visual Basic) estático, como <xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%29> ou <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType>. O parâmetro `options` é uma combinação OR bit a bit de valores enumerados <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType>.

  Quando as opções são fornecidas a uma instância <xref:System.Text.RegularExpressions.Regex> mediante uso do parâmetro `options` de um construtor de classe, elas são atribuídas à propriedade <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType>. No entanto, a propriedade <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> não reflete opções embutidas no próprio padrão de expressão regular.

  O exemplo a seguir ilustra esse cenário. Ele usa o parâmetro `options` do método <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> para habilitar correspondência sem diferenciação entre maiúsculas e minúsculas e ignorar espaço em branco do parâmetro ao identificar palavras que começam com a letra "d".

  [!code-csharp[Conceptual.Regex.Language.Options#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#6)]
  [!code-vb[Conceptual.Regex.Language.Options#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#6)]

- Aplicando opções embutidas em um padrão de expressão regular com a sintaxe `(?imnsx-imnsx)`. A opção se aplica ao padrão do ponto em que a opção é definida até o fim do padrão ou o ponto em que a opção tem é indefinida por outra opção embutida. Observe que a propriedade <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> de uma instância <xref:System.Text.RegularExpressions.Regex> não reflete essas opções embutidas. Para saber mais, confira o tópico [Constructos diversos](miscellaneous-constructs-in-regular-expressions.md).

  O exemplo a seguir ilustra esse cenário. Ele usa opções embutidas para habilitar a correspondência sem diferenciação entre maiúsculas e minúsculas e ignorar o espaço em branco do padrão ao identificar palavras que começam com a letra “d”.

  [!code-csharp[Conceptual.Regex.Language.Options#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#7)]
  [!code-vb[Conceptual.Regex.Language.Options#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#7)]

- Aplicando opções embutidas em uma determinada construção de agrupamento em um padrão de expressão regular com a `(?imnsx-imnsx:` *subexpressão*Syntax `)` . Nenhum sinal antes de um conjunto de opções ativa o conjunto; um sinal de subtração antes de um conjunto de opções desativa o conjunto. ( `?` é uma parte fixa da sintaxe da construção de linguagem que é necessária se as opções estão habilitadas ou desabilitadas.) A opção se aplica somente a esse grupo. Para saber mais, confira [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).

  O exemplo a seguir ilustra esse cenário. Ele usa opções embutidas em um constructo de agrupamento para habilitar a correspondência sem diferenciação entre maiúsculas e minúsculas e ignorar espaço em branco do padrão ao identificar palavras que começam com a letra “d”.

  [!code-csharp[Conceptual.Regex.Language.Options#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#8)]
  [!code-vb[Conceptual.Regex.Language.Options#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#8)]

Se as opções forem embutidas especificadas, um sinal de menos (`-`) antes de uma opção ou conjunto de opções desativa essas opções. Por exemplo, a construção embutida `(?ix-ms)` ativa as opções <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> e <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> e desativa as opções <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> e <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType>. Todas as opções de expressões regulares são desativadas por padrão.

> [!NOTE]
> Se as opções de expressão regular especificadas no parâmetro `options` de uma chamada de construtor ou método entrar em conflito com as opções especificadas embutidas em um padrão de expressão regular, serão usadas as opções embutidas.

As cinco opções de expressão regular a seguir podem ser definidas com parâmetro de opções e embutidas:

- <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType>

As cinco opções de expressão regular a seguir podem ser definidas usando o parâmetro `options`, mas não podem ser definidas embutidas:

- <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType>

## <a name="determining-the-options"></a>Determinando as opções

É possível determinar que opções foram fornecidas a um objeto <xref:System.Text.RegularExpressions.Regex> quando ele tiver sido instanciado recuperando o valor da propriedade <xref:System.Text.RegularExpressions.Regex.Options%2A?displayProperty=nameWithType> somente leitura. Essa propriedade é particularmente útil para determinar as opções definidas para uma expressão regular compilada criada pelo método <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType>.

Para testar a presença de qualquer opção, exceto <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType>, realize uma operação AND com o valor da propriedade <xref:System.Text.RegularExpressions.Regex.Options%2A?displayProperty=nameWithType> e o valor <xref:System.Text.RegularExpressions.RegexOptions> no qual você está interessado. Em seguida, teste se o resultado é igual ao valor de <xref:System.Text.RegularExpressions.RegexOptions>. O exemplo a seguir testa se a opção <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> foi definida.

[!code-csharp[Conceptual.Regex.Language.Options#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/determine1.cs#19)]
[!code-vb[Conceptual.Regex.Language.Options#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/determine1.vb#19)]

Para testar <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType>, determine se o valor da propriedade <xref:System.Text.RegularExpressions.Regex.Options%2A?displayProperty=nameWithType> é igual a <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType>, como ilustra o exemplo a seguir.

[!code-csharp[Conceptual.Regex.Language.Options#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/determine1.cs#20)]
[!code-vb[Conceptual.Regex.Language.Options#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/determine1.vb#20)]

As seções a seguir listam as opções com suporte na expressão regular no .NET.

## <a name="default-options"></a>Opções padrão

A opção <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> indica que nenhuma opção foi especificada, e o mecanismo de expressão regular usa seu comportamento padrão. Isso inclui o seguinte:

- O padrão é interpretado como canônico e não como uma expressão regular ECMAScript.

- O padrão da expressão regular é combinado na cadeia de caracteres de entrada da esquerda para a direita.

- As comparações diferenciam maiúsculas de minúsculas.

- Os elementos de linguagem `^` e `$` correspondem com o início e o fim da cadeia de caracteres de entrada.

- O elemento de linguagem `.` corresponde com todos os caracteres, exceto `\n`.

- Qualquer espaço em branco em um padrão de expressão regular é interpretado como caractere de espaço literal.

- As convenções da cultura atual são usadas ao comparar o padrão com a cadeia de caracteres de entrada.

- Os grupos de capturas no padrão de expressão regular são implícitos e explícitos.

> [!NOTE]
> A opção <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> não tem equivalente embutido. Quando as opções de expressões regulares são embutidas aplicadas, o comportamento padrão é restaurado de modo opção a opção, desativando uma opção em particular. Por exemplo, `(?i)` ativa a comparação sem diferenciar maiúsculas de minúsculas e `(?-i)` restaura a comparação que diferencia maiúsculas de minúsculas.

Como a opção <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> representa o comportamento padrão do mecanismo de expressão regular, raramente, ela é explicitamente especificada em uma chamada de método. Em vez disso, é chamado um método de construtor ou de correspondência padrão estático sem um parâmetro `options`.

## <a name="case-insensitive-matching"></a>Correspondência sem diferenciação entre maiúsculas e minúsculas

A opção <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase> ou a opção embutida `i` fornece correspondência sem diferenciação entre maiúsculas e minúsculas. Por padrão, são usadas as convenções de diferenciação entre maiúsculas e minúsculas da cultura atual.

O exemplo a seguir define um padrão de expressão regular, `\bthe\w*\b`, que corresponde a todas as palavras que começam com “the”. Como a primeira chamada para o método <xref:System.Text.RegularExpressions.Regex.Match%2A> usa a comparação que diferencia maiúsculas de minúsculas padrão, a saída indica que a cadeia de caracteres "The", que inicia a frase, não é combinada. Ela é combinada quando o método <xref:System.Text.RegularExpressions.Regex.Match%2A> é chamado com opções definidas para <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase>.

[!code-csharp[Conceptual.Regex.Language.Options#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/case1.cs#1)]
[!code-vb[Conceptual.Regex.Language.Options#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/case1.vb#1)]

O exemplo a seguir modifica o padrão da expressão regular do exemplo anterior para usar opções embutidas, em vez do parâmetro `options` para fornecer comparação de diferenciação entre maiúsculas e minúsculas. O primeiro padrão define a opção que não diferencia maiúsculas de minúsculas em um constructo de agrupamento que se aplica apenas à letra “t” na cadeia de caracteres “the”. Como o constructo da opção ocorre no início do padrão, o segundo padrão aplica a opção que não diferencia maiúsculas de minúsculas a toda a expressão regular.

[!code-csharp[Conceptual.Regex.Language.Options#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/case2.cs#2)]
[!code-vb[Conceptual.Regex.Language.Options#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/case2.vb#2)]

## <a name="multiline-mode"></a>Modo multilinha

A opção <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> ou a opção embutida `m` habilita o mecanismo de expressão regular para processar uma cadeia de caracteres de entrada que consiste em várias linhas. Ele altera a interpretação dos elementos de linguagem `^` e `$` para que correspondem com o início e o fim de uma linha, em vez do início e fim da cadeia de caracteres de entrada.

Por padrão, `$` corresponde apenas com o final da cadeia de caracteres de entrada. Se você especificar a opção <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>, ela corresponde com o caractere newline (`\n`) ou com o fim da cadeia de caracteres de entrada. Porém, não corresponde à combinação de caractere de retorno de carro/avanço de linha. Para uma correspondência bem-sucedida, use a subexpressão `\r?$` em vez de apenas `$`.

O exemplo a seguir extrai nomes e pontuações de jogadores de boliche e os adiciona a uma coleção <xref:System.Collections.Generic.SortedList%602>, que os classifica em ordem decrescente. O método <xref:System.Text.RegularExpressions.Regex.Matches%2A> é chamado duas vezes. Na primeira chamada do método, a expressão regular é `^(\w+)\s(\d+)$` e nenhuma opção é definida. Como a saída mostra, uma vez que o mecanismo de expressões regulares não pode corresponder ao padrão de entrada junto com o início e o fim da cadeia de caracteres de entrada, nenhuma correspondência é encontrada. Na segunda chamada do método, a expressão regular é alterada para `^(\w+)\s(\d+)\r?$` e as opções são definidas para <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>. Como a saída mostra, os nomes e pontuações são combinados com sucesso e as pontuações são exibidas em ordem decrescente.

[!code-csharp[Conceptual.Regex.Language.Options#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/multiline1.cs#3)]
[!code-vb[Conceptual.Regex.Language.Options#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/multiline1.vb#3)]

O padrão de expressão regular `^(\w+)\s(\d+)\r*$` é definido conforme mostrado na tabela a seguir.

|Padrão|Descrição|
|-------------|-----------------|
|`^`|Começar no início da linha.|
|`(\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. Este é o primeiro grupo de captura.|
|`\s`|Corresponde a um caractere de espaço em branco.|
|`(\d+)`|Corresponde a um ou mais dígitos decimais. Este é o segundo grupo de captura.|
|`\r?`|Corresponder a zero ou um caractere de retorno de carro.|
|`$`|Terminar no fim da linha.|

O exemplo a seguir é equivalente ao anterior, exceto que ele usa a opção embutida `(?m)` para definir a opção de multilinhas.

[!code-csharp[Conceptual.Regex.Language.Options#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/multiline2.cs#4)]
[!code-vb[Conceptual.Regex.Language.Options#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/multiline2.vb#4)]

## <a name="single-line-mode"></a>Modo de linha única

A opção <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType>, ou a opção embutida `s`, faz o mecanismo de expressão regular tratar a cadeia de caracteres de entrada como se consistisse em uma única linha. Ele faz isso mudando o comportamento do elemento de linguagem de ponto (`.`) para que corresponda a todos os caracteres, em vez de corresponder a todo caractere exceto pelo newline `\n` ou \u000A.

O exemplo a seguir ilustra como o comportamento do elemento de linguagem `.` muda quando se usa a opção <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType>. A expressão regular `^.+` começa no início da cadeia de caracteres e corresponde a todos os caracteres. Por padrão, a correspondência termina no final da primeira linha; o padrão de expressão regular corresponde ao caractere de retorno de carro, `\r` ou \ u000D, mas não corresponde a `\n`. Como a opção <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> interpreta toda a cadeia de caracteres de entrada como uma única linha, ela corresponde a cada caractere na cadeia de caracteres de entrada, incluindo `\n`.

[!code-csharp[Conceptual.Regex.Language.CharacterClasses#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/any2.cs#5)]
[!code-vb[Conceptual.Regex.Language.CharacterClasses#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/any2.vb#5)]

O exemplo a seguir é equivalente ao anterior, exceto que ele usa a opção embutida `(?s)` para habilitar o modo de linha única.

[!code-csharp[Conceptual.Regex.Language.Options#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/singleline1.cs#5)]
[!code-vb[Conceptual.Regex.Language.Options#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/singleline1.vb#5)]

## <a name="explicit-captures-only"></a>Apenas capturas explícitas

Por padrão, grupos de capturas são definidos pelo uso de parênteses no padrão de expressão regular. Os grupos nomeados recebem um nome ou número pela `(?<` *name* `>` opção de linguagem de*subexpressão* de nome `)` , enquanto os grupos não nomeados são acessíveis por índice. No objeto <xref:System.Text.RegularExpressions.GroupCollection>, grupos não nomeados precedem grupos nomeados.

Constructos de agrupamento costumam ser usados apenas para aplicar quantificadores a vários elementos de linguagem; as subcadeias de caracteres capturadas não são de interesse. Por exemplo, se a seguinte expressão regular:

`\b\(?((\w+),?\s?)+[\.!?]\)?`

for feita somente para extrair frases que terminem com um ponto, ponto de exclamação ou ponto de interrogação de um documento, apenas a frase resultante (representada pelo objeto <xref:System.Text.RegularExpressions.Match>) é de interesse. As palavras individuais na coleção não são.

Capturar grupos que não serão usados posteriormente pode ser caro, pois o mecanismo de expressão regular precisa preencher os objetos de coleção <xref:System.Text.RegularExpressions.GroupCollection> e <xref:System.Text.RegularExpressions.CaptureCollection>. Como alternativa, você pode usar a opção <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> ou a opção embutida `n` para especificar que apenas as capturas válidas são explicitamente nomeadas ou grupos numerados que são projetados pelo constructo `(?<`*nome*`>` *subexpressão*`)`.

O exemplo a seguir exibe informações sobre as correspondências retornadas pelo padrão de expressão regular `\b\(?((\w+),?\s?)+[\.!?]\)?` quando o método <xref:System.Text.RegularExpressions.Regex.Match%2A> é chamado com e sem a opção <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType>. Como mostra a saída da chamada do primeiro método, o mecanismo de expressão regular preenche totalmente os objetos da coleção <xref:System.Text.RegularExpressions.GroupCollection> e <xref:System.Text.RegularExpressions.CaptureCollection> com informações sobre subcadeias de caracteres capturadas. Como o segundo método é chamado com `options` definido para <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType>, ele não captura informações sobre grupos.

[!code-csharp[Conceptual.Regex.Language.Options#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/explicit1.cs#9)]
[!code-vb[Conceptual.Regex.Language.Options#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/explicit1.vb#9)]

O padrão de expressão regular`\b\(?((?>\w+),?\s?)+[\.!?]\)?` é definido como mostra a tabela a seguir.

|Padrão|Descrição|
|-------------|-----------------|
|`\b`|Começar em um limite de palavra.|
|`\(?`|Corresponder zero ou uma ocorrência do parêntese de abertura (“(“).|
|`(?>\w+),?`|Corresponder um ou mais caracteres de palavra seguidos por zero ou uma vírgula. Não retroceda ao corresponder caracteres de palavra.|
|`\s?`|Corresponder a zero ou a um caractere de espaço em branco.|
|`((\w+),?\s?)+`|Corresponder a combinação de um ou mais caracteres de palavra, zero ou mais vírgulas e zero ou um caractere de espaço em branco uma ou mais vezes.|
|`[\.!?]\)?`|Corresponder qualquer um dos três símbolos de pontuação seguidos por zero ou um parêntese de fechamento (“)”).|

Você também pode usar o elemento embutido `(?n)` para suprimir capturas automáticas. O exemplo a seguir modifica o padrão de expressão regular anterior para usar o elemento embutido `(?n)` em vez da opção <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType>.

[!code-csharp[Conceptual.Regex.Language.Options#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/explicit2.cs#10)]
[!code-vb[Conceptual.Regex.Language.Options#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/explicit2.vb#10)]

Por fim, é possível usar o elemento do grupo embutido `(?n:)` para suprimir capturas automáticas grupo a grupo. O exemplo a seguir modifica o padrão anterior para suprimir capturas sem nome no grupo externo, `((?>\w+),?\s?)`. Observe que isso também suprime capturas sem nome no grupo interno.

[!code-csharp[Conceptual.Regex.Language.Options#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/explicit3.cs#11)]
[!code-vb[Conceptual.Regex.Language.Options#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/explicit3.vb#11)]

## <a name="compiled-regular-expressions"></a>Expressões regulares compiladas

Por padrão, as expressões regulares no .NET são interpretadas. Quando um objeto <xref:System.Text.RegularExpressions.Regex> é instanciado ou um método <xref:System.Text.RegularExpressions.Regex> estático é chamado, o padrão de expressão regular é analisado em um conjunto de opcodes personalizados, e um interpretador usa esses opcodes para executar a expressão regular. Isso envolve uma troca: o custo de inicializar o mecanismo de expressões regulares é minimizado com prejuízo do desempenho do tempo de execução.

Você pode usar expressões regulares compiladas, em vez de interpretadas, usando a opção <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType>. Neste caso, quando um padrão é enviado ao mecanismo de expressões regulares, ele é analisado em um subconjunto de opcodes e convertido para a MSIL (linguagem intermediária da Microsoft), que pode ser enviada diretamente ao Common Language Runtime. Expressões regulares compiladas maximizam o desempenho do tempo de execução às custas do tempo de inicialização.

> [!NOTE]
> Uma expressão regular só pode ser compilada fornecendo o valor <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> ao parâmetro `options` de um construtor de classe <xref:System.Text.RegularExpressions.Regex> ou um método de correspondência padrão estático. Não está disponível como uma opção embutida.

É possível usar expressões regulares compiladas em chamadas para expressões regulares estáticas e de instância. Em expressões regulares estáticas, a opção <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> é enviada ao parâmetro `options` do método de correspondência padrão de expressão regular. Em expressões regulares de instância, é enviado ao parâmetro `options` do construtor de classe <xref:System.Text.RegularExpressions.Regex>. Em ambos os casos, resulta em melhoria de desempenho.

Porém, essa melhoria de desempenho ocorre apenas sob as seguintes condições:

- É usado um objeto <xref:System.Text.RegularExpressions.Regex> que representa uma expressão regular em particular, em várias chamadas para métodos de correspondência padrão de expressão regular.

- O objeto <xref:System.Text.RegularExpressions.Regex> não pode sair do escopo, então pode ser reutilizado.

- Uma expressão regular estática é usada em várias chamadas para métodos de correspondência padrão de expressão regular. (A melhoria de desempenho é possível porque as expressões regulares usadas em chamadas de método estático são armazenadas em cache pelo mecanismo de expressões regulares.)

> [!NOTE]
> A opção <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> não está relacionada ao método <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType>, que cria um assembly de uso especial contendo expressões regulares compiladas predefinidas.

## <a name="ignore-white-space"></a>Ignorar espaço em branco

Por padrão, o espaço em branco em um padrão de expressão regular é significativo; ele força o mecanismo de expressões regulares para combinar um caractere de espaço em branco na cadeia de caracteres de entrada. Devido a isso, a expressão regular "`\b\w+\s`" e "`\b\w+`" são, de um modo geral, equivalentes. Além disso, quando a tecla de cerquilha (#) é encontrada em um padrão de expressão regular, ela é interpretada como um caractere literal a ser combinado.

A opção <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> ou a opção embutida `x` muda seu comportamento padrão da seguinte maneira:

- É ignorado o espaço em branco sem escape no padrão de expressão regular. Para fazer parte de um padrão de expressão regular, os caracteres de espaço em branco devem ser escapados (por exemplo, `\s` ou "`\`").

- A tecla de cerquilha (#) é interpretada como o início de um comentário, em vez de um caractere literal. Todo o texto no padrão de expressão regular do caractere # até o fim da cadeia de caracteres é interpretado como comentário.

No entanto, nos casos a seguir, os caracteres de espaço em branco em uma expressão regular não serão ignorados, mesmo se você usar a opção <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType>:

- O espaço em branco dentro de uma classe de caractere sempre é interpretado literalmente. Por exemplo, o padrão da expressão regular `[ .,;:]` corresponde a qualquer caractere de espaço em branco, ponto, vírgula, dois pontos ou ponto e vírgula único.

- O espaço em branco não é permitido dentro de um quantificador entre colchetes, como `{` *n* `}` , `{` *n* `,}` e `{` *n* `,` *m* `}` . Por exemplo, o padrão de expressão regular `\d{1, 3}` falha ao corresponder quaisquer sequências de dígitos de um a três dígitos porque contém um caractere de espaço em branco.

- Não é permitido espaço em branco dentro da sequência de caracteres que introduz um elemento de linguagem. Por exemplo:

  - A subexpressão de elemento de linguagem `(?:` *subexpression* `)` representa um grupo de não captura e a `(?:` parte do elemento não pode ter espaços inseridos. A `(? :` *subexpressão* Pattern `)` lança um <xref:System.ArgumentException> em tempo de execução porque o mecanismo de expressão regular não pode analisar o padrão e a `( ?:` *subexpressão* Pattern `)` não corresponde à *subexpressão*.

  - O nome do elemento de linguagem `\p{` *name* `}` , que representa uma categoria Unicode ou um bloco nomeado, não pode incluir espaços incorporados na `\p{` parte do elemento. Se você incluir um espaço em branco, o elemento lança uma <xref:System.ArgumentException> no tempo de execução.

Habilitar essa opção ajuda a simplificar expressões regulares que costumam ser difíceis de analisar e entender. Melhora a legibilidade e torna possível documentar uma expressão regular.

O exemplo a seguir define o padrão de expressão regular a seguir:

`\b \(? ( (?>\w+) ,?\s? )+  [\.!?] \)? # Matches an entire sentence.`

Esse padrão é similar ao padrão definido na seção [Apenas capturas explícitas](#explicit-captures-only), exceto por usar a opção <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> para ignorar espaço em branco de padrão.

[!code-csharp[Conceptual.Regex.Language.Options#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/whitespace1.cs#12)]
[!code-vb[Conceptual.Regex.Language.Options#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/whitespace1.vb#12)]

O exemplo a seguir usa a opção embutida `(?x)` para ignorar o espaço em branco padrão.

[!code-csharp[Conceptual.Regex.Language.Options#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/whitespace2.cs#13)]
[!code-vb[Conceptual.Regex.Language.Options#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/whitespace2.vb#13)]

## <a name="right-to-left-mode"></a>Modo da direita para a esquerda

Por padrão, o mecanismo de expressões regulares pesquisa da esquerda para a direita. É possível reverter a direção de pesquisa usando a opção <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType>. A pesquisa inicia automaticamente na última posição de caractere da cadeia de caracteres. Para métodos de correspondência padrão que incluem um parâmetro de posição inicial, como <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.Int32%29?displayProperty=nameWithType>, a posição inicial é o índice da posição do caractere mais à direita em que a pesquisa deve iniciar.

> [!NOTE]
> O modo de padrão da direita para a esquerda está disponível apenas fornecendo o valor <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> para o parâmetro `options` de um construtor de classe <xref:System.Text.RegularExpressions.Regex> ou o método de correspondência padrão estático. Não está disponível como uma opção embutida.

A opção <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> só muda a direção de pesquisa, ela não interpreta o padrão de expressão regular da direita para a esquerda. Por exemplo, a expressão regular `\bb\w+\s` corresponde palavras que começam com a letra “b” e são seguidas por um caractere de espaço em branco. No exemplo a seguir, a cadeia de caracteres de entrada consiste em três palavras que incluem um ou mais caracteres “b”. A primeira palavra começa com “b”, a segunda termina com “b” e a terceira inclui dois caracteres “b” no meio da palavra. Como a saída do exemplo mostra, apenas a primeira palavra corresponde ao padrão da expressão regular.

[!code-csharp[Conceptual.Regex.Language.Options#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/righttoleft1.cs#17)]
[!code-vb[Conceptual.Regex.Language.Options#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/righttoleft1.vb#17)]

Observe também que a asserção de antecipação (o elemento de linguagem de `(?=` *subexpressão* `)` ) e a asserção lookbehind (o `(?<=` elemento de linguagem *subexpressão* `)` ) não alteram a direção. As asserções lookahead buscam à direita; as asserções lookbehind buscam à esquerda. Por exemplo, a expressão regular `(?<=\d{1,2}\s)\w+,?\s\d{4}` usa a asserção lookbehind para testar uma data que antecede um nome de mês. A expressão regular corresponde ao mês e o ano. Saiba mais sobre as asserções lookahead e lookbehind em [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).

[!code-csharp[Conceptual.Regex.Language.Options#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/righttoleft2.cs#18)]
[!code-vb[Conceptual.Regex.Language.Options#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/righttoleft2.vb#18)]

O padrão de expressão regular é definido como mostra a tabela a seguir.

|Padrão|Descrição|
|-------------|-----------------|
|`(?<=\d{1,2}\s)`|O início da correspondência deve ser antecedido por um ou dois dígitos decimais seguidos por um espaço.|
|`\w+`|Fazer a correspondência a um ou mais caracteres de palavra.|
|`,?`|Corresponder a zero ou um caractere de vírgula.|
|`\s`|Corresponde a um caractere de espaço em branco.|
|`\d{4}`|Corresponder a quatro dígitos decimais.|

## <a name="ecmascript-matching-behavior"></a>Comportamento de correspondência de ECMAScript

Por padrão, o mecanismo de expressões regulares usa comportamento canônico ao corresponder um padrão de expressão regular a um texto de entrada. Porém, é possível instruir o mecanismo de expressão regular a usar o comportamento de correspondência ECMAScript especificando a opção <xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType>.

> [!NOTE]
> O comportamento compatível com ECMAScript está disponível apenas fornecendo o valor <xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType> para o parâmetro `options` de um construtor de classe <xref:System.Text.RegularExpressions.Regex> ou método de correspondência padrão estático. Não está disponível como uma opção embutida.

A opção <xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType> só pode ser combinada com as opções <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> e <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>. O uso de qualquer outra opção em uma expressão regular resulta em <xref:System.ArgumentOutOfRangeException>.

O comportamento das expressões regulares ECMAScript e canônicas difere em três áreas: sintaxe da classe de caractere, grupos de captura autorreferidos e interpretação octal versus de referência inversa.

- Sintaxe da classe de caractere. Como as expressões regulares canônicas dão suporte a Unicode e o ECMAScrip não, as classes de caractere no ECMAScrip têm uma sintaxe mais limitada e alguns elementos de linguagem da classe de caractere têm um significado diferente. Por exemplo, o ECMAScript não oferece suporte a elementos de linguagem como a categoria Unicode ou elementos de bloco `\p` e `\P`. De modo similar, o elemento `\w`, que corresponde um caractere de palavra, é equivalente à classe de caractere `[a-zA-Z_0-9]` ao usar ECMAScript e `[\p{Ll}\p{Lu}\p{Lt}\p{Lo}\p{Nd}\p{Pc}\p{Lm}]` ao usar comportamento canônico. Para saber mais, confira [Classes de caracteres](character-classes-in-regular-expressions.md).

  O exemplo a seguir ilustra a diferença entre correspondência padrão ECMAScript e canônica. Define uma expressão regular, `\b(\w+\s*)+`, que combina palavras seguidas por caracteres de espaço em branco. A entrada consiste em duas cadeias de caracteres, uma que usa o conjunto de caracteres latinos e outra que usa o conjunto de caracteres cirílicos. Como a saída mostra, a chamada para o método <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> que usa correspondência ECMAScript falha ao combinar as palavras cirílicas, enquanto a chamada de método que usa correspondência canônica não corresponde essas palavras.

  [!code-csharp[Conceptual.Regex.Language.Options#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/ecmascript1.cs#16)]
  [!code-vb[Conceptual.Regex.Language.Options#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/ecmascript1.vb#16)]

- Grupos de capturas com autorreferência. Uma classe de captura de expressão regular com uma referência inversa para si mesma deve ser atualizada com cada iteração de captura. Como o exemplo a seguir mostra, esse recurso habilita a expressão regular `((a+)(\1) ?)+` para corresponder a cadeia de caracteres de entrada “ aa aaaa aaaaaa ” ao usar ECMAScript, mas não ao usar correspondência canônica.

  [!code-csharp[Conceptual.Regex.Language.Options#21](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/ecmascript2.cs#21)]
  [!code-vb[Conceptual.Regex.Language.Options#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/ecmascript2.vb#21)]

  A expressão regular é definida como mostrado na tabela a seguir.

  |Padrão|Descrição|
  |-------------|-----------------|
  |(a+)|Corresponda a letra "a" uma ou mais vezes. Este é o segundo grupo de captura.|
  |(\1)|Corresponda a subcadeia de caracteres capturada pelo primeiro grupo de captura. Este é o terceiro grupo de captura.|
  |?|Corresponda zero ou um caractere de espaço.|
  |((a+)(\1) ?)+|Corresponda o padrão de um ou mais caracteres "a" seguidos por uma cadeia de caracteres que corresponda o primeiro grupo de capturas seguido por zero ou um caractere de espaço uma ou mais vezes. Este é o primeiro grupo de captura.|

- Resolução de ambiguidades entre escapes octais e referências inversas. A tabela a seguir resume as diferenças na interpretação octal versus de referência inversa por expressões regulares canônicas e ECMAScript.

  |Expressão regular|Comportamento canônico|Comportamento de ECMAScript|
  |------------------------|------------------------|-------------------------|
  |`\0` seguido por 0 - 2 dígitos octais|Interprete como um octal. Por exemplo, `\044` é sempre interpretado como um valor octal e significa "$".|Mesmo comportamento.|
  |`\` seguido por um dígito de 1 a 9, seguido por nenhum dígito decimal adicional,|Interprete como referência inversa. Por exemplo, `\9` sempre significa referência inversa 9, mesmo que um nono grupo de capturas não exista. Se o grupo de captura não existir, o analisador de expressão regular lança uma <xref:System.ArgumentException>.|Se existir um grupo de capturas de um único dígito decimal, faça referência inversa a esse dígito. Caso contrário, interprete o valor como literal.|
  |`\` seguido por um dígito de 1 a 9, seguido por dígitos decimais adicionais|Interprete os dígitos como um valor decimal. Se o grupo de capturas existir, interprete a expressão como referência inversa.<br /><br /> Caso contrário, interprete os dígitos octais iniciais até o octal 377; ou seja, considere apenas 8 bits inferiores do valor. Interprete os dígitos restantes como literais. Por exemplo, na expressão `\3000`, se o grupo de capturas 300 existir, interprete como referência inversa 300; se o grupo de capturas 300 não existir, interprete como um octal 300 seguido por 0.|Interprete como uma referência inversa convertendo todos os dígitos possíveis para um valor decimal que pode fazer referência a uma captura. Se nenhum dígito puder ser convertido, interprete como um octal usando os dígitos octais iniciais até o octal 377; interprete os dígitos restantes como literais.|

## <a name="comparison-using-the-invariant-culture"></a>Comparação usando a cultura invariável

Por padrão, quando o mecanismo de expressão regular realiza comparações sem diferenciar maiúsculas de minúsculas, ele usa as convenções de maiúsculas de minúsculas da cultura atual para determinar caracteres equivalentes em maiúsculas e minúsculas.

Porém, esse comportamento é indesejável para alguns tipos de comparações, especialmente ao comparar a entrada do usuário com os nomes de recursos do sistema, como senhas, arquivos ou URLs. O exemplo a seguir ilustra esse cenário. O código tem como objetivo bloquear o acesso a qualquer recurso cuja URL seja prefixada com **FILE://**. A expressão regular tenta uma correspondência sem diferenciar maiúsculas e minúsculas com a cadeia de caracteres usando a expressão regular `$FILE://`. Porém, quando a cultura do sistema atual é tr-TR (Turco-Turquia), "I" não é o equivalente maiúsculo de "i". Como resultado, a chamada para o método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> retorna `false` e o acesso ao arquivo é permitido.

[!code-csharp[Conceptual.Regex.Language.Options#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/culture1.cs#14)]
[!code-vb[Conceptual.Regex.Language.Options#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/culture1.vb#14)]

> [!NOTE]
> Para obter mais informações sobre comparações de cadeias de caracteres que diferenciam maiúsculas de minúsculas e usam cultura invariável, consulte [Práticas recomendadas para o uso de cadeias de caracteres](best-practices-strings.md).

Em vez de usar comparações que não diferenciam maiúsculas de minúsculas da cultura atual, é possível especificar a opção <xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType> para ignorar diferenças culturais no idioma e usar as convenções da cultura invariável.

> [!NOTE]
> A comparação usando cultura invariável está disponível apenas fornecendo o valor <xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType> para o parâmetro `options` de um construtor de classe <xref:System.Text.RegularExpressions.Regex> ou método de correspondência padrão estático. Não está disponível como uma opção embutida.

O exemplo a seguir é idêntico ao anterior, exceto que o método estático <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> é chamado com opções que incluem <xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType>. Mesmo quando a cultura atual é definida como turco (Turquia), o mecanismo de expressões regulares consegue corresponder com sucesso “FILE” e “file” e bloquear o acesso ao recurso do arquivo.

[!code-csharp[Conceptual.Regex.Language.Options#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/culture1.cs#15)]
[!code-vb[Conceptual.Regex.Language.Options#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/culture1.vb#15)]

## <a name="see-also"></a>Confira também

- [Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)
