---
title: Quantificadores em expressões regulares
description: Saiba mais sobre quantificadores de expressão regular, que especificam quantas instâncias de um caractere, grupo ou classe de caractere devem estar presentes na entrada para correspondência.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, quantifiers
- metacharacters, quantifiers
- minimal matching quantifiers
- quantifiers in regular expressions
- .NET Framework regular expressions, quantifiers
- quantifiers
- lazy quantifiers
ms.assetid: 36b81212-6511-49ed-a8f1-ff080415312f
ms.openlocfilehash: 3ffdd481ac001b4e1bd229c6f5fa0bf285b508b2
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063803"
---
# <a name="quantifiers-in-regular-expressions"></a>Quantificadores em expressões regulares
Os quantificadores especificam quantas instâncias de um caractere, grupo ou classe de caracteres devem estar presentes na entrada para encontrar uma correspondência.  A tabela a seguir lista os quantificadores tem suporte no .NET.  
  
|Quantificador Greedy|Quantificador lento|Descrição|  
|-----------------------|---------------------|-----------------|  
|`*`|`*?`|Corresponder a zero ou mais vezes.|  
|`+`|`+?`|Corresponder a um ou mais vezes.|  
|`?`|`??`|Corresponder a zero ou uma vez.|  
|`{`*n*`}`|`{`*n*`}?`|Corresponder exatamente *n* vezes.|  
|`{`*n*`,}`|`{`*n*`,}?`|Corresponder pelo menos *n* vezes.|  
|`{`*n* `,` *m*`}`|`{`*n* `,` *m*`}?`|Corresponder de *n* a *m* vezes.|  
  
 As quantidades `n` e `m` são constantes inteiras. Normalmente, os quantificadores são Greedy; eles fazem com que o mecanismo de expressões regulares corresponda a quantas ocorrências de padrões determinados forem possíveis. Acrescentar o caractere `?` a um quantificador o torna lento; faz com que o mecanismo de expressões regulares corresponda ao mínimo de ocorrências possíveis. Para obter uma descrição completa da diferença entre quantificadores Greedy e lentos, consulte a seção [Quantificadores Greedy e lentos](#Greedy) mais adiante neste tópico.  
  
> [!IMPORTANT]
> O aninhamento de quantificadores (por exemplo, como o padrão de expressão regular `(a*)*` faz) pode aumentar o número de comparações que o mecanismo de expressões regulares deve executar, como uma função exponencial do número de caracteres na cadeia de caracteres de entrada. Para saber mais sobre esse comportamento e suas soluções, veja [Retrocesso](backtracking-in-regular-expressions.md).  
  
## <a name="regular-expression-quantifiers"></a>Quantificadores em expressões regulares  
 As seções a seguir listam os quantificadores com suporte nas expressões regulares no .NET.  
  
> [!NOTE]
> Se os caracteres *, +, ?, { e } forem encontrados em um padrão de expressão regular, o mecanismo de expressões regulares vai interpretá-los como quantificadores ou parte de constructos de quantificador, exceto se estiverem incluídos em uma [classe de caracteres](character-classes-in-regular-expressions.md). Para interpretá-los como caracteres literais fora de uma classe de caracteres, você precisa fazer o escape, antecedendo-os com uma barra invertida. Por exemplo, a cadeia de caracteres `\*` em um padrão de expressão regular é interpretada como um caractere asterisco ("\*") integral.  
  
### <a name="match-zero-or-more-times-"></a>Corresponder a zero ou mais vezes: *  
 O quantificador `*` corresponde ao elemento anterior zero ou mais vezes. É equivalente ao quantificador `{0,}`. `*` é um quantificador Greedy, cujo equivalente lento é `*?`.  
  
 O exemplo a seguir ilustra essa expressão regular. Dos nove grupos de dígitos na cadeia de caracteres de entrada, cinco correspondem ao padrão e quatro ( `95` , `929` , `9219` e `9919` ) não.  
  
 [!code-csharp[RegularExpressions.Quantifiers#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#1)]
 [!code-vb[RegularExpressions.Quantifiers#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#1)]  
  
 O padrão de expressão regular é definido como mostra a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`91*`|Corresponder a “9” seguido por zero ou mais caracteres “1”.|  
|`9*`|Corresponder a zero ou mais caracteres “9”.|  
|`\b`|Terminar em um limite de palavra.|  
  
### <a name="match-one-or-more-times-"></a>Corresponder a um ou mais vezes: +  
 O quantificador `+` corresponde ao elemento anterior uma ou mais vezes. É equivalente a `{1,}`. `+` é um quantificador Greedy, cujo equivalente lento é `+?`.  
  
 Por exemplo, a expressão regular `\ban+\w*?\b` tenta corresponder a palavras inteiras que começam com a letra `a` seguidas por uma ou mais instâncias da letra `n`. O exemplo a seguir ilustra essa expressão regular. A expressão regular corresponde às palavras `an`, `annual`, `announcement` e `antique`, e não correspondem corretamente a `autumn` e `all`.  
  
 [!code-csharp[RegularExpressions.Quantifiers#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#2)]
 [!code-vb[RegularExpressions.Quantifiers#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#2)]  
  
 O padrão de expressão regular é definido como mostra a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`an+`|Corresponder a um “a” seguido por um ou mais caracteres “n”.|  
|`\w*?`|Corresponder a um caractere de palavra zero ou mais vezes, mas o menor número de vezes possível.|  
|`\b`|Terminar em um limite de palavra.|  
  
### <a name="match-zero-or-one-time-"></a>Corresponder a zero ou uma vez: ?  
 O quantificador `?` corresponde ao elemento anterior zero ou uma vez. É equivalente a `{0,1}`. `?` é um quantificador Greedy, cujo equivalente lento é `??`.  
  
 Por exemplo, a expressão regular `\ban?\b` tenta corresponder a palavras inteiras que começam com a letra `a` seguidas por zero ou uma instância da letra `n`. Em outras palavras, ele tenta corresponder às palavras `a` e `an`. O exemplo a seguir ilustra essa expressão regular.  
  
 [!code-csharp[RegularExpressions.Quantifiers#3](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#3)]
 [!code-vb[RegularExpressions.Quantifiers#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#3)]  
  
 O padrão de expressão regular é definido como mostra a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`an?`|Corresponder a um “a” seguido por zero ou um caractere “n”.|  
|`\b`|Terminar em um limite de palavra.|  
  
### <a name="match-exactly-n-times-n"></a>Corresponder exatamente a n vezes: {n}  
 O `{` *n* `}` quantificador n corresponde ao elemento anterior exatamente *n* vezes, em que *n* é qualquer inteiro. `{`*n* `}` é um quantificador de ávido cujo equivalente lento é `{` *n* `}?` .  
  
 Por exemplo, a expressão regular `\b\d+\,\d{3}\b` tenta corresponder a um limite de palavra seguido por um ou mais dígitos decimais seguidos por três dígitos decimais seguidos por um limite de palavra. O exemplo a seguir ilustra essa expressão regular.  
  
 [!code-csharp[RegularExpressions.Quantifiers#4](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#4)]
 [!code-vb[RegularExpressions.Quantifiers#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#4)]  
  
 O padrão de expressão regular é definido como mostra a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`\d+`|Corresponde a um ou mais dígitos decimais.|  
|`\,`|Corresponder a um caractere de vírgula.|  
|`\d{3}`|Corresponder a três dígitos decimais.|  
|`\b`|Terminar em um limite de palavra.|  
  
### <a name="match-at-least-n-times-n"></a>Corresponder a pelo menos n vezes: {n,}  
 O `{` *n* `,}` quantificador n corresponde ao elemento anterior pelo menos *n* vezes, em que *n* é qualquer inteiro. `{`*n* `,}` é um quantificador de ávido cujo equivalente lento é `{` *n* `,}?` .  
  
 Por exemplo, a expressão regular `\b\d{2,}\b\D+` tenta corresponder a um limite de palavra seguido por pelo menos dois dígitos seguidos por um limite de palavra e um caractere não dígito. O exemplo a seguir ilustra essa expressão regular. A expressão regular não corresponde à frase `"7 days"` porque contém apenas um dígito decimal, mas corresponde com êxito às frases `"10 weeks and 300 years"`.  
  
 [!code-csharp[RegularExpressions.Quantifiers#5](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#5)]
 [!code-vb[RegularExpressions.Quantifiers#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#5)]  
  
 O padrão de expressão regular é definido como mostra a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`\d{2,}`|Corresponder a pelo menos dois dígitos decimais.|  
|`\b`|Corresponder a um limite de palavra.|  
|`\D+`|Corresponder a pelo menos uma casa não decimal.|  
  
### <a name="match-between-n-and-m-times-nm"></a>Corresponder entre n e m vezes: {n,m}  
 O `{` *n* `,` *m* `}` quantificador n m corresponde ao elemento anterior pelo menos *n* vezes, mas não mais que *m* vezes, em que *n* e *m* são inteiros. `{`*n* `,` *m* `}` é um quantificador de ávido cujo equivalente lento é `{` *n* `,` *m* `}?` .  
  
 No exemplo a seguir, a expressão regular `(00\s){2,4}` tenta corresponder a entre duas e quatro ocorrências de dois dígitos zero seguidos por um espaço. Observe que a parte final da cadeia de caracteres de entrada inclui esse padrão de cinco vezes em vez de no máximo quatro. No entanto, apenas a parte inicial dessa subcadeia de caracteres (até o espaço e o quinto par de zeros) corresponde ao padrão de expressão regular.  
  
 [!code-csharp[RegularExpressions.Quantifiers#6](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#6)]
 [!code-vb[RegularExpressions.Quantifiers#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#6)]  
  
### <a name="match-zero-or-more-times-lazy-match-"></a>Corresponder a zero ou mais vezes (correspondência lenta): *?  
 O quantificador `*?` corresponde ao elemento anterior zero ou mais vezes, mas o menor número de vezes possível. É a contraparte lenta do quantificador Greedy `*`.  
  
 No exemplo a seguir, a expressão regular `\b\w*?oo\w*?\b` corresponde a todas as palavras que contêm a cadeia de caracteres `oo`.  
  
 [!code-csharp[RegularExpressions.Quantifiers#7](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#7)]
 [!code-vb[RegularExpressions.Quantifiers#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#7)]  
  
 O padrão de expressão regular é definido como mostra a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`\w*?`|Corresponder a zero ou mais caracteres de palavra, mas o menor número de caracteres possível.|  
|`oo`|Corresponder à cadeia de caracteres “oo”.|  
|`\w*?`|Corresponder a zero ou mais caracteres de palavra, mas o menor número de caracteres possível.|  
|`\b`|Terminar em um limite de palavra.|  
  
### <a name="match-one-or-more-times-lazy-match-"></a>Corresponder a uma ou mais vezes (correspondência lenta): +?  
 O quantificador `+?` corresponde ao elemento anterior uma ou mais vezes, mas o menor número de vezes possível. É a contraparte lenta do quantificador Greedy `+`.  
  
 Por exemplo, a expressão regular `\b\w+?\b` corresponde a um ou mais caracteres separados por limites de palavra. O exemplo a seguir ilustra essa expressão regular.  
  
 [!code-csharp[RegularExpressions.Quantifiers#8](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#8)]
 [!code-vb[RegularExpressions.Quantifiers#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#8)]  
  
### <a name="match-zero-or-one-time-lazy-match-"></a>Corresponder a zero ou uma vez (correspondência lenta): ??  
 O quantificador `??` corresponde ao elemento anterior zero ou uma vez, mas o menor número de vezes possível. É a contraparte lenta do quantificador Greedy `?`.  
  
 Por exemplo, a expressão regular `^\s*(System.)??Console.Write(Line)??\(??` tenta corresponder às cadeias de caracteres “Console.Write” ou “Console.WriteLine”. A cadeia de caracteres também pode incluir "System." antes de “Console” e pode ser seguida por um parêntese de abertura. A cadeia de caracteres deve estar no início de uma linha, embora possa ser antecedida por espaço em branco. O exemplo a seguir ilustra essa expressão regular.  
  
 [!code-csharp[RegularExpressions.Quantifiers#9](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#9)]
 [!code-vb[RegularExpressions.Quantifiers#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#9)]  
  
 O padrão de expressão regular é definido como mostra a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`^`|Corresponder ao início do fluxo de entrada.|  
|`\s*`|Corresponder a zero ou mais caracteres de espaço em branco.|  
|`(System.)??`|Corresponde a zero ou uma ocorrência da cadeia de caracteres “System.”.|  
|`Console.Write`|Corresponder à cadeia de caracteres “Console.Write”.|  
|`(Line)??`|Corresponde a zero ou uma ocorrência da cadeia de caracteres “Line”.|  
|`\(??`|Corresponder a zero ou uma ocorrência do parêntese de abertura.|  
  
### <a name="match-exactly-n-times-lazy-match-n"></a>Corresponder exatamente a n vezes (correspondência lenta): {n}?  
 O `{` *n* `}?` quantificador n corresponde ao elemento anterior exatamente `n` vezes, em que *n* é qualquer inteiro. É a contraparte lenta do quantificador de ávido `{` *n* `}` .  
  
 No exemplo a seguir, a expressão regular `\b(\w{3,}?\.){2}?\w{3,}?\b` é usada para identificar um endereço de site. Observe que corresponde a “www.microsoft.com” e “msdn.microsoft.com”, mas não corresponde a “mywebsite” ou “mycompany.com”.  
  
 [!code-csharp[RegularExpressions.Quantifiers#10](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#10)]
 [!code-vb[RegularExpressions.Quantifiers#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#10)]  
  
 O padrão de expressão regular é definido como mostra a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`(\w{3,}?\.)`|Corresponder a pelo menos três caracteres de palavra, mas o menor número de caracteres possível, seguido por um caractere de ponto. Este é o primeiro grupo de captura.|  
|`(\w{3,}?\.){2}?`|Corresponder ao padrão no primeiro grupo duas vezes, mas o menor número de vezes possível.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
  
### <a name="match-at-least-n-times-lazy-match-n"></a>Corresponder a pelo menos n vezes (correspondência lenta): {n,}?  
 O `{` *n* `,}?` quantificador n corresponde ao elemento anterior pelo menos `n` vezes, em que *n* é qualquer inteiro, mas o mínimo de vezes possível. É a contraparte lenta do quantificador de ávido `{` *n* `,}` .  
  
 Consulte o exemplo do `{` *n* `}?` quantificador n na seção anterior para obter uma ilustração. A expressão regular nesse exemplo usa o `{` *n* `,}` quantificador n para corresponder uma cadeia de caracteres que tenha pelo menos três caracteres seguidos por um ponto.  
  
### <a name="match-between-n-and-m-times-lazy-match-nm"></a>Corresponder entre n e m vezes (correspondência lenta): {n,m}?  
 O `{` *n* `,` *m* `}?` quantificador n m corresponde ao elemento anterior entre `n` e `m` horas, em que *n* e *m* são inteiros, mas tantas vezes quanto possível. É a contraparte lenta do quantificador de ávido `{` *n* `,` *m* `}` .  
  
 No exemplo a seguir, a expressão regular `\b[A-Z](\w*?\s*?){1,10}[.!?]` corresponde a frases que contêm entre uma e dez palavras. Corresponde a todas as frases na cadeia de caracteres de entrada, exceto por uma frase que contém 18 palavras.  
  
 [!code-csharp[RegularExpressions.Quantifiers#12](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#12)]
 [!code-vb[RegularExpressions.Quantifiers#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#12)]  
  
 O padrão de expressão regular é definido como mostra a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Iniciar em um limite de palavra.|  
|`[A-Z]`|Corresponder a um caractere maiúscula de A a Z.|  
|`(\w*?\s*?)`|Corresponder a zero ou mais caracteres de palavra, seguidos por um ou mais caracteres de espaço em branco, mas o menor número de vezes possível. Este é o primeiro grupo de captura.|  
|`{1,10}`|Corresponder ao padrão anterior entre 1 e 10 vezes.|  
|`[.!?]`|Corresponder a qualquer um dos caracteres de pontuação “.”, “!” ou “?”.|  
  
<a name="Greedy"></a>
## <a name="greedy-and-lazy-quantifiers"></a>Quantificadores Greedy e lentos  
 Alguns quantificadores têm duas versões:  
  
- Uma versão Greedy.  
  
     Um quantificador Greedy tenta corresponder a um elemento tantas vezes quanto possível.  
  
- Uma versão não Greedy (ou lenta).  
  
     Um quantificador não Greedy tenta corresponder a um elemento o menor número de vezes possível. Você pode transformar um quantificador Greedy em um quantificador lento simplesmente adicionando um `?`.  
  
 Considere uma expressão regular simples que se destina a extrair os últimos quatro dígitos de uma cadeia de caracteres de números, como um número de cartão de crédito. A versão da expressão regular que usa o quantificador Greedy `*` é `\b.*([0-9]{4})\b`. No entanto, se uma cadeia de caracteres contiver dois números, essa expressão regular corresponde aos últimos quatro dígitos do segundo número, como mostra o exemplo a seguir.  
  
 [!code-csharp[RegularExpressions.Quantifiers.Greedy#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/cs/Greedy.cs#1)]
 [!code-vb[RegularExpressions.Quantifiers.Greedy#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/vb/Greedy.vb#1)]  
  
 A expressão regular não corresponde ao primeiro número porque o quantificador `*` tenta corresponder ao elemento anterior tantas vezes quanto possível em toda a cadeia de caracteres e encontra sua correspondência no final da cadeia de caracteres.  
  
 Esse não é o comportamento desejado. Em vez disso, é possível usar o quantificador lento `*?` para extrair dígitos de ambos os números, como mostra o exemplo a seguir.  
  
 [!code-csharp[RegularExpressions.Quantifiers.Greedy#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/cs/Greedy.cs#2)]
 [!code-vb[RegularExpressions.Quantifiers.Greedy#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/vb/Greedy.vb#2)]  
  
 Na maioria dos casos, expressões regulares com quantificadores Greedy e lentos retornam as mesmas correspondências. Geralmente retornam resultados diferentes quando são usadas com o metacaractere curinga (`.`), que corresponde a qualquer caractere.  
  
## <a name="quantifiers-and-empty-matches"></a>Quantificadores e correspondências vazias  
 Os quantificadores `*` , `+` e `{` *n* `,` *m* `}` e suas contrapartes lentas nunca se repetirão após uma correspondência vazia quando o número mínimo de capturas for encontrado. Essa regra impede que quantificadores entrem em loops infinitos em correspondências vazias de subexpressão quando o número máximo de capturas de grupo possíveis é infinito ou quase infinito.  
  
 Por exemplo, o código a seguir mostra o resultado de uma chamada para o método <xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType> com o padrão de expressão regular `(a?)*` que corresponde a zero ou a um caractere "a" zero ou mais vezes. Observe que o grupo de captura único captura cada “a” bem como <xref:System.String.Empty?displayProperty=nameWithType>, mas que não há uma segunda correspondência vazia, porque a primeira correspondência vazia faz com que o quantificador pare de se repetir.  
  
 [!code-csharp[RegularExpressions.Quantifiers.EmptyMatch#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/cs/emptymatch1.cs#1)]
 [!code-vb[RegularExpressions.Quantifiers.EmptyMatch#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/vb/emptymatch1.vb#1)]  
  
 Para ver a diferença prática entre um grupo de captura que define um número mínimo e máximo de captura e um que define um número fixo de capturas, considere os padrões de expressão regular `(a\1|(?(1)\1)){0,2}` e `(a\1|(?(1)\1)){2}`. Ambas as expressões regulares consistem em um único grupo de captura, que é definido como mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`(a\1`|Faça qualquer correspondência a “a” juntamente com o valor do primeiro grupo capturado…|  
|<code>&#124;(?(1)</code>|… ou teste se o primeiro grupo capturado foi definido. (Observe que o constructo `(?(1)` não define um grupo de captura).|  
|`\1))`|Se o primeiro grupo capturado existir, faça uma correspondência ao valor. Se o grupo não existir, será correspondente a <xref:System.String.Empty?displayProperty=nameWithType>.|  
  
 A primeira expressão regular tenta corresponder a esse padrão entre zero e duas vezes; a segunda, exatamente duas vezes. Como o primeiro padrão atinge o número mínimo de capturas com a primeira captura de <xref:System.String.Empty?displayProperty=nameWithType>, ele nunca se repete para tentar corresponder a `a\1`; o quantificador `{0,2}` permite apenas correspondências vazias na última iteração. Por outro lado, a segunda expressão regular corresponde a “a” porque avalia `a\1` uma segunda vez; o número mínimo de iterações, 2, força o mecanismo a se repetir após uma correspondência vazia.  
  
 [!code-csharp[RegularExpressions.Quantifiers.EmptyMatch#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/cs/emptymatch4.cs#2)]
 [!code-vb[RegularExpressions.Quantifiers.EmptyMatch#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/vb/emptymatch4.vb#2)]  
  
## <a name="see-also"></a>Consulte também

- [Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)
- [Retrocesso](backtracking-in-regular-expressions.md)
