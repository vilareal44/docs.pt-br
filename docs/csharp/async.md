---
title: Programação assíncrona – C#
description: Saiba mais sobre o modelo de programação assíncrona em nível linguagem do C# fornecido pelo .NET Core.
author: cartermp
ms.date: 05/20/2020
ms.technology: csharp-async
ms.assetid: b878c34c-a78f-419e-a594-a2b44fa521a4
ms.openlocfilehash: 35ba90f978b1993f80451a28a4cd08129afddd85
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864495"
---
# <a name="asynchronous-programming"></a>Programação assíncrona

Se você tiver alguma necessidade de ligação de e/s (como solicitar dados de uma rede, acessar um banco de dado ou ler e gravar em um sistema de arquivos), você desejará utilizar a programação assíncrona. Você também pode ter código vinculado à CPU, como a execução de um cálculo dispendioso, que também é um bom cenário para escrever código assíncrono.

O C# tem um modelo de programação assíncrona de nível de linguagem, que permite escrever código assíncrono facilmente, sem precisar fazer malabarismos com retornos de chamada ou estar em conformidade com uma biblioteca com suporte para assincronia. Ele segue o que é conhecido como [TAP (Padrão assíncrono baseado em tarefa)](../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md).

## <a name="overview-of-the-asynchronous-model"></a>Visão geral do modelo assíncrono

O núcleo da programação assíncrona são os objetos `Task` e `Task<T>`, que modelam as operações assíncronas. Eles têm suporte das palavras-chave `async` e `await`. O modelo é bastante simples na maioria dos casos:

- Para O código ligado à e/s, você aguarda uma operação que retorna um `Task` ou `Task<T>` dentro de um `async` método.
- Para código vinculado à CPU, você aguarda uma operação iniciada em um thread em segundo plano com o <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> método.

É na palavra-chave `await` que a mágica acontece. Ela cede o controle para o chamador do método que executou `await` e, em última instância, permite que uma interface do usuário tenha capacidade de resposta ou que um serviço seja elástico. Embora [haja maneiras](../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md) de abordar um código assíncrono diferente de `async` e `await` , este artigo se concentra nas construções de nível de linguagem.

### <a name="io-bound-example-download-data-from-a-web-service"></a>Exemplo associado de e/s: baixar dados de um serviço Web

Talvez seja necessário baixar alguns dados de um serviço Web quando um botão for pressionado, mas não quiser bloquear o thread da interface do usuário. Isso pode ser feito da seguinte maneira:

```csharp
private readonly HttpClient _httpClient = new HttpClient();

downloadButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI as the request
    // from the web service is happening.
    //
    // The UI thread is now free to perform other work.
    var stringData = await _httpClient.GetStringAsync(URL);
    DoSomethingWithData(stringData);
};
```

O código expressa a intenção (Baixando dados de forma assíncrona) sem sobrecarga-lo na interação com `Task` objetos.

### <a name="cpu-bound-example-perform-a-calculation-for-a-game"></a>Exemplo associado à CPU: executar um cálculo para um jogo

Digamos que você está escrevendo um jogo para dispositivo móvel em que, ao pressionar um botão, poderá causar danos a muitos inimigos na tela. A realização do cálculo de dano pode ser dispendiosa e fazê-lo no thread da interface do usuário faria com que o jogo parecesse pausar durante a realização do cálculo!

A melhor maneira de lidar com isso é iniciar um thread em segundo plano, que faz o trabalho usando `Task.Run` e aguardar seu resultado usando `await` . Isso permite que a interface do usuário fique tranqüila, pois o trabalho está sendo feito.

```csharp
private DamageResult CalculateDamageDone()
{
    // Code omitted:
    //
    // Does an expensive calculation and returns
    // the result of that calculation.
}

calculateButton.Clicked += async (o, e) =>
{
    // This line will yield control to the UI while CalculateDamageDone()
    // performs its work. The UI thread is free to perform other work.
    var damageResult = await Task.Run(() => CalculateDamageDone());
    DisplayDamage(damageResult);
};
```

Esse código expressa claramente a intenção do evento de clique do botão. Ele não requer o gerenciamento manual de um thread em segundo plano e ele faz isso sem bloqueios.

### <a name="what-happens-under-the-covers"></a>O que acontece nos bastidores

Há muitas partes móveis nas quais as operações assíncronas se preocupam. Se você estiver curioso sobre o que está acontecendo nos bastidores do `Task` e do `Task<T>` , consulte o artigo sobre o [Async em profundidade](../standard/async-in-depth.md) para obter mais informações.

No lado do C# das coisas, o compilador transforma seu código em um computador de estado que controla coisas como produzir a execução quando um `await` é atingido e retomar a execução quando um trabalho em segundo plano é concluído.

Para os que gostam da teoria, essa é uma implementação do [Modelo Promise de assincronia](https://en.wikipedia.org/wiki/Futures_and_promises).

## <a name="key-pieces-to-understand"></a>Principais partes a serem compreendidas

* O código assíncrono pode ser usado tanto para o código vinculado à E/S quanto vinculado à CPU, mas de maneira diferente para cada cenário.
* O código assíncrono usa `Task<T>` e `Task`, que são constructos usados para modelar o trabalho que está sendo feito em segundo plano.
* A palavra-chave `async` transforma um método em um método assíncrono, o que permite que você use a palavra-chave `await` em seu corpo.
* Quando a palavra-chave `await` é aplicada, ela suspende o método de chamada e transfere o controle de volta ao seu chamador até que a tarefa em espera seja concluída.
* A `await` só pode ser usada dentro de um método assíncrono.

## <a name="recognize-cpu-bound-and-io-bound-work"></a>Reconhecer O trabalho associado à CPU e à e/s

Os primeiros dois exemplos deste guia mostraram como você pode usar `async` e `await` para trabalho vinculado à E/S e vinculado à CPU. É fundamental que você saiba identificar quando um trabalho que você precisa fazer é vinculado à E/S ou vinculado à CPU, porque isso pode afetar significativamente o desempenho do seu código e poderia potencialmente levar ao uso indevido de determinados constructos.

Aqui estão duas perguntas que devem ser feitas antes de escrever qualquer código:

1. Seu código ficará em "espera" por alguma coisa, como dados de um banco de dados?

   Se a resposta é "sim", seu trabalho é **vinculado à E/S**.

1. Seu código estará executando uma computação cara?

   Se você respondeu "sim", seu trabalho é **vinculado à CPU**.

Se o seu trabalho for **vinculado à E/S**, use `async` e `await` *sem* `Task.Run`. Você *não deve* usar a biblioteca de paralelismo de tarefas. O motivo disso é descrito em [Async em profundidade](../standard/async-in-depth.md).

Se o trabalho que você tem está **vinculado à CPU** e você se preocupa com a capacidade de resposta, use `async` and `await` , mas gera o trabalho em outro thread *com* `Task.Run` . Se o trabalho for apropriado para simultaneidade e paralelismo, considere também usar a [biblioteca de tarefas paralelas](../standard/parallel-programming/task-parallel-library-tpl.md).

Além disso, você sempre deve medir a execução do seu código. Por exemplo, talvez você tenha uma situação em que seu trabalho vinculado à CPU não é caro o suficiente em comparação com os custos gerais das trocas de contexto ao realizar o multithreading. Cada opção tem vantagens e desvantagens e você deve escolher o que é correto para a sua situação.

## <a name="more-examples"></a>Mais exemplos

Os exemplos a seguir demonstram várias maneiras para escrever código assíncrono no C#. Elas abordam alguns cenários diferentes que você pode encontrar.

### <a name="extract-data-from-a-network"></a>Extrair dados de uma rede

Esse trecho de código baixa o HTML da Home Page em <https://dotnetfoundation.org> e conta o número de vezes que a cadeia de caracteres ".net" ocorre no HTML. Ele usa ASP.NET para definir um método de controlador da API Web, que executa essa tarefa e retorna o número.

> [!NOTE]
> Se você pretende fazer análise de HTML no código de produção, não use expressões regulares. Use uma biblioteca de análise.

```csharp
private readonly HttpClient _httpClient = new HttpClient();

[HttpGet, Route("DotNetCount")]
public async Task<int> GetDotNetCount()
{
    // Suspends GetDotNetCount() to allow the caller (the web server)
    // to accept another request, rather than blocking on this one.
    var html = await _httpClient.GetStringAsync("https://dotnetfoundation.org");

    return Regex.Matches(html, @"\.NET").Count;
}
```

Aqui está o mesmo cenário escrito para um aplicativo universal do Windows, que executa a mesma tarefa quando um botão for pressionado:

```csharp
private readonly HttpClient _httpClient = new HttpClient();

private async void OnSeeTheDotNetsButtonClick(object sender, RoutedEventArgs e)
{
    // Capture the task handle here so we can await the background task later.
    var getDotNetFoundationHtmlTask = _httpClient.GetStringAsync("https://dotnetfoundation.org");

    // Any other work on the UI thread can be done here, such as enabling a Progress Bar.
    // This is important to do here, before the "await" call, so that the user
    // sees the progress bar before execution of this method is yielded.
    NetworkProgressBar.IsEnabled = true;
    NetworkProgressBar.Visibility = Visibility.Visible;

    // The await operator suspends OnSeeTheDotNetsButtonClick(), returning control to its caller.
    // This is what allows the app to be responsive and not block the UI thread.
    var html = await getDotNetFoundationHtmlTask;
    int count = Regex.Matches(html, @"\.NET").Count;

    DotNetCountLabel.Text = $"Number of .NETs on dotnetfoundation.org: {count}";

    NetworkProgressBar.IsEnabled = false;
    NetworkProgressBar.Visibility = Visibility.Collapsed;
}
```

### <a name="wait-for-multiple-tasks-to-complete"></a>Aguardar a conclusão de várias tarefas

Você pode encontrar em uma situação em que precisa recuperar várias partes de dados simultaneamente. A `Task` API contém dois métodos <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> , que permitem que você escreva um código assíncrono que executa uma espera sem bloqueio em vários trabalhos em segundo plano.

Este exemplo mostra como você pode obter os dados `User` para um conjunto de `userId`s.

```csharp
public async Task<User> GetUserAsync(int userId)
{
    // Code omitted:
    //
    // Given a user Id {userId}, retrieves a User object corresponding
    // to the entry in the database with {userId} as its Id.
}

public static async Task<IEnumerable<User>> GetUsersAsync(IEnumerable<int> userIds)
{
    var getUserTasks = new List<Task<User>>();
    foreach (int userId in userIds)
    {
        getUserTasks.Add(GetUserAsync(userId));
    }

    return await Task.WhenAll(getUserTasks);
}
```

Esta é outra maneira de escrever isso de forma mais sucinta, usando LINQ:

```csharp
public async Task<User> GetUserAsync(int userId)
{
    // Code omitted:
    //
    // Given a user Id {userId}, retrieves a User object corresponding
    // to the entry in the database with {userId} as its Id.
}

public static async Task<User[]> GetUsersAsync(IEnumerable<int> userIds)
{
    var getUserTasks = userIds.Select(id => GetUserAsync(id));
    return await Task.WhenAll(getUserTasks);
}
```

Embora seja menos código, tome cuidado ao misturar LINQ com código assíncrono. Como o LINQ utiliza a execução adiada (lenta), as chamadas assíncronas não acontecerão imediatamente como em um loop `foreach`, a menos que você force a sequência gerada a iterar com uma chamada a `.ToList()` ou `.ToArray()`.

## <a name="important-info-and-advice"></a>Informações importantes e conselhos

Com a programação assíncrona, há alguns detalhes a serem considerados, o que pode impedir um comportamento inesperado.

* Os métodos `async` ** precisam ter uma palavra-chave** `await` ** no corpo ou eles nunca transferirão!**

  É importante ter isso em mente. Se `await` não for usado no corpo de um `async` método, o compilador C# gerará um aviso, mas o código será compilado e executado como se fosse um método normal. Isso é incrivelmente ineficiente, pois a máquina de estado gerada pelo compilador C# para o método assíncrono não está realizando nada.

* **Você deve adicionar "Async" como o sufixo de cada nome de método assíncrono que escrever.**

Essa é a convenção usada no .NET para diferenciar mais facilmente os métodos síncronos e assíncronos. Determinados métodos que não são chamados explicitamente pelo seu código (como manipuladores de eventos ou métodos de controlador da Web) não necessariamente se aplicam. Como eles não são chamados explicitamente pelo seu código, ser explícito sobre a nomenclatura não é tão importante.

*  O `async void` ** só deve ser usado para manipuladores de eventos.**

O `async void` é a única maneira de permitir que os manipuladores de eventos assíncronos trabalhem, pois os eventos não têm tipos de retorno (portanto, não podem fazer uso de `Task` e `Task<T>`). Qualquer outro uso de `async void` não segue o modelo TAP e pode ser um desafio utilizá-lo, como:

* Exceções geradas em um `async void` método não podem ser detectadas fora desse método.
* `async void`os métodos são difíceis de testar.
* `async void`os métodos podem causar efeitos colaterais ruins se o chamador não estiver esperando que eles sejam assíncronos.

* **Vá com cuidado ao usar lambdas assíncronas em expressões LINQ**

As expressões lambda no LINQ usam a execução adiada, o que significa que o código pode acabar sendo executado por vez, quando você não está esperando. A introdução de tarefas de bloqueio no meio disso poderia facilmente resultar em um deadlock, se não estivessem escritas corretamente. Além disso, o aninhamento de código assíncrono dessa maneira também pode dificultar a ponderação a respeito da execução do código. A assíncrona e a LINQ são poderosas, mas devem ser usadas de uma maneira mais cuidadosa e clara possível.

* **Escrever código que aguarda tarefas de uma maneira sem bloqueio**

O bloqueio do thread atual como um meio de esperar por um `Task` para concluir pode resultar em deadlocks e threads de contexto bloqueados e pode exigir tratamento de erros mais complexo. A tabela a seguir fornece orientação sobre como lidar com a espera de tarefas de uma maneira sem bloqueio:

| Use isto...          | Em vez disto...           | Quando desejar fazer isso...                 |
|----------------------|------------------------------|--------------------------------------------|
| `await`              | `Task.Wait` ou `Task.Result` | Recuperação do resultado de uma tarefa em segundo plano |
| `await Task.WhenAny` | `Task.WaitAny`               | Aguardar a conclusão de qualquer tarefa           |
| `await Task.WhenAll` | `Task.WaitAll`               | Aguardar a conclusão de todas as tarefas          |
| `await Task.Delay`   | `Thread.Sleep`               | Aguardar por um período de tempo               |

* **Considere usar** `ValueTask` **sempre que possível**

Retornar um objeto `Task` de métodos assíncronos pode introduzir gargalos de desempenho em determinados caminhos. `Task` é um tipo de referência, portanto, usá-lo significa alocar um objeto. Nos casos em que um método declarado com o `async` modificador retorna um resultado em cache ou é concluído de forma síncrona, as alocações extras podem se tornar um custo de tempo significativo nas seções críticas de desempenho do código. Isso pode se tornar caro se essas alocações ocorrem em loops rígidos. Para obter mais informações, consulte [tipos de retorno assíncrono generalizados](whats-new/csharp-7.md#generalized-async-return-types).

* **Considere usar** `ConfigureAwait(false)`

Uma pergunta comum é "quando devo usar o <xref:System.Threading.Tasks.Task.ConfigureAwait(System.Boolean)?displayProperty=nameWithType> método?". O método permite que uma `Task` instância configure seu aguardador. Essa é uma consideração importante e a configuração incorreta pode potencialmente ter implicações de desempenho e até mesmo deadlocks. Para obter mais informações sobre `ConfigureAwait` o, consulte as [perguntas frequentes do ConfigureAwait](https://devblogs.microsoft.com/dotnet/configureawait-faq).

* **Escrever código com menos monitoração de estado**

Não dependa do estado de objetos globais ou da execução de determinados métodos. Em vez disso, depender apenas dos valores retornados dos métodos. Por quê?

* Será mais fácil raciocinar sobre o código.
* O código será mais fácil de testar.
* Misturar código assíncrono e síncrono será muito mais simples.
* As condições de corrida poderão, normalmente, ser completamente evitadas.
* Dependendo dos valores retornados, a coordenação de código assíncrono se tornará simples.
* (Bônus) funciona muito bem com a injeção de dependência.

Uma meta recomendada é alcançar a [Transparência referencial](https://en.wikipedia.org/wiki/Referential_transparency_%28computer_science%29) completa ou quase completa em seu código. Isso resultará em uma base de código extremamente previsível, testável e de fácil manutenção.

## <a name="other-resources"></a>Outros recursos

* A [Programação assíncrona em detalhes](../standard/async-in-depth.md) fornece mais informações sobre o funcionamento de Tarefas.
* [Programação assíncrona com async e await (C#)](./programming-guide/concepts/async/index.md)
* [Seis dicas essenciais para a programação assíncrona](https://channel9.msdn.com/Series/Three-Essential-Tips-for-Async) de Lucian Wischik é um ótimo recurso para a programação assíncrona
