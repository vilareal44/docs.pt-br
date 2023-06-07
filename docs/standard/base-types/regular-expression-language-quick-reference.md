---
title: Linguagem de expressões regulares – referência rápida
description: Nesta referência rápida, aprenda a usar padrões de expressão regular para corresponder o texto de entrada. Um padrão tem um ou mais literais de caractere, operadores ou construções.
ms.date: 03/30/2017
ms.technology: dotnet-standard
f1_keywords:
- VS.RegularExpressionBuilder
helpviewer_keywords:
- regex cheat sheet
- parsing text with regular expressions, language elements
- searching with regular expressions, language elements
- pattern-matching with regular expressions, language elements
- regular expressions, language elements
- regular expressions [.NET Framework]
- cheat sheet
- .NET Framework regular expressions, language elements
ms.assetid: 930653a6-95d2-4697-9d5a-52d11bb6fd4c
ms.openlocfilehash: 4788c84be76a5cc9a9a6327fcd054e08db4d1872
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556794"
---
# <a name="regular-expression-language---quick-reference"></a>Linguagem de expressões regulares – referência rápida

Uma expressão regular é um padrão ao qual o mecanismo de expressões regulares tenta corresponder no texto de entrada. Um padrão consiste em um ou mais literais de caracteres, operadores ou constructos. Para ver uma breve introdução, confira [Expressões regulares no .NET](regular-expressions.md).

Cada seção desta referência rápida lista uma categoria específica de caracteres, operadores e constructos que podem ser usados para definir expressões regulares.

Também fornecemos essas informações em dois formatos que você pode baixar e imprimir para facilitar a referência:

- [Baixar no formato Word (.docx)](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.docx)
- [Baixar no formato PDF (.pdf)](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.pdf)

## <a name="character-escapes"></a>Escapes de caracteres

O caractere de barra invertida (\\) em uma expressão regular indica que o próximo caractere é um caractere especial (conforme mostrado na tabela a seguir) ou se deve ser interpretado literalmente. Para obter mais informações, consulte [Escapes de Caracteres](character-escapes-in-regular-expressions.md).

|Caractere com escape|Descrição|Padrão|Correspondências|
|-----------------------|-----------------|-------------|-------------|
|`\a`|Corresponde a um caractere de sino, \u0007.|`\a`|`"\u0007"` em `"Error!" + '\u0007'`|
|`\b`|Em uma classe de caractere, corresponde a um backspace, \ u0008.|`[\b]{3,}`|`"\b\b\b\b"` em `"\b\b\b\b"`|
|`\t`|Corresponde a uma tabulação, \u0009.|`(\w+)\t`|`"item1\t"` e `"item2\t"` em `"item1\titem2\t"`|
|`\r`|Corresponde a um retorno de carro, \u000D. (`\r` não é equivalente ao caractere newline, `\n`.)|`\r\n(\w+)`|`"\r\nThese"` em `"\r\nThese are\ntwo lines."`|
|`\v`|Corresponde a uma tabulação vertical, \u000B.|`[\v]{2,}`|`"\v\v\v"` em `"\v\v\v"`|
|`\f`|Corresponde a um avanço de página, \u000C.|`[\f]{2,}`|`"\f\f\f"` em `"\f\f\f"`|
|`\n`|Corresponde a uma nova linha, \u000A.|`\r\n(\w+)`|`"\r\nThese"` em `"\r\nThese are\ntwo lines."`|
|`\e`|Corresponde a um escape, \u001B.|`\e`|`"\x001B"` em `"\x001B"`|
|`\` *nnn*|Usa representação octal para especificar um caractere (*nnn* consiste em dois ou três dígitos).|`\w\040\w`|`"a b"` e `"c d"` em `"a bc d"`|
|`\x` *nn*|Usa representação hexadecimal para especificar um caractere (*nn* consiste exatamente em dois dígitos).|`\w\x20\w`|`"a b"` e `"c d"` em `"a bc d"`|
|`\c` *X*<br /><br /> `\c` *x*|Corresponde ao caractere de controle ASCII especificado por *X* ou *x*, em que *X* ou *x* é a letra do caractere de controle.|`\cC`|`"\x0003"` em `"\x0003"` (Ctrl-C)|
|`\u` *nnnn*|Corresponde a um caractere Unicode usando representação hexadecimal (exatamente quatro dígitos, como representado por *nnnn*).|`\w\u0020\w`|`"a b"` e `"c d"` em `"a bc d"`|
|`\`|Quando seguido por um caractere que não é reconhecido como um caractere de escape nesta e em outras tabelas deste tópico, corresponde a esse caractere. Por exemplo, `\*` é igual a `\x2A`, e `\.` é igual a `\x2E`. Isso permite que o mecanismo de expressões regulares remova ambiguidades de elementos da linguagem (como \* ou ?) e caracteres literais (representados por `\*` ou `\?`).|`\d+[\+-x\*]\d+`|`"2+2"` e `"3*9"` em `"(2+2) * 3*9"`|

## <a name="character-classes"></a>Classes de caracteres

Uma classe de caractere corresponde a qualquer um dos conjuntos de caracteres. As classes de caracteres incluem os elementos de linguagem listados na tabela a seguir. Para saber mais, confira [Classes de caracteres](character-classes-in-regular-expressions.md).

|Classe de caractere|Descrição|Padrão|Correspondências|
|---------------------|-----------------|-------------|-------------|
|`[`*character_group*`]`|Corresponde a qualquer caractere único em *character_group*. Por padrão, a correspondência diferencia maiúsculas de minúsculas.|`[ae]`|`"a"` em `"gray"`<br /><br /> `"a"` e `"e"` em `"lane"`|
|`[^`*character_group*`]`|Negação: corresponde a qualquer caractere único que não esteja em *character_group*. Por padrão, caracteres em *character_group* diferenciam maiúsculas de minúsculas.|`[^aei]`|`"r"`, `"g"` e `"n"` em `"reign"`|
|`[` *primeiro* `-` *último* `]`|Intervalo de caracteres: corresponde a qualquer caractere único no intervalo entre *first* e *last*.|`[A-Z]`|`"A"` e `"B"` em `"AB123"`|
|`.`|Curinga: corresponde a qualquer caractere único, exceto \n.<br /><br /> Para corresponder a um caractere literal de ponto (. ou `\u002E`), você deve precedê-lo com o caractere de escape (`\.`).|`a.e`|`"ave"` em `"nave"`<br /><br /> `"ate"` em `"water"`|
|`\p{`*nome* do`}`|Corresponde a qualquer caractere único na categoria geral Unicode ou no bloco nomeado especificado por *nome*.|`\p{Lu}`<br /><br /> `\p{IsCyrillic}`|`"C"` e `"L"` em `"City Lights"`<br /><br /> `"Д"` e `"Ж"` em `"ДЖem"`|
|`\P{`*nome* do`}`|Corresponde a qualquer caractere único que não esteja na categoria geral Unicode ou no bloco nomeado especificado por *nome*.|`\P{Lu}`<br /><br /> `\P{IsCyrillic}`|`"i"`, `"t"` e `"y"` em `"City"`<br /><br /> `"e"` e `"m"` em `"ДЖem"`|
|`\w`|Corresponde a qualquer caractere de palavra.|`\w`|`"I"`, `"D"`, `"A"`, `"1"` e `"3"` em `"ID A1.3"`|
|`\W`|Corresponde a qualquer caractere que não seja uma palavra.|`\W`|`" "` e `"."` em `"ID A1.3"`|
|`\s`|Corresponde a qualquer caractere de espaço em branco.|`\w\s`|`"D "` em `"ID A1.3"`|
|`\S`|Corresponde a qualquer caractere que não seja um caractere de espaço em branco.|`\s\S`|`" _"` em `"int __ctr"`|
|`\d`|Corresponde a qualquer dígito decimal.|`\d`|`"4"` em `"4 = IV"`|
|`\D`|Corresponde a qualquer caractere que não seja uma dígito decimal.|`\D`|`" "`, `"="`, `" "`, `"I"` e `"V"` em `"4 = IV"`|

## <a name="anchors"></a>Âncoras

Âncoras ou asserções atômicas de largura zero, fazem com que uma correspondência tenha êxito ou falha dependendo da posição atual na cadeia de caracteres, mas não fazem com que o mecanismo avance na cadeia de caracteres ou consuma caracteres. Os metacaracteres listados na tabela a seguir são âncoras. Para saber mais, confira [Âncoras](anchors-in-regular-expressions.md).

|Asserção|Descrição|Padrão|Correspondências|
|---------------|-----------------|-------------|-------------|
|`^`|Por padrão, a correspondência precisa começar no início da cadeia de caracteres. No modo multilinha, precisa começar no início da linha.|`^\d{3}`|`"901"` em `"901-333-"`|
|`$`|Por padrão, a correspondência deve ocorrer no fim da cadeia de caracteres ou antes de `\n` no fim da cadeia de caracteres. No modo multilinha, deve antes do fim da linha ou antes de `\n` no fim da linha.|`-\d{3}$`|`"-333"` em `"-901-333"`|
|`\A`|A correspondência deve ocorrer no início da cadeia de caracteres.|`\A\d{3}`|`"901"` em `"901-333-"`|
|`\Z`|A correspondência deve ocorrer no final da cadeia de caracteres ou antes de `\n` no final da cadeia de caracteres.|`-\d{3}\Z`|`"-333"` em `"-901-333"`|
|`\z`|A correspondência deve ocorrer no final da cadeia de caracteres.|`-\d{3}\z`|`"-333"` em `"-901-333"`|
|`\G`|A correspondência deve ocorrer no ponto em que a correspondência anterior foi encerrada.|`\G\(\d\)`|`"(1)"`, `"(3)"` e `"(5)"` em `"(1)(3)(5)[7](9)"`|
|`\b`|A correspondência deve ocorrer em um limite entre um caractere `\w` (alfanumérico) e um caractere `\W` (não alfanumérico).|`\b\w+\s\w+\b`|`"them theme"` e `"them them"` em `"them theme them them"`|
|`\B`|A correspondência não deve ocorrer em um limite `\b`.|`\Bend\w*\b`|`"ends"` e `"ender"` em `"end sends endure lender"`|

## <a name="grouping-constructs"></a>Constructos de agrupamento

Os constructos de agrupamento delineiam subexpressões de uma expressão regular e, em geral, capturam subcadeias de caracteres de uma cadeia de caracteres de entrada. Os constructos de agrupamento incluem os elementos de linguagem listados na tabela a seguir. Para saber mais, confira [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).

|Constructo de agrupamento|Descrição|Padrão|Correspondências|
|------------------------|-----------------|-------------|-------------|
|`(`*subexpressão*`)`|Captura a subexpressão correspondente e atribui a ela um número ordinal com base um.|`(\w)\1`|`"ee"` em `"deep"`|
|`(?<` *nome* `>` *subexpressão* `)`<br /> ou <br />`(?'` *nome* `'` *subexpressão* `)`|Captura a subexpressão correspondente em um grupo nomeado.|`(?<double>\w)\k<double>`|`"ee"` em `"deep"`|
|`(?<` *nome1* `-` *nome2* `>` *subexpressão* `)` <br /> ou <br /> `(?'` *nome1* `-` *nome2* `'` *subexpressão* `)`|Especifica uma definição de grupo de balanceamento. Para saber mais, confira a seção "Definição de grupo de balanceamento" em [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).|`(((?'Open'\()[^\(\)]*)+((?'Close-Open'\))[^\(\)]*)+)*(?(Open)(?!))$`|`"((1-3)*(3-1))"` em `"3+2^((1-3)*(3-1))"`|
|`(?:`*subexpressão*`)`|Define um grupo de não captura.|`Write(?:Line)?`|`"WriteLine"` em `"Console.WriteLine()"`<br /><br /> `"Write"` em `"Console.Write(value)"`|
|`(?imnsx-imnsx:`*subexpressão*`)`|Aplica ou desabilita as opções especificadas em *subexpressão*. Para obter mais informações, consulte [Opções de expressão regular](regular-expression-options.md).|`A\d{2}(?i:\w+)\b`|`"A12xl"` e `"A12XL"` em `"A12xl A12XL a12xl"`|
|`(?=`*subexpressão*`)`|Asserções lookahead positivas de largura zero.|`\w+(?=\.)`|`"is"`, `"ran"` e `"out"` em `"He is. The dog ran. The sun is out."`|
|`(?!`*subexpressão*`)`|Asserções lookahead negativas de largura zero.|`\b(?!un)\w+\b`|`"sure"` e `"used"` em `"unsure sure unity used"`|
|`(?<=`*subexpressão*`)`|Asserção lookbehind positiva de largura zero.|`(?<=19)\d{2}\b`|`"99"`, `"50"` e `"05"` em `"1851 1999 1950 1905 2003"`|
|`(?<!`*subexpressão*`)`|Asserção lookbehind negativa de largura zero.|`(?<!19)\d{2}\b`|`"51"` e `"03"` em `"1851 1999 1950 1905 2003"`|
|`(?>`*subexpressão*`)`|Grupo atômico.|`[13579](?>A+B+)`|`"1ABB"`, `"3ABB"` e `"5AB"` em `"1ABB 3ABBC 5AB 5AC"`|

## <a name="quantifiers"></a>Quantificadores

Um quantificador especifica quantas instâncias do elemento anterior (que pode ser um caractere, um grupo ou uma classe de caracteres) devem estar presentes na cadeia de caracteres de entrada para que uma correspondência ocorra. Os quantificadores incluem os elementos de linguagem listados na tabela a seguir. Para saber mais, confira [Quantificadores](quantifiers-in-regular-expressions.md).

|Quantificador|Descrição|Padrão|Correspondências|
|----------------|-----------------|-------------|-------------|
|`*`|Corresponde ao elemento anterior zero ou mais vezes.|`\d*\.\d`|`".0"`, `"19.9"`, `"219.9"`|
|`+`|Corresponde ao elemento anterior uma ou mais vezes.|`"be+"`|`"bee"` em `"been"`, `"be"` em `"bent"`|
|`?`|Corresponde ao elemento anterior zero ou uma vez.|`"rai?n"`|`"ran"`, `"rain"`|
|`{`*n*`}`|Corresponde ao elemento anterior exatamente *n* vezes.|`",\d{3}"`|`",043"` em `"1,043.6"`, `",876"`, `",543"` e `",210"` em `"9,876,543,210"`|
|`{`*n*`,}`|Corresponde ao elemento anterior pelo menos *n* vezes.|`"\d{2,}"`|`"166"`, `"29"`, `"1930"`|
|`{`*n* `,` *m*`}`|Corresponde ao elemento anterior pelo menos *n* vezes, mas não mais do que *m* vezes.|`"\d{3,5}"`|`"166"`, `"17668"`<br /><br /> `"19302"` em `"193024"`|
|`*?`|Corresponde ao elemento anterior zero ou mais vezes, mas o menor número de vezes possível.|`\d*?\.\d`|`".0"`, `"19.9"`, `"219.9"`|
|`+?`|Corresponde ao elemento anterior uma ou mais vezes, mas o menor número de vezes possível.|`"be+?"`|`"be"` em `"been"`, `"be"` em `"bent"`|
|`??`|Corresponde ao elemento anterior zero ou uma vez, mas o menor número de vezes possível.|`"rai??n"`|`"ran"`, `"rain"`|
|`{`*n*`}?`|Corresponde ao elemento anterior exatamente *n* vezes.|`",\d{3}?"`|`",043"` em `"1,043.6"`, `",876"`, `",543"` e `",210"` em `"9,876,543,210"`|
|`{`*n*`,}?`|Corresponde ao elemento anterior pelo menos *n* vezes, mas o menor número de vezes possível.|`"\d{2,}?"`|`"166"`, `"29"`, `"1930"`|
|`{`*n* `,` *m*`}?`|Corresponde ao elemento anterior entre *n* e *m* vezes, mas o menor número de vezes possível.|`"\d{3,5}?"`|`"166"`, `"17668"`<br /><br /> `"193"` e `"024"` em `"193024"`|

## <a name="backreference-constructs"></a>Construtores de referência inversa

Um referência inversa permite que uma subexpressão correspondida anteriormente seja identificada posteriormente na mesma expressão regular. A tabela a seguir lista os constructos de referência inversa tem suporte nas expressões regulares do .NET. Para saber mais, confira [Constructos de referência inversa](backreference-constructs-in-regular-expressions.md).

|Constructo de referência inversa|Descrição|Padrão|Correspondências|
|-----------------------------|-----------------|-------------|-------------|
|`\` *número*|Referência inversa. Corresponde ao valor de uma subexpressão numerada.|`(\w)\1`|`"ee"` em `"seek"`|
|`\k<`*nome* do`>`|Referência inversa nomeada. Corresponde ao valor de uma expressão nomeada.|`(?<char>\w)\k<char>`|`"ee"` em `"seek"`|

## <a name="alternation-constructs"></a>Construtores de alternância

Os constructos de alternância modificam uma expressão regular para habilitar uma correspondência do tipo um/ou outro. Esses constructos incluem os elementos de linguagem listados na tabela a seguir. Para saber mais, confira [Constructos de alternância](alternation-constructs-in-regular-expressions.md).

|Constructo de alternância|Descrição|Padrão|Correspondências|
|---------------------------|-----------------|-------------|-------------|
|<code>&#124;</code>|Corresponde a qualquer elemento separado pelo caractere de barra vertical (<code>&#124;</code>).|<code>th(e&#124;is&#124;at)</code>|`"the"` e `"this"` em `"this is the day."`|
|`(?(`*expressão* `)` de *Sim* <code>&#124;</code> *não*`)`|Corresponde a *yes* se o padrão de expressão regular designado por *expression* for correspondente. Do contrário, corresponde à parte *no* opcional. *expressão* é interpretado como uma asserção de largura zero.|<code>(?(A)A\d{2}\b&#124;\b\d{3}\b)</code>|`"A10"` e `"910"` em `"A10 C103 910"`|
|`(?(` *nome* `)` *sim* <code>&#124;</code> *não* `)`|Corresponde a *yes* se *name*, um grupo de captura nomeado ou numerado, apresenta uma correspondência. Do contrário, corresponde a *no* opcional.|<code>(?&lt;quoted&gt;&quot;)?(?(quoted).+?&quot;&#124;\S+\s)</code>|`"Dogs.jpg "` e `"\"Yiska playing.jpg\""` em `"Dogs.jpg \"Yiska playing.jpg\""`|

## <a name="substitutions"></a>Substituições

As substituições são elementos de linguagem de expressões regulares com suporte em padrões de substituição. Para saber mais, confira [Substituições](substitutions-in-regular-expressions.md). Os metacaracteres listados na tabela a seguir são asserções atômicas de largura zero.

|Caractere|Descrição|Padrão|Padrão de substituição|Cadeia de caracteres de entrada|Cadeia de caracteres de resultado|
|---------------|-----------------|-------------|-------------------------|------------------|-------------------|
|`$` *número*|Substitui a subcadeia de caracteres correspondida pelo grupo *number*.|`\b(\w+)(\s)(\w+)\b`|`$3$2$1`|`"one two"`|`"two one"`|
|`${`*nome* do`}`|Substitui a substring de caracteres correspondida pelo grupo chamado *nome*.|`\b(?<word1>\w+)(\s)(?<word2>\w+)\b`|`${word2} ${word1}`|`"one two"`|`"two one"`|
|`$$`|Substitui um literal "$".|`\b(\d+)\s?USD`|`$$$1`|`"103 USD"`|`"$103"`|
|`$&`|Substitui uma cópia da correspondência inteira.|`\$?\d*\.?\d+`|`**$&**`|`"$1.30"`|`"**$1.30**"`|
|``$` ``|Substitui todo o texto da cadeia de caracteres de entrada antes da correspondência.|`B+`|``$` ``|`"AABBCC"`|`"AAAACC"`|
|`$'`|Substitui todo o texto da cadeia de caracteres de entrada após a correspondência.|`B+`|`$'`|`"AABBCC"`|`"AACCCC"`|
|`$+`|Substitui o último grupo que foi capturado.|`B+(C+)`|`$+`|`"AABBCCDD"`|`"AACCDD"`|
|`$_`|Substitui a cadeia de caracteres de entrada inteira.|`B+`|`$_`|`"AABBCC"`|`"AAAABBCCCC"`|

## <a name="regular-expression-options"></a>Opções de expressões regulares

Você pode especificar opções que controlam como o mecanismo de expressões regulares interpreta uma expressão regular padrão. Muitas dessas opções podem ser especificadas de maneira embutida (no padrão da expressão regular) ou como uma ou mais constantes <xref:System.Text.RegularExpressions.RegexOptions>. Essa referência rápida lista somente as opções embutidas. Para obter mais informações sobre opções embutidas e <xref:System.Text.RegularExpressions.RegexOptions>, consulte o artigo [Opções de expressões regulares](regular-expression-options.md).

É possível especificar uma opção embutida de duas formas:

- Usando o [constructo diverso](miscellaneous-constructs-in-regular-expressions.md) `(?imnsx-imnsx)`, em que um sinal de subtração (-) antes de uma opção ou um conjunto de opções desativa essas opções. Por exemplo, `(?i-mn)` ativa a correspondência sem diferenciação de maiúsculas e minúsculas (`i`), desativa o modo de várias linhas (`m`) e desativa capturas de grupo sem nome (`n`). A opção se aplica ao padrão de expressão regular no ponto em que a opção é definida e entra em vigor no final do padrão ou no ponto em que outro constructo inverte a opção.
- Usando a subexpressão de [construção de agrupamento](grouping-constructs-in-regular-expressions.md) `(?imnsx-imnsx:` *subexpression* `)` , que define as opções somente para o grupo especificado.

O mecanismo de expressões regulares do .NET oferece suporte às seguintes opções embutidas:

|Opção|DESCRIÇÃO|Padrão|Correspondências|
|------------|-----------------|-------------|-------------|
|`i`|Use correspondência sem diferenciação de maiúsculas e minúsculas.|`\b(?i)a(?-i)a\w+\b`|`"aardvark"` e `"aaaAuto"` em `"aardvark AAAuto aaaAuto Adam breakfast"`|
|`m`|Use o modo multilinha. `^` e `$` correspondem ao início e ao fim de uma linha em vez do início e fim de uma cadeia de caracteres.|Para obter um exemplo, consulte a seção "modo multilinha" em [Opções de expressão regular](regular-expression-options.md).||
|`n`|Não capture grupos sem nome.|Para obter um exemplo, consulte a seção "capturas explícitas somente" em [Opções de expressão regular](regular-expression-options.md).||
|`s`|Use o modo de linha única.|Para obter um exemplo, consulte a seção "modo de linha única" em [Opções de expressão regular](regular-expression-options.md).||
|`x`|Ignore espaços em branco sem escape no padrão da expressão regular.|`\b(?x) \d+ \s \w+`|`"1 aardvark"` e `"2 cats"` em `"1 aardvark 2 cats IV centurions"`|

## <a name="miscellaneous-constructs"></a>Constructos diversos

Os constructos diversos modificam um expressão regular padrão ou fornecem informações sobre ela. A tabela a seguir lista os constructos diversos que têm suporte no .NET. Para saber mais, confira [Constructos diversos](miscellaneous-constructs-in-regular-expressions.md).

|Constructo|Definição|Exemplo|
|---------------|----------------|-------------|
|`(?imnsx-imnsx)`|Define ou desabilita opções como não diferenciação de maiúsculas e minúsculas no meio de um padrão. Para saber mais, confira [Opções de expressão regular](regular-expression-options.md).|`\bA(?i)b\w+\b` corresponde a `"ABA"` e `"Able"` em `"ABA Able Act"`|
|`(?#`*Comentário*`)`|Comentário embutido. O comentário é encerrado no primeiro caractere de fechar parênteses.|`\bA(?#Matches words starting with A)\w+\b`|
|`#` [até o final da linha]|Comentário do modo X. O comentário começa em um `#` sem escape e continua até o final da linha.|`(?x)\bA\w+\b#Matches words starting with A`|

## <a name="see-also"></a>Confira também

- <xref:System.Text.RegularExpressions?displayProperty=nameWithType>
- <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType>
- [Expressões regulares](regular-expressions.md)
- [Classes de expressões regulares](the-regular-expression-object-model.md)
- [Expressões regulares - referência rápida (fazer download no formato Word)](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.docx)
- [Expressões regulares - referência rápida (fazer download no formato PDF)](https://download.microsoft.com/download/D/2/4/D240EBF6-A9BA-4E4F-A63F-AEB6DA0B921C/Regular%20expressions%20quick%20reference.pdf)
