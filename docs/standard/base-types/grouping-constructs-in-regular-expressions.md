---
title: Agrupando construtores em expressões regulares
description: Aprenda a usar construções de agrupamento no .NET. As construções de agrupamento delineam subexpressãos de uma expressão regular e capturam subcadeias de caracteres de uma cadeia de entrada.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- lookbehinds
- regular expressions, grouping constructs
- lookaheads
- .NET Framework regular expressions, grouping constructs
- constructs, grouping
- grouping constructs
ms.assetid: 0fc18634-f590-4062-8d5c-f0b71abe405b
ms.openlocfilehash: d737e5758ee7a940aeea3ded9a7937d687393116
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662622"
---
# <a name="grouping-constructs-in-regular-expressions"></a>Agrupando construtores em expressões regulares
As construções de agrupamento delineiam as subexpressões de uma expressão regular e capturam a subcadeia de caracteres de uma cadeia de caracteres de entrada. Você pode usar construções de agrupamento para fazer isto:  
  
- Fazer a correspondência de uma subexpressão que é repetida na cadeia de caracteres de entrada.  
  
- Aplicar um quantificador a uma subexpressão com diversos elementos de linguagem de expressão regular. Para saber mais sobre quantificadores, confira [Quantificadores](quantifiers-in-regular-expressions.md).  
  
- Inclua uma subexpressão na cadeia de caracteres retornada pelos métodos <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> e <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType>.  
  
- Recupere subexpressões individuais da propriedade <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> e processe-as separadamente do texto correspondente como um todo.  
  
 A tabela a seguir lista os constructos de agrupamento com suporte do mecanismo de expressões regulares .NET e indica captura ou não captura.  
  
|Constructo de agrupamento|Captura ou não captura|  
|------------------------|-------------------------------|  
|[Subexpressões correspondentes](#matched_subexpression)|Capturando|  
|[Subexpressões correspondentes nomeadas](#named_matched_subexpression)|Capturando|  
|[Equilibrando definições de grupo](#balancing_group_definition)|Capturando|  
|[Grupos de não captura](#noncapturing_group)|Não captura|  
|[Opções de grupo](#group_options)|Não captura|  
|[Asserções lookahead positivas de largura zero](#zerowidth_positive_lookahead_assertion)|Não captura|  
|[Asserções lookahead negativas de largura zero](#zerowidth_negative_lookahead_assertion)|Não captura|  
|[Asserções lookbehind positivas de largura zero](#zerowidth_positive_lookbehind_assertion)|Não captura|  
|[Asserções lookbehind negativas de largura zero](#zerowidth_negative_lookbehind_assertion)|Não captura|  
|[Grupos atômicos](#atomic_groups)|Não captura|  
  
 Para obter informações sobre grupos e o modelo de objeto de expressão regular, consulte [agrupando construções e objetos de expressão regular](#Objects).  
  
<a name="matched_subexpression"></a>
## <a name="matched-subexpressions"></a>Subexpressões Coincidentes  
 O constructo de agrupamento a seguir captura uma subexpressão correspondente:  
  
 `(`*subexpressão*`)`  
  
 em que *subexpression* é qualquer padrão de expressão regular válido. As capturas que usam parênteses são numeradas automaticamente, da esquerda para a direita, com base na ordem do parêntese de abertura na expressão regular, começando em um. A captura que recebe o número zero é o texto que coincide com todo o padrão da expressão regular.  
  
> [!NOTE]
> Por padrão, o `(` elemento de linguagem de *subexpressão* `)` captura a subexpressão correspondente. Porém, se o parâmetro <xref:System.Text.RegularExpressions.RegexOptions> de um padrão de expressão regular que corresponde ao método inclui o sinalizador <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> ou se a opção `n` é aplicada a essa subexpressão (consulte [Opções de grupo](#group_options) mais adiante neste tópico), a subexpressão coincidente não é capturada.  
  
 Você pode acessar os grupos capturados de quatro formas:  
  
- Usando o constructo de referência inversa na expressão regular. A subexpressão correspondente é referenciada na mesma expressão regular usando a sintaxe `\`*number*, em que *number* é o número ordinal da subexpressão capturada.  
  
- Usando o constructo de referência inversa nomeado na expressão regular. A subexpressão correspondente é referenciada na mesma expressão regular usando o nome da sintaxe `\k<` *name* `>` , em que *Name* é o nome de um grupo de captura, ou `\k<` *número* `>` , em que *Number* é o número ordinal de um grupo de captura. O grupo de captura tem um nome padrão que é idêntico a seu número ordinal. Para saber mais, confira [Subexpressões coincidentes nomeadas](#named_matched_subexpression) mais adiante neste tópico.  
  
- Ao usar a sequência de substituição `$`*number* em uma chamada de método <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> ou <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType>, em que *number* é o número ordinal da subexpressão capturada.  
  
- Usando de forma programática o objeto <xref:System.Text.RegularExpressions.GroupCollection> retornado pela propriedade <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType>. O membro na posição zero da coleção representa toda a correspondência da expressão regular. Cada membro subsequente representa uma subexpressão correspondente. Para obter mais informações, veja a seção [Construções de agrupamento e objetos de expressão regular](#Objects).  
  
 O exemplo a seguir mostra uma expressão regular que identifica palavras duplicadas no texto. Os dois grupos de captura padrão da expressão regular representam as duas instâncias da palavra duplicada. A segunda instância é capturada para relatar a posição inicial na cadeia de caracteres de entrada.  
  
 [!code-csharp[RegularExpressions.Language.Grouping#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/grouping1.cs#1)]
 [!code-vb[RegularExpressions.Language.Grouping#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/grouping1.vb#1)]  
  
 Este é o padrão da expressão regular:  
  
`(\w+)\s(\1)\W`  
  
 A tabela a seguir mostra como o padrão da expressão regular é interpretado.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`(\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. Este é o primeiro grupo de captura.|  
|`\s`|Corresponde a um caractere de espaço em branco.|  
|`(\1)`|Corresponde à cadeia de caracteres no primeiro grupo capturado. Este é o segundo grupo de captura. O exemplo o atribui a um grupo capturado para que seja possível recuperar a posição inicial da palavra duplicada da propriedade `Match.Index`.|  
|`\W`|Estabeleça a correspondência com caracteres que não compõem palavras, como espaços em branco e pontuação. Isso impede a correspondência de um padrão de expressão regular com uma expressão que começa com a palavra do primeiro grupo capturado.|  
  
<a name="named_matched_subexpression"></a>
## <a name="named-matched-subexpressions"></a>Subexpressões Correspondentes Nomeadas  
 O constructo de agrupamento a seguir captura uma subexpressão correspondente e permite acessá-la usando seu nome ou número:  
  
`(?<name>subexpression)`  
  
 ou:  
  
`(?'name'subexpression)`  
  
 em que *name* é um nome de grupo válido e *subexpression* é qualquer padrão de expressão regular válido. *name* não deve conter caracteres de pontuação nem começar com um número.  
  
> [!NOTE]
> Se o parâmetro <xref:System.Text.RegularExpressions.RegexOptions> de um padrão de expressão regular que corresponde ao método inclui o sinalizador <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> ou se a opção `n` é aplicada a essa subexpressão (consulte [Opções de grupo](#group_options) mais adiante neste tópico), a única forma de capturar uma subexpressão é atribuir nomes explícitos a grupos de captura.  
  
 Você pode acessar grupos capturados nomeados destas formas:  
  
- Usando o constructo de referência inversa nomeado na expressão regular. A subexpressão correspondente é referenciada na mesma expressão regular usando o nome da sintaxe `\k<` *name* `>` , em que *Name* é o nome da subexpressão capturada.  
  
- Usando o constructo de referência inversa na expressão regular. A subexpressão correspondente é referenciada na mesma expressão regular usando a sintaxe `\`*number*, em que *number* é o número ordinal da subexpressão capturada. As subexpressões correspondentes nomeadas são numeradas em ordem de sequência, da esquerda da direita, após as subexpressões correspondentes.  
  
- Usando a sequência de substituição de `${` *nome* `}` em <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> uma <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType> chamada de método ou, em que *Name* é o nome da subexpressão capturada.  
  
- Ao usar a sequência de substituição `$`*number* em uma chamada de método <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> ou <xref:System.Text.RegularExpressions.Match.Result%2A?displayProperty=nameWithType>, em que *number* é o número ordinal da subexpressão capturada.  
  
- Usando de forma programática o objeto <xref:System.Text.RegularExpressions.GroupCollection> retornado pela propriedade <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType>. O membro na posição zero da coleção representa toda a correspondência da expressão regular. Cada membro subsequente representa uma subexpressão correspondente. Os grupos capturados nomeados são armazenados na coleção depois dos grupos capturados numerados.  
  
- De forma programática, ao fornecer o nome da subexpressão ao indexador do objeto <xref:System.Text.RegularExpressions.GroupCollection> (no C#) ou a sua propriedade <xref:System.Text.RegularExpressions.GroupCollection.Item%2A> (no Visual Basic).  
  
 Um padrão de expressão regular simples mostra como os grupos numerados (sem nome) e nomeados podem ser usados como referência de forma programática ou usando sintaxe de linguagem de expressão regular. A expressão regular `((?<One>abc)\d+)?(?<Two>xyz)(.*)` gera os seguintes grupos de captura por número e nome. O primeiro grupo de captura (o número 0) sempre faz referência a todo o padrão.  
  
|Número|Nome|Padrão|  
|------------|----------|-------------|  
|0|0 (nome padrão)|`((?<One>abc)\d+)?(?<Two>xyz)(.*)`|  
|1|1 (nome padrão)|`((?<One>abc)\d+)`|  
|2|2 (nome padrão)|`(.*)`|  
|3|Um|`(?<One>abc)`|  
|4|Dois|`(?<Two>xyz)`|  
  
 O exemplo a seguir mostra uma expressão regular que identifica palavras duplicadas e a palavra que aparece logo na sequência. O padrão da expressão regular define duas subexpressões nomeadas: `duplicateWord`, que representa a palavra duplicada e `nextWord`, que representa a palavra que aparece logo na sequência.  
  
 [!code-csharp[RegularExpressions.Language.Grouping#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/grouping2.cs#2)]
 [!code-vb[RegularExpressions.Language.Grouping#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/grouping2.vb#2)]  
  
 Este é o padrão da expressão regular:  
  
`(?<duplicateWord>\w+)\s\k<duplicateWord>\W(?<nextWord>\w+)`  
  
 A tabela a seguir mostra como a expressão regular é interpretada.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`(?<duplicateWord>\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. Atribua um nome ao grupo de captura `duplicateWord`.|  
|`\s`|Corresponde a um caractere de espaço em branco.|  
|`\k<duplicateWord>`|Estabeleça a correspondência com o grupo de captura chamado `duplicateWord`.|  
|`\W`|Estabeleça a correspondência com caracteres que não compõem palavras, como espaços em branco e pontuação. Isso impede a correspondência de um padrão de expressão regular com uma expressão que começa com a palavra do primeiro grupo capturado.|  
|`(?<nextWord>\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. Atribua um nome ao grupo de captura `nextWord`.|  
  
 Observe que o nome de um grupo pode ser repetido em uma expressão regular. Por exemplo, é possível que mais de um grupo seja nomeado como `digit`, como mostra o exemplo a seguir. No caso de nomes duplicados, o valor do objeto <xref:System.Text.RegularExpressions.Group> é determinado pela última captura bem-sucedida na cadeia de caracteres de entrada. Além disso, a <xref:System.Text.RegularExpressions.CaptureCollection> é preenchida com informações sobre cada captura exatamente como seria se o nome do grupo não fosse duplicado.  
  
 No exemplo a seguir, a expressão regular `\D+(?<digit>\d+)\D+(?<digit>\d+)?` inclui duas ocorrências de um grupo chamado `digit`. O primeiro grupo chamado `digit` captura um ou mais caracteres de dígito. O primeiro grupo chamado `digit` captura zero ou uma ocorrência de um ou mais caracteres de dígito. Como a saída do exemplo apresentado mostra, se o segundo grupo de captura corresponder ao texto com êxito, o valor desse texto definirá o valor do objeto <xref:System.Text.RegularExpressions.Group>. Se o segundo grupo de captura não corresponder à cadeia de caracteres de entrada, o valor da última correspondência bem-sucedida definirá o valor do objeto <xref:System.Text.RegularExpressions.Group>.  
  
 [!code-csharp[RegularExpressions.Language.Grouping#12](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/duplicate1.cs#12)]
 [!code-vb[RegularExpressions.Language.Grouping#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/duplicate1.vb#12)]  
  
 A tabela a seguir mostra como a expressão regular é interpretada.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\D+`|Corresponder a um ou mais caracteres de dígito não decimal.|  
|`(?<digit>\d+)`|Corresponder a um ou mais caracteres de dígito decimal. Atribuir a correspondência ao grupo chamado `digit`.|  
|`\D+`|Corresponder a um ou mais caracteres de dígito não decimal.|  
|`(?<digit>\d+)?`|Faz a correspondência de zero ou uma ocorrência de um período seguido por um ou mais caracteres de dígito decimal. Atribuir a correspondência ao grupo chamado `digit`.|  
  
<a name="balancing_group_definition"></a>
## <a name="balancing-group-definitions"></a>Definições de grupo de balanceamento  
 Uma definição de grupo de balanceamento exclui a definição de um grupo definido anteriormente e armazena, no grupo atual, o intervalo entre o grupo definido anteriormente e o atual. Esse constructo de agrupamento tem o seguinte formato:  
  
`(?<name1-name2>subexpression)`  
  
 ou:  
  
`(?'name1-name2' subexpression)`
  
 em que *name1* é o grupo atual (opcional), *name2* é um grupo definido anteriormente e *subexpression* é qualquer padrão de expressão regular válido. A definição de grupo de balanceamento exclui a definição de *name2* e armazena o intervalo entre *name2* e *name1* em *name1*. Se nenhum grupo *name2* for definido, a correspondência retrocede. Como a exclusão da última definição de *name2* revela a definição anterior de *name2*, essa construção permite que você use a pilha de capturas para o grupo *name2* como um contador, a fim de registrar construções aninhadas, como parênteses e colchetes.  
  
 A definição de grupo de balanceamento usa *name2* como pilha. O caractere inicial de cada construção aninhada é colocado no grupo e em sua coleção <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType>. Quando o caractere de fechamento passa pela correspondência, seu caractere de abertura é removido do grupo e a coleção <xref:System.Text.RegularExpressions.Group.Captures%2A> diminui em um. Depois da correspondência dos caracteres de abertura e fechamento de todas as construções aninhadas, *name2* fica vazio.  
  
> [!NOTE]
> Depois de modificar a expressão regular do exemplo a seguir para usar o caractere de abertura e fechamento adequado de um constructo aninhado, você pode usá-lo para gerenciar a maioria dos constructo aninhados, como expressões matemáticas ou linhas do código do programa que incluem diversas chamadas de método aninhado.  
  
 O exemplo a seguir usa uma definição de grupo de balanceamento para estabelecer a correspondência entre os sinais de menor e maior (<>) em uma cadeia de caracteres de entrada. O exemplo define dois grupos nomeados, `Open` e `Close`, que são usados como pilha para acompanhar a correspondência entre esses sinais. Cada sinal de menor capturado é enviado para a coleção da captura do grupo `Open` e cada sinal de maior capturado é enviado para a coleção de captura do grupo `Close`. A definição do grupo de balanceamento assegura que há um sinal de maior para cada sinal de menor. Caso não haja, o subpadrão final `(?(Open)(?!))`, só será avaliado se o grupo `Open` não estiver vazio, ou seja, se todas as construções aninhadas não tiverem sido fechadas. Se o subpadrão final for avaliado, a correspondência falha, porque o subpadrão `(?!)` é uma asserção lookahead negativa de largura zero que sempre falha.  
  
 [!code-csharp[RegularExpressions.Language.Grouping#3](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/grouping3.cs#3)]
 [!code-vb[RegularExpressions.Language.Grouping#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/grouping3.vb#3)]  
  
 O padrão da expressão regular é:  
  
`^[^<>]*(((?'Open'<)[^<>]*)+((?'Close-Open'>)[^<>]*)+)*(?(Open)(?!))$`  
  
 A expressão regular é interpretada da seguinte forma:  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`^`|Começa no início da cadeia de caracteres.|  
|`[^<>]*`|Corresponde a zero ou mais caracteres que não são sinais de menor nem maior.|  
|`(?'Open'<)`|Corresponde a um sinal de menor e o atribui a um grupo nomeado `Open`.|  
|`[^<>]*`|Corresponde a zero ou mais caracteres que não são sinais de menor nem maior.|  
|`((?'Open'<)[^<>]*)+`|Corresponde uma ou mais ocorrências de um sinal de menor, seguida por zero ou mais caracteres que não são sinais de menor ou maior. Este é o segundo grupo de captura.|  
|`(?'Close-Open'>)`|Corresponde a um sinal de maior, atribui a subcadeia de caracteres entre o grupo `Open` e o grupo atual ao grupo `Close` e exclui a definição do grupo `Open`.|  
|`[^<>]*`|Corresponde a zero ou mais ocorrências de caracteres que não são sinais de menor ou maior.|  
|`((?'Close-Open'>)[^<>]*)+`|Corresponde a uma ou mais ocorrências de sinal de menor seguidas por zero ou mais ocorrências de qualquer caractere que não seja um sinal de menor ou maior. Ao estabelecer a correspondência de um sinal de maior, atribua a subcadeia de caracteres entre o grupo `Open` e o grupo atual ao grupo `Close` e exclua a definição do grupo `Open`. Este é o terceiro grupo de captura.|  
|`(((?'Open'<)[^<>]*)+((?'Close-Open'>)[^<>]*)+)*`|Corresponde a zero ou mais ocorrências do seguinte padrão: uma ou mais ocorrências de sinal de menor seguidas por zero ou mais ocorrências de qualquer caractere que não seja um sinal de menor ou maior, seguida por uma ou mais ocorrências de sinal de maior, seguida por zero ou mais ocorrências de caracteres que não sejam sinais de menor ou maior. Ao estabelecer a correspondência de um sinal de maior, exclua a definição do grupo `Open` e atribua a subcadeia entre o grupo `Open` e o grupo atual ao grupo `Close`. Este é o primeiro grupo de captura.|  
|`(?(Open)(?!))`|Se o grupo `Open` existir, abandone a correspondência se for possível estabelecer a correspondência de uma cadeia de caracteres vazia, mas não avance a posição do mecanismo de expressão regular na cadeia de caracteres. Trata-se de uma asserção lookahead negativa de largura zero. Como sempre há cadeias de caracteres vazias presentes em cadeias de caracteres de entrada, essa correspondência sempre falha. A falha nessa correspondência indica que os sinais de menor e maior não estão balanceados.|  
|`$`|Corresponder ao final da cadeia de caracteres de entrada.|  
  
 A subexpressão final, `(?(Open)(?!))`, indica se a construção de aninhamento da cadeia de caracteres de entrada está balanceada corretamente (por exemplo, se cada sinal de maior corresponde a um sinal de menor). Ela usa a correspondência condicional com base em um grupo capturado válido; para obter mais informações, veja [Construções de alternância](alternation-constructs-in-regular-expressions.md). Se o grupo `Open` for definido, o mecanismo de expressão regular tenta estabelecer a correspondência da subexpressão `(?!)` na cadeia de caracteres de saída. O grupo `Open` só deve ser definido se as construções de aninhamento não estiverem balanceadas. Portanto, o padrão para correspondência na cadeia de caracteres de entrada sempre deve fazer com que a correspondência falhe. Nesse caso, `(?!)` é uma asserção lookahead negativa de largura zero que sempre falha, porque sempre há cadeias de caracteres vazias presentes na próxima posição da cadeia de caracteres de entrada.  
  
 No exemplo, o mecanismo da expressão regular avalia a cadeia de caracteres de entrada "\<abc><mno\<xyz>>", conforme mostra a tabela a seguir.  
  
|Etapa|Padrão|Result|  
|----------|-------------|------------|  
|1|`^`|Começa a correspondência no início da cadeia de caracteres de entrada|  
|2|`[^<>]*`|Procura caracteres que não sejam sinais de menor e maior antes do sinal de menor e não encontra correspondências.|  
|3|`(((?'Open'<)`|Corresponde ao sinal de menor em "\<abc>" e o atribui ao grupo `Open`.|  
|4|`[^<>]*`|Corresponde a "abc".|  
|5|`)+`|"<abc" é o valor do segundo grupo capturado.<br /><br /> O próximo caractere na cadeia de caracteres de entrada não é um sinal de menor. Por isso, o mecanismo de expressão regular não volta para o subpadrão `(?'Open'<)[^<>]*)`.|  
|6|`((?'Close-Open'>)`|Corresponde ao sinal de maior de "\<abc>", atribui "abc", que é a subcadeia de caracteres entre o grupo `Open` e o sinal de menor, para o grupo `Close` e exclui o valor atual ("<") do grupo `Open`, deixando-o vazio.|  
|7|`[^<>]*`|Procura caracteres que não sejam sinais de menor e maior depois do sinal de maior e não encontra correspondências.|  
|8|`)+`|O valor do terceiro grupo capturado é ">".<br /><br /> O próximo caractere na cadeia de caracteres de entrada não é um sinal de maior. Por isso, o mecanismo de expressão regular não volta para o subpadrão `((?'Close-Open'>)[^<>]*)`.|  
|9|`)*`|O valor do primeiro grupo capturado é "\<abc>".<br /><br /> O próximo caractere na cadeia de caracteres de entrada é um sinal de menor. Por isso, o mecanismo de expressão regular volta para o subpadrão `(((?'Open'<)`.|  
|10|`(((?'Open'<)`|Corresponde ao colchete angular esquerdo em " \<mno" and assigns it to the `Open` group. Its <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> a coleção agora tem um único valor" < ".|  
|11|`[^<>]*`|Corresponde a "mno".|  
|12|`)+`|"<mno" é o valor do segundo grupo capturado.<br /><br /> O próximo caractere na cadeia de caracteres de entrada é um sinal de menor. Por isso, o mecanismo de expressão regular volta para o subpadrão `(?'Open'<)[^<>]*)`.|  
|13|`(((?'Open'<)`|Corresponde ao sinal de menor em "\<xyz>" e o atribui ao grupo `Open`. A <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> coleção do `Open` grupo agora inclui duas capturas: o colchete angular esquerdo de " \<mno", and the left angle bracket from "\<xyz> ".|  
|14|`[^<>]*`|Corresponde a "xyz".|  
|15|`)+`|"<xyz" é o valor do segundo grupo capturado.<br /><br /> O próximo caractere na cadeia de caracteres de entrada não é um sinal de menor. Por isso, o mecanismo de expressão regular não volta para o subpadrão `(?'Open'<)[^<>]*)`.|  
|16|`((?'Close-Open'>)`|Corresponde ao sinal de maior em "\<xyz>". "xyz" atribui a subcadeia de caracteres entre o grupo `Open` e o sinal de maior ao grupo `Close` e exclui o valor atual do grupo `Open`. O valor da captura anterior (o colchete angular esquerdo em " \<mno") becomes the current value of the `Open` group. The <xref:System.Text.RegularExpressions.Group.Captures%2A> a coleção do `Open` grupo agora inclui uma única captura, o colchete angular esquerdo de" \<xyz> ".|  
|17|`[^<>]*`|Procura caracteres que não sejam sinais de menor e maior e não encontra correspondências.|  
|18|`)+`|O valor do terceiro grupo capturado é ">".<br /><br /> O próximo caractere na cadeia de caracteres de entrada é um sinal de maior. Por isso, o mecanismo de expressão regular volta para o subpadrão `((?'Close-Open'>)[^<>]*)`.|  
|19|`((?'Close-Open'>)`|Corresponde ao sinal de maior final de "xyz>>", atribui "mno\<xyz>" (a subcadeia de caracteres entre o grupo `Open` e o sinal de maior) ao grupo `Close` e exclui o valor atual do grupo `Open`. Agora, o grupo `Open` está vazio.|  
|20|`[^<>]*`|Procura caracteres que não sejam sinais de menor e maior e não encontra correspondências.|  
|21|`)+`|O valor do terceiro grupo capturado é ">".<br /><br /> O próximo caractere na cadeia de caracteres de entrada não é um sinal de maior. Por isso, o mecanismo de expressão regular não volta para o subpadrão `((?'Close-Open'>)[^<>]*)`.|  
|22|`)*`|O valor do primeiro grupo capturado é "<mno\<xyz>>".<br /><br /> O próximo caractere na cadeia de caracteres de entrada não é um sinal de menor. Por isso, o mecanismo de expressão regular não volta para o subpadrão `(((?'Open'<)`.|  
|23|`(?(Open)(?!))`|O grupo `Open` não é definido. Por isso, não há tentativa de correspondência.|  
|24|`$`|Corresponde ao final da cadeia de caracteres de entrada.|  
  
<a name="noncapturing_group"></a>
## <a name="noncapturing-groups"></a>Grupos de Não Captura  
 O constructo de grupo a seguir não captura a subcadeia de caracteres correspondente a uma subexpressão:  
  
`(?:subexpression)`
  
 em que *subexpression* é qualquer padrão de expressão regular válido. O constructo de grupo de não captura geralmente é usada quando um quantificador é aplicado a um grupo, mas as subcadeias de caracteres capturadas pelo grupo não são úteis.  
  
> [!NOTE]
> Se uma expressão regular inclui constructos aninhados de agrupamento, um constructo de grupo de não captura externo não se aplica às construções aninhadas internas de grupo.  
  
 O exemplo a seguir mostra uma expressão regular que inclui grupos de não captura. O resultado não inclui grupos capturados.  
  
 [!code-csharp[RegularExpressions.Language.Grouping#5](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/noncapture1.cs#5)]
 [!code-vb[RegularExpressions.Language.Grouping#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/noncapture1.vb#5)]  
  
 A expressão regular `(?:\b(?:\w+)\W*)+\.` corresponde a uma sentença terminada por um ponto final. Como a expressão regular concentra-se em sentenças, e não em palavras, as construções de agrupamento são usadas apenas como quantificadores. O padrão da expressão regular é interpretado conforme a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`(?:\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. Não atribui o texto coincidente a um grupo capturado.|  
|`\W*`|Corresponde a zero ou mais caracteres que não compõem palavras.|  
|`(?:\b(?:\w+)\W*)+`|Corresponde ao padrão de um ou mais caracteres de palavra que começam com um limite de palavra, seguido por zero ou um espaço em branco uma ou mais vezes. Não atribui o texto coincidente a um grupo capturado.|  
|`\.`|Corresponde a um ponto final.|  
  
<a name="group_options"></a>
## <a name="group-options"></a>Opções de Grupo  
 O constructo de agrupamento a seguir aplica ou desabilita as opções especificadas em uma subexpressão:  
  
 `(?imnsx-imnsx:`*subexpressão*`)`  
  
 em que *subexpression* é qualquer padrão de expressão regular válido. Por exemplo, `(?i-s:)` ativa a diferenciação de maiúsculas e minúsculas e desabilita o modo de linha única. Para obter mais informações sobre as opções embutidas que você pode especificar, consulte [Opções de expressão regular](regular-expression-options.md).  
  
> [!NOTE]
> Você pode especificar opções que se aplicam a toda uma expressão regular, em vez de a uma subexpressão, usando um construtor de classe <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> ou método estático. Você também pode especificar opções embutidas que são aplicadas após um ponto específico de uma expressão regular, usando o constructo de linguagem `(?imnsx-imnsx)`.  
  
 O constructo de opções de grupo não é um grupo de captura. Ou seja, embora todas as porções de uma cadeia de caracteres que são capturadas pela *subexpressão* sejam incluídas na correspondência, elas não são incluídas em grupos capturados, nem usadas para preencher o objeto <xref:System.Text.RegularExpressions.GroupCollection>.  
  
 Por exemplo, a expressão regular `\b(?ix: d \w+)\s` no exemplo a seguir usa opções embutidas em um constructo de agrupamento para habilitar a correspondência sem diferenciação de maiúsculas e minúsculas e ignorar os espaços em branco do padrão, ao identificar todas as palavras que começam com a letra "d". A expressão regular é definida como mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`(?ix: d \w+)`|Usa a correspondência sem diferenciar letras maiúsculas e minúsculas e ignora espaços em branco nesse padrão e corresponde um "d" seguido por um ou mais caracteres que compõem palavras.|  
|`\s`|Corresponde a um caractere de espaço em branco.|  
  
 [!code-csharp[Conceptual.Regex.Language.Options#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#8)]
 [!code-vb[Conceptual.Regex.Language.Options#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#8)]  
  
<a name="zerowidth_positive_lookahead_assertion"></a>
## <a name="zero-width-positive-lookahead-assertions"></a>Asserções Lookahead Positivas de Largura Zero  
 O constructo de agrupamento a seguir define uma asserção lookahead positiva de largura zero:  
  
 `(?=`*subexpressão*`)`  
  
 em que *subexpression* é qualquer padrão de expressão regular. Para que a correspondência seja executada com êxito, a cadeia de caracteres de entrada deve corresponder ao padrão de expressão regular na *subexpression*, embora a subcadeia de caracteres com a qual a correspondência foi estabelecida não conste no resultado. A asserção lookahead positiva de largura zero não retrocede.  
  
 Geralmente, as asserções desse tipo podem ser encontradas no final de um padrão de expressão regular. Isso define uma subcadeia de caracteres que deve estar presente no final da cadeia de caracteres para que seja possível estabelecer a correspondência, mas que não seja incluída na correspondência. Isso também é útil para evitar retrocessos em excesso. Você pode usar asserções lookahead positivas de largura zero que garantam que um grupo capturado específico seja iniciado por um texto que corresponda a um subconjunto do padrão definido para o grupo capturado. Por exemplo, se um grupo de captura corresponder a caracteres de palavras em sequência, você pode usar uma asserção desse tipo para que o primeiro caractere seja uma caractere alfabético maiúsculo.  
  
 O exemplo a seguir usa asserção lookahead positiva de largura zero para estabelecer a correspondência da palavra que precede o verbo "is" na cadeia de caracteres de entrada.  
  
 [!code-csharp[RegularExpressions.Language.Grouping#6](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/lookahead1.cs#6)]
 [!code-vb[RegularExpressions.Language.Grouping#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/lookahead1.vb#6)]  
  
 A expressão regular `\b\w+(?=\sis\b)` é interpretada conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`\w+`|Fazer a correspondência a um ou mais caracteres de palavra.|  
|`(?=\sis\b)`|Determina se os caracteres que compõem palavras são seguidos por um caractere de espaço em branco e pela cadeia de caracteres "is", que termina com um limite de palavra. Nesse caso, a correspondência ocorreu com êxito.|  
  
<a name="zerowidth_negative_lookahead_assertion"></a>
## <a name="zero-width-negative-lookahead-assertions"></a>Asserções Lookahead Negativas de Largura Zero  
 O constructo de agrupamento a seguir define uma asserção lookahead negativa de largura zero:  
  
 `(?!`*subexpressão*`)`  
  
 em que *subexpression* é qualquer padrão de expressão regular. Para que a correspondência seja executada com êxito, a cadeia de caracteres de entrada não deve corresponder ao padrão de expressão regular na *subexpression*, embora a cadeia de caracteres com a qual a correspondência foi estabelecida não conste no resultado.  
  
 Geralmente, as asserções desse tipo podem ser encontradas no início ou no final de uma expressão regular. No início da expressão regular, elas podem definir um padrão específico que não deve ser correspondido quando o início da expressão regular definir um padrão parecido, mas mais geral, para a correspondência. Nesse caso, ela geralmente é usada para limitar o retrocesso. No final de uma expressão regular, pode definir uma subexpressão que pode não ocorrer no final de uma correspondência.  
  
 O exemplo a seguir define uma expressão regular que usa uma asserção lookahead de largura zero no início da expressão regular para fazer a correspondência de palavras que não começam com "un".  
  
 [!code-csharp[RegularExpressions.Language.Grouping#7](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/negativelookahead1.cs#7)]
 [!code-vb[RegularExpressions.Language.Grouping#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/negativelookahead1.vb#7)]  
  
 A expressão regular `\b(?!un)\w+\b` é interpretada conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`(?!un)`|Determina se os dois caracteres seguintes são "un". Caso não sejam, é possível estabelecer a correspondência.|  
|`\w+`|Fazer a correspondência a um ou mais caracteres de palavra.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
  
 O exemplo a seguir define uma expressão regular que usa uma asserção lookahead de largura zero no final da expressão regular para fazer a correspondência de palavras que não terminam com um caractere de pontuação.  
  
 [!code-csharp[RegularExpressions.Language.Grouping#8](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/negativelookahead2.cs#8)]
 [!code-vb[RegularExpressions.Language.Grouping#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/negativelookahead2.vb#8)]  
  
 A expressão regular `\b\w+\b(?!\p{P})` é interpretada conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`\w+`|Fazer a correspondência a um ou mais caracteres de palavra.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
|`\p{P})`|Se o caractere seguinte não for um símbolo de pontuação (por exemplo, um ponto final ou uma vírgula), a correspondência é estabelecida.|  
  
<a name="zerowidth_positive_lookbehind_assertion"></a>
## <a name="zero-width-positive-lookbehind-assertions"></a>Asserções Lookbehind Positivas de Largura Zero  
 O constructo de agrupamento a seguir define uma asserção lookbehind positiva de largura zero:  
  
 `(?<=`*subexpressão*`)`  
  
 em que *subexpression* é qualquer padrão de expressão regular. Para que a correspondência seja executada com êxito, a *subexpressão* deve ocorrer na cadeia de caracteres de entrada à esquerda da posição atual, embora `subexpression` não conste no resultado. A asserção lookbehind positiva de largura zero não retrocede.  
  
 As asserções lookbehind positivas de largura zero geralmente são usadas no início de expressões regulares. O padrão que elas definem é uma pré-condição para a correspondência, embora não constem no resultado.  
  
 Por exemplo, o exemplo a seguir estabelece a correspondência para os dois últimos dígitos de ano no século XXI (ou seja, ele requer que os dígitos "20" apareçam antes da cadeia de caracteres coincidente).  
  
 [!code-csharp[RegularExpressions.Language.Grouping#9](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/lookbehind1.cs#9)]
 [!code-vb[RegularExpressions.Language.Grouping#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/lookbehind1.vb#9)]  
  
 O padrão da expressão regular `(?<=\b20)\d{2}\b` é interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\d{2}`|Corresponde a dois dígitos decimais.|  
|`(?<=\b20)`|Continua a estabelecer a correspondência se os dois dígitos decimais forem precedidos por dígitos decimais "20" em um limite de palavra.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
  
 As asserções lookbehind positivas de largura zero também são usadas para limitar o retrocesso quando o último caractere de um grupo capturado precisar ser um subconjunto de caracteres que corresponde ao padrão de expressão regular desse grupo. Por exemplo, se um grupo captura todos os caracteres de palavras em sequência, você pode usar uma asserção lookbehind positivas de largura zero tipo para que o primeiro caractere seja uma letra.  
  
<a name="zerowidth_negative_lookbehind_assertion"></a>
## <a name="zero-width-negative-lookbehind-assertions"></a>Asserções Lookbehind Negativas de Largura Zero  
 O constructo de agrupamento a seguir define uma asserção lookbehind negativa de largura zero:  
  
 `(?<!`*subexpressão*`)`  
  
 em que *subexpression* é qualquer padrão de expressão regular. Para que a correspondência seja executada com êxito, a *subexpressão* não deve ocorrer na cadeia de caracteres de entrada à esquerda da posição atual. No entanto, todas as subcadeias de caracteres que não corresponderem a `subexpression`, não serão incluídas no resultado.  
  
 As asserções lookbehind negativas de largura zero geralmente são usadas no início de expressões regulares. O padrão definido por elas elimina uma correspondência na cadeia de caracteres seguinte. Elas também são usadas para limitar o retrocesso quando o último caractere de um grupo capturado não precisar ser um ou mais caracteres que corresponde ao padrão de expressão regular desse grupo. Por exemplo, se um grupo capturar todos os caracteres de palavras em sequência, você poderá usar uma asserção lookbehind positiva de largura zero para que o primeiro caractere não seja um sublinhado (\_).  
  
 O exemplo a seguir estabelece a correspondência de data para qualquer dia da semana que não seja sábado nem domingo.  
  
 [!code-csharp[RegularExpressions.Language.Grouping#10](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/negativelookbehind1.cs#10)]
 [!code-vb[RegularExpressions.Language.Grouping#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/negativelookbehind1.vb#10)]  
  
 O padrão da expressão regular `(?<!(Saturday|Sunday) )\b\w+ \d{1,2}, \d{4}\b` é interpretado conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`\w+`|Corresponde a um ou mais caracteres de palavra seguido por um espaço em branco.|  
|`\d{1,2},`|Corresponde a um ou dois dígitos decimais seguidos por uma caractere de espaço em branco e uma vírgula.|  
|`\d{4}\b`|Corresponde a quatro dígitos decimais e encerra a correspondência em um limite de palavra.|  
|<code>(?<!(Saturday&#124;Sunday) )</code>|Se a correspondência for precedida por algo que não seja as cadeias de caracteres "Sábado" ou "Domingo" seguidas por um espaço, a correspondência é realizada com êxito.|  
  
<a name="atomic_groups"></a>
## <a name="atomic-groups"></a>Grupos atômicos  
 A construção de agrupamento a seguir representa um grupo atômico (conhecido em alguns outros mecanismos de expressão regular como uma subexpressão sem retrocesso, uma subexpressão atômica ou uma subexpressão somente uma vez):
  
 `(?>`*subexpressão*`)`  
  
 em que *subexpression* é qualquer padrão de expressão regular.  
  
 Geralmente, se uma expressão regular incluir um padrão de correspondência opcional ou alternativo e a correspondência não for realizada com êxito, o mecanismo da expressão regular pode ramificar em diversas orientações para estabelecer a correspondência entre uma cadeia de caracteres de entrada e um padrão. Se a correspondência não for encontrada na primeira ramificação, o mecanismo da expressão regular pode voltar ou retroceder ao ponto da primeira correspondência e tentar estabelecer a correspondência usando a segunda ramificação. Esse processo pode continuar até que as tentativas se esgotem.  
  
 A `(?>` construção da linguagem de *subexpressão* `)` desabilita a retrocesso. O mecanismo de expressão regular estabelece a correspondência entre o máximo de caracteres da cadeia de caracteres de entrada. Quando não for possível estabelecer outras correspondências, ele não retrocede para tentar alternativas. Ou seja, a subexpressão corresponde somente às cadeias de caracteres que poderiam corresponder somente à subexpressão. Ela não tenta fazer a correspondência de uma cadeia de caracteres com base na subexpressão em questão e nas subexpressões seguintes.  
  
 Essa opção é recomendada quando você sabe que o retrocesso não terá êxito. Evitar que o mecanismo de expressão regular faça pesquisas desnecessárias, melhora o desempenho.  
  
 O exemplo a seguir ilustra como um grupo atômico modifica os resultados de uma correspondência de padrão. A expressão regular de retrocesso estabelece a correspondência de diversos caracteres repetidos, seguidos por outra ocorrência do mesmo caractere de um limite de palavra, mas a expressão regular sem retrocesso não faz isso.  
  
 [!code-csharp[RegularExpressions.Language.Grouping#11](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/nonbacktracking1.cs#11)]
 [!code-vb[RegularExpressions.Language.Grouping#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/nonbacktracking1.vb#11)]  
  
 A expressão regular sem retrocesso `(?>(\w)\1+).\b` é definida como mostra a tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`(\w)`|Corresponde a um único caractere que compõe palavras e o atribui ao primeiro grupo de captura.|  
|`\1+`|Corresponde ao valor da primeira subcadeia de caracteres de captura uma ou mais vezes.|  
|`.`|Corresponde a qualquer caractere.|  
|`\b`|Termina a correspondência em um limite de palavra.|  
|`(?>(\w)\1+)`|Corresponde a uma ou mais ocorrências de um caractere de palavra duplicado, mas não executa o retrocesso para estabelecer a correspondência com o último caractere em um limite de palavra.|  
  
<a name="Objects"></a>
## <a name="grouping-constructs-and-regular-expression-objects"></a>Agrupando Constructos e Objetos de Expressão Regulares  
 As subcadeias de caracteres que correspondem a um grupo de captura de expressão regular são representadas por objetos <xref:System.Text.RegularExpressions.Group?displayProperty=nameWithType>, que podem ser recuperados do objeto <xref:System.Text.RegularExpressions.GroupCollection?displayProperty=nameWithType> que é retornado pela propriedade <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType>. O objeto <xref:System.Text.RegularExpressions.GroupCollection> é preenchido desta forma:  
  
- O primeiro objeto <xref:System.Text.RegularExpressions.Group> da coleção (o objeto de índice zero) representa a correspondência total.  
  
- O conjunto seguinte de objetos <xref:System.Text.RegularExpressions.Group> representa os grupos de captura não nomeados (numerados). Eles aparecem na ordem em que são definidos na expressão regular, da esquerda para a direita. Os valores dos índices desses grupos variam de 1 até o número de grupos de captura não nomeados presentes na coleção. O índice de um determinado grupo equivale a sua referência inversa numerada. Para obter mais informações sobre referências anteriores, consulte [construções de referência anterior](backreference-constructs-in-regular-expressions.md).)  
  
- O conjunto final de objetos <xref:System.Text.RegularExpressions.Group> representa os grupos de captura nomeados. Eles aparecem na ordem em que são definidos na expressão regular, da esquerda para a direita. O valor do índice do primeiro grupo de captura nomeado é um número maior que o índice do último grupo de captura não nomeado. Se não houver grupos de captura não nomeados na expressão regular, o valor do índice do primeiro grupo de captura nomeado será um.  
  
 Se você aplicar um quantificador a um grupo de captura, as propriedades <xref:System.Text.RegularExpressions.Group><xref:System.Text.RegularExpressions.Capture.Value%2A?displayProperty=nameWithType>, <xref:System.Text.RegularExpressions.Capture.Index%2A?displayProperty=nameWithType> e <xref:System.Text.RegularExpressions.Capture.Length%2A?displayProperty=nameWithType> do objeto refletirão a última subcadeia de caracteres capturada por um grupo de captura. Você pode recuperar todo o conjunto de subcadeias de caracteres capturadas por grupos que têm quantificadores do objeto <xref:System.Text.RegularExpressions.CaptureCollection> e que são retornados pela propriedade <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType>.  
  
 O exemplo a seguir esclarece a relação entre os objetos <xref:System.Text.RegularExpressions.Group> e <xref:System.Text.RegularExpressions.Capture>.  
  
 [!code-csharp[RegularExpressions.Language.Grouping#4](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.grouping/cs/objectmodel1.cs#4)]
 [!code-vb[RegularExpressions.Language.Grouping#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.grouping/vb/objectmodel1.vb#4)]  
  
 O padrão da expressão regular `(\b(\w+)\W+)+` extrai palavras, uma a uma, da cadeia de caracteres. Ele é definido conforme mostrado na tabela a seguir.  
  
|Padrão|Descrição|  
|-------------|-----------------|  
|`\b`|Começar a correspondência em um limite de palavra.|  
|`(\w+)`|Fazer a correspondência a um ou mais caracteres de palavra. Juntos, esses caracteres compõem uma palavra. Este é o segundo grupo de captura.|  
|`\W+`|Estabeleça a correspondência com um ou mais caracteres que não compõem palavras.|  
|`(\b(\w+)\W+)`|Corresponde ao padrão de um ou mais caracteres de palavra, seguido por um ou mais caracteres que não compõem palavras, uma ou mais vezes. Este é o primeiro grupo de captura.|  
  
 O segundo grupo de captura corresponde a cada palavra da frase. O primeiro grupo de captura corresponde a cada palavra com a pontuação e o espaço em branco que a segue. O objeto <xref:System.Text.RegularExpressions.Group>, cujo índice é 2, fornece informações sobre o texto correspondente ao segundo grupo de captura. O conjunto completo de palavras capturadas pelo grupo de captura está disponível no objeto <xref:System.Text.RegularExpressions.CaptureCollection>, retornado pela propriedade <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType>.  
  
## <a name="see-also"></a>Confira também

- [Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)
- [Retrocesso](backtracking-in-regular-expressions.md)
