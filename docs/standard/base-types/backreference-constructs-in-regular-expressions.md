---
title: Constructos de referência inversa em expressões regulares do .NET
description: Saiba como identificar elementos de texto repetidos por meio de constructos de referência inversa em uma expressão regular.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- backreferences
- constructs, backreference
- .NET Framework regular expressions, backreference constructs
- regular expressions, backreference constructs
ms.assetid: 567a4b8d-0e79-49dc-8df9-f4b1aa376a2a
ms.openlocfilehash: 87c3dbde2eb2b5a19b91f34bb2b088af5c0d1827
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290598"
---
# <a name="backreference-constructs-in-regular-expressions"></a>Construtores de referência inversa em expressões regulares

As referências inversas fornecem uma maneira conveniente de identificar um caractere ou subcadeia de caracteres repetida em uma cadeia de caracteres. Por exemplo, se a cadeia de caracteres de entrada contiver várias ocorrências de uma subcadeia de caracteres arbitrária, você poderá corresponder a primeira ocorrência a um grupo de captura e, em seguida, usar uma referência inversa para corresponder às ocorrências subsequentes da subcadeia de caracteres.

> [!NOTE]
> Uma sintaxe separada é usada para se referir a grupos de captura nomeados e numerados em cadeias de caracteres de substituição. Para saber mais, confira [Substituições](substitutions-in-regular-expressions.md).

O .NET define elementos de linguagem separados para se referir a grupos de captura nomeados e numerados. Para saber mais sobre captura de grupos, confira [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).

## <a name="numbered-backreferences"></a>Referências inversas numeradas

Uma referência inversa numerada usa a seguinte sintaxe:

`\` *número*

em que *number* é a posição ordinal do grupo de captura na expressão regular. Por exemplo, `\4` corresponde ao conteúdo do quarto grupo de captura. Se *number* não for definido no padrão da expressão regular, ocorrerá um erro de análise e o mecanismo de expressões regulares gerará um <xref:System.ArgumentException>. Por exemplo, a expressão regular `\b(\w+)\s\1` é válida porque `(\w+)` é o primeiro e único grupo de captura na expressão. Por outro lado, `\b(\w+)\s\2` é inválida e gera uma exceção de argumento porque não há nenhum grupo de captura com o número `\2`. Além disso, quando *number* identifica um grupo de captura em uma determinada posição ordinal, mas um nome numérico diferente da posição ordinal desse grupo de captura é atribuído a ele, o analisador de expressões regulares também gera um <xref:System.ArgumentException>.

Observe a ambiguidade entre códigos de escape octais (como `\16`) e as referências inversas `\`*number* que usam a mesma notação. Essa ambiguidade é resolvida da seguinte maneira:

- As expressões `\1` a `\9` sempre são interpretadas como referências inversas e não como códigos octais.

- Se o primeiro dígito de uma expressão de diversos for 8 ou 9 (como `\80` ou `\91`), a expressão será interpretada como uma literal.

- Expressões de `\10` e maiores serão consideradas referências inversas se houver uma referência inversa correspondente àquele número, caso contrário, elas serão interpretadas como códigos octais.

- Se uma expressão regular contiver uma referência inversa para um número de grupo indefinido, ocorrerá um erro de análise e o mecanismo de expressões regulares gerará um <xref:System.ArgumentException>.

Se a ambiguidade for um problema, você poderá usar a `\k<` *name* `>` notação de nome, que não é ambígua e não pode ser confundida com códigos de caracteres octais. Da mesma forma, os códigos hexadecimais como `\xdd` são não ambíguos e não podem ser confundidos com referências inversas.

O exemplo a seguir localiza caracteres de palavra duplicados em uma cadeia de caracteres. Ele define uma expressão regular, `(\w)\1`, que consiste nos elementos a seguir.

|Elemento|Description|
|-------------|-----------------|
|`(\w)`|Corresponde a um caractere de palavra e o atribui ao primeiro grupo de captura.|
|`\1`|Corresponde ao próximo caractere que é o mesmo que o valor do primeiro grupo de captura.|

[!code-csharp[RegularExpressions.Language.Backreferences#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference1.cs#1)]
[!code-vb[RegularExpressions.Language.Backreferences#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference1.vb#1)]

## <a name="named-backreferences"></a>Referências inversas nomeadas

Uma referência inversa nomeada é definida usando a sintaxe a seguir:

`\k<`*nome* do`>`

ou:

`\k'`*nome* do`'`

em que *name* é o nome de um grupo de captura definido no padrão da expressão regular. Se *name* não for definido no padrão da expressão regular, ocorrerá um erro de análise e o mecanismo de expressões regulares gerará um <xref:System.ArgumentException>.

O exemplo a seguir localiza caracteres de palavra duplicados em uma cadeia de caracteres. Ele define uma expressão regular, `(?<char>\w)\k<char>`, que consiste nos elementos a seguir.

|Elemento|Description|
|-------------|-----------------|
|`(?<char>\w)`|Corresponde a um caractere de palavra e o atribui a um grupo de captura chamado `char`.|
|`\k<char>`|Corresponde ao próximo caractere que é o mesmo que o valor do grupo de captura `char`.|

[!code-csharp[RegularExpressions.Language.Backreferences#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference2.cs#2)]
[!code-vb[RegularExpressions.Language.Backreferences#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference2.vb#2)]

## <a name="named-numeric-backreferences"></a>Referências inversas numéricas nomeadas

Em uma referência inversa nomeada com `\k`, *name* também pode ser a representação da cadeia de caracteres de um número. Por exemplo, o exemplo a seguir usa a expressão regular `(?<2>\w)\k<2>` para localizar caracteres de palavra duplicados em uma cadeia de caracteres. Nesse caso, o exemplo define um grupo de captura explicitamente nomeado como "2", e a referência inversa é correspondentemente denominada "2".

[!code-csharp[RegularExpressions.Language.Backreferences#3](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference3.cs#3)]
[!code-vb[RegularExpressions.Language.Backreferences#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference3.vb#3)]

Se *name* é a representação de cadeia de caracteres de um número e nenhum grupo de captura tem esse nome, `\k<`*name*`>` é o mesmo que o `\`*number* da referência inversa, em que *number* é a posição ordinal da captura. No exemplo a seguir, há um único grupo de captura nomeado `char`. O constructo de referência inversa se refere a ele como `\k<1>`. Conforme demonstrado pela saída do exemplo, a chamada para o <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> é bem-sucedida porque `char` é o primeiro grupo de captura.

[!code-csharp[Ordinal.Backreference](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference6.cs)]
[!code-vb[Ordinal.BackReference](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference6.vb)]

No entanto, se *name* é a representação de cadeia de caracteres de um número e um nome numérico foi explicitamente atribuído a um grupo de captura nessa posição, o analisador de expressão regular não pode identificar o grupo de captura por sua posição ordinal. Em vez disso, ele gera um <xref:System.ArgumentException> . O único grupo de captura no exemplo a seguir é denominado "2". Já que o constructo `\k` é usado para definir uma referência inversa denominada "1", o analisador de expressão regular não pode identificar o primeiro grupo de captura e gera uma exceção.

[!code-csharp[Ordinal.Backreference](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference7.cs)]
[!code-vb[Ordinal.BackReference](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference7.vb)]

## <a name="what-backreferences-match"></a>A que as referências inversas correspondem

Uma referência inversa refere-se à definição mais recente de um grupo (a definição mais imediatamente à esquerda, ao fazer a correspondência da esquerda para a direita). Quando um grupo faz várias capturas, uma referência inversa refere-se à captura mais recente.

O exemplo a seguir inclui um padrão de expressão regular, `(?<1>a)(?<1>\1b)*`, que redefine o grupo nomeado \1. A tabela a seguir descreve cada padrão na expressão regular.

|Padrão|Descrição|
|-------------|-----------------|
|`(?<1>a)`|Corresponde ao caractere "a" e atribui o resultado ao grupo de captura chamado `1`.|
|`(?<1>\1b)*`|Corresponda zero ou mais ocorrências ao grupo chamado `1` junto com "b" e atribua o resultado ao grupo de captura chamado `1`.|

[!code-csharp[RegularExpressions.Language.Backreferences#4](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference4.cs#4)]
[!code-vb[RegularExpressions.Language.Backreferences#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference4.vb#4)]

Comparando a expressão regular com a cadeia de caracteres de entrada ("aababb"), o mecanismo de expressões regulares realiza as seguintes operações:

1. Ele começa no início da cadeia de caracteres e corresponde com êxito o “a” com a expressão `(?<1>a)`. O valor do grupo `1` agora é "a".

2. Ele avança para o segundo caractere e corresponde com êxito a cadeia de caracteres "ab" com a expressão `\1b`, ou "ab". Em seguida, atribui o resultado, "ab", a `\1`.

3. Ele avança para o quarto caractere. A expressão `(?<1>\1b)*` deve ser correspondida zero ou mais vezes, de forma que corresponda com êxito a cadeia de caracteres “abb” à expressão `\1b`. Ela atribui o resultado, “abb”, a `\1`.

Neste exemplo, `*` é um quantificador looping – ele é avaliado repetidamente até que o mecanismo de expressões regulares não corresponda ao padrão definido. Os quantificadores de looping não limpam definições de grupo.

Se um grupo não capturou nenhuma subcadeia de caracteres, uma referência inversa a esse grupo é indefinida e nunca corresponde. Isso é ilustrado pelo padrão de expressão regular `\b(\p{Lu}{2})(\d{2})?(\p{Lu}{2})\b` que é definido da seguinte maneira:

|Padrão|Descrição|
|-------------|-----------------|
|`\b`|Começa a correspondência em um limite de palavra.|
|`(\p{Lu}{2})`|Corresponde a duas letras maiúsculas. Este é o primeiro grupo de captura.|
|`(\d{2})?`|Corresponde a zero ou uma ocorrência de dois dígitos decimais. Este é o segundo grupo de captura.|
|`(\p{Lu}{2})`|Corresponde a duas letras maiúsculas. Este é o terceiro grupo de captura.|
|`\b`|Termina a correspondência em um limite de palavra.|

Uma cadeia de caracteres de entrada pode corresponder a essa expressão regular, mesmo se os dois dígitos decimais que são definidos pelo segundo grupo de captura não estiverem presentes. O exemplo a seguir mostra que, embora a correspondência seja bem-sucedida, um grupo de captura vazio foi encontrado entre dois grupos de captura com êxito.

[!code-csharp[RegularExpressions.Language.Backreferences#5](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.backreferences/cs/backreference5.cs#5)]
[!code-vb[RegularExpressions.Language.Backreferences#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.backreferences/vb/backreference5.vb#5)]

## <a name="see-also"></a>Veja também

- [Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)
