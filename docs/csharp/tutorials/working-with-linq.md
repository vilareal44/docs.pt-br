---
title: Trabalhando com LINQ
description: Este tutorial ensina a gerar sequências com LINQ, escrever métodos para uso em consultas LINQ e diferenciar entre avaliação lenta e detalhada.
ms.date: 10/29/2018
ms.technology: csharp-linq
ms.assetid: 0db12548-82cb-4903-ac88-13103d70aa77
ms.openlocfilehash: 9bc17700e22ea29b1861945a220e397a90b9a7c1
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656990"
---
# <a name="work-with-language-integrated-query-linq"></a>Trabalhar com consulta integrada à linguagem (LINQ)

## <a name="introduction"></a>Introdução

Este tutorial ensina os recursos no .NET Core e da linguagem C#. Você aprenderá a:

- Gerar sequências com LINQ.
- Métodos de gravação que podem ser facilmente usados em consultas LINQ.
- Distinguir entre a avaliação rápida e lenta.

Você aprenderá essas técnicas ao compilar um aplicativo que demonstra uma das habilidades básicas de qualquer mágico: o [embaralhamento faro](https://en.wikipedia.org/wiki/Faro_shuffle). Em resumo, um embaralhamento faro é uma técnica em que você divide um baralho de cartas exatamente na metade, então as cartas de cada metade são colocadas em ordem aleatória até recriar o conjunto original.

Os mágicos usam essa técnica porque cada carta é fica em um local conhecido após o embaralhamento e a ordem é um padrão de repetição.

Para os seus propósitos, vamos examinar rapidamente as sequências de manipulação de dados. O aplicativo que você criar construirá um baralho e, em seguida, executará uma sequência de embaralhamentos, gravando a sequência a cada vez. Você também comparará a ordem atualizada com a ordem original.

Este tutorial tem várias etapas. Após cada etapa, você poderá executar o aplicativo e ver o progresso. Você também poderá ver o [exemplo concluído](https://github.com/dotnet/samples/blob/master/csharp/getting-started/console-linq) no repositório dotnet/samples do GitHub. Para obter instruções de download, consulte [Exemplos e tutoriais](../../samples-and-tutorials/index.md#view-and-download-samples).

## <a name="prerequisites"></a>Pré-requisitos

Você precisará configurar seu computador para executar o .NET Core. Você pode encontrar as instruções de instalação na página de [download do .NET Core](https://dotnet.microsoft.com/download) . Você pode executar esse aplicativo no Windows, Ubuntu Linux ou OS X ou em um contêiner do Docker. Será necessário instalar o editor de código de sua preferência. As descrições abaixo usam [Visual Studio Code](https://code.visualstudio.com/) que é um editor de plataforma cruzada de software livre. No entanto, você pode usar quaisquer ferramentas que esteja familiarizado.

## <a name="create-the-application"></a>Criar o aplicativo

A primeira etapa é criar um novo aplicativo. Abra um prompt de comando e crie um novo diretório para seu aplicativo. Torne ele o diretório atual. Digite o comando `dotnet new console` no prompt de comando. Isso cria os arquivos iniciais de um aplicativo "Olá, Mundo" básico.

Se você nunca usou C# antes, [este tutorial](console-teleprompter.md) explicará a estrutura de um programa C#. Você pode ler e, em seguida, voltar aqui para saber mais sobre o LINQ.

## <a name="create-the-data-set"></a>Criar o conjunto de dados

Antes de começar, verifique se as linhas a seguir estão na parte superior do arquivo `Program.cs` gerado pelo `dotnet new console`:

```csharp
// Program.cs
using System;
using System.Collections.Generic;
using System.Linq;
```

Se essas três linhas (instruções `using`) não estiverem na parte superior do arquivo, nosso programa não será compilado.

Agora que você tem todas as referências necessárias, considere o que forma um baralho de cartas. Um baralho de cartas costuma ter quatro naipes, e cada naipe tem treze valores. Normalmente, talvez você pense em criar uma classe `Card` logo de cara e preencher uma coleção de objetos `Card` manualmente. Com o LINQ, dá para ser mais conciso do que a forma comum de criação de um baralho de cartas. Em vez de criar uma classe `Card`, você pode criar duas sequências para representar naipes e valores, respectivamente. Você vai criar um par muito simples de [*métodos iteradores*](../iterators.md#enumeration-sources-with-iterator-methods) que gerará as valores e naipes como <xref:System.Collections.Generic.IEnumerable%601>s de cadeias de caracteres:

```csharp
// Program.cs
// The Main() method

static IEnumerable<string> Suits()
{
    yield return "clubs";
    yield return "diamonds";
    yield return "hearts";
    yield return "spades";
}

static IEnumerable<string> Ranks()
{
    yield return "two";
    yield return "three";
    yield return "four";
    yield return "five";
    yield return "six";
    yield return "seven";
    yield return "eight";
    yield return "nine";
    yield return "ten";
    yield return "jack";
    yield return "queen";
    yield return "king";
    yield return "ace";
}
```

Coloque-as sob o método `Main` em seu arquivo `Program.cs`. Esses dois métodos utilizam a sintaxe `yield return` para produzir uma sequência à medida que eles são executados. O compilador compila um objeto que implementa <xref:System.Collections.Generic.IEnumerable%601> e gera a sequência de cadeias de caracteres conforme solicitado.

Agora, use esses métodos iteradores para criar o baralho de cartas. Você colocará a consulta do LINQ em nosso método `Main`. Dê uma olhada:

```csharp
// Program.cs
static void Main(string[] args)
{
    var startingDeck = from s in Suits()
                       from r in Ranks()
                       select new { Suit = s, Rank = r };

    // Display each card that we've generated and placed in startingDeck in the console
    foreach (var card in startingDeck)
    {
        Console.WriteLine(card);
    }
}
```

As várias cláusulas `from` produzem um <xref:System.Linq.Enumerable.SelectMany%2A>, que cria uma única sequência da combinação entre cada elemento na primeira sequência com cada elemento na segunda sequência. A ordem é importante para nossos objetivos. O primeiro elemento na primeira sequência de fonte (Naipes) é combinado com cada elemento na segunda sequência (Valores). Isso produz todas as treze cartas do primeiro naipe. Esse processo é repetido com cada elemento na primeira sequência (naipes). O resultado final é um baralho ordenado por naipes, seguido pelos valores.

É importante lembrar que se você optar por escrever seu LINQ na sintaxe de consulta usada acima, ou se decidir usar a sintaxe de método, sempre será possível alternar entre as formas de sintaxe. A consulta acima escrita em sintaxe de consulta pode ser escrita na sintaxe de método como:

```csharp
var startingDeck = Suits().SelectMany(suit => Ranks().Select(rank => new { Suit = suit, Rank = rank }));
```

O compilador traduz instruções LINQ escritas com a sintaxe de consulta na sintaxe de chamada do método equivalente. Portanto, independentemente de sua escolha de sintaxe, as duas versões da consulta produzem o mesmo resultado. Escolha qual sintaxe funciona melhor para a sua situação: por exemplo, se você estiver trabalhando em uma equipe em que alguns dos membros têm dificuldade com a sintaxe de método, prefira usar a sintaxe de consulta.

Vá em frente e execute o exemplo que você criou neste momento. Ele exibirá todas as 52 cartas do baralho. Talvez seja muito útil executar esse exemplo em um depurador para observar como os métodos `Suits()` e `Ranks()` são executados. Você pode ver claramente que cada cadeia de caracteres em cada sequência é gerada apenas conforme o necessário.

![Uma janela de console mostrando o aplicativo gravando 52 cartas.](./media/working-with-linq/console-52-card-application.png)

## <a name="manipulate-the-order"></a>Manipular o pedido

Em seguida, concentre-se em como você vai embaralhar as cartas no baralho. A primeira etapa de qualquer embaralhada é dividir o baralho em dois. Os métodos <xref:System.Linq.Enumerable.Take%2A> e <xref:System.Linq.Enumerable.Skip%2A> que fazem parte das APIs do LINQ fornecem esse recurso para você. Coloque-os sob o loop `foreach`:

```csharp
// Program.cs
public static void Main(string[] args)
{
    var startingDeck = from s in Suits()
                       from r in Ranks()
                       select new { Suit = s, Rank = r };

    foreach (var c in startingDeck)
    {
        Console.WriteLine(c);
    }

    // 52 cards in a deck, so 52 / 2 = 26
    var top = startingDeck.Take(26);
    var bottom = startingDeck.Skip(26);
}
```

No entanto, não há método de embaralhamento na biblioteca padrão, portanto, você precisará escrever o seu. O método de embaralhamento que você criará ilustra várias técnicas que você usará com programas baseados em LINQ. Portanto, cada parte desse processo será explicado nas etapas.

Para adicionar funcionalidade ao seu modo de interação com o <xref:System.Collections.Generic.IEnumerable%601> recebido de volta das consultas do LINQ, precisará escrever alguns tipos especiais de métodos chamados [métodos de extensão](../programming-guide/classes-and-structs/extension-methods.md). Em resumo, um método de extensão é um *método estático* de objetivo especial que adiciona novas funcionalidades a um tipo já existentes, sem ter que modificar o tipo original ao qual você deseja adicionar funcionalidade.

Dê aos seus métodos de extensão uma nova casa adicionando um novo arquivo de classe *estático* ao seu programa chamado `Extensions.cs`, depois, comece a criar o primeiro método de extensão:

```csharp
// Extensions.cs
using System;
using System.Collections.Generic;
using System.Linq;

namespace LinqFaroShuffle
{
    public static class Extensions
    {
        public static IEnumerable<T> InterleaveSequenceWith<T>(this IEnumerable<T> first, IEnumerable<T> second)
        {
            // Your implementation will go here soon enough
        }
    }
}
```

Examine a assinatura do método por um momento, principalmente os parâmetros:

```csharp
public static IEnumerable<T> InterleaveSequenceWith<T> (this IEnumerable<T> first, IEnumerable<T> second)
```

Você pode ver a adição do modificador `this` no primeiro argumento para o método. Isso significa que você chama o método como se fosse um método de membro do tipo do primeiro argumento. Esta declaração de método também segue um idioma padrão no qual os tipos de entrada e saídas são `IEnumerable<T>`. Essa prática permite que os métodos LINQ sejam encadeados para executar consultas mais complexas.

Naturalmente, como você dividiu o baralho em metades, precisará unir essas metades. No código, isso significa que você estará enumerando as duas sequências adquiridas por meio de <xref:System.Linq.Enumerable.Take%2A> e <xref:System.Linq.Enumerable.Skip%2A> ao mesmo tempo, *`interleaving`* os elementos e a criação de uma sequência: seu baralho de cartas agora em ordem aleatória. Escrever um método LINQ que funciona com duas sequências exige que você compreenda como <xref:System.Collections.Generic.IEnumerable%601> funciona.

A interface <xref:System.Collections.Generic.IEnumerable%601> tem um método: <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A>. O objeto retornado por <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> tem um método para mover para o próximo elemento e uma propriedade que recupera o elemento atual na sequência. Você usará esses dois membros para enumerar a coleção e retornar os elementos. Esse método de Intercalação será um método iterador, portanto, em vez de criar uma coleção e retornar a coleção, você usará a sintaxe `yield return` mostrada acima.

Aqui está a implementação desse método:

[!CODE-csharp[InterleaveSequenceWith](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet1)]

Agora que você escreveu esse método, vá até o método `Main` e embaralhe uma vez:

```csharp
// Program.cs
public static void Main(string[] args)
{
    var startingDeck = from s in Suits()
                       from r in Ranks()
                       select new { Suit = s, Rank = r };

    foreach (var c in startingDeck)
    {
        Console.WriteLine(c);
    }

    var top = startingDeck.Take(26);
    var bottom = startingDeck.Skip(26);
    var shuffle = top.InterleaveSequenceWith(bottom);

    foreach (var c in shuffle)
    {
        Console.WriteLine(c);
    }
}
```

## <a name="comparisons"></a>Comparações

Quantos embaralhamentos são necessários para colocar o baralho em sua ordem original? Para descobrir, você precisará escrever um método que determina se duas sequências são iguais. Depois de ter esse método, você precisará colocar o código de embaralhamento em um loop e verificar quando a apresentação estiver na ordem.

Escrever um método para determinar se as duas sequências são iguais deve ser simples. É uma estrutura semelhante para o método que você escreveu para embaralhar as cartas. Somente desta vez, em vez de o `yield return`rendimento retornar cada elemento, você comparará os elementos correspondentes de cada sequência. Quando toda a sequência tiver sido enumerada, se os elementos corresponderem, as sequências serão as mesmas:

[!CODE-csharp[SequenceEquals](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet2)]

Isso mostra uma segunda linguagem LINQ: métodos de terminal. Eles consideram uma sequência como entrada (ou, neste caso, duas sequências) e retornam um único valor escalar. Ao usar métodos de terminal, eles são sempre o método final em uma cadeia de métodos para uma consulta LINQ, por isso, o nome "terminal".

Você pode ver isso em ação ao usá-lo para determinar quando o baralho está em sua ordem original. Coloque o código de embaralhamento dentro de um loop e pare quando a sequência estiver em sua ordem original, aplicando o método `SequenceEquals()`. Você pode ver que esse sempre será o método final em qualquer consulta, porque ele retorna um valor único em vez de uma sequência:

```csharp
// Program.cs
static void Main(string[] args)
{
    // Query for building the deck

    // Shuffling using InterleaveSequenceWith<T>();

    var times = 0;
    // We can re-use the shuffle variable from earlier, or you can make a new one
    shuffle = startingDeck;
    do
    {
        shuffle = shuffle.Take(26).InterleaveSequenceWith(shuffle.Skip(26));

        foreach (var card in shuffle)
        {
            Console.WriteLine(card);
        }
        Console.WriteLine();
        times++;

    } while (!startingDeck.SequenceEquals(shuffle));

    Console.WriteLine(times);
}
```

Execute o código que obtivemos até agora e observe como o baralho é reorganizado em cada embaralhamento. Após 8 embaralhamentos (iterações do loop do-while), o baralho retorna à configuração original que estava quando você o criou pela primeira vez a partir da consulta LINQ inicial.

## <a name="optimizations"></a>Otimizações

O exemplo que você compilou até agora executa um *embaralhamento externo*, no qual as cartas superiores e inferiores permanecem as mesmas em cada execução. Vamos fazer uma alteração: em vez disso, usaremos um *embaralhamento interno*, em que todas as 52 cartas trocam de posição. Para um embaralhamento interno, intercale o baralho para que a primeira carta da metade inferior torne-se a primeira carta do baralho. Isso significa que a última carta na metade superior torna-se a carta inferior. Essa é uma alteração simples em uma única linha de código. Atualize a consulta atual de embaralhamento, alternando as posições de <xref:System.Linq.Enumerable.Take%2A> e <xref:System.Linq.Enumerable.Skip%2A>. Isso alterará a ordem das metades superior e inferior do baralho:

```csharp
shuffle = shuffle.Skip(26).InterleaveSequenceWith(shuffle.Take(26));
```

Execute o programa novamente e você verá que leva 52 iterações para o baralho ser reordenado. Você também começará a observar algumas degradações de desempenho graves à medida que o programa continuar a ser executado.

Existem muitas razões para isso. Você pode abordar uma das principais causas dessa queda de desempenho: uso ineficiente da [*avaliação lenta*](../programming-guide/concepts/linq/deferred-execution-and-lazy-evaluation-in-linq-to-xml.md).

Em resumo, a avaliação lenta informa que a avaliação de uma instrução não será executada até que seu valor seja necessário. Consultas LINQ são instruções avaliadas lentamente. As sequências são geradas somente quando os elementos são solicitados. Geralmente, esse é o principal benefício do LINQ. No entanto, em uso como esse programa, isso causa um crescimento exponencial no tempo de execução.

Lembre-se de que geramos o baralho original usando uma consulta LINQ. Cada embaralhamento é gerado executando três consultas LINQ no baralho anterior. Todos eles são executados lentamente. Isso também significa que eles são executados novamente sempre que a sequência é solicitada. Ao obter a 52ª iteração, você estará regenerando o baralho original muitas e muitas vezes. Vamos escrever um log para demonstrar esse comportamento. Em seguida, você poderá corrigir isso.

Em seu arquivo `Extensions.cs`, digite ou copie o método a seguir. Esse método de extensão cria um novo arquivo chamado `debug.log` em seu diretório do projeto, e registra qual consulta está sendo executada atualmente para o arquivo de log. Este método de extensão pode ser anexado a qualquer consulta para marcar que a consulta foi executada.

[!CODE-csharp[LogQuery](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet3)]

Você verá um rabisco vermelho sob `File`, que significa que ele não existe. Ele não será compilado, pois o compilador não sabe o que é `File`. Para resolver esse problema, é preciso que você adicione a linha de código a seguir abaixo da primeira linha em `Extensions.cs`:

```csharp
using System.IO;
```

Isso deve resolver o problema e o erro vermelho desaparece.

Em seguida, instrumente a definição de cada consulta com uma mensagem de log:

```csharp
// Program.cs
public static void Main(string[] args)
{
    var startingDeck = (from s in Suits().LogQuery("Suit Generation")
                        from r in Ranks().LogQuery("Rank Generation")
                        select new { Suit = s, Rank = r }).LogQuery("Starting Deck");

    foreach (var c in startingDeck)
    {
        Console.WriteLine(c);
    }

    Console.WriteLine();
    var times = 0;
    var shuffle = startingDeck;

    do
    {
        // Out shuffle
        /*
        shuffle = shuffle.Take(26)
            .LogQuery("Top Half")
            .InterleaveSequenceWith(shuffle.Skip(26)
            .LogQuery("Bottom Half"))
            .LogQuery("Shuffle");
        */

        // In shuffle
        shuffle = shuffle.Skip(26).LogQuery("Bottom Half")
                .InterleaveSequenceWith(shuffle.Take(26).LogQuery("Top Half"))
                .LogQuery("Shuffle");

        foreach (var c in shuffle)
        {
            Console.WriteLine(c);
        }

        times++;
        Console.WriteLine(times);
    } while (!startingDeck.SequenceEquals(shuffle));

    Console.WriteLine(times);
}
```

Observe que você não precisa fazer o registro sempre que acessar uma consulta. Você faz o registro ao criar a consulta original. O programa ainda leva muito tempo para ser executado, mas agora você pode ver o motivo. Se você não tiver paciência para executar o embaralhamento interno com o registro em log ativado, volte para o embaralhamento externo. Você ainda verá os efeitos da avaliação lenta. Em uma execução, ele faz 2592 consultas, incluindo a geração de todos os valores e naipes.

Aqui, você pode melhorar o desempenho do código para reduzir o número de execuções feitas. Uma correção simples possível é *armazenar em cache* os resultados da consulta do LINQ original que constrói o baralho de cartas. Atualmente, você executa as consultas novamente sempre que o loop do-while passa por uma iteração, construindo novamente o baralho de cartas e o embaralhamento de novo todas as vezes. Para armazenar em cache o baralho de cartas, aproveite os métodos LINQ <xref:System.Linq.Enumerable.ToArray%2A> e <xref:System.Linq.Enumerable.ToList%2A>; ao anexá-los às consultas, eles executarão as mesmas ações paras quais foram instruídos, mas agora armazenarão os resultados em uma matriz ou lista, dependendo de qual método você optar por chamar. Anexe o método LINQ <xref:System.Linq.Enumerable.ToArray%2A> às duas consultas e execute o programa novamente:

[!CODE-csharp[Main](../../../samples/snippets/csharp/getting-started/console-linq/Program.cs?name=snippet1)]

Agora, o embaralhamento externo contém 30 consultas. Execute novamente com o embaralhamento interno e você verá melhorias semelhantes: agora, executa 162 consultas.

Observe que esse exemplo é **projetado** para realçar os casos de uso em que a avaliação lenta pode causar problemas de desempenho. Embora seja importante ver onde a avaliação lenta pode afetar o desempenho do código, é igualmente importante entender que nem todas as consultas devem ser executadas avidamente. O desempenho incorrido sem usar <xref:System.Linq.Enumerable.ToArray%2A> ocorre porque cada nova disposição do baralho de cartas é criada com base na disposição anterior. Usar a avaliação lenta significa que cada nova disposição do baralho é criada do baralho original, até mesmo a execução do código que criou o `startingDeck`. Isso causa uma grande quantidade de trabalho extra.

Na prática, alguns algoritmos funcionam bem usando a avaliação detalhada, e outros executam funcionam melhor usando a avaliação lenta. Para o uso diário, a avaliação lenta é uma opção melhor quando a fonte de dados é um processo separado, como um mecanismo de banco de dados. Para os bancos de dados, a avaliação lenta permite que as consultas mais complexas executem apenas uma viagem de ida e volta para o processo de banco de dados e de volta para o restante do seu código. O LINQ é flexível, não importa se você optar por utilizar a avaliação lenta ou detalhada, portanto, meça seus processos e escolha o tipo de avaliação que ofereça o melhor desempenho.

## <a name="conclusion"></a>Conclusão

Neste projeto, abordamos:

- o uso de consultas LINQ para agregar dados em uma sequência significativa
- a produção de métodos de Extensão para adicionar nossa própria funcionalidade personalizada a consultas LINQ
- a localização de áreas em nosso código nas quais nossas consultas LINQ podem enfrentar problemas de desempenho, como diminuição da velocidade
- avaliação lenta e detalhada com relação às consultas LINQ, e as implicações que elas podem ter no desempenho da consulta

Além do LINQ, você aprendeu um pouco sobre uma técnica usada por mágicos para truques de carta. Os mágicos usam o embaralhamento Faro porque podem controlar onde cada carta fica no baralho. Agora que você sabe, não conte para os outros!

Para saber mais sobre o LINQ, consulte:

- [LINQ (Consulta Integrada à Linguagem)](../programming-guide/concepts/linq/index.md)
- [Introdução ao LINQ](../programming-guide/concepts/linq/index.md)
- [Operações de consulta LINQ básica (C#)](../programming-guide/concepts/linq/basic-linq-query-operations.md)
- [Transformações de dados com LINQ (C#)](../programming-guide/concepts/linq/data-transformations-with-linq.md)
- [Sintaxe de consulta e sintaxe de método em LINQ (C#)](../programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq.md)
- [funcionalidades do C# que dão suporte a LINQ](../programming-guide/concepts/linq/features-that-support-linq.md)
