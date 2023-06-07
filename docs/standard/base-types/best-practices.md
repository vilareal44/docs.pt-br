---
title: Práticas recomendadas para expressões regulares no .NET
description: Saiba como criar expressões regulares eficientes e eficazes no .NET.
ms.date: 06/30/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- .NET Framework regular expressions, best practices
- regular expressions, best practices
ms.assetid: 618e5afb-3a97-440d-831a-70e4c526a51c
ms.openlocfilehash: 30d4a8f6ddc4ae1f83f5c0802e872661cbe6c6f1
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85802918"
---
# <a name="best-practices-for-regular-expressions-in-net"></a>Práticas recomendadas para expressões regulares no .NET

O mecanismo de expressões regulares no .NET é uma ferramenta poderosa e repleta de recursos que processa o texto com base em correspondências de padrões em vez de em comparar e corresponder o texto literal. Na maioria dos casos, ele realiza a correspondência de padrões de forma rápida e eficiente. No entanto, em alguns casos, o mecanismo de expressões regulares pode parecer ser muito lento. Em casos extremos, pode até mesmo parecer parar de responder enquanto processa uma entrada relativamente pequena em um período de horas ou até mesmo dias.

Este tópico descreve algumas das práticas recomendadas que os desenvolvedores podem adotar para garantir que as expressões regulares obtenham o máximo de desempenho.

[!INCLUDE [regex](../../../includes/regex.md)]

## <a name="consider-the-input-source"></a>Considere a fonte de entrada

Em geral, as expressões regulares podem aceitar dois tipos de entrada: restrita e não restrita. Uma entrada restrita é o texto proveniente de uma fonte conhecida ou confiável e segue um formato predefinido. Uma entrada irrestrita é um texto que se origina de uma origem não confiável como um usuário não confiável e não pode seguir um formato predefinido ou esperado.

Os padrões de expressões regulares geralmente são escritos para corresponder a entradas válidas. Ou seja, os desenvolvedores examinam o texto que desejam corresponder e escrevem um padrão de expressão regular que corresponde a ele. Os desenvolvedores então determinam se esse padrão requer correção ou uma elaboração adicional testando-o com vários itens de entrada válidos. Quando o padrão corresponde a todas as entradas válidas presumidas, é declarado como pronto para produção e pode ser incluído em um aplicativo final. Isso torna um padrão de expressão regular apropriado para corresponder uma entrada restrita. No entanto, ele não o torna adequado para corresponder à entrada sem restrição.

Para corresponder a uma entrada sem restrição, uma expressão regular deve ser capaz de manipular três tipos de texto:

- Texto que corresponda ao padrão da expressão regular.

- Texto que não corresponda ao padrão da expressão regular.

- Texto que quase corresponda ao padrão da expressão regular.

O último tipo de texto é particularmente problemático para uma expressão regular que foi escrita para lidar com entradas restritas. Se essa expressão regular também depender de [retrocesso](backtracking-in-regular-expressions.md) abrangente, o mecanismo de expressões regulares poderá gastar um período fora do normal (em alguns casos, muitas horas ou dias) processando texto aparentemente inócuo.

> [!WARNING]
> O exemplo a seguir usa uma expressão regular que é propensa a retrocesso excessivo e que pode rejeitar endereços de email válidos. Você não deve usá-lo em uma rotina de validação de email. Se você desejar uma expressão regular que valida endereços de email, confira [Como verificar se cadeias de caracteres estão em um formato de email válido](how-to-verify-that-strings-are-in-valid-email-format.md).

Por exemplo, considere uma expressão regular muito usada, mas extremamente problemática, para validar o alias de um endereço de email. A expressão regular `^[0-9A-Z]([-.\w]*[0-9A-Z])*$` é escrita para processar o que é considerado um endereço de email válido, o que consiste em um caractere alfanumérico seguido por zero ou mais caracteres que podem ser alfanuméricos, pontos ou hifens. A expressão regular deve terminar com um caractere alfanumérico. No entanto, como mostra o exemplo a seguir, embora esta expressão regular manipule entradas válidas facilmente, seu desempenho é muito ineficiente ao processar uma entrada quase válida.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/design2.cs#1)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/design2.vb#1)]

Conforme mostrado pela saída do exemplo, o mecanismo de expressões regulares processa o alias de email válido quase no mesmo tempo, independentemente do seu comprimento. Por outro lado, quando o endereço de email quase válido tem mais de cinco caracteres, o tempo de processamento chega a dobrar para cada caractere adicional na cadeia de caracteres. Isso significa que uma cadeia de 28 caracteres quase válidos levaria mais de uma hora para ser processada e uma cadeia de 33 caracteres quase válidos demoraria quase um dia.

Como essa expressão regular foi desenvolvida considerando apenas a correspondência com o formato da entrada, ela não leva em consideração entradas que não correspondem ao padrão. Isso, por sua vez, pode permitir que entradas não restritas quase correspondentes ao padrão da expressão regular prejudiquem significativamente o desempenho.

Para resolver este problema, você pode fazer o seguinte:

- Ao desenvolver um padrão, você deve considerar como o retrocesso pode afetar o desempenho do mecanismo de expressões regulares, especialmente se a expressão regular for criada para processar entradas sem restrição. Para obter mais informações, consulte a seção [fazer encargos de retrocesso](#take-charge-of-backtracking) .

- Teste rigorosamente sua expressão regular usando entradas inválidas e quase válidas, bem como entradas válidas. Para gerar entradas para uma expressão regular específica aleatoriamente, você pode usar [Rex](https://www.microsoft.com/research/project/rex-regular-expression-exploration/), que é uma ferramenta de exploração de expressões regulares da Microsoft Research.

## <a name="handle-object-instantiation-appropriately"></a>Trate a instanciação de objetos adequadamente

No coração do modelo de objeto de expressões regulares do .NET está a classe <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType>, a qual representa o mecanismo de expressões regulares. Muitas vezes, o maior fator individual que afeta o desempenho das expressões regulares é a forma como o mecanismo <xref:System.Text.RegularExpressions.Regex> é usado. Definir uma expressão regular envolve o acoplamento vigoroso do mecanismo de expressões regulares com um padrão de expressão regular. Esse processo de acoplamento, seja envolvendo a instanciação de um objeto <xref:System.Text.RegularExpressions.Regex> ao passar para seu construtor um padrão de expressão regular ou chamando um método estático ao passar o padrão de expressão regular com a cadeia de caracteres a ser analisada, é necessariamente caro.

> [!NOTE]
> Para uma discussão mais detalhada das implicações de desempenho do uso de expressões regulares interpretadas e compiladas, confira [Otimizando o desempenho de expressões regulares, parte II: Tomando conta do retrocesso](https://docs.microsoft.com/archive/blogs/bclteam/optimizing-regular-expression-performance-part-ii-taking-charge-of-backtracking-ron-petrusha) no blog da equipe de BCL.

É possível acoplar o mecanismo de expressões regulares com um padrão específico de expressão regular e usar o mecanismo para fazer a correspondência com texto de várias maneiras:

- Você pode chamar um método estático de correspondência de padrões, <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%29?displayProperty=nameWithType>. Isso não requer a instanciação de um objeto de expressão regular.

- É possível criar uma instância de um objeto <xref:System.Text.RegularExpressions.Regex> e chamar um método instanciado de correspondência de padrões de uma expressão regular interpretada. Esse é o método padrão para associar o mecanismo de expressões regulares a uma expressão regular padrão. Ele ocorre quando um objeto <xref:System.Text.RegularExpressions.Regex> é instanciado sem um argumento `options` que inclua o sinalizador <xref:System.Text.RegularExpressions.RegexOptions.Compiled>.

- Você pode criar uma instância de um objeto <xref:System.Text.RegularExpressions.Regex> e chamar um método instanciado de correspondência de padrões de uma expressão regular compilada. Os objetos de expressão regular representam padrões compilados quando um objeto <xref:System.Text.RegularExpressions.Regex> é instanciado com um argumento `options` que inclui o sinalizador <xref:System.Text.RegularExpressions.RegexOptions.Compiled>.

- Você pode criar um objeto <xref:System.Text.RegularExpressions.Regex> com propósito especial que é fortemente acoplado a um padrão de expressão regular específico, compilá-lo e salvá-lo em um assembly autônomo. Você pode fazer ao chamar o método <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType>.

A maneira específica como você chama métodos de correspondência de expressões regulares pode ter impacto significativo em seu aplicativo. As seções a seguir abordam quando usar chamadas de métodos estáticos, expressões regulares interpretadas e expressões regulares compiladas para melhorar o desempenho do seu aplicativo.

> [!IMPORTANT]
> A forma da chamada de método (estático, interpretada, compilada) afeta o desempenho se a mesma expressão regular é usada repetidamente em chamadas de método ou se um aplicativo faz uso extensivo de objetos de expressão regular.

### <a name="static-regular-expressions"></a>Expressões regulares estáticas

Os métodos de expressões regulares estáticas são recomendados como uma alternativa a criar repetidamente instâncias de um objeto de expressão regular com a mesma expressão regular. Ao contrário dos padrões de expressão regulares usados por objetos de expressão regular, os códigos de operação ou a MSIL (Microsoft Intermediate Language) compilada dos padrões usados em chamadas de método estáticos são armazenados em cache internamente pelo mecanismo de expressão regular.

Por exemplo, um manipulador de eventos chama frequentemente outro método para validar a entrada do usuário. Isso é refletido no código a seguir, no qual o evento <xref:System.Windows.Forms.Button> de um controle <xref:System.Windows.Forms.Control.Click> é usado para chamar um método `IsValidCurrency`, o qu al verifica se o usuário inseriu um símbolo de moeda seguido por pelo menos um dígito decimal.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/static1.cs#2)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/static1.vb#2)]

Uma implementação bem ineficiente do método `IsValidCurrency` é mostrada no exemplo a seguir. Observe que cada chamada de método reinstancia um objeto <xref:System.Text.RegularExpressions.Regex> com o mesmo padrão. Isso, por sua vez, significa que o padrão de expressão regular deve ser recompilado toda vez que o método é chamado.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/static1.cs#3)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/static1.vb#3)]

Você deve substituir esse código ineficiente por uma chamada ao método estático <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%29?displayProperty=nameWithType>. Isso elimina a necessidade de criar uma instância de um objeto <xref:System.Text.RegularExpressions.Regex> toda vez que você deseja chamar um método de correspondência de padrões e permite que o mecanismo de expressões regulares recupere uma versão compilada da expressão regular do cache.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/static2.cs#4)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/static2.vb#4)]

Por padrão, os 15 padrões de expressões regulares estáticas usados mais recentemente são armazenados no cache. Para aplicativos que requerem um número maior de expressões regulares estáticas armazenadas no cache, o tamanho do cache pode ser ajustado com a definição da propriedade <xref:System.Text.RegularExpressions.Regex.CacheSize%2A?displayProperty=nameWithType>.

A expressão regular `\p{Sc}+\s*\d+` que é usada neste exemplo verifica que a cadeia de caracteres de entrada consiste em um símbolo de moeda e pelo menos um dígito decimal. O padrão é definido conforme mostrado na tabela a seguir.

|Padrão|Descrição|
|-------------|-----------------|
|`\p{Sc}+`|Corresponde a um ou mais caracteres no símbolo Unicode, categoria de moeda.|
|`\s*`|Corresponder a zero ou mais caracteres de espaço em branco.|
|`\d+`|Corresponde a um ou mais dígitos decimais.|

### <a name="interpreted-vs-compiled-regular-expressions"></a>Expressões regulares compiladas vs. interpretadas

Os padrões de expressões regulares não são associados ao mecanismo de expressões regulares com a especificação da opção <xref:System.Text.RegularExpressions.RegexOptions.Compiled> são interpretados. Quando um objeto de expressão regular é instanciado, o mecanismo de expressões regulares converte a expressão regular em um conjunto de códigos de operação. Quando um método de instância é chamado, os códigos de operação são convertidos para MSIL e executados pelo compilador JIT. Da mesma forma, quando um método estático de expressão regular é chamado e a expressão regular não pode ser encontrada no cache, o mecanismo de expressão regular converte a expressão regular em um conjunto de códigos operacionais e os armazena no cache. Em seguida, converte esses códigos de operação para MSIL de modo que o compilador JIT possa executá-los. As expressões regulares interpretadas reduzem o tempo de inicialização ao custo de um tempo de execução mais lento. Devido a isso, eles são melhores utilizados quando a expressão regular é usada em um pequeno número de chamadas de método ou se o número exato de chamadas para métodos de expressão regular é desconhecido, mas com a expectativa de ser pequeno. À medida que número de chamadas de método aumenta, o ganho de desempenho do tempo de inicialização reduzido é superado pela velocidade de execução mais lenta.

Os padrões de expressões regulares associados ao mecanismo de expressões regulares com a especificação da opção <xref:System.Text.RegularExpressions.RegexOptions.Compiled> são compilados. Isso significa que, quando um objeto de expressão regular é instanciado ou quando um método de expressão regular estática é chamado e a expressão regular não pode ser encontrada no cache, o mecanismo de expressões regulares converte a expressão regular para um conjunto intermediário de códigos de operação, que em seguida é convertido para MSIL. Quando um método é chamado, o compilador JIT executa o MSIL. Em contraste com as expressões regulares interpretadas, as expressões regulares compiladas aumentam o tempo de inicialização, mas executam os métodos individuais de correspondência padrão de forma mais rápida. Como resultado, o benefício de desempenho que resulta da compilação da expressão regular aumenta em proporção ao número de métodos de expressões regulares chamados.

Para resumir, recomendamos que você use expressões regulares interpretadas ao chamar métodos de expressões regulares com uma expressão regular específica com relativa pouca frequência. Você deve usar expressões regulares compiladas ao chamar os métodos de expressões regulares com uma expressão regular específica com uma relativa frequência. É difícil determinar o limite exato em que as velocidades de execução mais lentas das expressões regulares interpretadas suplantam os ganhos proporcionados pelo tempo de inicialização reduzido ou o limite em que os tempos de inicialização mais lentos das expressões regulares compiladas suplantam os ganhos gerados por suas velocidades de execução mais rápida. O limite depende de vários fatores, incluindo a complexidade da expressão regular e dos dados específicos que são processados. Para determinar se expressões regulares interpretadas ou compiladas oferecem o melhor desempenho para seu cenário de aplicativo específico, você pode usar a classe <xref:System.Diagnostics.Stopwatch> para comparar seu tempo de execução.

O exemplo a seguir compara o desempenho de expressões regulares compiladas e interpretadas ao ler as dez primeiras frases e ao ler todas as frases no texto *The Financier* de Theodore Dreiser. Conforme mostrado pela saída do exemplo, quando apenas dez chamadas são feitas para os métodos correspondentes de expressão regular, uma expressão regular interpretada oferece um desempenho melhor do que uma expressão regular compilada. No entanto, uma expressão regular compilada oferece melhor desempenho quando um grande número de chamadas (neste caso, mais 13.000) é feito.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/compare1.cs#5)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/compare1.vb#5)]

O padrão de expressão regular usado neste exemplo, `\b(\w+((\r?\n)|,?\s))*\w+[.?:;!]`, é definido como mostrado na tabela a seguir.

|Padrão|Descrição|
|-------------|-----------------|
|`\b`|Começar a correspondência em um limite de palavra.|
|`\w+`|Fazer a correspondência a um ou mais caracteres de palavra.|
|<code>(\r?\n)&#124;,?\s)</code>|Corresponde a um zero ou um retorno de carro seguido por um caractere de nova linha, ou zero ou uma vírgula seguida por um caractere de espaço em branco.|
|<code>(\w+((\r?\n)&#124;,?\s))*</code>|Corresponde a zero ou mais ocorrências de um ou mais caracteres de palavra que são seguidos por zero ou por retornos de carro e por um caractere de nova linha ou por zero ou uma vírgula seguida por um caractere de espaço em branco.|
|`\w+`|Fazer a correspondência a um ou mais caracteres de palavra.|
|`[.?:;!]`|Corresponde a um ponto, um ponto de interrogação, dois-pontos, ponto-e-vírgula ou ponto de exclamação.|

### <a name="regular-expressions-compiled-to-an-assembly"></a>Expressões regulares: compiladas para um assembly

O .NET também permite criar um assembly que contém expressões regulares compiladas. Isso move a ocorrência de desempenho da compilação de expressões regulares do tempo de execução para o tempo de design. No entanto, também envolve trabalho extra: você deve definir as expressões regulares com antecedência e compilá-las em um assembly. O compilador pode então fazer referência a esse assembly ao compilar código-fonte que usa expressões regulares do assembly. Cada expressão regular compilada no assembly é representada por uma classe que deriva de <xref:System.Text.RegularExpressions.Regex>.

Para compilar expressões regulares em um assembly, é necessário chamar o método <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%28System.Text.RegularExpressions.RegexCompilationInfo%5B%5D%2CSystem.Reflection.AssemblyName%29?displayProperty=nameWithType> e passar a ele uma matriz de objetos <xref:System.Text.RegularExpressions.RegexCompilationInfo> que representam as expressões regulares que serão compiladas e um objeto de <xref:System.Reflection.AssemblyName> que contém informações sobre o assembly a ser criado.

Recomendamos que você compile as expressões regulares em um assembly nestas situações:

- Se você é um desenvolvedor de componentes que deseja criar uma biblioteca de expressões regulares reutilizáveis.

- Se você pretende que seus métodos de correspondência de padrões das expressões regulares sejam chamados um número indefinido de vezes -- de uma ou duas vezes a milhares ou dezenas de milhares de vezes. Ao contrário de expressões regulares compiladas ou interpretadas, as expressões regulares compiladas em assemblies separados oferecem desempenho consistente, independentemente do número de chamadas a métodos.

Se estiver usando expressões regulares compiladas para otimizar o desempenho, não use reflexão para criar o assembly, carregar o mecanismo de expressões regulares e executar os métodos de correspondência de padrões. Isso requer que você evite criar padrões de expressões regulares dinamicamente, e que você especifique as opções de correspondência de padrões (como correspondência de padrões sem diferenciação de maiúsculas e minúsculas) no momento em que o assembly é criado. Exige também que você separe o código que cria o assembly do código que usa a expressão regular.

O exemplo a seguir mostra como criar um assembly que contém uma expressão regular compilada. Ele cria um assembly chamado `RegexLib.dll` com uma única classe de expressão regular, `SentencePattern` , que contém o padrão de expressão regular de correspondência de sentença usado na seção de [expressões regulares interpretadas versus compiladas](#interpreted-vs-compiled-regular-expressions) .

[!code-csharp[Conceptual.RegularExpressions.BestPractices#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/compile1.cs#6)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/compile1.vb#6)]

Quando o exemplo é compilado em um executável e executado, ele cria um assembly denominado `RegexLib.dll`. A expressão regular é representada por uma classe denominada `Utilities.RegularExpressions.SentencePattern` que é derivada de <xref:System.Text.RegularExpressions.Regex>. O exemplo a seguir usa a expressão regular compilada para extrair as sentenças do texto *The Financier*, de Theodore Dreiser.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/compile2.cs#7)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/compile2.vb#7)]

## <a name="take-charge-of-backtracking"></a>Tome conta do retrocesso

Normalmente, o mecanismo de expressões regulares usa progressão linear para percorrer uma cadeia de caracteres de entrada e compará-la a uma expressão regular padrão. No entanto, quando quantificadores indefinidos, como `*`, `+` e `?` são usados em uma expressão regular padrão, o mecanismo de expressões regulares pode desistir de uma parte das correspondências parciais com êxito e retornar a um estado salvo anteriormente para procurar uma correspondência com êxito para o padrão inteiro. Esse processo é conhecido como retrocesso.

> [!NOTE]
> Para saber mais sobre o retrocesso, confira [Detalhes do comportamento de expressões regulares](details-of-regular-expression-behavior.md) e [Retrocesso](backtracking-in-regular-expressions.md). Para uma discussão mais detalhada do retrocesso, confira [Como otimizar o desempenho de expressões regulares, Parte II: Como tolar conta do retrocesso](https://docs.microsoft.com/archive/blogs/bclteam/optimizing-regular-expression-performance-part-ii-taking-charge-of-backtracking-ron-petrusha) no blog da equipe do BCL.

O suporte ao retrocesso proporciona poder e flexibilidade às expressões regulares. Ele também coloca a responsabilidade por controlar o funcionamento do mecanismo de expressões regulares nas mãos dos desenvolvedores de expressões regulares. Como os desenvolvedores geralmente não estão cientes dessa responsabilidade, o uso indevido do retrocesso ou a confiança no retrocesso excessivo geralmente exerce o papel mais significativo na degradação do desempenho da expressão regular. Em um cenário de pior caso, o tempo de execução pode dobrar para cada caractere adicional na cadeia de caracteres de entrada. Na verdade, quando o retrocesso é usado excessivamente, é fácil criar o equivalente programático de um loop infinito se a entrada quase corresponder ao padrão da expressão regular; o mecanismo de expressões regulares pode levar horas ou mesmo dias para processar uma cadeia de caracteres de entrada relativamente curta.

Frequentemente, os aplicativos pagam uma penalidade de desempenho por usar o retrocesso, mesmo ele não sendo essencial para uma correspondência. Por exemplo, a expressão regular `\b\p{Lu}\w*\b` corresponde a todas as palavras que começam com um caractere maiúsculo, como mostra a tabela a seguir.

|Padrão|Descrição|
|-|-|
|`\b`|Começar a correspondência em um limite de palavra.|
|`\p{Lu}`|Corresponder a um caractere maiúsculo.|
|`\w*`|Corresponder a zero ou mais caracteres de palavra.|
|`\b`|Termina a correspondência em um limite de palavra.|

Como um limite de palavra não é o mesmo que ou um subconjunto de, um caractere de palavra, não há nenhuma possibilidade de o mecanismo de expressões regulares cruzar um limite de palavra ao corresponder caracteres de palavra. Isso significa que, para esta expressão regular, o retrocesso nunca pode contribuir para o êxito total de qualquer correspondência – ele só pode prejudicar o desempenho, pois o mecanismo de expressões regulares é forçado a salvar o estado para cada correspondência preliminar bem-sucedida de um caractere de palavra.

Se você determinar que o retrocesso não é necessário, poderá desabilitá-lo usando o `(?>subexpression)` elemento Language, conhecido como um grupo atômico. O exemplo a seguir analisa uma cadeia de caracteres de entrada usando duas expressões regulares. A primeira, `\b\p{Lu}\w*\b`, depende do retrocesso. A segunda, `\b\p{Lu}(?>\w*)\b`, desabilita o retrocesso. Conforme mostrado pela saída do exemplo, ambas produzem o mesmo resultado.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/backtrack2.cs#10)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/backtrack2.vb#10)]

Em muitos casos, o retrocesso é essencial para corresponder um padrão de expressão regular ao texto de entrada. No entanto, o retrocesso excessivo pode prejudicar severamente o desempenho e criar a impressão de que um aplicativo parou de responder. Em particular, isso acontece quando quantificadores são aninhados e o texto que corresponde à subexpressão externa é um subconjunto do texto que corresponde à subexpressão interna.

> [!WARNING]
> Além de evitar retrocessos excessivos, você deve usar o recurso de tempo limite para garantir que retrocessos excessivos não degradem severamente o desempenho da expressão regular. Para obter mais informações, confira a seção [Usar valores de tempo limite](#use-time-out-values).

Por exemplo, o padrão de expressão regular `^[0-9A-Z]([-.\w]*[0-9A-Z])*\$$` destina-se a corresponder a um número de peça que consiste em pelo menos um caractere alfanumérico. Todos os demais caracteres podem consistir em um caractere alfanumérico, um hífen, um sublinhado ou um ponto, embora o último caractere deva ser alfanumérico. Um cifrão termina o número da peça. Em alguns casos, esse padrão de expressão regular pode exibir um desempenho muito ruim porque os quantificadores estão aninhados e porque a subexpressão `[0-9A-Z]` é um subconjunto da subexpressão `[-.\w]*`.

Nesses casos, você pode otimizar o desempenho da expressão regular ao remover os quantificadores aninhados e substituir a subexpressão externa por uma declaração de lookahead ou lookbehind de largura zero. As asserções lookahead e lookbehind são âncoras; elas não movem o ponteiro na cadeia de caracteres de entrada, mas fazem uma verificação para verificar se uma condição especificada foi atendida. Por exemplo, a expressão regular do número de peça pode ser reescrita como `^[0-9A-Z][-.\w]*(?<=[0-9A-Z])\$$`. Esse padrão de expressão regular é definido conforme mostrado na tabela a seguir.

|Padrão|Descrição|
|-------------|-----------------|
|`^`|Começar a correspondência no início da cadeia de caracteres de entrada.|
|`[0-9A-Z]`|Corresponde a um caractere alfanumérico. O número de peça deve consistir em pelo menos este caractere.|
|`[-.\w]*`|Corresponde a zero ou mais ocorrências de qualquer caractere de palavra, hífen ou ponto.|
|`\$`|Corresponde a um cifrão.|
|`(?<=[0-9A-Z])`|Examine além do cifrão final para garantir que o caractere anterior seja alfanumérico.|
|`$`|Finalizar a correspondência no final da cadeia de caracteres de entrada.|

O exemplo a seguir ilustra o uso dessa expressão regular para corresponder a uma que contém possíveis números de peças.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/backtrack4.cs#11)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/backtrack4.vb#11)]

A linguagem de expressões regulares no .NET inclui os seguintes elementos de linguagem que você pode usar para eliminar quantificadores aninhados. Para saber mais, confira [Constructos de agrupamento](grouping-constructs-in-regular-expressions.md).

|Elemento de linguagem|Descrição|
|----------------------|-----------------|
|`(?=` `subexpression` `)`|Lookahead positivo de largura zero. Examine além da posição atual para determinar se `subexpression` coincide com a cadeia de caracteres de entrada.|
|`(?!` `subexpression` `)`|Lookahead negativo de largura zero. Examine além da posição atual para determinar se `subexpression` não coincide com a cadeia de caracteres de entrada.|
|`(?<=` `subexpression` `)`|Lookbehind positivo de largura zero. Examine o conteúdo que antecede a posição atual para determinar se `subexpression` coincide com a cadeia de caracteres de entrada.|
|`(?<!` `subexpression` `)`|Lookbehind negativo de largura zero. Examine o conteúdo que antecede a posição atual para determinar se `subexpression` não coincide com a cadeia de caracteres de entrada.|

## <a name="use-time-out-values"></a>Use valores de tempo limite

Se suas expressões regulares processarem entradas quase correspondentes ao padrão da expressão regular, elas poderão frequentemente confiar no retrocesso excessivo, o que afeta significativamente o desempenho. Além de considerar cuidadosamente o uso do retrocesso e testar a expressão regular contra entradas quase correspondentes, você deve sempre definir um valor de tempo limite para garantir que o impacto do retrocesso excessivo, caso ocorra, seja minimizado.

O intervalo de tempo limite de expressão regular define o período de tempo que o mecanismo de expressão regular procurará por uma única correspondência antes de atingir o tempo limite. O intervalo de tempo limite padrão é <xref:System.Text.RegularExpressions.Regex.InfiniteMatchTimeout?displayProperty=nameWithType> , o que significa que a expressão regular não atingirá o tempo limite. Você pode substituir esse valor e definir um intervalo de tempo limite da seguinte maneira:

- Ao fornecer um valor de tempo limite ao criar uma instância de um objeto <xref:System.Text.RegularExpressions.Regex> ao chamar o construtor <xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29>.

- Ao chamar um método estático de correspondência de padrão, como <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29?displayProperty=nameWithType> ou <xref:System.Text.RegularExpressions.Regex.Replace%28System.String%2CSystem.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29?displayProperty=nameWithType>, que inclui um parâmetro `matchTimeout`.

- Para expressões regulares compiladas que são criadas chamando-se o método <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType>, ao chamar o construtor que tem um parâmetro do tipo <xref:System.TimeSpan>.

Se você tiver definido um intervalo de tempo limite e uma correspondência não for localizada no final do intervalo, o método de expressão regular gerará uma exceção <xref:System.Text.RegularExpressions.RegexMatchTimeoutException>. No manipulador de exceção, você pode optar por tentar fazer novamente a correspondência com um intervalo de tempo limite mais longo, abandonar a tentativa de correspondência e assumir que não há nenhuma correspondência ou abandonar a tentativa de correspondência e registrar as informações de exceção para análise futura.

O exemplo a seguir define um método `GetWordData` que instancia uma expressão regular com um intervalo de tempo limite de 350 milissegundos para calcular o número de palavras e o número médio de caracteres em uma palavra em um documento de texto. Se a operação de correspondência exceder o tempo limite, o intervalo de tempo limite será aumentado em 350 milissegundos e o objeto de <xref:System.Text.RegularExpressions.Regex> será reinstanciado. Se o novo intervalo de tempo limite exceder 1 segundo, o método gerará novamente a exceção no chamador.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/timeout1.cs#12)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/timeout1.vb#12)]

## <a name="capture-only-when-necessary"></a>Capture somente quando necessário

As expressões regulares no .NET dão suporte a vários constructos de agrupamento, que permitem a você agrupar um padrão de expressão regular em uma ou mais subexpressões. As construções de agrupamento usadas com mais frequência na linguagem de expressão regular do .NET são `(` *subexpressão* `)` , que define um grupo de captura numerada e a `(?<` *name* `>` *subexpressão*de nome `)` , que define um grupo de captura nomeado. Os construtores de agrupamento são essenciais para criar referências reversas e definir uma subexpressão à qual um quantificador é aplicado.

No entanto, o uso desses elementos de linguagem tem um custo. Eles fazem com que o objeto <xref:System.Text.RegularExpressions.GroupCollection> retornado pela propriedade <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> seja preenchido com as capturas sem nome ou nomeadas mais recentes. Além disso, se uma única construção de agrupamento capturou várias subcadeias de caracteres na cadeia de caracteres de entrada, também preenchem o objeto <xref:System.Text.RegularExpressions.CaptureCollection> retornado pela propriedade <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType> de um grupo de captura específico com vários objetos <xref:System.Text.RegularExpressions.Capture>.

Muitas vezes, os construtores de agrupamento são usados somente em uma expressão regular de modo que quantificadores possam ser aplicados a eles e os grupos capturados por essas subexpressão não são usados posteriormente. Por exemplo, a expressão regular `\b(\w+[;,]?\s?)+[.?!]` é criada para capturar uma frase inteira. A tabela a seguir descreve os elementos de linguagem nesse padrão de expressão regular e seu efeito nas coleções <xref:System.Text.RegularExpressions.Match> e <xref:System.Text.RegularExpressions.Match.Groups%2A?displayProperty=nameWithType> do objeto <xref:System.Text.RegularExpressions.Group.Captures%2A?displayProperty=nameWithType>.

|Padrão|Descrição|
|-------------|-----------------|
|`\b`|Começar a correspondência em um limite de palavra.|
|`\w+`|Fazer a correspondência a um ou mais caracteres de palavra.|
|`[;,]?`|Corresponde a zero ou uma vírgula ou ponto e vírgula.|
|`\s?`|Corresponder a zero ou a um caractere de espaço em branco.|
|`(\w+[;,]?\s?)+`|Corresponde a uma ou mais ocorrências de um ou mais caracteres de palavra seguidos por uma vírgula opcional ou por ponto-e-vírgula seguido por um caractere de espaço em branco opcional. Isso define o primeiro grupo de captura, que é necessário para que a combinação de vários caracteres de palavra (ou seja, uma palavra) seguido por um símbolo de pontuação opcional seja repetida até que o mecanismo de expressões regulares atinja o final de uma sentença.|
|`[.?!]`|Corresponde a um ponto, um ponto de interrogação ou um ponto de exclamação.|

Como o exemplo a seguir mostra, quando uma correspondência é encontrada, os objetos <xref:System.Text.RegularExpressions.GroupCollection> e de <xref:System.Text.RegularExpressions.CaptureCollection> são preenchidos com as capturas da correspondência. Nesse caso, o grupo de captura `(\w+[;,]?\s?)` existe para que o quantificador `+` possa ser aplicado a ele, o que permite que o padrão de expressão regular corresponda a cada palavra em uma sentença. Caso contrário, ele corresponderia à última palavra em uma sentença.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/group1.cs#8)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/group1.vb#8)]

Quando você usa subexpressões apenas para aplicar quantificadores a elas e não está interessado em texto capturado, desabilite as capturas de grupo. Por exemplo, o elemento de linguagem `(?:subexpression)` evita que o grupo ao qual ele se aplica capture subcadeias de caracteres correspondidas. No exemplo a seguir, o padrão de expressão regular do exemplo anterior é alterado para `\b(?:\w+[;,]?\s?)+[.?!]`. Conforme mostrado pela saída, ele impede que o mecanismo de expressões regulares preencha as coleções <xref:System.Text.RegularExpressions.GroupCollection> e de <xref:System.Text.RegularExpressions.CaptureCollection>.

[!code-csharp[Conceptual.RegularExpressions.BestPractices#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/cs/group2.cs#9)]
[!code-vb[Conceptual.RegularExpressions.BestPractices#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.bestpractices/vb/group2.vb#9)]

É possível desabilitar as capturas de uma das seguintes formas:

- Use o elemento de linguagem `(?:subexpression)`. Esse elemento impede a captura de subcadeias de caracteres correspondidas no grupo ao qual se ele aplica. Ele não desabilita capturas de subcadeias de caracteres em grupos aninhados.

- Use a opção <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture>. Ela desabilita todas as capturas sem nome ou implícitas no padrão de expressão regular. Quando você usa essa opção, somente as subcadeias de caracteres que correspondem aos grupos nomeados definidos com o elemento de linguagem `(?<name>subexpression)` podem ser capturadas. O sinalizador <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture> pode ser passado para o parâmetro `options` de um construtor de classe <xref:System.Text.RegularExpressions.Regex> ou para o parâmetro `options` de um método de correspondência estática <xref:System.Text.RegularExpressions.Regex>.

- Use a opção `n` no elemento de linguagem `(?imnsx)`. Esta opção desabilita todas as capturas sem nome ou implícitas a partir do ponto no padrão de expressão regular em que o elemento aparece. As capturas são desabilitadas até o final do padrão ou até a opção `(-n)` permitir capturas sem nome ou implícitas. Para saber mais, confira [Constructos diversos](miscellaneous-constructs-in-regular-expressions.md).

- Use a opção `n` no elemento de linguagem `(?imnsx:subexpression)`. Esta opção desativa todas as capturas sem nome ou implícitas em `subexpression`. As capturas por grupos de capturas aninhadas sem nome ou implícitas também são desabilitadas.

## <a name="related-topics"></a>Tópicos relacionados

|Title|Descrição|
|-----------|-----------------|
|[Detalhes do comportamento de expressões regulares](details-of-regular-expression-behavior.md)|Examina a implementação do mecanismo de expressões regulares no .NET. O tópico concentra-se na flexibilidade de expressões regulares e explica a responsabilidade do desenvolvedor para garantir o funcionamento eficiente e robusto do mecanismo de expressões regulares.|
|[Retrocesso](backtracking-in-regular-expressions.md)|Explica o que é o retrocesso é como ele afeta o desempenho da expressão regular e examina os elementos de linguagem que fornecem alternativas ao retrocesso.|
|[Linguagem de expressões regulares – referência rápida](regular-expression-language-quick-reference.md)|Descreve os elementos de linguagem de expressões regulares do .NET e fornece links para a documentação detalhada de cada elemento da linguagem.|
