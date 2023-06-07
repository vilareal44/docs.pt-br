---
title: Comportamento de expressão regular
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, behavior
- .NET Framework regular expressions, behavior
ms.assetid: 0ee1a6b8-caac-41d2-917f-d35570021b10
ms.openlocfilehash: 802c84bf93b3821459ab652e69a12fcc50280b9e
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290546"
---
# <a name="details-of-regular-expression-behavior"></a>Detalhes do comportamento de expressões regulares

O mecanismo de expressões regulares do .NET Framework é um correspondente de expressão regular de retrocesso que incorpora um mecanismo de NFA (Automação Finita Não Determinística) tradicional, como o usado pelo Perl, Python, Emacs e Tcl. Isso o distingue de mecanismos de DFA (Autômato finito determinístico) de expressões regulares puras mais rápidos, porém mais limitados, como os encontrados em awk, egrep ou lex. Também o distingue de NFAs POSIX padronizados, porém mais lentos. A seção a seguir descreve os três tipos de mecanismos de expressões regulares e explica por que as expressões regulares no .NET Framework são implementadas usando um mecanismo de NFA tradicional.

## <a name="benefits-of-the-nfa-engine"></a>Benefícios do mecanismo NFA

 Quando mecanismos de DFA executam a correspondência de padrões, a ordem de processamento é orientada pela cadeia de caracteres de entrada. O mecanismo começa no início da cadeia de caracteres de entrada e continua sequencialmente para determinar se o próximo caractere corresponde ao padrão de expressão regular. Podem assegurar uma correspondência com a cadeia de caracteres mais longa possível. Como nunca testam o mesmo caractere duas vezes, os mecanismos de DFA não dão suporte ao retrocesso. No entanto, como um mecanismo de DFA contém somente o estado finito, não pode corresponder a um padrão com referências inversas; além disso, uma vez que não constrói uma expansão explícita, não pode capturar subexpressões.

 Ao contrário de mecanismos de DFA, quando mecanismos de NFA tradicionais executam a correspondência de padrões, a ordem de processamento é orientada pelo padrão de expressão regular. Como processa um elemento de linguagem específico, o mecanismo usa a correspondência Greedy; ou seja, corresponde à maior parte possível da cadeia de caracteres de entrada. Contudo, também consegue salvar seu estado correspondendo a uma subexpressão. Se uma correspondência falhar, o mecanismo poderá retornar para um estado salvo a fim de tentar correspondências adicionais. Esse processo de abandonar uma correspondência de subexpressão bem-sucedida para que elementos de linguagem posteriores na expressão regular também possam corresponder é conhecido como *retrocesso*. Os mecanismos de NFA usam o retrocesso para testar todas as possíveis expansões de uma expressão regular em uma ordem específica e aceitam a primeira correspondência. Como um mecanismo de NFA tradicional constrói uma expansão específica da expressão regular para uma correspondência de sucesso, pode capturar correspondências de subexpressões e referências inversas correspondentes. Entretanto, como um NFA tradicional retrocede, pode visitar o mesmo estado diversas vezes se chegar no estado por diferentes caminhos. Como resultado, pode executar de modo exponencial lentamente na pior das hipóteses. Já que um mecanismo de NFA tradicional aceita a primeira correspondência que encontra, também pode deixar outras correspondências (possivelmente mais longas) não descobertas.

 Os mecanismos de NFA POSIX são como mecanismos de NFA tradicionais, exceto pelo fato de continuarem retrocedendo até poderem assegurar que encontraram a correspondência mais longa possível. Como resultado, um mecanismo de NFA POSIX é mais lento do que um mecanismo de NFA tradicional; quando você usa um mecanismo de NFA POSIX, não pode favorecer uma correspondência menor em detrimento de uma maior alterando a ordem da pesquisa de retrocesso.

 Os mecanismos de NFA tradicionais são favorecidos por programadores porque oferecem maior controle sobre a correspondência da cadeia de caracteres do que mecanismos de DFA ou NFA POSIX. Embora, na pior das hipóteses, possam ser executados mais lentamente, você pode orientá-los para encontrar correspondências em tempo linear ou polinomial usando padrões que reduzem ambiguidades e limitam o retrocesso. Em outras palavras, embora os mecanismos de NFA negociem o desempenho de energia e flexibilidade, na maioria dos casos eles oferecem bom desempenho aceitável se uma expressão regular é bem escrita e evita casos em que o retrocesso degrada o desempenho exponencialmente.

> [!NOTE]
> Para obter informações sobre a penalidade de desempenho causada por retrocesso excessivo e maneiras de criar uma expressão regular para solucioná-lo, consulte [Retrocesso](backtracking-in-regular-expressions.md).

## <a name="net-framework-engine-capabilities"></a>Recursos do mecanismo de .NET Framework

 Para tirar proveito dos benefícios de um mecanismo de NFA tradicional, o mecanismo de expressões regulares do .NET Framework inclui um conjunto completo de constructos para permitir que os programadores conduzam o mecanismo de retrocesso. Tais constructos podem ser usados para encontrar correspondências mais rapidamente ou favorecer expansões específicas em detrimento de outras.

 Outros recursos do mecanismo de expressões regulares do .NET Framework incluem o seguinte:

- Quantificadores lentos: `??` , `*?` , `+?` , `{` *n* `,` *m* `}?` . Esses constructos instruem o mecanismo de retrocesso a pesquisar o número mínimo de repetições primeiro. Por outro lado, quantificadores Greedy comuns tentam corresponder ao número máximo de repetições primeiro. O exemplo a seguir mostra a diferença entre os dois. Uma expressão regular corresponde a uma frase que termina em um número e um grupo de captura deve extrair esse número. A expressão regular `.+(\d+)\.` inclui o quantificador Greedy `.+`, que faz com que o mecanismo de expressões regulares capture o último dígito do número. Por outro lado, a expressão regular `.+?(\d+)\.` inclui o quantificador lento `.+?`, que faz com que o mecanismo de expressões regulares capture o número inteiro.

     [!code-csharp[Conceptual.RegularExpressions.Design#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lazy1.cs#1)]
     [!code-vb[Conceptual.RegularExpressions.Design#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lazy1.vb#1)]

     As versões Greedy e lenta dessa expressão regular são definidas como mostrado na tabela a seguir:

    |Padrão|Descrição|
    |-------------|-----------------|
    |`.+` (quantificador Greedy)|Corresponder a pelo menos uma ocorrência de qualquer caractere. Isso faz com que o mecanismo de expressões regulares corresponda à cadeia de caracteres inteira e, em seguida, retroceda da forma necessária para corresponder ao restante do padrão.|
    |`.+?` (quantificador lento)|Corresponder a pelo menos uma ocorrência de qualquer caractere, mas ao menor número possível.|
    |`(\d+)`|Corresponder a pelo menos um caractere numérico e atribuí-lo ao primeiro grupo de captura.|
    |`\.`|Corresponde a um ponto final.|

     Para obter mais informações sobre quantificadores lentos, confira [Quantificadores](quantifiers-in-regular-expressions.md).

- Lookahead positivo: `(?=` *subexpressão* `)` . Esse recurso permite que o mecanismo de retrocesso retorne ao mesmo ponto no texto após corresponder a uma subexpressão. É útil para pesquisar em todo o texto verificando vários padrões que iniciam na mesma posição. Também permite que o mecanismo verifique se existe uma subcadeia de caracteres no final da correspondência sem incluir a subcadeia de caracteres no texto correspondente. O exemplo a seguir usa lookahead positivo para extrair as palavras de uma frase que não são seguidas por símbolos de pontuação.

     [!code-csharp[Conceptual.RegularExpressions.Design#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookahead1.cs#2)]
     [!code-vb[Conceptual.RegularExpressions.Design#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookahead1.vb#2)]

     A expressão regular `\b[A-Z]+\b(?=\P{P})` é definida conforme mostrado na tabela a seguir.

    |Padrão|Descrição|
    |-------------|-----------------|
    |`\b`|Começar a correspondência em um limite de palavra.|
    |`[A-Z]+`|Corresponder a qualquer caractere alfabético uma ou mais vezes. Como o método <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> é chamado com a opção <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType>, essa comparação não diferencia maiúsculas de minúsculas.|
    |`\b`|Termina a correspondência em um limite de palavra.|
    |`(?=\P{P})`|Antecipe para determinar se o próximo caractere é um símbolo de pontuação. Se não for, a correspondência será bem-sucedida.|

     Para obter mais informações sobre as asserções de lookahead positivo, consulte [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).

- Visão antecipada negativa: `(?!` *subexpressão* `)` . Esse recurso adiciona a capacidade de corresponder a uma expressão somente se uma subexpressão não corresponder. Isso é poderoso para remover uma pesquisa, pois geralmente é mais simples fornecer uma expressão para um caso que deve ser eliminado do que uma expressão para casos que devem ser incluídos. Por exemplo, é difícil escrever uma expressão para palavras que não começam com “non”. O exemplo a seguir usa lookahead negativo para excluir.

     [!code-csharp[Conceptual.RegularExpressions.Design#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookahead2.cs#3)]
     [!code-vb[Conceptual.RegularExpressions.Design#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookahead2.vb#3)]

     O padrão de expressão regular `\b(?!non)\w+\b` é definido conforme mostrado na tabela a seguir.

    |Padrão|Descrição|
    |-------------|-----------------|
    |`\b`|Começar a correspondência em um limite de palavra.|
    |`(?!non)`|Antecipar para garantir que a cadeia de caracteres atual não comece com “non”. Se isso acontecer, a correspondência falha.|
    |`(\w+)`|Fazer a correspondência a um ou mais caracteres de palavra.|
    |`\b`|Termina a correspondência em um limite de palavra.|

     Para obter mais informações sobre as asserções de lookahead negativo, consulte [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).

- Avaliação condicional: `(?(` *expressão* `)` *Sim* `|` *não* `)` e `(?(` *nome* `)` *Sim* `|` *não* `)` , em que *expressão* é uma subexpressão para corresponder, *nome* é o nome de um grupo de captura, *Sim* é a cadeia de caracteres a corresponder se *name* a *expressão* for correspondida ou se o nome for um grupo capturado válido, não vazio e *não* for a subexpressão a corresponder se a *expressão* não for correspondida ou o *nome* não for um grupo capturado válido Esse recurso permite que o mecanismo pesquise usando mais de um padrão alternativo, dependendo do resultado de uma correspondência de subexpressão anterior ou do resultado de uma asserção de largura zero. Isso possibilita uma forma mais potente de referência inversa que permite, por exemplo, corresponder a uma subexpressão com base no fato de uma subexpressão anterior ser correspondente. A expressão regular no exemplo a seguir corresponde a parágrafos que são destinados a uso público e interno. Os parágrafos destinados apenas a uso interno começam com uma marca `<PRIVATE>`. O padrão de expressão regular `^(?<Pvt>\<PRIVATE\>\s)?(?(Pvt)((\w+\p{P}?\s)+)|((\w+\p{P}?\s)+))\r?$` usa avaliação condicional para atribuir o conteúdo de parágrafos destinados a uso público e interno a grupos de captura separados. Esses parágrafos podem ser tratados de maneiras diferentes.

     [!code-csharp[Conceptual.RegularExpressions.Design#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/conditional1.cs#4)]
     [!code-vb[Conceptual.RegularExpressions.Design#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/conditional1.vb#4)]

     O padrão de expressão regular é definido como mostra a tabela a seguir.

    |Padrão|Descrição|
    |-------------|-----------------|
    |`^`|Começar a correspondência no início de uma linha.|
    |`(?<Pvt>\<PRIVATE\>\s)?`|Corresponder a zero ou uma ocorrência da cadeia de caracteres `<PRIVATE>` seguida para um caractere de espaço em branco. Atribuir a correspondência a um grupo de captura chamado `Pvt`.|
    |`(?(Pvt)((\w+\p{P}?\s)+)`|Se o grupo de captura `Pvt` existir, corresponder a uma ou mais ocorrências de um ou mais caracteres de palavra seguidos por zero ou um separador de pontuação, seguido por um caractere de espaço em branco. Atribuir a subcadeia de caracteres ao primeiro grupo de captura.|
    |<code>&#124;((\w+\p{P}?\s)+))</code>|Se o grupo de captura `Pvt` não existir, corresponder a uma ou mais ocorrências de um ou mais caracteres de palavra seguidos por zero ou um separador de pontuação, seguido por um caractere de espaço em branco. Atribuir a subcadeia de caracteres ao terceiro grupo de captura.|
    |`\r?$`|Corresponder ao final de uma linha ou ao final da cadeia de caracteres.|

     Para obter mais informações sobre a avaliação condicional, consulte [Constructos de alternância](alternation-constructs-in-regular-expressions.md).

- Definições de grupo de `(?<` *balanceamento:* `-` *name2* `>` *subexpressão*nome2property `)` . Esse recurso permite que o mecanismo de expressões regulares controle constructos aninhados, como parênteses ou colchetes de abertura e fechamento. Para ver um exemplo, consulte [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).

- Grupos atômicos: `(?>` *subexpressão* `)` . Esse recurso permite que o mecanismo de retrocesso assegure que uma subexpressão corresponda apenas à primeira correspondência encontrada para ela, como se a expressão estivesse sendo executada independentemente da expressão que a contém. Se você não usar esse constructo, as pesquisas de retrocesso de expressões maiores poderão alterar o comportamento de uma subexpressão. Por exemplo, a expressão regular `(a+)\w` corresponde a um ou mais caracteres "a", juntamente com um caractere de palavra que segue a sequência de caracteres "a" e atribui a sequência de caracteres "a" para o primeiro grupo de captura. No entanto, se o caractere final da cadeia de caracteres de entrada também for um "a", ele será correspondido pelo `\w` elemento Language e não será incluído no grupo capturado.

     [!code-csharp[Conceptual.RegularExpressions.Design#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/nonbacktracking2.cs#7)]
     [!code-vb[Conceptual.RegularExpressions.Design#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/nonbacktracking2.vb#7)]

     A expressão regular `((?>a+))\w` impede esse comportamento. Como todos os caracteres “a” consecutivos são correspondidos sem retrocesso, o primeiro grupo de captura inclui todos os caracteres “a” consecutivos. Se os caracteres “a” não forem seguidos por pelo menos um caractere diferente de “a”, a correspondência falhará.

     [!code-csharp[Conceptual.RegularExpressions.Design#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/nonbacktracking1.cs#8)]
     [!code-vb[Conceptual.RegularExpressions.Design#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/nonbacktracking1.vb#8)]

     Para obter mais informações sobre grupos atômicos, consulte [agrupando construções](grouping-constructs-in-regular-expressions.md).

- Correspondência da direita para a esquerda, que é especificada fornecendo a opção <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> para um construtor de classe <xref:System.Text.RegularExpressions.Regex> ou um método de correspondência de instância estática. Esse recurso é útil durante a pesquisa da direita para a esquerda em vez da esquerda para direita ou nos casos em que é mais eficiente iniciar uma correspondência na parte direita do padrão em vez de à esquerda. Como mostra o exemplo a seguir, o uso da correspondência da direita para esquerda pode alterar o comportamento de quantificadores Greedy. O exemplo realiza duas pesquisas por uma frase que termina em número. A pesquisa da esquerda para a direita que usa o quantificador Greedy `+` corresponde a um dos seis dígitos na frase, enquanto a pesquisa da direita para a esquerda corresponde a todos os seis dígitos. Para obter uma descrição do padrão de expressão regular, consulte o exemplo que ilustra quantificadores lentos anteriormente nesta seção.

     [!code-csharp[Conceptual.RegularExpressions.Design#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/rtl1.cs#6)]
     [!code-vb[Conceptual.RegularExpressions.Design#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/rtl1.vb#6)]

     Para obter mais informações sobre correspondência da direita para a esquerda, consulte [Opções de expressão regular](regular-expression-options.md).

- Lookbehind positivo e negativo: `(?<=` *subexpressão* `)` para lookbehind positivo e `(?<!` *subexpressão* `)` para lookbehind negativa. Esse recurso é semelhante ao lookahead, que é discutido neste tópico. Como o mecanismo de expressões regulares possibilita uma correspondência completa da direita para a esquerda, expressões regulares permitem lookbehinds irrestritos. O lookbehind positivo e negativo também pode ser usado para evitar o aninhamento de quantificadores quando a subexpressão aninhada é um superconjunto de uma expressão externa. Expressões regulares com tais quantificadores aninhados geralmente oferecem um desempenho ruim. Por exemplo, o exemplo a seguir verifica se uma cadeia de caracteres começa e termina com um caractere alfanumérico e se qualquer outro caractere na cadeia de caracteres faz parte de um subconjunto maior. Faz parte da expressão regular usada para validar endereços de email. Para obter mais informações, consulte [Como verificar se cadeias de caracteres estão em um formato de email válido](how-to-verify-that-strings-are-in-valid-email-format.md).

     [!code-csharp[Conceptual.RegularExpressions.Design#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookbehind1.cs#5)]
     [!code-vb[Conceptual.RegularExpressions.Design#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookbehind1.vb#5)]

     A expressão regular ``^[A-Z0-9]([-!#$%&'.*+/=?^`{}|~\w])*(?<=[A-Z0-9])$`` é definida conforme mostrado na tabela a seguir.

    |Padrão|Descrição|
    |-------------|-----------------|
    |`^`|Começar a correspondência no início da cadeia de caracteres.|
    |`[A-Z0-9]`|Corresponder a qualquer caractere numérico ou alfanumérico. (A comparação não diferencia maiúsculas de minúsculas.)|
    |<code>([-!#$%&'.*+/=?^\`{}&#124;~\w])\*</code>|Coincida com zero ou mais ocorrências de qualquer caractere de palavra ou qualquer um dos seguintes caracteres:-,!, #, $,%, &, ',., \* , +,/, =,?, ^, &#96;, {,}, &#124; ou ~.|
    |`(?<=[A-Z0-9])`|Olhar para o caractere anterior, que precisa ser numérico ou alfanumérico. (A comparação não diferencia maiúsculas de minúsculas.)|
    |`$`|Encerrar a correspondência ao final da cadeia de caracteres.|

     Para obter mais informações sobre lookbehind positivo e negativo, consulte [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).

## <a name="related-articles"></a>Artigos relacionados

|Title|Description|
|-----------|-----------------|
|[Retrocesso](backtracking-in-regular-expressions.md)|Fornece informações sobre como o retrocesso de expressões regulares se ramifica para encontrar correspondências alternativas.|
|[Compilação e reutilização](compilation-and-reuse-in-regular-expressions.md)|Fornece informações sobre a compilação e a reutilização de expressões regulares para aumentar o desempenho.|
|[Acesso thread-safe](thread-safety-in-regular-expressions.md)|Fornece informações sobre a segurança de thread de expressões regulares e explica quando você deve sincronizar o acesso a objetos de expressão regular.|
|[Expressões regulares do .NET Framework](regular-expressions.md)|Fornece uma visão geral sobre o aspecto de linguagem de programação das expressões regulares.|
|[O modelo de objeto de expressão regular](the-regular-expression-object-model.md)|Oferece informações e exemplos de código que mostram como usar as classes de expressão regular.|
|[Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)|Oferece informações a respeito do conjunto de caracteres, operadores e constructos que você pode usar para definir expressões regulares.|

## <a name="reference"></a>Referência

- <xref:System.Text.RegularExpressions?displayProperty=nameWithType>
