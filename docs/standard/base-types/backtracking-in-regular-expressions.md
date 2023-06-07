---
title: Retrocesso em expressões regulares do .NET
description: Saiba como controlar o retrocesso na correspondência de padrões de expressão regular.
ms.date: 11/12/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- .NET Framework regular expressions, backtracking
- alternative matching patterns
- optional matching patterns
- searching with regular expressions, backtracking
- pattern-matching with regular expressions, backtracking
- backtracking
- regular expressions [.NET Framework], backtracking
- strings [.NET Framework], regular expressions
- parsing text with regular expressions, backtracking
ms.assetid: 34df1152-0b22-4a1c-a76c-3c28c47b70d8
ms.openlocfilehash: d9fb976c73891646df60b5329beb09493acbae8a
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84277798"
---
# <a name="backtracking-in-regular-expressions"></a>Retrocesso em expressões regulares
O retrocesso ocorre quando um padrão de expressão regular contém [quantificadores](quantifiers-in-regular-expressions.md) opcionais ou [constructos de alternância](alternation-constructs-in-regular-expressions.md) e o mecanismo de expressões regulares retorna a um estado salvo anterior para retomar sua pesquisa por uma correspondência. O retrocesso é indispensável para o poder das expressões regulares, ele permite que as expressões sejam poderosas e flexíveis e correspondam a padrões muito complexos. No entanto, todo esse poder tem um custo. O retrocesso muitas vezes é o fator individual que mais afeta o desempenho do mecanismo de expressões regulares. Felizmente, o desenvolvedor tem controle sobre o comportamento do mecanismo de expressões regulares e como ele usa o retrocesso. Este tópico explica como o retrocesso funciona e como ele pode ser controlado.  
  
> [!NOTE]
> Em geral, um mecanismo de NFA (Automação Finita Não Determinística), como o mecanismo de expressões regulares .NET, coloca a responsabilidade pela criação de expressões regulares eficientes e rápidas nas mãos do desenvolvedor.  

## <a name="linear-comparison-without-backtracking"></a>Comparação linear sem retrocesso  
 Se um padrão de expressão regular não tem quantificadores ou constructos de alternância opcionais, o mecanismo de expressões regulares é executado em tempo linear. Ou seja, depois que o mecanismo de expressões regulares corresponde o primeiro elemento de linguagem no padrão com o texto da cadeia de caracteres de entrada, ele tenta corresponder o elemento de linguagem seguinte no padrão com o próximo caractere ou grupo de caracteres na cadeia de caracteres de entrada. Esse processo continuará até que a correspondência obtenha êxito ou falhe. Em ambos os casos, o mecanismo de expressões regulares avança um caractere de cada vez na cadeia de caracteres de entrada.  
  
 O exemplo a seguir ilustra esse cenário. A expressão regular `e{2}\w\b` procura duas ocorrências da letra “e” seguidas por qualquer caractere de palavra seguido por um limite de palavra.  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking1.cs#1)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking1.vb#1)]  
  
 Embora essa expressão regular inclua o quantificador `{2}`, ela é avaliada de uma maneira linear. O mecanismo de expressões regulares não retrocede porque `{2}` não é um quantificador opcional, ele especifica um número exato e não um número variável de vezes que a subexpressão anterior deve corresponder. Como resultado, o mecanismo de expressões regulares tenta corresponder o padrão da expressão regular com a cadeia de caracteres de entrada conforme mostrado na tabela a seguir.  
  
|Operação|Posição no padrão|Posição na cadeia de caracteres|Resultado|  
|---------------|-------------------------|------------------------|------------|  
|1|e|"needing a reed" (índice 0)|Nenhuma correspondência.|  
|2|e|"eeding a reed" (índice 1)|Possível correspondência.|  
|3|e{2}|"eding a reed" (índice 2)|Possível correspondência.|  
|4|\w|"ding a reed" (índice 3)|Possível correspondência.|  
|5|\b|"ing a reed" (índice 4)|Possível falha de correspondência.|  
|6|e|"eding a reed" (índice 2)|Possível correspondência.|  
|7|e{2}|"ding a reed" (índice 3)|Possível falha de correspondência.|  
|8|e|"ding a reed" (índice 3)|Falha de correspondência.|  
|9|e|"ing a reed" (índice 4)|Nenhuma correspondência.|  
|10|e|"ng a reed" (índice 5)|Nenhuma correspondência.|  
|11|e|"g a reed" (índice 6)|Nenhuma correspondência.|  
|12|e|" a reed" (índice 7)|Nenhuma correspondência.|  
|13|e|"a reed" (índice 8)|Nenhuma correspondência.|  
|14|e|" reed" (índice 9)|Nenhuma correspondência.|  
|15|e|"reed" (índice 10)|Nenhuma correspondência|  
|16|e|"eed" (índice 11)|Possível correspondência.|  
|17|e{2}|"ed" (índice 12)|Possível correspondência.|  
|18|\w|"d" (índice 13)|Possível correspondência.|  
|19|\b|"" (índice 14)|Correspondência.|  
  
 Se um padrão de expressão regular não inclui nenhum quantificador ou construtor de alternância opcional, o número máximo de comparações necessárias para corresponder ao padrão da expressão regular com a cadeia de caracteres de entrada é aproximadamente equivalente ao número de caracteres na cadeia de caracteres de entrada. Nesse caso, o mecanismo de expressões regulares usa 19 comparações para identificar possíveis correspondências nesta cadeia de 13 caracteres.  Em outras palavras, o mecanismo de expressões regulares é executado em tempo quase linear se não contém quantificadores ou construtores de alternância opcionais.

## <a name="backtracking-with-optional-quantifiers-or-alternation-constructs"></a>Retrocesso com quantificadores opcionais ou construtores de alternância  
 Quando uma expressão regular inclui quantificadores ou construtores de alternância opcionais, a avaliação da cadeia de caracteres de entrada deixa de ser linear. A correspondência de padrões com um mecanismo NFA é orientada pelos elementos de linguagem da expressão regular e não pelos caracteres a serem correspondidos na cadeia de caracteres de entrada. Assim, o mecanismo de expressões regulares tenta fazer a correspondência total de subexpressões opcionais ou alternativas. Quando ele avança para o elemento de linguagem seguinte na subexpressão e a correspondência falha, o mecanismo de expressões regulares pode abandonar uma parte de sua correspondência bem-sucedida e retornar a um estado salvo anteriormente com o objetivo de corresponder a expressão regular inteira com a cadeia de caracteres de entrada. Esse processo de retornar a um estado salvo anterior para localizar uma correspondência é conhecido como o retrocesso.  
  
 Por exemplo, considere o padrão de expressão regular `.*(es)`, o qual corresponde os caracteres “es” e todos os caracteres que os precedem. Como mostra o exemplo a seguir, se a cadeia de caracteres de entrada é "Essential services are provided by regular expressions." (Serviços essenciais são fornecidos por expressões regulares.), o padrão corresponde a cadeia de caracteres até o “es” (inclusive) em "expressions”.  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking2.cs#2)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking2.vb#2)]  
  
 Para fazer isso, o mecanismo de expressões regulares usa o retrocesso da seguinte forma:  
  
- Ele corresponde o `.*` (que corresponde a zero, uma ou mais ocorrências de qualquer caractere) com a cadeia de caracteres de entrada inteira.  
  
- Ele tenta corresponder “e” no padrão da expressão regular. No entanto, a cadeia de caracteres de entrada não tem nenhum caractere restante disponível para corresponder.  
  
- Ele retrocede para sua última correspondência bem-sucedida, "Essential services are provided by regular expressions", e tenta corresponder “e” com o ponto no final da frase. A correspondência falha.  
  
- Ele continua a retroceder para uma correspondência bem-sucedida anterior um caractere de cada vez até que a subcadeia de caracteres provisória correspondente seja “Essential services are provided by regular expr". Ele então compara o “e” no padrão com o segundo “e” em “expressions” e encontra uma correspondência.  
  
- Ele compara o “s” no padrão com o “s” após o caractere “e” que já foi correspondido (o primeiro “s” em “expressions”). A correspondência é bem-sucedida.  
  
 Quando o retrocesso é usado, corresponder o padrão de expressão regular com a cadeia de caracteres de entrada, que tem 55 caracteres de comprimento, requer 67 operações de comparação. Geralmente, se um padrão de expressão regular tem um único constructo de alternância ou um único quantificador opcional, o número de operações de comparação necessárias para corresponder ao padrão é mais que duas vezes maior do que o número de caracteres na cadeia de caracteres de entrada.

## <a name="backtracking-with-nested-optional-quantifiers"></a>Retrocesso com quantificadores opcionais aninhados  
 O número de operações de comparação necessárias para corresponder a um padrão de expressão regular pode aumentar exponencialmente se o padrão inclui um grande número de construtores de alternância, se ele inclui construtores de alternância aninhados ou, mais comumente, se ele inclui quantificadores opcionais aninhados. Por exemplo, o padrão de expressão regular `^(a+)+$` foi criado para corresponder a uma cadeia de caracteres completa que contém um ou mais caracteres “a”. O exemplo fornece duas cadeias de caracteres de entrada de comprimento idêntico, mas somente a primeira cadeia de caracteres corresponde ao padrão. A classe <xref:System.Diagnostics.Stopwatch?displayProperty=nameWithType> é usada para determinar a duração da operação de correspondência.  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking3.cs#3)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking3.vb#3)]  
  
 Como a saída do exemplo mostra, o mecanismo de expressões regulares demora aproximadamente duas vezes mais tempo para descobrir que uma cadeia de caracteres de entrada não correspondeu ao padrão do que o tempo que foi necessário para identificar uma cadeia de caracteres compatível. Isso acontece porque uma correspondência malsucedida sempre representa um cenário de pior caso. O mecanismo de expressões regulares deve usar a expressão regular para seguir todos os caminhos possíveis através dos dados antes de concluir que a correspondência falhou e os parênteses aninhados criam vários caminhos adicionais nos dados. O mecanismo de expressões regulares conclui que a segunda cadeia de caracteres não correspondeu ao padrão ao fazer o seguinte:  
  
- Ele verifica que estava no início da cadeia de caracteres e então corresponde os primeiros cinco caracteres da cadeia de caracteres com o padrão `a+`. Ele então determina que não há grupos adicionais de caracteres “a” na cadeia de caracteres. Finalmente, ele testa o final da cadeia de caracteres. Como um caractere adicional permanece na cadeia de caracteres, a correspondência falha. Essa correspondência com falha requer 9 comparações. O mecanismo de expressões regulares também salva as informações de estado de suas correspondências de “a” (as quais chamaremos a correspondência 1), "aa” (correspondência 2), "aaa" (correspondência 3) e “aaaa" (correspondência 4).  
  
- Ele retorna à correspondência 4 salva anteriormente. Ele determina que há um caractere adicional “a” a ser atribuído a um grupo capturado adicional. Finalmente, ele testa o final da cadeia de caracteres. Como um caractere adicional permanece na cadeia de caracteres, a correspondência falha. Essa correspondência com falha requer 4 comparações. Até agora, foi executado um total de 13 comparações.  
  
- Ele retorna à correspondência 3 salva anteriormente. Ele determina que há dois caracteres adicionais “a” a serem atribuídos a um grupo capturado adicional. No entanto, o teste de fim da cadeia de caracteres falha. Ele então retorna para a correspondência 3 e tenta corresponder os dois caracteres adicionais “a” em dois grupos capturados adicionais. No entanto, o teste de fim da cadeia de caracteres continua a falhar. Essas correspondências com falha exigem 12 comparações. Até agora, foi executado um total de 25 comparações.  
  
 A comparação de cadeia de caracteres de entrada com a expressão regular continuará dessa forma até que o mecanismo de expressão regular tente todas as combinações possíveis de correspondências e conclua que não há nenhuma correspondência. Devido aos quantificadores aninhados, essa comparação é O(2<sup>n</sup>) ou uma operação exponencial, em que *n* é o número de caracteres na cadeia de caracteres de entrada. Isso significa que, no pior caso, uma cadeia de caracteres de entrada com 30 caracteres requer aproximadamente 1.073.741.824 comparações e uma cadeia de caracteres de entrada com 40 caracteres requer aproximadamente 1.099.511.627.776 comparações. Se você usar cadeias de caracteres com esses tamanhos ou até mesmo com tamanhos maiores, os métodos de expressões regulares poderão demorar um tempo extremamente longo para terminar ao processarem uma entrada que não correspondam ao padrão de expressão regular.

## <a name="controlling-backtracking"></a>Controlando o retrocesso  
 O retrocesso permite a você criar expressões regulares avançadas e flexíveis. No entanto, conforme mostrado na seção anterior, esses benefícios podem estar associados a um baixo desempenho inaceitável. Para evitar o retrocesso excessivo, você deve definir um intervalo de tempo limite no qual você criará uma instância de um objeto <xref:System.Text.RegularExpressions.Regex> ou chamará um método de correspondência de expressão regular estático. Isso é abordado na próxima seção. Além disso, o .NET dá suporte a três elementos de linguagem de expressão regular que limitam ou suprimem o retrocesso e que dão suporte a expressões regulares complexas com pouca ou nenhuma penalidade de desempenho: [grupos atômicos](#atomic-groups), [asserções lookbehind](#lookbehind-assertions)e [declarações de lookahead](#lookahead-assertions). Para saber mais sobre cada elemento de linguagem, consulte [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).  

### <a name="defining-a-time-out-interval"></a>Definindo um intervalo de tempo limite  
 Do .NET Framework 4.5 em diante, você pode definir um valor de tempo limite que representa o intervalo mais longo durante o qual o mecanismo de expressão regular pesquisará uma única correspondência antes de abandonar a tentativa e gerar uma exceção <xref:System.Text.RegularExpressions.RegexMatchTimeoutException>. Você especifica o intervalo de tempo limite ao fornecer um valor de <xref:System.TimeSpan> para o construtor <xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29> para instanciar expressões regulares. Além disso, cada método de correspondência de padrão estático tem uma sobrecarga com um parâmetro <xref:System.TimeSpan> que permite a você especificar um valor de tempo limite. Por padrão, o intervalo de tempo limite é definido para <xref:System.Text.RegularExpressions.Regex.InfiniteMatchTimeout?displayProperty=nameWithType>, o que significa que o mecanismo de expressões regulares nunca excede o tempo limite.  
  
> [!IMPORTANT]
> É recomendável sempre definir um intervalo de tempo limite se a sua expressão regular depende de retrocesso.  
  
 Uma exceção <xref:System.Text.RegularExpressions.RegexMatchTimeoutException> indica que o mecanismo de expressões regulares não pôde localizar uma correspondência dentro do intervalo de tempo limite especificado, mas não indica como a exceção foi gerada. O motivo poderia ser o retrocesso excessivo, mas também é possível que o intervalo de tempo limite tenha sido definido como um valor muito baixo considerando a carga do sistema no momento em que a exceção foi gerada. Ao tratar a exceção, você pode escolher entre abandonar as correspondências adicionais com a cadeia de caracteres de entrada ou aumentar o intervalo de tempo limite e repetir a operação de correspondência.  
  
 Por exemplo, o código a seguir chama o construtor <xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29> para criar uma instância de um objeto <xref:System.Text.RegularExpressions.Regex> com um valor de tempo limite de um segundo. O padrão de expressão regular `(a+)+$`, que faz a correspondência de uma ou várias sequências de um ou mais caracteres de “a” no final de uma linha, está sujeito ao retrocesso excessivo. Se <xref:System.Text.RegularExpressions.RegexMatchTimeoutException> gerada, o exemplo aumenta o valor do tempo limite até um intervalo máximo de três segundos. Depois disso, ele abandona a tentativa de corresponder o padrão.  
  
 [!code-csharp[System.Text.RegularExpressions.Regex.ctor#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.text.regularexpressions.regex.ctor/cs/ctor1.cs#1)]
 [!code-vb[System.Text.RegularExpressions.Regex.ctor#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.text.regularexpressions.regex.ctor/vb/ctor1.vb#1)]  

### <a name="atomic-groups"></a>Grupos atômicos
 O `(?>` elemento de linguagem de *subexpressão* `)` suprime a retrocesso na subexpressão. Depois que ele for correspondido com êxito, ele não resultará em nenhuma parte de sua correspondência para o retrocesso posterior. Por exemplo, no padrão `(?>\w*\d*)1` , se o `1` não puder ser correspondido, o não resultará em `\d*` nenhuma de suas correspondências, mesmo que isso signifique que isso permitiria que a `1` correspondência fosse bem-sucedida. Os grupos atômicos podem ajudar a evitar os problemas de desempenho associados às correspondências com falha.
  
 O exemplo a seguir ilustra como suprimir o retrocesso melhora o desempenho quando quantificadores aninhados são usados. Ele mede o tempo necessário para que o mecanismo de expressão regular determine que uma cadeia de caracteres de entrada não corresponde a duas expressões regulares. A primeira expressão regular usa o retrocesso para tentar corresponder uma cadeia de caracteres que contém uma ou mais ocorrências de um ou mais dígitos hexadecimais, seguidos por dois-pontos, seguido por um ou mais dígitos hexadecimais, seguidos por dois dois-pontos. A segunda expressão regular é idêntica à primeira, exceto que ela desabilita o retrocesso. Como a saída do exemplo mostra, a melhora do desempenho resultante da desabilitação do retrocesso é significativa.  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking4.cs#4)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking4.vb#4)]  

### <a name="lookbehind-assertions"></a>Asserções lookbehind  
 O .NET inclui dois elementos de linguagem, `(?<=` *subexpressão* `)` e `(?<!` *subexpressão* `)` , que correspondem ao caractere ou caracteres anteriores na cadeia de caracteres de entrada. Ambos os elementos de linguagem são asserções de largura zero, ou seja, eles determinam se o caractere ou os caracteres que precedem imediatamente o caractere atual podem ser correspondidos pela *subexpressão*, sem avanço ou retrocesso.  
  
 `(?<=`*subexpressão* `)` é uma asserção lookbehind positiva; ou seja, o caractere ou os caracteres antes da posição atual devem corresponder à *subexpressão*. `(?<!`*subexpressão* `)` é uma asserção lookbehind negativa; ou seja, o caractere ou os caracteres antes da posição atual não devem corresponder à *subexpressão*. As asserções lookbehind positivas e negativas são mais úteis quando a *subexpressão* é um subconjunto da subexpressão anterior.  
  
 O exemplo a seguir usa dois padrões equivalentes à expressão regular que validam o nome de usuário em um endereço de email. O primeiro padrão está sujeito a baixo desempenho devido ao retrocesso excessivo. O segundo padrão modifica a primeira expressão regular ao substituir um quantificador aninhado por uma asserção lookbehind positiva. A saída do exemplo exibe, o tempo de execução do método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType>.  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking5.cs#5)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking5.vb#5)]  
  
 O primeiro padrão de expressão regular, `^[0-9A-Z]([-.\w]*[0-9A-Z])*@`, é definido como mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`^`|Começa a correspondência no início da cadeia de caracteres.|  
|`[0-9A-Z]`|Corresponde a um caractere alfanumérico. Essa comparação não diferencia maiúsculas de minúsculas porque o método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> é chamado com a opção <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType>.|  
|`[-.\w]*`|Corresponde a zero, uma ou mais ocorrências de um hífen, ponto ou caractere de palavra.|  
|`[0-9A-Z]`|Corresponde a um caractere alfanumérico.|  
|`([-.\w]*[0-9A-Z])*`|Corresponde a zero ou mais ocorrências da combinação de zero ou mais hífens, pontos ou caracteres de palavra, seguidos por um caractere alfanumérico. Este é o primeiro grupo de captura.|  
|`@`|Corresponde a um sinal ("\@").|  
  
 O segundo padrão de expressão regular, `^[0-9A-Z][-.\w]*(?<=[0-9A-Z])@`, usa uma asserção lookbehind positiva. Ele é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`^`|Começa a correspondência no início da cadeia de caracteres.|  
|`[0-9A-Z]`|Corresponde a um caractere alfanumérico. Essa comparação não diferencia maiúsculas de minúsculas porque o método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> é chamado com a opção <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType>.|  
|`[-.\w]*`|Corresponde a zero ou mais ocorrências de um hífen, ponto ou caractere de palavra.|  
|`(?<=[0-9A-Z])`|Examina de volta o último caractere correspondente e continua a correspondência se ele é alfanumérico. Observe que os caracteres alfanuméricos são um subconjunto do conjunto que consiste em pontos, hífens e todos os caracteres de palavra.|  
|`@`|Corresponde a um sinal ("\@").|  

### <a name="lookahead-assertions"></a>Asserções lookahead  
 O .NET inclui dois elementos de linguagem, `(?=` *subexpressão* `)` e `(?!` *subexpressão* `)` , que correspondem ao próximo caractere ou caracteres na cadeia de caracteres de entrada. Ambos os elementos de linguagem são asserções de largura zero, ou seja, eles determinam se o caractere ou os caracteres que seguem imediatamente o caractere atual podem ser correspondidos pela *subexpressão*, sem avanço ou retrocesso.  
  
 `(?=`*subexpressão* `)` é uma asserção de antecipação positiva; ou seja, o caractere ou os caracteres após a posição atual devem corresponder à *subexpressão*. `(?!`*subexpressão* `)` é uma asserção de antecipação negativa; ou seja, o caractere ou os caracteres após a posição atual não devem corresponder à *subexpressão*. As declarações de lookahead positiva e negativa são mais úteis quando a *subexpressão* é um subconjunto da próxima subexpressão.  
  
 O exemplo a seguir usa dois padrões de expressão regular equivalentes que validam um nome de tipo totalmente qualificado. O primeiro padrão está sujeito a baixo desempenho devido ao retrocesso excessivo. O segundo modifica a primeira expressão regular ao substituir um quantificador aninhado por uma asserção lookahead positiva. A saída do exemplo exibe, o tempo de execução do método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType>.  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking6.cs#6)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking6.vb#6)]  
  
 O primeiro padrão de expressão regular, `^(([A-Z]\w*)+\.)*[A-Z]\w*$`, é definido como mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`^`|Começa a correspondência no início da cadeia de caracteres.|  
|`([A-Z]\w*)+\.`|Corresponde a um caractere alfabético (A-Z) seguido por zero ou mais caracteres de palavra uma ou mais vezes, seguidos de um ponto. Essa comparação não diferencia maiúsculas de minúsculas porque o método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> é chamado com a opção <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType>.|  
|`(([A-Z]\w*)+\.)*`|Corresponde ao padrão anterior zero vezes ou mais.|  
|`[A-Z]\w*`|Corresponder a um caractere alfabético seguido por zero ou mais caracteres de palavra.|  
|`$`|Finalizar a correspondência no final da cadeia de caracteres de entrada.|  
  
 O segundo padrão de expressão regular, `^((?=[A-Z])\w+\.)*[A-Z]\w*$`, usa uma asserção lookahead positiva. Ele é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`^`|Começa a correspondência no início da cadeia de caracteres.|  
|`(?=[A-Z])`|Examine além do primeiro caractere e continue a correspondência se ele for alfabético (A-Z). Essa comparação não diferencia maiúsculas de minúsculas porque o método <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> é chamado com a opção <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType>.|  
|`\w+\.`|Corresponde a um ou mais caracteres de palavra seguidos por um ponto.|  
|`((?=[A-Z])\w+\.)*`|Corresponde ao padrão de um ou mais caracteres de palavra seguidos por um ponto zero ou mais vezes. O caractere de palavra inicial deve ser alfabético.|  
|`[A-Z]\w*`|Corresponder a um caractere alfabético seguido por zero ou mais caracteres de palavra.|  
|`$`|Finalizar a correspondência no final da cadeia de caracteres de entrada.|  
  
## <a name="see-also"></a>Veja também

- [Expressões regulares do .NET](regular-expressions.md)
- [Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)
- [Quantificadores](quantifiers-in-regular-expressions.md)
- [Constructos de alternância](alternation-constructs-in-regular-expressions.md)
- [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md)
