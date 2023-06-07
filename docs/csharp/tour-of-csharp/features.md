---
title: Um tour pelo C# – áreas de linguagem principal
description: Novato em C#? Conheça os fundamentos da linguagem.
ms.date: 08/06/2020
ms.openlocfilehash: f0e9bff144cc3c853a82f2ee6b400049df60683d
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88068504"
---
# <a name="major-language-areas"></a>Principais áreas de idioma

## <a name="arrays-collections-and-linq"></a>Matrizes, coleções e LINQ

O C# e o .NET fornecem muitos tipos de coleção diferentes. As matrizes têm a sintaxe definida pelo idioma. Os tipos de coleção genéricos são listados no <xref:System.Collections.Generic?displayProperty=fullName> namespace. As coleções especializadas incluem <xref:System.Span%601?displayProperty=nameWithType> para acessar a memória contínua no registro de ativação e <xref:System.Memory%601?displayProperty=nameWithType> para acessar a memória contínua no heap gerenciado. Todas as coleções, incluindo matrizes, <xref:System.Span%601> e <xref:System.Memory%601> compartilham um princípio unificado para iteração. Você usa a <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> interface. Esse princípio unificado significa que qualquer um dos tipos de coleção pode ser usado com consultas LINQ ou outros algoritmos. Você escreve métodos usando <xref:System.Collections.Generic.IEnumerable%601> e esses algoritmos funcionam com qualquer coleção.

### <a name="arrays"></a>Matrizes

Uma [***matriz***](../programming-guide/arrays/index.md) é uma estrutura de dados que contém um número de variáveis que são acessadas por meio de índices computados. As variáveis contidas em uma matriz, também chamadas de ***elementos*** da matriz, são do mesmo tipo. Esse tipo é chamado de ***tipo de elemento*** da matriz.

Os tipos de matriz são tipos de referência, e a declaração de uma variável de matriz simplesmente reserva espaço para uma referência a uma instância de matriz. As instâncias de matriz reais são criadas dinamicamente no tempo de execução usando o `new` operador. A `new` operação especifica o ***comprimento*** da nova instância de matriz, que é então corrigida para o tempo de vida da instância. Os índices dos elementos de uma matriz variam de `0` a `Length - 1`. O operador `new` inicializa automaticamente os elementos de uma matriz usando o valor padrão, que, por exemplo, é zero para todos os tipos numéricos e `null` para todos os tipos de referência.

O exemplo a seguir cria uma matriz de elementos `int`, inicializa a matriz e imprime o conteúdo da matriz.

:::code language="csharp" source="./snippets/shared/Features.cs" ID="ArraysSample":::

Este exemplo cria e opera em uma ***matriz unidimensional***. O C# também oferece suporte a ***matrizes multidimensionais***. O número de dimensões de um tipo de matriz, também conhecido como ***classificação*** do tipo de matriz, é o número um mais o número de vírgulas escrito entre os colchetes do tipo de matriz. O exemplo a seguir aloca uma matriz unidimensional, bidimensional e tridimensional, respectivamente.

:::code language="csharp" source="./snippets/shared/Features.cs" ID="DeclareArrays":::

A matriz `a1` contém 10 elementos, a matriz `a2` contém 50 (10 × 5) elementos e a matriz `a3` contém 100 (10 × 5 × 2) elementos.
O tipo do elemento de uma matriz pode ser qualquer tipo, incluindo um tipo de matriz. Uma matriz com elementos de um tipo de matriz, às vezes, é chamada de ***matriz denteada*** porque os comprimentos das matrizes de elementos nem todos precisam ser iguais. O exemplo a seguir aloca uma matriz de matrizes de `int`:

:::code language="csharp" source="./snippets/shared/Features.cs" ID="ArrayOfArrays":::

A primeira linha cria uma matriz com três elementos, cada um do tipo `int[]`, e cada um com um valor inicial de `null`. Em seguida, as linhas seguintes inicializam os três elementos com referências a instâncias de matriz individuais de comprimentos variáveis.

O `new` operador permite que os valores iniciais dos elementos da matriz sejam especificados usando um ***inicializador de matriz***, que é uma lista de expressões gravadas entre os delimitadores `{` e `}` . O exemplo a seguir aloca e inicializa um `int[]` com três elementos.

:::code language="csharp" source="./snippets/shared/Features.cs" ID="InitializeArray":::

O comprimento da matriz é inferido a partir do número de expressões entre `{` e `}` . As declarações de campo e variável local podem ser reduzidas ainda mais, de modo que o tipo de matriz não precise ser renovado.

:::code language="csharp" source="./snippets/shared/Features.cs" ID="InitializeShortened":::

Os dois exemplos anteriores são equivalentes ao seguinte código:

:::code language="csharp" source="./snippets/shared/Features.cs" ID="InitializeGenerated":::

A `foreach` instrução pode ser usada para enumerar os elementos de qualquer coleção. O código a seguir enumera a matriz do exemplo anterior:

:::code language="csharp" source="./snippets/shared/Features.cs" ID="EnumerateArray":::

A `foreach` instrução usa a <xref:System.Collections.Generic.IEnumerable%601> interface, portanto, pode funcionar com qualquer coleção.

## <a name="string-interpolation"></a>Interpolação de cadeia de caracteres

A [***interpolação de cadeia de caracteres***](../language-reference/tokens/interpolated.md) C# permite que você formate as cadeias definindo expressões cujos resultados são colocados em uma cadeia de caracteres de formato. Por exemplo, o exemplo a seguir imprime a temperatura em um determinado dia a partir de um conjunto de dados meteorológicos:

:::code language="csharp" source="./snippets/shared/Features.cs" ID="StringInterpolation":::

Uma cadeia de caracteres interpolada é declarada usando o `$` token. A interpolação de cadeia de caracteres avalia as expressões entre `{` e `}` , em seguida, converte o resultado em um `string` e substitui o texto entre colchetes pelo resultado da cadeia de caracteres da expressão. O `:` na primeira expressão `{weatherData.Data:MM-DD-YYYY}` especifica a cadeia de *caracteres de formato*. No exemplo anterior, ele especifica que a data deve ser impressa no formato "MM-DD-AAAA".

## <a name="pattern-matching"></a>Correspondência de padrões

A linguagem C# fornece expressões de [***correspondência de padrões***](../pattern-matching.md) para consultar o estado de um objeto e executar código com base nesse estado. Você pode inspecionar os tipos e os valores de propriedades e campos para determinar qual ação deve ser tomada. A `switch` expressão é a principal expressão para correspondência de padrões.

## <a name="delegates-and-lambda-expressions"></a>Delegados e expressões lambda

Um [***tipo delegado***](../delegates-overview.md) representa referências a métodos com uma lista de parâmetros e um tipo de retorno específicos. Delegados possibilitam o tratamento de métodos como entidades que podem ser atribuídos a variáveis e passadas como parâmetros. Delegados são semelhantes ao conceito de ponteiros de função encontrados em algumas outras linguagens. Diferentemente de ponteiros de função, os delegados são orientados a objeto e são de tipo seguro.

O exemplo a seguir declara e usa um tipo delegado chamado `Function`.

:::code language="csharp" source="./snippets/shared/Features.cs" ID="DelegateExample":::

Uma instância do tipo delegado `Function` pode fazer referência a qualquer método que usa um argumento `double` e retorna um valor `double`. O `Apply` método aplica um dado `Function` aos elementos de a `double[]` , retornando um `double[]` com os resultados. No método `Main`, `Apply` é usado para aplicar três funções diferentes para um `double[]`.

Um delegado pode referenciar um método estático (como `Square` ou `Math.Sin` no exemplo anterior) ou um método de instância (como `m.Multiply` no exemplo anterior). Um delegado que referencia um método de instância também referencia um objeto específico, e quando o método de instância é invocado por meio do delegado, esse objeto se torna `this` na invocação.

Os delegados também podem ser criados usando funções anônimas, que são "métodos embutidos" que são criados quando declarados. As funções anônimas podem ver as variáveis locais dos métodos ao redor. O exemplo a seguir não cria uma classe:

:::code language="csharp" source="./snippets/shared/Features.cs" ID="UseDelegate":::

Um delegado não conhece ou se preocupa com a classe do método referenciado. Tudo o que importa é que o método referenciado tem os mesmos parâmetros e tipo de retorno como o delegado.

## <a name="async--await"></a>Async/Await

O C# dá suporte a programas assíncronos com duas palavras-chave: `async` e `await` . Você adiciona o `async` modificador a uma declaração de método para declarar que o método é assíncrono. O `await` operador informa ao compilador para aguardar de forma assíncrona que um resultado seja concluído. O controle é retornado para o chamador e o método retorna uma estrutura que gerencia o estado do trabalho assíncrono. A estrutura é normalmente um <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType> , mas pode ser qualquer tipo que ofereça suporte ao padrão de aguardador. Esses recursos permitem que você escreva o código que lê como sua contraparte síncrona, mas é executado de forma assíncrona. Por exemplo, o código a seguir baixa o home page do [Microsoft docs](https://docs.microsoft.com):

:::code language="csharp" source="./snippets/shared/Features.cs" ID="AsyncExample":::

Este pequeno exemplo mostra os principais recursos para programação assíncrona:

- A declaração do método inclui o `async` modificador.
- O corpo do método `await` s retorna o retorno do `GetByteArrayAsync` método.
- O tipo especificado na `return` instrução corresponde ao argumento de tipo na `Task<T>` declaração do método. (Um método que retorna `Task` instruções de uso `return` sem nenhum argumento).

## <a name="attributes"></a>Atributos

Tipos, membros e outras entidades em um programa C# dão suporte a modificadores que controlam determinados aspectos de seu comportamento. Por exemplo, a acessibilidade de um método é controlada usando os modificadores `public`, `protected`, `internal` e `private`. O C# generaliza essa funcionalidade, de modo que os tipos definidos pelo usuário de informações declarativas podem ser anexados a entidades de programa e recuperados no tempo de execução. Os programas especificam essas informações declarativas adicionais definindo e usando [***atributos***](../programming-guide/concepts/attributes/index.md).

O exemplo a seguir declara um atributo `HelpAttribute` que pode ser colocado em entidades de programa para fornecem links para a documentação associada.

:::code language="csharp" source="./snippets/shared/Features.cs" ID="DefineAttribute":::

Todas as classes de atributo derivam da <xref:System.Attribute> classe base fornecida pela biblioteca .net. Os atributos podem ser aplicados, fornecendo seu nome, junto com quaisquer argumentos, dentro dos colchetes pouco antes da declaração associada. Se o nome de um atributo termina em `Attribute`, essa parte do nome pode ser omitida quando o atributo é referenciado. Por exemplo, o atributo `HelpAttribute` pode ser usado da seguinte maneira.

:::code language="csharp" source="./snippets/shared/Features.cs" ID="UseAttributes":::

Este exemplo anexa um `HelpAttribute` à classe `Widget`. Ele adiciona outro `HelpAttribute` ao método `Display` na classe. Os construtores públicos de uma classe de atributo controlam as informações que devem ser fornecidas quando o atributo é anexado a uma entidade de programa. As informações adicionais podem ser fornecidas ao referenciar propriedades públicas de leitura-gravação da classe de atributo (como a referência anterior à propriedade `Topic`).

Os metadados definidos por atributos podem ser lidos e manipulados em runtime usando reflexão. Quando um atributo específico for solicitado usando esta técnica, o construtor para a classe de atributo será invocado com as informações fornecidas na origem do programa e a instância do atributo resultante será retornada. Se forem fornecidas informações adicionais por meio de propriedades, essas propriedades serão definidas para os valores fornecidos antes que a instância do atributo seja retornada.

O exemplo de código a seguir demonstra como obter as instâncias `HelpAttribute` associadas à classe `Widget` e seu método `Display`.

:::code language="csharp" source="./snippets/shared/Features.cs" ID="ReadAttributes":::

## <a name="learn-more"></a>Saiba mais

Você pode explorar mais sobre C# experimentando um dos nossos [tutoriais](../tutorials/index.md).

>[!div class="step-by-step"]
>[Anterior](program-building-blocks.md)
