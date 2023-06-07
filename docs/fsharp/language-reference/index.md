---
title: Referência de linguagem
description: 'Encontre informações de recursos de linguagem F # desta referência a tokens de idioma, conceitos, tipos, expressões e tópicos de construção com suporte do compilador.'
ms.date: 08/13/2020
ms.openlocfilehash: 02711489c214c1fcdb2da80f30bff63d67769c17
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558147"
---
# <a name="f-language-reference"></a>Referência da linguagem F#

Esta seção é uma referência para a linguagem F #, uma linguagem de programação de vários paradigmas voltada para .NET. A linguagem F # dá suporte a modelos de programação funcionais, orientados a objeto e imperativos.

## <a name="f-core-library-api-reference"></a>Referência da API da biblioteca principal F #

A [referência de API de f # principal (FSharp. Core)](https://fsharp.github.io/fsharp-core-docs/) é a referência para todos os namespaces da biblioteca principal do f #, módulos, tipos e funções.

## <a name="f-tokens"></a>Tokens do F#

A tabela a seguir mostra os artigos de referência que fornecem tabelas de palavras-chave, símbolos e literais que são usados como tokens em F #.

|Title|Descrição|
|-----|-----------|
|[Referência de palavras-chave](keyword-reference.md)|Contém links para informações sobre todas as palavras-chave do F#.|
|[Referência de símbolos e operadores](./symbol-and-operator-reference/index.md)|Contém uma tabela de símbolos e operadores usados na linguagem F#.|
|[Literais](literals.md)|Descreve a sintaxe para valores literais no F# e como especificar informações de tipo para literais do F#.|

## <a name="f-language-concepts"></a>Conceitos da linguagem F#

A tabela a seguir mostra os tópicos de referência disponíveis que descrevem os conceitos de linguagem.

|Title|Descrição|
|-----|-----------|
|[Funções](./functions/index.md)|As funções são a unidade fundamental de execução do programa em qualquer linguagem de programação. Como em outras linguagens, uma função do F# tem um nome, pode ter parâmetros e receber argumentos, e tem um corpo. O F# também oferece suporte a construções de programação funcional como tratamento de funções como valores, uso de funções sem nome em expressões, composição de funções para formar novas funções, funções via currying e a definição implícita de funções por meio da aplicação parcial dos argumentos da função.|
|[Tipos F#](fsharp-types.md)|Descreve os tipos que são usados no F# e como os tipos F# são nomeados e descritos.|
|[Inferência de tipos](type-inference.md)|Descreve como o compilador F # infere os tipos de valores, variáveis, parâmetros e valores de retorno.|
|[Generalização automática](./generics/automatic-generalization.md)|Descreve as construções genéricas no F#.|
|[Herança](inheritance.md)|Descreve a herança, que é usada para modelar a relação "é um" ou subtipagem na programação orientada a objetos.|
|[Membros](./members/index.md)|Descreve os membros de tipos de objeto do F#.|
|[Parâmetros e argumentos](Parameters-and-Arguments.md)|Descreve o suporte de linguagem para definir parâmetros e passar argumentos para funções, métodos e propriedades. Inclui informações sobre como passar referência.|
|[Sobrecarga de operador](operator-overloading.md)|Descreve como sobrecarregar operadores aritméticos em uma classe ou tipo de registro e no nível global.|
|[Conversões cast e conversões](casting-and-conversions.md)|Descreve o suporte para conversões de tipos no F#.|
|[Controle de acesso](access-control.md)|Descreve o controle de acesso no F#. Controle de acesso significa declarar quais clientes são capazes de usar determinados elementos do programa, como tipos, métodos, funções e assim por diante.|
|[Correspondência de padrões](pattern-matching.md)|Descreve os padrões, que são regras para transformar dados de entrada e são usados em toda a linguagem F #. Você pode comparar dados com um padrão, decompor dados em partes constituintes ou extrair informações de dados de várias maneiras.|
|[Padrões ativos](active-patterns.md)|Descreve padrões ativos. Padrões ativos permitem definir partições nomeadas que subdividem os dados de entrada. Você pode usar padrões ativos para decompor os dados de uma maneira personalizada para cada partição.|
|[Asserções](assertions.md)|Descreve a expressão `assert`, que é um recurso de depuração que você pode usar para testar uma expressão. Em caso de falha no modo de depuração, uma asserção gera uma caixa de diálogo de erro do sistema.|
|[Tratamento de Exceção](./exception-handling/index.md)|Contém informações sobre suporte de manipulação de exceção na linguagem F#.|
|[attributes](attributes.md)|Descreve os atributos, que permitem os metadados a serem aplicados a uma construção de programação.|
|[Gerenciamento de Recursos: a palavra-chave `use`](resource-management-the-use-keyword.md)|Descreve as palavras-chave `use` e `using`, que podem controlar a inicialização e a liberação de recursos|
|[namespaces](namespaces.md)|Descreve o suporte a namespace no F#. Um namespace permite organizar o código em áreas de funcionalidade relacionada ao permitir que você anexe um nome a um agrupamento de elementos de programação.|
|[Módulos](modules.md)|Descreve os módulos. Um módulo do F# é um agrupamento de código F#, como valores, tipos e valores de função em um programa no F#. O agrupamento de código em módulos ajuda a manter junto o código relacionado e ajuda a evitar conflitos de nome em seu programa.|
|[Declarações de Importação: a palavra-chave `open`](import-declarations-the-open-keyword.md)|Descreve como `open` funciona. Uma declaração de importação especifica um módulo ou namespace cujos elementos você pode referenciar sem usar um nome totalmente qualificado.|
|[Assinaturas](signature-files.md)|Descreve as assinaturas e os arquivos de assinatura. Um arquivo de assinatura contém informações sobre as assinaturas públicas de um conjunto de elementos de programação de F#, como tipos, namespaces e módulos. Ele pode ser usado para especificar a acessibilidade desses elementos de programa.|
|[Documentação XML](xml-documentation.md)|Descreve o suporte para gerar arquivos de documentação para comentários de documento XML, também conhecidos como comentários de barra tripla. Você pode produzir documentação de comentários de código em F # como em outras linguagens .NET.|
|[Sintaxe detalhada](verbose-syntax.md)|Descreve a sintaxe para construções no F# quando a sintaxe leve não está habilitada. A sintaxe detalhada está indicada pela diretiva `#light "off"` na parte superior do arquivo de código.|
|[Formatação de texto sem formatação](plaintext-formatting.md)|Saiba como usar o sprintf e outras formatações de texto sem formatação em aplicativos e scripts em F #.|

## <a name="f-types"></a>Tipos F#

A tabela a seguir mostra os tópicos de referência disponíveis que descrevem os tipos com suporte pela linguagem F#.

|Title|Descrição|
|-----|-----------|
|[os](./values/index.md)|Descreve os valores, que são quantidades imutáveis que têm um tipo específico; valores podem ser números inteiros ou de ponto flutuante, caracteres ou texto, listas, sequências, matrizes, tuplas, uniões discriminadas, registros, tipos de classe ou valores de função.|
|[Tipos Básicos](basic-types.md)|Descreve os tipos básicos fundamentais que são usados na linguagem F #. Ele também fornece os tipos .NET correspondentes e os valores mínimos e máximos para cada tipo.|
|[Tipo unit](unit-type.md)|Descreve o tipo `unit`, que é um tipo que indica a ausência de um valor específico; o tipo `unit` tem apenas um único valor, que atua como espaço reservado quando nenhum outro valor existe ou é necessário.|
|[Cadeias de caracteres](strings.md)|Descreve cadeias de caracteres no F#. O tipo `string` representa texto imutável, como uma sequência de caracteres Unicode. `string` é um alias para `System.String` no .NET Framework.|
|[Tuplas](tuples.md)|Descreve as tuplas, que são agrupamentos de valores sem nome, mas valores ordenados de tipos possivelmente diferentes.|
|[Tipos de coleção de F#](fsharp-collection-types.md)|Visão geral dos tipos de coleção funcional do F#, incluindo tipos de matrizes, listas, sequências (seq), mapas e conjuntos.|
|[Listas](lists.md)|Descreve as listas. Uma lista no F# é uma série imutável, ordenada, de elementos do mesmo tipo.|
|[Opções](options.md)|Descreve o tipo de opção. Uma opção no F# é usada quando um valor pode ou não existir. Uma opção tem um tipo subjacente e pode conter um valor desse tipo ou pode não ter um valor.|
|[Sequências](sequences.md)|Descreve as sequências. Uma sequência é uma série lógica de elementos todos de um tipo. Os elementos de sequência individuais são computados apenas se necessário, portanto, a representação pode ser menor do que uma contagem de elementos literais indica.|
|[matrizes](arrays.md)|Descreve as matrizes. As matrizes são sequências de tamanho fixo, com base em zero e mutáveis de elementos de dados consecutivos, todos do mesmo tipo.|
|[Registros](records.md)|Descreve os registros. Registros representam agregações simples de valores nomeados, opcionalmente com membros.|
|[Uniões discriminadas](discriminated-unions.md)|Descreve uniões discriminadas, que fornecem suporte para valores que podem ser uma das diversas ocorrências nomeadas, cada uma com valores e tipos possivelmente diferentes.|
|[Enumerações](enumerations.md)|Descreve as enumerações que são tipos que têm um conjunto definido de valores nomeados. Você pode usá-los no lugar de literais para tornar o código mais legível e fácil de manter.|
|[Células de referência](reference-cells.md)|Descreve as células de referência, que são locais de armazenamento que permitem criar variáveis mutáveis com semântica de referência.|
|[Abreviações de tipo](type-abbreviations.md)|Descreve as abreviações de tipo, que são nomes alternativos para tipos.|
|[Classes](classes.md)|Descreve as classes, que são tipos que representam os objetos que podem ter propriedades, métodos e eventos.|
|[Estruturas](structures.md)|Descreve estruturas, que são tipos de objeto compacto que podem ser mais eficiente do que uma classe para tipos que tenham uma pequena quantidade de dados e comportamento simples.|
|[Interfaces](interfaces.md)|Descreve interfaces, que especificam os conjuntos de membros relacionados que outras classes implementam.|
|[Classes abstratas](abstract-classes.md)|Descreve classes abstratas, que são classes que deixam alguns ou todos os membros não implementados, para que as implementações possam ser fornecidas por classes derivadas.|
|[Extensões de tipo](type-extensions.md)|Descreve as extensões de tipo, que permitem que você adicione novos membros a um tipo de objeto definido anteriormente.|
|[Tipos flexíveis](flexible-types.md)|Descreve os tipos flexíveis. Uma anotação de tipo flexível é uma indicação de que um parâmetro, uma variável ou um valor tem um tipo compatível com o tipo especificado, onde a compatibilidade é determinada pela posição em uma hierarquia orientada a objeto de classes ou interfaces.|
|[Representantes](delegates.md)|Descreve delegados, que representam uma chamada de função como um objeto.|
|[Unidades de medida](units-of-measure.md)|Descreve as unidades de medida. Valores de ponto flutuante no F# podem ter unidades de medida associadas, que normalmente são usadas para indicar o comprimento, o volume, a massa e assim por diante.|
|[Provedores de tipos](../tutorials/type-providers/index.md)|Descreve provedores de tipo e fornece links para instruções passo a passo sobre como usar os provedores de tipos internos para acessar bancos de dados e serviços da Web.|

## <a name="f-expressions"></a>Expressões em F#

A tabela a seguir lista os tópicos que descrevem as expressões no F#.

|Title|Descrição|
|-----|-----------|
|[Expressões condicionais: `if...then...else`](conditional-expressions-if-then-else.md)|Descreve a expressão `if...then...else`, que executa diferentes ramificações do código e também é avaliada como um valor diferente dependendo da expressão booliana fornecida.|
|[Expressões de correspondência](match-expressions.md)|Descreve a expressão `match`, que fornece controle de ramificação com base na comparação de uma expressão com um conjunto de padrões.|
|[Loops: `for...to` expressão](loops-for-to-expression.md)|Descreve a expressão `for...to`, que é usada para iterar em um loop em um intervalo de valores de uma variável de loop.|
|[Loops: `for...in` expressão](loops-for-in-expression.md)|Descreve a expressão `for...in`, uma construção de loop usada para iterar sobre as correspondências de um padrão em uma coleção enumerável como uma expressão de intervalo, sequência, lista, matriz ou outra construção que oferece suporte à enumeração.|
|[Loops: `while...do` expressão](loops-while-do-expression.md)|Descreve a expressão `while...do`, que é usada para uma execução iterativa (loop) enquanto uma condição de teste especificada é verdadeira.|
|[Expressões de objeto](object-expressions.md)|Descreve as expressões de objeto, que são expressões que criam novas instâncias de um tipo de objeto anônimo e criado dinamicamente, com base em um tipo base existente, interface ou conjunto de interfaces.|
|[Expressões lentas](lazy-expressions.md)|Descreve as expressões lentas, que são computações que não são avaliadas imediatamente, mas, em vez disso, são avaliadas quando o resultado é realmente necessário.|
|[Expressões de computação](computation-expressions.md)|Descreve expressões de computação no F#, que fornece uma sintaxe conveniente para criar cálculos que podem ser sequenciados e combinados usando construções de fluxo de controle e associações. Eles podem ser usados para fornecer uma sintaxe conveniente para o *monads*, um recurso de programação funcional que pode ser usado para gerenciar dados, controle e efeitos colaterais em programas funcionais. Um tipo de expressão de computação, o fluxo de trabalho assíncrono, oferece suporte para computações paralelas e assíncronas. Para saber mais, consulte [Fluxos de Trabalho Assíncronos](asynchronous-workflows.md).|
|[Fluxos de trabalho assíncronos](asynchronous-workflows.md)|Descreve os fluxos de trabalho assíncronos, um recurso de linguagem que permite que você escreva código assíncrono de forma que seja muito próximo da maneira como você escreveria naturalmente código síncrono.|
|[Citações de código](code-quotations.md)|Descreve citações de código, um recurso de linguagem que permite gerar e trabalhar com expressões de código F# programaticamente.|
|[Expressões de consulta](query-expressions.md)|Descreve expressões de consulta, um recurso de linguagem que implementa o LINQ para F# e permite escrever consultas em uma fonte de dados ou uma coleção enumerável.|

## <a name="compiler-supported-constructs"></a>Construções com suporte do compilador

A tabela a seguir lista tópicos que descrevem as construções especiais com suporte do compilador.

|Tópico|Descrição|
|-----|-----------|
|[Opções do compilador](compiler-options.md)|Descreve as opções de linha de comando para o compilador do F#.|
|[Diretivas de compilador](compiler-directives.md)|Descreve as diretivas de processador e diretivas de compilador.|
|[Identificadores de linha, arquivo e caminho de origem](source-line-file-path-identifiers.md)|Descreve os identificadores `__LINE__` , `__SOURCE_DIRECTORY__` e `__SOURCE_FILE__` , que são valores internos que permitem que você acesse o número da linha de origem, o diretório e o nome do arquivo em seu código.|
