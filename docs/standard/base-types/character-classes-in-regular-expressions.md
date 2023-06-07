---
title: Classes de caracteres em expressões regulares do .NET
description: Saiba como usar classes de caracteres para representar um conjunto de caracteres em expressões regulares do .NET.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- character classes
- regular expressions, character classes
- characters, matching syntax
- .NET Framework regular expressions, character classes
ms.assetid: 0f8bffab-ee0d-4e0e-9a96-2b4a252bb7e4
ms.openlocfilehash: 85107bf2234eda1705126e524acd5b35952094bc
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84292092"
---
# <a name="character-classes-in-regular-expressions"></a>Classes de caracteres em expressões regulares

Uma classe de caracteres define um conjunto de caracteres, qualquer dos quais pode ocorrer em uma cadeia de caracteres de entrada para que uma correspondência seja bem-sucedida. A linguagem de expressões regulares no .NET dá suporte às seguintes classes de caracteres:  
  
- Grupos de caracteres positivos. Um caractere na cadeia de caracteres de entrada deve corresponder a um de um conjunto especificado de caracteres. Para obter mais informações, consulte [grupo de caracteres positivo](#PositiveGroup).  
  
- Grupo de caracteres negativos. Um caractere na cadeia de caracteres de entrada não deve corresponder a um de um conjunto especificado de caracteres. Para obter mais informações, consulte [negativo, grupo de caracteres](#NegativeGroup).  
  
- Qualquer caractere. O `.` (ponto) em uma expressão regular é um caractere curinga que corresponde a qualquer caractere, exceto `\n`. Para obter mais informações, consulte [qualquer caractere](#AnyCharacter).  
  
- Uma categoria geral de Unicode ou bloco nomeado. Um caractere na cadeia de caracteres de entrada deve ser um membro de uma categoria de Unicode específica ou deve estar dentro de um intervalo contíguo de caracteres Unicode para que uma correspondência seja bem-sucedida. Para obter mais informações, consulte a [categoria Unicode ou o bloco Unicode](#CategoryOrBlock).  
  
- Uma categoria geral de Unicode ou bloco nomeado negativos. Um caractere na cadeia de caracteres de entrada não deve ser um membro de uma categoria de Unicode específica ou não deve estar dentro de um intervalo contíguo de caracteres Unicode para que uma correspondência seja bem-sucedida. Para obter mais informações, veja [categoria Unicode negativo ou bloco Unicode](#NegativeCategoryOrBlock).  
  
- Um caractere de palavra. Um caractere na cadeia de caracteres de entrada pode pertencer a qualquer uma das categorias Unicode que são apropriados para caracteres usados em palavras. Para obter mais informações, consulte [Word Character](#WordCharacter).  
  
- Um caractere não pertencente a palavras. Um caractere na cadeia de caracteres de entrada pode pertencer a qualquer categoria Unicode que não seja um caractere de palavra. Para obter mais informações, consulte [caracteres que não são palavras](#NonWordCharacter).  
  
- Um caractere de espaço em branco. Um caractere na cadeia de caracteres de entrada pode ser qualquer caractere separador Unicode, bem como qualquer um entre vários caracteres de controle. Para obter mais informações, consulte [caracteres de espaço em branco](#WhitespaceCharacter).  
  
- Um caractere diferente de espaço em branco. Um caractere na cadeia de caracteres de entrada pode ser qualquer caractere que não seja um caractere de espaço em branco. Para obter mais informações, consulte [caracteres que não são de espaço em branco](#NonWhitespaceCharacter).  
  
- Um dígito decimal. Um caractere na cadeia de caracteres de entrada pode ser qualquer um entre vários caracteres classificados como dígitos decimais Unicode. Para obter mais informações, consulte [caractere de dígito decimal](#DigitCharacter).  
  
- Um dígito não decimal. Um caractere na cadeia de caracteres de entrada pode ser qualquer coisa que não seja um dígito decimal Unicode. Para obter mais informações, consulte [caractere de dígito decimal](#NonDigitCharacter).  
  
 O .NET dá suporte a expressões de subtração de classes de caracteres, o que permite a você definir um conjunto de caracteres como resultado da exclusão de uma classe de caracteres de outra classe de caracteres. Para obter mais informações, consulte [subtração de classe de caractere](#CharacterClassSubtraction).  
  
> [!NOTE]
> Classes de caractere que fazem a correspondência de caracteres por categoria, como [\w](#WordCharacter) para a correspondência de caracteres de palavra ou [\p{}](#CategoryOrBlock) para uma categoria de Unicode, dependem da classe <xref:System.Globalization.CharUnicodeInfo> para fornecer informações sobre categorias de caractere.  Do .NET Framework 4.6.2 em diante, as categorias de caracteres se baseiam no [Padrão Unicode, versão 8.0.0](https://www.unicode.org/versions/Unicode8.0.0/). No .NET Framework 4 até o .NET Framework 4.6.1, eles se baseiam no [Padrão Unicode, versão 6.3.0](https://www.unicode.org/versions/Unicode6.3.0/).  
  
<a name="PositiveGroup"></a>
## <a name="positive-character-group--"></a>Grupo de caracteres positivo: [ ]  
 Um grupo de caracteres positivos especifica uma lista de caracteres, qualquer caractere dela pode aparecer em uma cadeia de caracteres de entrada para que uma correspondência ocorra. Essa lista de caracteres pode ser especificada individualmente, como um intervalo ou ambos.  
  
 A sintaxe para especificar uma lista de caracteres individuais é a seguinte:  

`[*character_group*]`

 em que *character_group* é uma lista dos caracteres individuais que podem aparecer na cadeia de caracteres de entrada para que uma correspondência seja bem-sucedida. *character_group* pode consistir em qualquer combinação de um ou mais caracteres literais, [caracteres de escape](character-escapes-in-regular-expressions.md)ou classes de caracteres.  
  
 A sintaxe para especificar um intervalo de caracteres é a seguinte:  
  
`[firstCharacter-lastCharacter]`  
  
 em que *firstCharacter* é o caractere que começa o intervalo e *lastCharacter* é o caractere que termina o intervalo. Um intervalo de caracteres é uma série contígua de caracteres definida pela especificação do primeiro caractere na série, um hífen (-) e o último caractere na série. Dois caracteres são contíguos se eles têm pontos de código Unicode adjacentes. *firstCharacter* precisa ser o caractere com o ponto de código menor, enquanto *lastCharacter* precisa ser o caractere com o ponto de código maior.

> [!NOTE]
> Visto que um grupo de caracteres positivos pode incluir um conjunto de caracteres e um intervalo de caracteres, um caractere de hífen (`-`) é sempre interpretado como o separador de intervalo, a menos que ele seja o primeiro ou último caractere do grupo.

Alguns padrões de expressões regulares comuns que contêm classes de caracteres positivos são listados na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`[aeiou]`|Corresponder a todas as vogais.|  
|`[\p{P}\d]`|Corresponder a todos os caracteres de pontuação e dígitos decimais.|  
|`[\s\p{P}]`|Corresponder a todos os espaços em branco e à pontuação.|  
  
 O exemplo a seguir define um grupo de caracteres positivos que contém os caracteres “a” e “e” para que a cadeia de caracteres de entrada contenha as palavras “grey” ou “gray” seguida de outra palavra para que uma correspondência ocorra.  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/positivecharclasses.cs#1)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/positivecharclasses.vb#1)]  
  
 A expressão regular `gr[ae]y\s\S+?[\s|\p{P}]` é definida da seguinte forma:  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`gr`|Corresponder aos caracteres literais “gr”.|  
|`[ae]`|Corresponder a um "a" ou "e".|  
|`y\s`|Corresponder ao caractere literal “y” seguido por um caractere de espaço em branco.|  
|`\S+?`|Corresponder a um ou mais caracteres diferentes de espaço em branco, mas o mínimo possível.|  
|`[\s\p{P}]`|Corresponder a um caractere de espaço em branco ou a um sinal de pontuação.|  
  
 O exemplo a seguir corresponde a palavras que começam com qualquer letra maiúscula. Ele usa a subexpressão `[A-Z]` para representar o intervalo de letras maiúsculas de A a Z.  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/range.cs#3)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/range.vb#3)]  
  
 A expressão regular `\b[A-Z]\w*\b` é definida conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`[A-Z]`|Corresponder a qualquer caractere maiúsculo de A a Z.|  
|`\w*`|Corresponder a zero ou mais caracteres de palavra.|  
|`\b`|Corresponder a um limite de palavra.|  
  
<a name="NegativeGroup"></a>
## <a name="negative-character-group-"></a>Grupo de caracteres negativos: [^]  
 Um grupo de caracteres negativos especifica uma lista de caracteres que não devem aparecer em uma cadeia de caracteres de entrada para que uma correspondência ocorra. A lista de caracteres pode ser especificada individualmente, como um intervalo ou ambos.  
  
A sintaxe para especificar uma lista de caracteres individuais é a seguinte:  

`[*^character_group*]`

 em que *character_group* é uma lista dos caracteres individuais que não podem aparecer na cadeia de caracteres de entrada para que uma correspondência seja bem-sucedida. *character_group* pode consistir em qualquer combinação de um ou mais caracteres literais, [caracteres de escape](character-escapes-in-regular-expressions.md)ou classes de caracteres.  
  
 A sintaxe para especificar um intervalo de caracteres é a seguinte:  

`[^*firstCharacter*-*lastCharacter*]`

em que *firstCharacter* é o caractere que começa o intervalo e *lastCharacter* é o caractere que termina o intervalo. Um intervalo de caracteres é uma série contígua de caracteres definida pela especificação do primeiro caractere na série, um hífen (-) e o último caractere na série. Dois caracteres são contíguos se eles têm pontos de código Unicode adjacentes. *firstCharacter* precisa ser o caractere com o ponto de código menor, enquanto *lastCharacter* precisa ser o caractere com o ponto de código maior.

> [!NOTE]
> Visto que um grupo de caracteres negativos pode incluir um conjunto de caracteres e um intervalo de caracteres, um caractere de hífen (`-`) é sempre interpretado como o separador de intervalo, a menos que ele seja o primeiro ou último caractere do grupo.
  
 Dois ou mais intervalos de caracteres podem ser concatenados. Por exemplo, para especificar o intervalo de dígitos decimais de "0 " a "9", o intervalo de letras minúsculas de "a" a "f" e o intervalo de letras maiúsculas "A" a "F", use `[0-9a-fA-F]`.  
  
 O caractere de cursor à esquerda ( `^` ) em um grupo de caracteres negativo é obrigatório e indica que o grupo de caracteres é um grupo de caracteres negativo em vez de um grupo de caracteres positivo.  
  
> [!IMPORTANT]
> Um grupo de caracteres negativos em uma expressão regular maior não é uma asserção de largura zero. Ou seja, depois de avaliar o grupo de caracteres negativos, o mecanismo de expressões regulares avança um caractere na cadeia de caracteres de entrada.  
  
 Alguns padrões de expressões regulares comuns que contêm grupos de caracteres negativos são listados na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`[^aeiou]`|Corresponder a todos os caracteres, exceto vogais.|  
|`[^\p{P}\d]`|Corresponder a todos os caracteres, exceto de pontuação e dígitos decimais.|  
  
 O exemplo a seguir corresponde a todas as palavras que começam com os caracteres “th” e não são seguidas por um “o”.  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/negativecharclasses.cs#2)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/negativecharclasses.vb#2)]  
  
 A expressão regular `\bth[^o]\w+\b` é definida conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`th`|Corresponder aos caracteres literais “th”.|  
|`[^o]`|Corresponder a qualquer caractere que não seja um “o”.|  
|`\w+`|Fazer a correspondência a um ou mais caracteres de palavra.|  
|`\b`|Terminar em um limite de palavra.|  
  
<a name="AnyCharacter"></a>
## <a name="any-character-"></a>Qualquer caractere: .  
 O caractere de ponto (.) corresponde a qualquer caractere, exceto `\n` (o caractere de nova linha, \u000A), com estas duas qualificações:  
  
- Se um padrão de expressão regular é modificado pela opção <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType>, ou se a parte do padrão que contém a classe de caracteres `.` é modificada pela opção `s`, `.` corresponde a qualquer caractere. Para obter mais informações, consulte [Opções de expressão regular](regular-expression-options.md).  
  
     O exemplo a seguir ilustra o comportamento diferente da classe de caracteres `.` por padrão e com a opção <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType>. A expressão regular `^.+` começa no início da cadeia de caracteres e corresponde a todos os caracteres. Por padrão, a correspondência termina no final da primeira linha; o padrão de expressão regular corresponde ao caractere de retorno de carro, `\r` ou \ u000D, mas não corresponde a `\n`. Como a opção <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> interpreta toda a cadeia de caracteres de entrada como uma única linha, ela corresponde a cada caractere na cadeia de caracteres de entrada, incluindo `\n`.  
  
     [!code-csharp[Conceptual.Regex.Language.CharacterClasses#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/any2.cs#5)]
     [!code-vb[Conceptual.Regex.Language.CharacterClasses#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/any2.vb#5)]  
  
> [!NOTE]
> Como ela corresponde a qualquer caractere, exceto `\n`, a classe de caracteres `.` também corresponde a `\r` (o caractere de retorno de carro, \u000D).  
  
- Em um grupo de caracteres positivos ou negativos, um ponto é tratado como um caractere de ponto literal e não como uma classe de caracteres. Para saber mais, confira [Grupo de caracteres positivos](#PositiveGroup) e [Grupo de caracteres negativos](#NegativeGroup) anteriormente neste tópico. O exemplo a seguir fornece uma ilustração ao definir uma expressão regular que inclui o caractere de ponto (`.`) como uma classe de caracteres e como um membro de um grupo de caracteres positivos. A expressão regular `\b.*[.?!;:](\s|\z)` começa em um limite de palavra, corresponde a qualquer caractere até encontrar um de cinco sinais de pontuação, incluindo um ponto, e corresponde a um caractere de espaço em branco ou ao final da cadeia de caracteres.  
  
     [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/any1.cs#4)]
     [!code-vb[Conceptual.RegEx.Language.CharacterClasses#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/any1.vb#4)]  
  
> [!NOTE]
> Como ele corresponde a qualquer caractere, o elemento de linguagem `.` é frequentemente usado com um quantificador lento se um padrão de expressão regular tenta corresponder a qualquer caractere várias vezes. Para saber mais, confira [Quantificadores](quantifiers-in-regular-expressions.md).  
  
<a name="CategoryOrBlock"></a>
## <a name="unicode-category-or-unicode-block-p"></a>Categoria de Unicode ou bloco Unicode: \p{}  
 O padrão Unicode atribui a cada caractere uma categoria geral. Por exemplo, um caractere específico pode ser uma letra maiúscula (representada pela categoria `Lu`), um dígito decimal (a categoria `Nd`), um símbolo matemático (a categoria `Sm`) ou um separador de parágrafo (a categoria `Zl`). Os conjuntos de caracteres específicos no padrão Unicode também ocupam um intervalo específico ou um bloco de pontos de código consecutivos. Por exemplo, o conjunto de caracteres latinos básico é de \u0000 a \u007F, enquanto que o conjunto de caracteres árabe vai de \u0600 a \u06FF.  
  
 A constructo da expressão regular  
  
 `\p{`*nome* do`}`  
  
 faz a correspondência de qualquer caractere que pertença a uma categoria geral Unicode ou a um bloco nomeado, em que *Name* é a abreviação da categoria ou o nome do bloco nomeado. Para obter uma lista de abreviações de categoria, consulte a seção [categorias gerais de Unicode com suporte](#SupportedUnicodeGeneralCategories) mais adiante neste tópico. Para obter uma lista de blocos nomeados, consulte a seção [blocos nomeados com suporte](#SupportedNamedBlocks) mais adiante neste tópico.  
  
 O exemplo a seguir usa a construção de `\p{` *nome* `}` para corresponder a uma categoria geral de Unicode (nesse caso, a `Pd` categoria, ou pontuação, travessão) e um bloco nomeado (o `IsGreek` e os `IsBasicLatin` blocos nomeados).  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/category1.cs#6)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/category1.vb#6)]  
  
 A expressão regular `\b(\p{IsGreek}+(\s)?)+\p{Pd}\s(\p{IsBasicLatin}+(\s)?)+` é definida conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`\p{IsGreek}+`|Corresponder a um ou mais caracteres gregos.|  
|`(\s)?`|Corresponder a zero ou a um caractere de espaço em branco.|  
|`(\p{IsGreek}+(\s)?)+`|Corresponder ao padrão de um ou mais caracteres gregos seguidos por zero ou um caractere de espaço em branco uma ou mais vezes.|  
|`\p{Pd}`|Corresponder a um caractere de pontuação, traço.|  
|`\s`|Corresponde a um caractere de espaço em branco.|  
|`\p{IsBasicLatin}+`|Corresponder a um ou mais caracteres latinos.|  
|`(\s)?`|Corresponder a zero ou a um caractere de espaço em branco.|  
|`(\p{IsBasicLatin}+(\s)?)+`|Corresponder ao padrão de um ou mais caracteres latinos básicos seguidos por zero ou um caractere de espaço em branco uma ou mais vezes.|  
  
<a name="NegativeCategoryOrBlock"></a>
## <a name="negative-unicode-category-or-unicode-block-p"></a>Categoria Unicode negativa ou bloco Unicode: \P{}  
 O padrão Unicode atribui a cada caractere uma categoria geral. Por exemplo, um caractere específico pode ser uma letra maiúscula (representada pela categoria `Lu`), um dígito decimal (a categoria `Nd`), um símbolo matemático (a categoria `Sm`) ou um separador de parágrafo (a categoria `Zl`). Os conjuntos de caracteres específicos no padrão Unicode também ocupam um intervalo específico ou um bloco de pontos de código consecutivos. Por exemplo, o conjunto de caracteres latinos básico é de \u0000 a \u007F, enquanto que o conjunto de caracteres árabe vai de \u0600 a \u06FF.  
  
 A constructo da expressão regular  
  
 `\P{`*nome* do`}`  
  
 corresponde a qualquer caractere que não pertença a uma categoria geral de Unicode ou a um bloco nomeado, em que *name* é a abreviação da categoria ou o nome do bloco nomeado. Para obter uma lista de abreviações de categoria, consulte a seção [categorias gerais de Unicode com suporte](#SupportedUnicodeGeneralCategories) mais adiante neste tópico. Para obter uma lista de blocos nomeados, consulte a seção [blocos nomeados com suporte](#SupportedNamedBlocks) mais adiante neste tópico.  
  
 O exemplo a seguir usa a construção de `\P{` *nome* `}` para remover qualquer símbolo de moeda (nesse caso, a `Sc` categoria, ou símbolo, moeda) das cadeias de caracteres numéricas.  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/notcategory1.cs#7)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/notcategory1.vb#7)]  
  
 O padrão de expressão regular `(\P{Sc})+` corresponde a um ou mais caracteres que não são símbolos de moeda; ele remove efetivamente qualquer símbolo de moeda da cadeia de caracteres de resultado.  
  
<a name="WordCharacter"></a>
## <a name="word-character-w"></a>Caractere de palavra: \w  
 `\w` corresponde a qualquer caractere de palavra. Um caractere de palavra é um membro de qualquer uma das categorias Unicode listadas na tabela a seguir.  
  
|Categoria|Description|  
|--------------|-----------------|  
|LI|Letra, minúscula|  
|Lu|Letra, maiúscula|  
|Lt|Letra, título|  
|Lo|Letra, outra|  
|Lm|Letra, modificador|  
|Mn|Marca, sem espaçamento|  
|Nd|Número, dígito decimal|  
|Pc|Pontuação, conector. Essa categoria inclui dez caracteres, o mais comumente usado deles é o LOWLINE (_), u+005F.|  
  
 Se o comportamento compatível com ECMAScript for especificado, `\w` será equivalente a `[a-zA-Z_0-9]`. Para obter informações sobre expressões regulares ECMAScript, consulte a seção "comportamento de correspondência ECMAScript" em [Opções de expressão regular](regular-expression-options.md).  
  
> [!NOTE]
> Como ele corresponde a qualquer caractere de palavra, o elemento de linguagem `\w` é frequentemente usado com um quantificador lento se um padrão de expressão regular tenta corresponder a qualquer caractere de palavra várias vezes, seguido por um caractere de palavra específico. Para saber mais, confira [Quantificadores](quantifiers-in-regular-expressions.md).  
  
 O exemplo a seguir usa o elemento de linguagem `\w` para corresponder a caracteres duplicados em uma palavra. O exemplo define um padrão de expressão regular, `(\w)\1`, que pode ser interpretado como a seguir.  
  
|Elemento|Description|  
|-------------|-----------------|  
|(\w)|Corresponder a um caractere de palavra. Este é o primeiro grupo de captura.|  
|\1|Corresponder ao valor da primeira captura.|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/wordchar1.cs#8)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/wordchar1.vb#8)]  
  
<a name="NonWordCharacter"></a>
## <a name="non-word-character-w"></a>Caractere não pertencente a palavras: \W  
 `\W` corresponde a qualquer caractere que não seja uma palavra. O elemento de linguagem \W é equivalente à seguinte classe de caracteres:  
  
`[^\p{Ll}\p{Lu}\p{Lt}\p{Lo}\p{Nd}\p{Pc}\p{Lm}]`  
  
 Em outras palavras, ele corresponde a qualquer caractere, com exceção dos listados nas categorias de Unicode na tabela a seguir.  
  
|Categoria|Descrição|  
|--------------|-----------------|  
|LI|Letra, minúscula|  
|Lu|Letra, maiúscula|  
|Lt|Letra, título|  
|Lo|Letra, outra|  
|Lm|Letra, modificador|  
|Mn|Marca, sem espaçamento|  
|Nd|Número, dígito decimal|  
|Pc|Pontuação, conector. Essa categoria inclui dez caracteres, o mais comumente usado deles é o LOWLINE (_), u+005F.|  
  
 Se o comportamento compatível com ECMAScript for especificado, `\W` será equivalente a `[^a-zA-Z_0-9]`. Para obter informações sobre expressões regulares ECMAScript, consulte a seção "comportamento de correspondência ECMAScript" em [Opções de expressão regular](regular-expression-options.md).  
  
> [!NOTE]
> Como ele corresponde a qualquer caractere não pertencente a palavras, o elemento de linguagem `\W` é frequentemente usado com um quantificador lento se um padrão de expressão regular tenta corresponder a qualquer caractere não pertencente a palavras várias vezes, seguido por um caractere não pertencente a palavras específico. Para saber mais, confira [Quantificadores](quantifiers-in-regular-expressions.md).  
  
 O exemplo a seguir ilustra a classe de caracteres `\W`.  Ele define um padrão de expressão regular, `\b(\w+)(\W){1,2}`, que corresponde a uma palavra seguida por um ou dois caracteres não pertencentes a palavras, como espaço em branco ou pontuação. A expressão regular é interpretada conforme mostrado na tabela a seguir.  
  
|Elemento|Description|  
|-------------|-----------------|  
|\b|Começar a correspondência em um limite de palavra.|  
|(\w+)|Fazer a correspondência a um ou mais caracteres de palavra. Este é o primeiro grupo de captura.|  
|(\W){1,2}|Corresponde a um caractere não pertencente a palavras uma ou duas vezes. Este é o segundo grupo de captura.|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/nonwordchar1.cs#9)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/nonwordchar1.vb#9)]  
  
 Como o objeto <xref:System.Text.RegularExpressions.Group> do segundo grupo de captura contém apenas um único caractere capturado não pertencente a palavras, o exemplo recupera todos os caracteres capturados não pertencentes a palavras do objeto <xref:System.Text.RegularExpressions.CaptureCollection> retornado pela propriedade <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType>.  
  
<a name="WhitespaceCharacter"></a>
## <a name="whitespace-character-s"></a>Caractere de espaço em branco: \s  
 `\s` corresponde a um caractere de espaço em branco. É equivalente às sequências de escape e às categorias de Unicode listadas na tabela a seguir.  
  
|Categoria|Descrição|  
|--------------|-----------------|  
|`\f`|O caractere de avanço de página, \u000C.|  
|`\n`|O caractere de nova linha, \u000A.|  
|`\r`|O caractere de retorno de carro, \u000D.|  
|`\t`|O caractere de tabulação, \u0009.|  
|`\v`|O caractere de tabulação vertical, \u000B.|  
|`\x85`|A elipse ou o caractere NEXT LINE (NEL) (...), \u0085.|  
|`\p{Z}`|Corresponde a qualquer caractere separador.|  
  
 Se o comportamento compatível com ECMAScript for especificado, `\s` será equivalente a `[ \f\n\r\t\v]`. Para obter informações sobre expressões regulares ECMAScript, consulte a seção "comportamento de correspondência ECMAScript" em [Opções de expressão regular](regular-expression-options.md).  
  
 O exemplo a seguir ilustra a classe de caracteres `\s`. Ele define um padrão de expressão regular, `\b\w+(e)?s(\s|$)`, que corresponde a uma palavra que termina em “s” ou em “es” seguida por um caractere de espaço em branco ou pelo final da cadeia de caracteres de entrada. A expressão regular é interpretada conforme mostrado na tabela a seguir.  
  
|Elemento|Description|  
|-------------|-----------------|  
|\b|Começar a correspondência em um limite de palavra.|  
|\w+|Fazer a correspondência a um ou mais caracteres de palavra.|  
|(e)?|Corresponder a um “e” zero ou uma vez.|  
|s|Corresponder a um “s”.|  
|(\s&#124;$)|Corresponder a um caractere de espaço em branco ou ao fim da cadeia de caracteres de entrada.|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/whitespace1.cs#10)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/whitespace1.vb#10)]  
  
<a name="NonWhitespaceCharacter"></a>
## <a name="non-whitespace-character-s"></a>Caractere diferente de espaço em branco: \S  
 `\S` corresponde a qualquer caractere que não seja um caractere de espaço em branco. Ele é equivalente ao padrão de expressão regular `[^\f\n\r\t\v\x85\p{Z}]` ou o oposto do padrão de expressão regular equivalente a `\s`, o qual corresponde a caracteres de espaço em branco. Para saber mais, confira [Caractere de espaço em branco: \s](#WhitespaceCharacter).  
  
 Se o comportamento compatível com ECMAScript for especificado, `\S` será equivalente a `[^ \f\n\r\t\v]`. Para obter informações sobre expressões regulares ECMAScript, consulte a seção "comportamento de correspondência ECMAScript" em [Opções de expressão regular](regular-expression-options.md).  
  
 O exemplo a seguir ilustra o elemento de linguagem `\S`. O padrão de expressão regular `\b(\S+)\s?` corresponde a cadeias de caracteres que são delimitadas por caracteres de espaço em branco. O segundo elemento no objeto <xref:System.Text.RegularExpressions.GroupCollection> da correspondência contém a cadeia de caracteres correspondida. A expressão regular pode ser interpretada conforme mostrado na tabela a seguir.  
  
|Elemento|Description|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`(\S+)`|Corresponder a um ou mais caracteres diferentes de espaço em branco. Este é o primeiro grupo de captura.|  
|`\s?`|Corresponder a zero ou a um caractere de espaço em branco.|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/nonwhitespace1.cs#11)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/nonwhitespace1.vb#11)]  
  
<a name="DigitCharacter"></a>
## <a name="decimal-digit-character-d"></a>Caractere de dígito decimal: \d  
 `\d` corresponde a qualquer dígito decimal. É equivalente ao padrão de expressão regular `\p{Nd}`, o qual inclui os dígitos decimais padrão 0-9, bem como os dígitos decimais de vários outros conjuntos de caracteres.  
  
 Se o comportamento compatível com ECMAScript for especificado, `\d` será equivalente a `[0-9]`. Para obter informações sobre expressões regulares ECMAScript, consulte a seção "comportamento de correspondência ECMAScript" em [Opções de expressão regular](regular-expression-options.md).  
  
 O exemplo a seguir ilustra o elemento de linguagem `\d`. Ele testa se uma cadeia de caracteres de entrada representa um número de telefone válido nos Estados Unidos e no Canadá. O padrão de expressão regular `^(\(?\d{3}\)?[\s-])?\d{3}-\d{4}$` é definido conforme mostrado na tabela a seguir.  
  
|Elemento|Description|  
|-------------|-----------------|  
|`^`|Começar a correspondência no início da cadeia de caracteres de entrada.|  
|`\(?`|Corresponder a zero ou a um caractere "(" literal.|  
|`\d{3}`|Corresponder a três dígitos decimais.|  
|`\)?`|Corresponder a zero ou a um caractere ")" literal.|  
|`[\s-]`|Corresponder a um hífen ou a caractere de espaço em branco.|  
|`(\(?\d{3}\)?[\s-])?`|Corresponder a um parêntese de abertura opcional seguido por três dígitos decimais, um parêntese de fechamento opcional e um caractere de espaço em branco ou um hífen zero ou uma vez. Este é o primeiro grupo de captura.|  
|`\d{3}-\d{4}`|Corresponder aos três dígitos decimais seguidos por um hífen e mais quatro dígitos decimais.|  
|`$`|Corresponder ao final da cadeia de caracteres de entrada.|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/digit1.cs#12)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/digit1.vb#12)]  
  
<a name="NonDigitCharacter"></a>
## <a name="non-digit-character-d"></a>Caractere que não seja dígito: \D  
 `\D` corresponde a qualquer caractere que não seja um dígito. É equivalente ao padrão de expressão regular `\P{Nd}`.  
  
 Se o comportamento compatível com ECMAScript for especificado, `\D` será equivalente a `[^0-9]`. Para obter informações sobre expressões regulares ECMAScript, consulte a seção "comportamento de correspondência ECMAScript" em [Opções de expressão regular](regular-expression-options.md).  
  
 O exemplo a seguir ilustra o elemento de linguagem \D. Ele testa se uma cadeia de caracteres, como um número de peça, consiste na combinação apropriada de caracteres decimais e não decimais. O padrão de expressão regular `^\D\d{1,5}\D*$` é definido conforme mostrado na tabela a seguir.  
  
|Elemento|Description|  
|-------------|-----------------|  
|`^`|Começar a correspondência no início da cadeia de caracteres de entrada.|  
|`\D`|Corresponder a um caractere que não seja um dígito.|  
|`\d{1,5}`|Corresponder a um a cinco dígitos decimais.|  
|`\D*`|Corresponder a zero ou a um ou mais caracteres não decimais.|  
|`$`|Corresponder ao final da cadeia de caracteres de entrada.|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/nondigit1.cs#13)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/nondigit1.vb#13)]  
  
<a name="SupportedUnicodeGeneralCategories"></a>
## <a name="supported-unicode-general-categories"></a>Categorias gerais Unicode com suporte  
 O Unicode define as categorias gerais listadas na tabela a seguir. Para obter mais informações, consulte os subtópicos "Formato de arquivo UCD" e "Valores de categoria geral" no [Banco de dados de caractere Unicode](https://www.unicode.org/reports/tr44/).  
  
|Categoria|Descrição|  
|--------------|-----------------|  
|`Lu`|Letra, maiúscula|  
|`Ll`|Letra, minúscula|  
|`Lt`|Letra, título|  
|`Lm`|Letra, modificador|  
|`Lo`|Letra, outra|  
|`L`|Todos os caracteres de letras. Isso inclui os caracteres `Lu`, `Ll`, `Lt`, `Lm` e `Lo`.|  
|`Mn`|Marca, sem espaçamento|  
|`Mc`|Marca, combinação de espaçamento|  
|`Me`|Marca, delimitador|  
|`M`|Todas as marcas diacríticas. Isso inclui as categorias `Mn`, `Mc` e `Me`.|  
|`Nd`|Número, dígito decimal|  
|`Nl`|Número, letra|  
|`No`|Número, outro|  
|`N`|Todos os números. Isso inclui as categorias `Nd`, `Nl` e `No`.|  
|`Pc`|Pontuação, conector|  
|`Pd`|Pontuação, traço|  
|`Ps`|Pontuação, abertura|  
|`Pe`|Pontuação, fechamento|  
|`Pi`|Pontuação, aspas iniciais (podem se comportar como Ps ou Pe, dependendo do uso)|  
|`Pf`|Pontuação, aspas finais (podem se comportar como Ps ou Pe, dependendo do uso)|  
|`Po`|Pontuação, outros|  
|`P`|Todos os caracteres de pontuação. Isso inclui as categorias `Pc`, `Pd`, `Ps`, `Pe`, `Pi`, `Pf` e `Po`.|  
|`Sm`|Símbolo, matemático|  
|`Sc`|Símbolo, moeda|  
|`Sk`|Símbolo, modificador|  
|`So`|Símbolo, outros|  
|`S`|Todos os símbolos. Isso inclui as categorias `Sm`, `Sc`, `Sk` e `So`.|  
|`Zs`|Separador, espaço|  
|`Zl`|Separador, linha|  
|`Zp`|Separador, parágrafo|  
|`Z`|Todos os caracteres de separador. Isso inclui as categorias `Zs`, `Zl` e `Zp`.|  
|`Cc`|Outros, controle|  
|`Cf`|Outros, formato|  
|`Cs`|Outros, substitutos|  
|`Co`|Outros, uso privado|  
|`Cn`|Outros, não atribuído (nenhum caractere tem esta propriedade)|  
|`C`|Todos os caracteres de controle. Isso inclui as categorias `Cc`, `Cf`, `Cs`, `Co` e `Cn`.|  
  
 Você pode determinar a categoria Unicode de qualquer caractere específico ao passar esse caractere para o método <xref:System.Char.GetUnicodeCategory%2A>. O exemplo a seguir usa o método <xref:System.Char.GetUnicodeCategory%2A> para determinar a categoria de cada elemento em uma matriz que contém caracteres latinos selecionados.  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/getunicodecategory1.cs#14)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/getunicodecategory1.vb#14)]  
  
<a name="SupportedNamedBlocks"></a>
## <a name="supported-named-blocks"></a>Blocos nomeados compatíveis

O .NET fornece os blocos nomeados listados na tabela a seguir. O conjunto de blocos nomeados com suporte baseia-se no Unicode 4.0 e no Perl 5.6. Para uma expressão regular que usa blocos nomeados, consulte a seção[Categoria Unicode ou bloco Unicode: \\p{}](#unicode-category-or-unicode-block-p).  
  
|Intervalo de ponto de código|Nome do bloco|  
|----------------------|----------------|  
|0000 - 007F|`IsBasicLatin`|  
|0080 - 00FF|`IsLatin-1Supplement`|  
|0100 - 017F|`IsLatinExtended-A`|  
|0180 - 024F|`IsLatinExtended-B`|  
|0250 - 02AF|`IsIPAExtensions`|  
|02B0 - 02FF|`IsSpacingModifierLetters`|  
|0300 - 036F|`IsCombiningDiacriticalMarks`|  
|0370 - 03FF|`IsGreek`<br /><br /> -ou-<br /><br /> `IsGreekandCoptic`|  
|0400 - 04FF|`IsCyrillic`|  
|0500 - 052F|`IsCyrillicSupplement`|  
|0530 - 058F|`IsArmenian`|  
|0590 - 05FF|`IsHebrew`|  
|0600 - 06FF|`IsArabic`|  
|0700 - 074F|`IsSyriac`|  
|0780 - 07BF|`IsThaana`|  
|0900 - 097F|`IsDevanagari`|  
|0980 - 09FF|`IsBengali`|  
|0A00 - 0A7F|`IsGurmukhi`|  
|0A80 - 0AFF|`IsGujarati`|  
|0B00 - 0B7F|`IsOriya`|  
|0B80 - 0BFF|`IsTamil`|  
|0C00 - 0C7F|`IsTelugu`|  
|0C80 - 0CFF|`IsKannada`|  
|0D00 - 0D7F|`IsMalayalam`|  
|0D80 - 0DFF|`IsSinhala`|  
|0E00 - 0E7F|`IsThai`|  
|0E80 - 0EFF|`IsLao`|  
|0F00 - 0FFF|`IsTibetan`|  
|1000 - 109F|`IsMyanmar`|  
|10A0 - 10FF|`IsGeorgian`|  
|1100 - 11FF|`IsHangulJamo`|  
|1200 - 137F|`IsEthiopic`|  
|13A0 - 13FF|`IsCherokee`|  
|1400 - 167F|`IsUnifiedCanadianAboriginalSyllabics`|  
|1680 - 169F|`IsOgham`|  
|16A0 - 16FF|`IsRunic`|  
|1700 - 171F|`IsTagalog`|  
|1720 - 173F|`IsHanunoo`|  
|1740 - 175F|`IsBuhid`|  
|1760 - 177F|`IsTagbanwa`|  
|1780 - 17FF|`IsKhmer`|  
|1800 - 18AF|`IsMongolian`|  
|1900 - 194F|`IsLimbu`|  
|1950 - 197F|`IsTaiLe`|  
|19E0 - 19FF|`IsKhmerSymbols`|  
|1D00 - 1D7F|`IsPhoneticExtensions`|  
|1E00 - 1EFF|`IsLatinExtendedAdditional`|  
|1F00 - 1FFF|`IsGreekExtended`|  
|2000 - 206F|`IsGeneralPunctuation`|  
|2070 - 209F|`IsSuperscriptsandSubscripts`|  
|20A0 - 20CF|`IsCurrencySymbols`|  
|20D0 - 20FF|`IsCombiningDiacriticalMarksforSymbols`<br /><br /> -ou-<br /><br /> `IsCombiningMarksforSymbols`|  
|2100 - 214F|`IsLetterlikeSymbols`|  
|2150 - 218F|`IsNumberForms`|  
|2190 - 21FF|`IsArrows`|  
|2200 - 22FF|`IsMathematicalOperators`|  
|2300 - 23FF|`IsMiscellaneousTechnical`|  
|2400 - 243F|`IsControlPictures`|  
|2440 - 245F|`IsOpticalCharacterRecognition`|  
|2460 - 24FF|`IsEnclosedAlphanumerics`|  
|2500 - 257F|`IsBoxDrawing`|  
|2580 - 259F|`IsBlockElements`|  
|25A0 - 25FF|`IsGeometricShapes`|  
|2600 - 26FF|`IsMiscellaneousSymbols`|  
|2700 - 27BF|`IsDingbats`|  
|27C0 - 27EF|`IsMiscellaneousMathematicalSymbols-A`|  
|27F0 - 27FF|`IsSupplementalArrows-A`|  
|2800 - 28FF|`IsBraillePatterns`|  
|2900 - 297F|`IsSupplementalArrows-B`|  
|2980 - 29FF|`IsMiscellaneousMathematicalSymbols-B`|  
|2A00 - 2AFF|`IsSupplementalMathematicalOperators`|  
|2B00 - 2BFF|`IsMiscellaneousSymbolsandArrows`|  
|2E80 - 2EFF|`IsCJKRadicalsSupplement`|  
|2F00 - 2FDF|`IsKangxiRadicals`|  
|2FF0 - 2FFF|`IsIdeographicDescriptionCharacters`|  
|3000 - 303F|`IsCJKSymbolsandPunctuation`|  
|3040 - 309F|`IsHiragana`|  
|30A0 - 30FF|`IsKatakana`|  
|3100 - 312F|`IsBopomofo`|  
|3130 - 318F|`IsHangulCompatibilityJamo`|  
|3190 - 319F|`IsKanbun`|  
|31A0 - 31BF|`IsBopomofoExtended`|  
|31F0 - 31FF|`IsKatakanaPhoneticExtensions`|  
|3200 - 32FF|`IsEnclosedCJKLettersandMonths`|  
|3300 - 33FF|`IsCJKCompatibility`|  
|3400 - 4DBF|`IsCJKUnifiedIdeographsExtensionA`|  
|4DC0 - 4DFF|`IsYijingHexagramSymbols`|  
|4E00 - 9FFF|`IsCJKUnifiedIdeographs`|  
|A000 - A48F|`IsYiSyllables`|  
|A490 - A4CF|`IsYiRadicals`|  
|AC00 - D7AF|`IsHangulSyllables`|  
|D800 - DB7F|`IsHighSurrogates`|  
|DB80 - DBFF|`IsHighPrivateUseSurrogates`|  
|DC00 - DFFF|`IsLowSurrogates`|  
|E000 - F8FF|`IsPrivateUse` ou `IsPrivateUseArea`|  
|F900 - FAFF|`IsCJKCompatibilityIdeographs`|  
|FB00 - FB4F|`IsAlphabeticPresentationForms`|  
|FB50 - FDFF|`IsArabicPresentationForms-A`|  
|FE00 - FE0F|`IsVariationSelectors`|  
|FE20 - FE2F|`IsCombiningHalfMarks`|  
|FE30 - FE4F|`IsCJKCompatibilityForms`|  
|FE50 - FE6F|`IsSmallFormVariants`|  
|FE70 - FEFF|`IsArabicPresentationForms-B`|  
|FF00 - FFEF|`IsHalfwidthandFullwidthForms`|  
|FFF0 - FFFF|`IsSpecials`|  
  
<a name="CharacterClassSubtraction"></a>
## <a name="character-class-subtraction-base_group---excluded_group"></a>Subtração de classes de caractere: [base_group - [excluded_group]]  
 Uma classe de caracteres define um conjunto de caracteres. A subtração de classes de caracteres fornece um conjunto de caracteres que é o resultado da exclusão dos caracteres em uma classe de caracteres em outra classe de caracteres.  
  
 A expressão de subtração de classes de caracteres tem a seguinte forma:  
  
 `[`*base_group* `-[` *excluded_group*`]]`  
  
 Os colchetes (`[]`) e hífen (`-`) são obrigatórios. O *base_group* é um [grupo de caracteres positivo](#PositiveGroup) ou um [grupo de caracteres negativo](#NegativeGroup). O componente *excluded_group* é outro grupo de caracteres positivos ou negativos ou outra expressão de subtração de classes de caracteres (ou seja, você pode aninhar expressões de subtração de classes de caracteres).  
  
 Por exemplo, suponha que você tenha um grupo base que consiste no intervalo de caracteres de "a" a "z". Para definir o conjunto de caracteres que consiste no grupo base, exceto pelo caractere "m", use `[a-z-[m]]`. Para definir o conjunto de caracteres que consiste no grupo base, exceto pelo conjunto de caracteres "d", "j" e "p", use `[a-z-[djp]]`. Para definir o conjunto de caracteres que consiste no base consiste, exceto pelo intervalo de "m" a "p", use `[a-z-[m-p]]`.  
  
 Considere a expressão de subtração de classes de caracteres aninhada, `[a-z-[d-w-[m-o]]]`. A expressão é calculada do intervalo de caracteres mais interno para fora. Primeiro, o intervalo de caracteres de "m" a "o" é subtraído do intervalo de caractere de "d" a "w", o que produz o conjunto de caracteres de "d" a "l" e "p" a "w". Esse conjunto é subtraído do intervalo de caracteres de "a" a "z", o que produz o conjunto de caracteres `[abcmnoxyz]`.  
  
 Você pode usar qualquer classe de caracteres com a subtração de classes de caracteres. Para definir o conjunto de caracteres que consiste em todos os caracteres Unicode de \u0000 a \uFFFF, exceto caracteres de espaço em branco (`\s`), os caracteres na categoria geral de pontuação (`\p{P}`), os caracteres no bloco nomeado `IsGreek` (`\p{IsGreek}`) e o caractere de controle Unicode NEXT LINE (\x85), utilize `[\u0000-\uFFFF-[\s\p{P}\p{IsGreek}\x85]]`.  
  
 Escolha classes de caracteres para uma expressão de subtração de classes de caracteres que produza resultados úteis. Evite uma expressão que gere um conjunto de caracteres vazio, que não podem corresponder a nada ou uma expressão equivalente ao grupo base original. Por exemplo, o conjunto vazio é o resultado da expressão `[\p{IsBasicLatin}-[\x00-\x7F]]`, que subtrai todos os caracteres do intervalo de caracteres `IsBasicLatin` da categoria geral `IsBasicLatin`. Da mesma forma, o grupo base original é o resultado da expressão `[a-z-[0-9]]`.  Isso ocorre, porque o grupo base, que é o intervalo de caracteres de letras de "a" a "z", não contém quaisquer caracteres no grupo excluído, que é o intervalo de caracteres de dígitos decimais de "0" a "9".  
  
 O exemplo a seguir define uma expressão regular, `^[0-9-[2468]]+$`, que corresponde aos dígitos zero e ímpares em uma cadeia de caracteres de entrada.  A expressão regular é interpretada conforme mostrado na tabela a seguir.  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|^|Começar a correspondência no início da cadeia de caracteres de entrada.|  
|`[0-9-[2468]]+`|Corresponder a uma ou mais ocorrências de qualquer caractere de 0 a 9, com exceção de 2, 4, 6 e 8. Em outras palavras, corresponder a uma ou mais ocorrências de zero ou de um dígito ímpar.|  
|$|Finalizar a correspondência no final da cadeia de caracteres de entrada.|  
  
 [!code-csharp[Conceptual.RegEx.Language.CharacterClasses#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/classsubtraction1.cs#15)]
 [!code-vb[Conceptual.RegEx.Language.CharacterClasses#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/classsubtraction1.vb#15)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Char.GetUnicodeCategory%2A>
- [Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)
- [Opções de expressão regular](regular-expression-options.md)
