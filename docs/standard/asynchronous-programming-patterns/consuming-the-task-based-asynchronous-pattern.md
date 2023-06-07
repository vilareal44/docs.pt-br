---
title: Consumindo o padrão assíncrono baseado em tarefa
description: Aprenda a consumir o padrão assíncrono baseado em tarefa (toque) ao trabalhar com operações assíncronas.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- .NET Framework, and TAP
- asynchronous design patterns, .NET Framework
- TAP, .NET Framework support for
- Task-based Asynchronous Pattern, .NET Framework support for
- .NET Framework, asynchronous design patterns
ms.assetid: 033cf871-ae24-433d-8939-7a3793e547bf
ms.openlocfilehash: 68b1f723b3dcc4fd16073a653a778aa480cfa32e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621750"
---
# <a name="consuming-the-task-based-asynchronous-pattern"></a>Consumindo o padrão assíncrono baseado em tarefa

Quando você usa o TAP (padrão assíncrono baseado em tarefa) para trabalhar com operações assíncronas, você pode usar retornos de chamada para obter uma espera sem bloqueio.  Para tarefas, isso é conseguido por meio de métodos como <xref:System.Threading.Tasks.Task.ContinueWith%2A?displayProperty=nameWithType>. O suporte assíncrono baseado em linguagem oculta retornos de chamada ao permitir que operações assíncronas sejam colocadas em espera no fluxo de controle normal, sendo que o código gerado pelo compilador fornece esse mesmo suporte de nível de API.

## <a name="suspending-execution-with-await"></a>Suspendendo a execução com Await
 Começando com o .NET Framework 4.5, você pode usar a palavra-chave [await](../../csharp/language-reference/operators/await.md) em C# e o [Operador Await](../../visual-basic/language-reference/operators/await-operator.md) no Visual Basic para aguardar os objetos <xref:System.Threading.Tasks.Task> e <xref:System.Threading.Tasks.Task%601> de modo assíncrono. Quando você está aguardando um <xref:System.Threading.Tasks.Task>, a expressão `await` é do tipo `void`. Quando você está aguardando um <xref:System.Threading.Tasks.Task%601>, a expressão `await` é do tipo `TResult`. Uma expressão `await` deve ocorrer dentro do corpo de um método assíncrono. Para obter mais informações sobre suporte às linguagens C# e Visual Basic no .NET Framework 4.5, consulte as especificações das linguagens C# e Visual Basic.

 Nos bastidores, a funcionalidade await instala um retorno de chamada na tarefa pelo uso de uma continuação.  Esse retorno de chamada retoma o método assíncrono no ponto de suspensão. Quando o método assíncrono é retomado, se a operação de espera foi concluída com êxito e era uma <xref:System.Threading.Tasks.Task%601>, o respectivo `TResult` é retornado.  Se o <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601> que foi aguardado terminou no estado <xref:System.Threading.Tasks.TaskStatus.Canceled>, uma exceção <xref:System.OperationCanceledException> será lançada.  Se o <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601> que foi aguardado terminou no estado <xref:System.Threading.Tasks.TaskStatus.Faulted>, uma exceção que causou a falha será lançada. Uma `Task` pode falhar como resultado de várias exceções, mas apenas uma dessas exceções é propagada. No entanto, a propriedade <xref:System.Threading.Tasks.Task.Exception%2A?displayProperty=nameWithType> retorna uma exceção <xref:System.AggregateException> que contém todos os erros.

 Se um contexto de sincronização ( <xref:System.Threading.SynchronizationContext> objeto) estiver associado ao thread que estava executando o método assíncrono no momento da suspensão (por exemplo, se a <xref:System.Threading.SynchronizationContext.Current%2A?displayProperty=nameWithType> propriedade não for `null` ), o método assíncrono retomará o mesmo contexto de sincronização usando o método do contexto <xref:System.Threading.SynchronizationContext.Post%2A> . Caso contrário, ele utiliza o Agendador de Tarefas (objeto <xref:System.Threading.Tasks.TaskScheduler>) que era atual no momento da suspensão. Normalmente, esse é que o agendador de tarefas padrão (<xref:System.Threading.Tasks.TaskScheduler.Default%2A?displayProperty=nameWithType>), que tem como alvo o pool de threads. O Agendador de Tarefas determina se a operação assíncrona aguardada deve continuar do ponto em que foi concluída ou se a retomada deve ser agendada. O agendador padrão geralmente permite que a continuação seja executada no thread em que a operação aguardada foi concluída.

 Quando um método assíncrono é chamado, ele executa o corpo da função de modo síncrono até a primeira expressão await em uma instância aguardável que ainda não tenha foi concluída, no ponto em que a invocação retorna ao chamador. Se o método assíncrono não retornar `void`, um objeto <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601> será retornado para representar o cálculo em andamento. Em um método assíncrono não nulo, se uma instrução return for encontrada ou o final do corpo do método for atingido, a tarefa será concluída no estado final <xref:System.Threading.Tasks.TaskStatus.RanToCompletion>. Se uma exceção sem tratamento fizer com que o controle deixe o corpo do método assíncrono, a tarefa terminará no estado <xref:System.Threading.Tasks.TaskStatus.Faulted>. Se essa exceção for um <xref:System.OperationCanceledException>, a tarefa terminará no estado <xref:System.Threading.Tasks.TaskStatus.Canceled>. Dessa maneira, a exceção ou o resultado é eventualmente publicado.

 Há diversas variações importantes desse comportamento.  Por motivos de desempenho, se uma tarefa já foi concluída até o momento em que a tarefa é aguardada, o controle não é suspenso e a função continua a executar.  Além disso, retornar para o contexto original nem sempre é o comportamento desejado e pode ser alterado; isso é descrito mais detalhadamente na próxima seção.

### <a name="configuring-suspension-and-resumption-with-yield-and-configureawait"></a>Configurando a suspensão e retomada com Yield e ConfigureAwait
 Vários métodos fornecem mais controle sobre a execução de um método assíncrono. Por exemplo, você pode usar o método <xref:System.Threading.Tasks.Task.Yield%2A?displayProperty=nameWithType> para introduzir um ponto de rendimento para o método assíncrono:

```csharp
public class Task : …
{
    public static YieldAwaitable Yield();
    …
}
```

 Isso é equivalente a postar assincronamente ou agendar de volta para o contexto atual.

```csharp
Task.Run(async delegate
{
    for(int i=0; i<1000000; i++)
    {
        await Task.Yield(); // fork the continuation into a separate work item
        ...
    }
});
```

 Você também pode usar o método <xref:System.Threading.Tasks.Task.ConfigureAwait%2A?displayProperty=nameWithType> para controlar melhor a suspensão e a retomada de um método assíncrono.  Como mencionado anteriormente, por padrão, o contexto atual é capturado no momento em que um método assíncrono é suspenso e esse contexto capturado é usado para invocar a continuação do método assíncrono após a retomada.  Em muitos casos, esse é o comportamento exato desejado.  Em outros casos, você pode não se importar com o contexto de continuação e pode obter o melhor desempenho ao evitar essas postagens de volta para o contexto original.  Para habilitar isso, use o método <xref:System.Threading.Tasks.Task.ConfigureAwait%2A?displayProperty=nameWithType> para informar à operação de espera que não capture e retome no contexto, mas sim continue a execução no ponto em que a operação assíncrona que estava sendo aguardada foi concluída, seja qual for esse ponto:

```csharp
await someTask.ConfigureAwait(continueOnCapturedContext:false);
```

## <a name="canceling-an-asynchronous-operation"></a>Cancelando uma operação assíncrona
 A partir do NET Framework 4, os métodos TAP que dão suporte ao cancelamento fornecem pelo menos uma sobrecarga que aceita um token de cancelamento (objeto <xref:System.Threading.CancellationToken>).

 Um token de cancelamento é criado por meio de uma origem de token de cancelamento (objeto <xref:System.Threading.CancellationTokenSource>).  A propriedade da fonte <xref:System.Threading.CancellationTokenSource.Token%2A> retorna o token de cancelamento que será sinalizado quando o método de origem <xref:System.Threading.CancellationTokenSource.Cancel%2A> for chamado.  Por exemplo, se você quiser baixar uma única página da Web e quiser poder cancelar a operação, crie um <xref:System.Threading.CancellationTokenSource> objeto, passe seu token para o método TAP e, em seguida, chame o método da origem <xref:System.Threading.CancellationTokenSource.Cancel%2A> quando estiver pronto para cancelar a operação:

```csharp
var cts = new CancellationTokenSource();
string result = await DownloadStringAsync(url, cts.Token);
… // at some point later, potentially on another thread
cts.Cancel();
```

 Para cancelar várias invocações assíncronas, você pode passar o mesmo token para todas as invocações:

```csharp
var cts = new CancellationTokenSource();
    IList<string> results = await Task.WhenAll(from url in urls select DownloadStringAsync(url, cts.Token));
    // at some point later, potentially on another thread
    …
    cts.Cancel();
```

 Ou então, você pode passar o mesmo token para um subconjunto seletivo de operações:

```csharp
var cts = new CancellationTokenSource();
    byte [] data = await DownloadDataAsync(url, cts.Token);
    await SaveToDiskAsync(outputPath, data, CancellationToken.None);
    … // at some point later, potentially on another thread
    cts.Cancel();
```

 Solicitações de cancelamento podem ser iniciadas de qualquer thread.

 Você pode passar o valor <xref:System.Threading.CancellationToken.None%2A?displayProperty=nameWithType> a qualquer método que aceite um token de cancelamento para indicar que o cancelamento nunca será solicitado.  Isso faz a propriedade <xref:System.Threading.CancellationToken.CanBeCanceled%2A?displayProperty=nameWithType> retornar `false`, e o método chamado pode ser otimizado adequadamente.  Para fins de teste, você também pode passar um token de cancelamento previamente cancelado que é instanciado pelo uso do construtor que aceita um valor booliano para indicar se o token deve iniciar em um estado não cancelável ou já cancelado.

 Essa abordagem para o cancelamento tem várias vantagens:

- Você pode passar o mesmo token de cancelamento para qualquer número de operações síncronas e assíncronas.

- A mesma solicitação de cancelamento pode ser proliferada para qualquer número de ouvintes.

- O desenvolvedor da API assíncrona tem total controle com relação ao cancelamento poder ou não ser solicitado e a quando isso pode entrar em vigor.

- O código que consome a API pode, seletivamente, determinar as invocações assíncronas para as quais as solicitações de cancelamento serão propagadas.

## <a name="monitoring-progress"></a>Monitorando o progresso
 Alguns métodos assíncronos expõem progresso através de uma interface de progresso passada para o método assíncrono.  Por exemplo, considere uma função que baixa de forma assíncrona uma cadeia de texto e, ao longo do processo, gera atualizações de andamento que incluem a porcentagem do download que foi concluída até o momento.  Tal método poderia ser consumido em um aplicativo do WPF (Windows Presentation Foundation) da seguinte maneira:

```csharp
private async void btnDownload_Click(object sender, RoutedEventArgs e)
{
    btnDownload.IsEnabled = false;
    try
    {
        txtResult.Text = await DownloadStringAsync(txtUrl.Text,
            new Progress<int>(p => pbDownloadProgress.Value = p));
    }
    finally { btnDownload.IsEnabled = true; }
}
```

<a name="combinators"></a>
## <a name="using-the-built-in-task-based-combinators"></a>Usando os combinadores baseados em tarefas internos
 O namespace <xref:System.Threading.Tasks> inclui vários métodos para compor tarefas e trabalhar com elas.

### <a name="taskrun"></a>Task.Run
 A classe <xref:System.Threading.Tasks.Task> inclui vários métodos <xref:System.Threading.Tasks.Task.Run%2A> que permitem descarregar com facilidade o trabalho como um <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601> para o pool de threads, por exemplo:

```csharp
public async void button1_Click(object sender, EventArgs e)
{
    textBox1.Text = await Task.Run(() =>
    {
        // … do compute-bound work here
        return answer;
    });
}
```

 Alguns desses métodos <xref:System.Threading.Tasks.Task.Run%2A>, como a sobrecarga de <xref:System.Threading.Tasks.Task.Run%28System.Func%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType>, existe como um atalho para o método <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType>.  Outras sobrecargas, como <xref:System.Threading.Tasks.Task.Run%28System.Func%7BSystem.Threading.Tasks.Task%7D%29?displayProperty=nameWithType>, permitem que você use await dentro do trabalho descarregado, por exemplo:

```csharp
public async void button1_Click(object sender, EventArgs e)
{
    pictureBox1.Image = await Task.Run(async() =>
    {
        using(Bitmap bmp1 = await DownloadFirstImageAsync())
        using(Bitmap bmp2 = await DownloadSecondImageAsync())
        return Mashup(bmp1, bmp2);
    });
}
```

 Essas sobrecargas são logicamente equivalentes a usar o método <xref:System.Threading.Tasks.TaskFactory.StartNew%2A?displayProperty=nameWithType> junto com o método de extensão <xref:System.Threading.Tasks.TaskExtensions.Unwrap%2A> na biblioteca de paralelismo de tarefas.

### <a name="taskfromresult"></a>Task.FromResult
 Use o método <xref:System.Threading.Tasks.Task.FromResult%2A> em cenários nos quais os dados talvez já estejam disponíveis e apenas precisem ser retornados de um método de retorno de tarefas elevado para uma <xref:System.Threading.Tasks.Task%601>:

```csharp
public Task<int> GetValueAsync(string key)
{
    int cachedValue;
    return TryGetCachedValue(out cachedValue) ?
        Task.FromResult(cachedValue) :
        GetValueAsyncInternal();
}

private async Task<int> GetValueAsyncInternal(string key)
{
    …
}
```

### <a name="taskwhenall"></a>Task.WhenAll
 Use o método <xref:System.Threading.Tasks.Task.WhenAll%2A> para aguardar de forma assíncrona várias operações assíncronas que são representadas como tarefas.  O método tem várias sobrecargas que dão suporte a um conjunto de tarefas não genéricas ou um conjunto não uniforme de tarefas genéricas (por exemplo, aguardar de forma assíncrona várias operações que retornam nulo ou aguardar vários métodos que retornam um valor em que cada valor pode ter um tipo diferente) e dão suporte a um conjunto uniforme de tarefas genéricas (como aguardar de forma assíncrona vários métodos que retornam `TResult`).

 Digamos que você deseja enviar mensagens de email para vários clientes. Você pode sobrepor o envio de mensagens de modo que você não aguarde a conclusão de uma mensagem antes de enviar a próxima. Você também pode descobrir quando as operações de envio foram concluídas e se ocorreu algum erro:

```csharp
IEnumerable<Task> asyncOps = from addr in addrs select SendMailAsync(addr);
await Task.WhenAll(asyncOps);
```

 Esse código não tratará explicitamente exceções que possam ocorrer, mas permitirá que as exceções sejam propagadas para fora do `await` na tarefa resultante de <xref:System.Threading.Tasks.Task.WhenAll%2A>.  Para tratar as exceções, você pode usar código como o seguinte:

```csharp
IEnumerable<Task> asyncOps = from addr in addrs select SendMailAsync(addr);
try
{
    await Task.WhenAll(asyncOps);
}
catch(Exception exc)
{
    ...
}
```

 Nesse caso, se alguma operação assíncrona falhar, todas as exceções serão consolidadas em uma exceção <xref:System.AggregateException>, que é armazenada no <xref:System.Threading.Tasks.Task> que é retornado do método <xref:System.Threading.Tasks.Task.WhenAll%2A>.  No entanto, apenas uma dessas exceções é propagada pela palavra-chave `await`.  Se você quiser examinar todas as exceções, poderá reescrever o código anterior da seguinte maneira:

```csharp
Task [] asyncOps = (from addr in addrs select SendMailAsync(addr)).ToArray();
try
{
    await Task.WhenAll(asyncOps);
}
catch(Exception exc)
{
    foreach(Task faulted in asyncOps.Where(t => t.IsFaulted))
    {
        … // work with faulted and faulted.Exception
    }
}
```

 Vamos considerar um exemplo em que baixamos vários arquivos da Web de forma assíncrona.  Nesse caso, todas as operações assíncronas têm tipos de resultado homogêneos e é fácil acessar os resultados:

```csharp
string [] pages = await Task.WhenAll(
    from url in urls select DownloadStringAsync(url));
```

 Você pode usar as mesmas técnicas de tratamento de exceções discutidas no cenário anterior, em que nulo é retornado:

```csharp
Task<string> [] asyncOps =
    (from url in urls select DownloadStringAsync(url)).ToArray();
try
{
    string [] pages = await Task.WhenAll(asyncOps);
    ...
}
catch(Exception exc)
{
    foreach(Task<string> faulted in asyncOps.Where(t => t.IsFaulted))
    {
        … // work with faulted and faulted.Exception
    }
}
```

### <a name="taskwhenany"></a>Task.WhenAny
 Você pode usar o método <xref:System.Threading.Tasks.Task.WhenAny%2A> para aguardar de forma assíncrona apenas uma de várias operações assíncronas que são representadas como tarefas.  Esse método tem quatro casos de uso principais:

- Redundância: executar uma operação várias vezes e selecionar aquela que é concluída primeiro (por exemplo, entrando em contato com vários serviços Web de cotação de ações que produzirão um único resultado e selecionar aquele que termina mais rápido).

- Intercalação: iniciar várias operações e aguardar que todas eles sejam concluídas, mas processando-as conforme são concluídas.

- Limitação: permitir que operações adicionais comecem conforme outras forem concluídas.  Essa é uma extensão do cenário de intercalação.

- Saída precoce: por exemplo, uma operação representada pela tarefa t1 pode ser agrupada em uma tarefa <xref:System.Threading.Tasks.Task.WhenAny%2A> com outra tarefa t2 e você pode aguardar na tarefa <xref:System.Threading.Tasks.Task.WhenAny%2A>. A tarefa t2 pode representar um tempo limite, um cancelamento ou algum outro sinal que faça com que a tarefa <xref:System.Threading.Tasks.Task.WhenAny%2A> seja concluída antes de t1.

#### <a name="redundancy"></a>Redundância
 Considere um caso em que você deseja tomar uma decisão sobre a possibilidade de comprar uma ação.  Há diversos serviços Web de recomendação de compra de ações nos quais você confia, mas dependendo da carga diária, cada serviço pode acabar sendo lento em horários diferentes.  Você pode usar o método <xref:System.Threading.Tasks.Task.WhenAny%2A> para receber uma notificação quando qualquer operação for concluída:

```csharp
var recommendations = new List<Task<bool>>()
{
    GetBuyRecommendation1Async(symbol),
    GetBuyRecommendation2Async(symbol),
    GetBuyRecommendation3Async(symbol)
};
Task<bool> recommendation = await Task.WhenAny(recommendations);
if (await recommendation) BuyStock(symbol);
```

 Ao contrário de <xref:System.Threading.Tasks.Task.WhenAll%2A>, que retorna os resultados não encapsulados de todas as tarefas concluídas com êxito, o <xref:System.Threading.Tasks.Task.WhenAny%2A> retorna a tarefa que foi concluída. Se uma tarefa falhar, é importante saber que ela falhou e, se uma tarefa for bem-sucedida, é importante saber a qual tarefa o valor de retorno está associado.  Portanto, você precisa acessar o resultado da tarefa retornada ou aguardá-la ainda mais, conforme mostra este exemplo.

 Assim como acontece com o <xref:System.Threading.Tasks.Task.WhenAll%2A>, você precisa ser capaz de acomodar as exceções.  Devido a você receber novamente a tarefa concluída, você poderá esperar a tarefa retornada para então propagar os erros e aplicar `try/catch` a eles adequadamente; por exemplo:

```csharp
Task<bool> [] recommendations = …;
while(recommendations.Count > 0)
{
    Task<bool> recommendation = await Task.WhenAny(recommendations);
    try
    {
        if (await recommendation) BuyStock(symbol);
        break;
    }
    catch(WebException exc)
    {
        recommendations.Remove(recommendation);
    }
}
```

 Além disso, mesmo que uma primeira tarefa seja concluída com êxito, as tarefas subsequentes poderão falhar.  Neste ponto, você tem várias opções para lidar com exceções: você pode esperar até que todas as tarefas iniciadas tenham sido concluídas, caso em que você pode usar o método <xref:System.Threading.Tasks.Task.WhenAll%2A>, ou você pode decidir que todas as exceções são importantes e devem estar conectadas.  Para isso, você pode usar as continuações para receber uma notificação quando tarefas foram concluídas de forma assíncrona:

```csharp
foreach(Task recommendation in recommendations)
{
    var ignored = recommendation.ContinueWith(
        t => { if (t.IsFaulted) Log(t.Exception); });
}
```

 ou:

```csharp
foreach(Task recommendation in recommendations)
{
    var ignored = recommendation.ContinueWith(
        t => Log(t.Exception), TaskContinuationOptions.OnlyOnFaulted);
}
```

 ou até mesmo:

```csharp
private static async void LogCompletionIfFailed(IEnumerable<Task> tasks)
{
    foreach(var task in tasks)
    {
        try { await task; }
        catch(Exception exc) { Log(exc); }
    }
}
…
LogCompletionIfFailed(recommendations);
```

 Finalmente, você talvez queira cancelar todas as operações restantes:

```csharp
var cts = new CancellationTokenSource();
var recommendations = new List<Task<bool>>()
{
    GetBuyRecommendation1Async(symbol, cts.Token),
    GetBuyRecommendation2Async(symbol, cts.Token),
    GetBuyRecommendation3Async(symbol, cts.Token)
};

Task<bool> recommendation = await Task.WhenAny(recommendations);
cts.Cancel();
if (await recommendation) BuyStock(symbol);
```

#### <a name="interleaving"></a>Intercalação
 Considere um caso em que você está baixando imagens da Web e processando cada imagem (por exemplo, adicionando a imagem a um controle de interface do usuário). Você processa as imagens sequencialmente no thread da interface do usuário, mas deseja baixar as imagens da maneira mais simultânea possível. Além disso, você não deseja manter a adição das imagens à interface do usuário até que todas sejam baixadas. Em vez disso, você deseja adicioná-los conforme eles são concluídos.

```csharp
List<Task<Bitmap>> imageTasks =
    (from imageUrl in urls select GetBitmapAsync(imageUrl)).ToList();
while(imageTasks.Count > 0)
{
    try
    {
        Task<Bitmap> imageTask = await Task.WhenAny(imageTasks);
        imageTasks.Remove(imageTask);

        Bitmap image = await imageTask;
        panel.AddImage(image);
    }
    catch{}
}
```

 Você também pode aplicar interpolação para um cenário que envolve o processamento de recursos computacionais no <xref:System.Threading.ThreadPool> das imagens baixadas; por exemplo:

```csharp
List<Task<Bitmap>> imageTasks =
    (from imageUrl in urls select GetBitmapAsync(imageUrl)
         .ContinueWith(t => ConvertImage(t.Result)).ToList();
while(imageTasks.Count > 0)
{
    try
    {
        Task<Bitmap> imageTask = await Task.WhenAny(imageTasks);
        imageTasks.Remove(imageTask);

        Bitmap image = await imageTask;
        panel.AddImage(image);
    }
    catch{}
}
```

#### <a name="throttling"></a>Limitação
 Considere o exemplo de intercalação, exceto que o usuário está baixando um número tão grande de imagens que os downloads precisam ser limitados; por exemplo, você deseja que apenas um número específico de downloads ocorram simultaneamente. Para conseguir isso, você pode iniciar um subconjunto das operações assíncronas.  Conforme as operações forem concluídas, você pode iniciar operações adicionais para assumir o lugar delas:

```csharp
const int CONCURRENCY_LEVEL = 15;
Uri [] urls = …;
int nextIndex = 0;
var imageTasks = new List<Task<Bitmap>>();
while(nextIndex < CONCURRENCY_LEVEL && nextIndex < urls.Length)
{
    imageTasks.Add(GetBitmapAsync(urls[nextIndex]));
    nextIndex++;
}

while(imageTasks.Count > 0)
{
    try
    {
        Task<Bitmap> imageTask = await Task.WhenAny(imageTasks);
        imageTasks.Remove(imageTask);

        Bitmap image = await imageTask;
        panel.AddImage(image);
    }
    catch(Exception exc) { Log(exc); }

    if (nextIndex < urls.Length)
    {
        imageTasks.Add(GetBitmapAsync(urls[nextIndex]));
        nextIndex++;
    }
}
```

#### <a name="early-bailout"></a>Saída precoce
 Considere que você está aguardando assincronamente que uma operação seja concluída enquanto responde simultaneamente à solicitação de cancelamento de um usuário (por exemplo, o usuário clicou em um botão Cancelar). O código a seguir ilustra esse cenário:

```csharp
private CancellationTokenSource m_cts;

public void btnCancel_Click(object sender, EventArgs e)
{
    if (m_cts != null) m_cts.Cancel();
}

public async void btnRun_Click(object sender, EventArgs e)
{
    m_cts = new CancellationTokenSource();
    btnRun.Enabled = false;
    try
    {
        Task<Bitmap> imageDownload = GetBitmapAsync(txtUrl.Text);
        await UntilCompletionOrCancellation(imageDownload, m_cts.Token);
        if (imageDownload.IsCompleted)
        {
            Bitmap image = await imageDownload;
            panel.AddImage(image);
        }
        else imageDownload.ContinueWith(t => Log(t));
    }
    finally { btnRun.Enabled = true; }
}

private static async Task UntilCompletionOrCancellation(
    Task asyncOp, CancellationToken ct)
{
    var tcs = new TaskCompletionSource<bool>();
    using(ct.Register(() => tcs.TrySetResult(true)))
        await Task.WhenAny(asyncOp, tcs.Task);
    return asyncOp;
}
```

 Essa implementação reabilita a interface do usuário assim que você decide sair, mas não as cancela as operações assíncronas subjacentes. Outra alternativa seria cancelar as operações pendentes quando você decidir desestabilizar, mas não restabelecer a interface do usuário até que as operações sejam concluídas, possivelmente devido ao encerramento antecipado devido à solicitação de cancelamento:

```csharp
private CancellationTokenSource m_cts;

public async void btnRun_Click(object sender, EventArgs e)
{
    m_cts = new CancellationTokenSource();

    btnRun.Enabled = false;
    try
    {
        Task<Bitmap> imageDownload = GetBitmapAsync(txtUrl.Text, m_cts.Token);
        await UntilCompletionOrCancellation(imageDownload, m_cts.Token);
        Bitmap image = await imageDownload;
        panel.AddImage(image);
    }
    catch(OperationCanceledException) {}
    finally { btnRun.Enabled = true; }
}
```

 Outro exemplo de saída precoce envolve o uso do método <xref:System.Threading.Tasks.Task.WhenAny%2A> junto com o método <xref:System.Threading.Tasks.Task.Delay%2A>, conforme discutido na próxima seção.

### <a name="taskdelay"></a>Task.Delay
 Você pode usar o <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> método para introduzir pausas em uma execução de método assíncrono.  Isso é útil para muitos tipos de funcionalidades, incluindo o build de loops de sondagem e o atraso da manipulação da entrada do usuário por um período de tempo predeterminado.  O método <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> também pode ser útil em combinação com o <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> para implementação de tempos limite em esperas.

 Se uma tarefa que faz parte de uma operação assíncrona maior (por exemplo, um serviço Web ASP.NET) demorar muito para ser concluída, a operação geral poderá ser prejudicada, especialmente se ela não for concluída.  Por esse motivo, é importante poder atingir o tempo limite ao aguardar uma operação assíncrona.  Os métodos <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType>, <xref:System.Threading.Tasks.Task.WaitAll%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Task.WaitAny%2A?displayProperty=nameWithType> aceitam valores de tempo limite, mas os correspondentes <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A?displayProperty=nameWithType>/<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> e os métodos <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>/<xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> mencionados anteriormente não.  Em vez disso, você pode usar <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> em conjunto para implementar um tempo limite.

 Por exemplo, em seu aplicativo de interface do usuário, digamos que você deseja baixar uma imagem e desabilitar a interface do usuário durante esse download. No entanto, se o download levar muito tempo, você desejará reabilitar a interface do usuário e descartar o download:

```csharp
public async void btnDownload_Click(object sender, EventArgs e)
{
    btnDownload.Enabled = false;
    try
    {
        Task<Bitmap> download = GetBitmapAsync(url);
        if (download == await Task.WhenAny(download, Task.Delay(3000)))
        {
            Bitmap bmp = await download;
            pictureBox.Image = bmp;
            status.Text = "Downloaded";
        }
        else
        {
            pictureBox.Image = null;
            status.Text = "Timed out";
            var ignored = download.ContinueWith(
                t => Trace("Task finally completed"));
        }
    }
    finally { btnDownload.Enabled = true; }
}
```

 O mesmo aplica-se a vários downloads, porque <xref:System.Threading.Tasks.Task.WhenAll%2A> retorna uma tarefa:

```csharp
public async void btnDownload_Click(object sender, RoutedEventArgs e)
{
    btnDownload.Enabled = false;
    try
    {
        Task<Bitmap[]> downloads =
            Task.WhenAll(from url in urls select GetBitmapAsync(url));
        if (downloads == await Task.WhenAny(downloads, Task.Delay(3000)))
        {
            foreach(var bmp in downloads.Result) panel.AddImage(bmp);
            status.Text = "Downloaded";
        }
        else
        {
            status.Text = "Timed out";
            downloads.ContinueWith(t => Log(t));
        }
    }
    finally { btnDownload.Enabled = true; }
}
```

## <a name="building-task-based-combinators"></a>Compilando combinadores baseados em tarefa
 Já que uma tarefa é capaz de representar totalmente uma operação assíncrona e fornecer recursos síncronos e assíncronos para ingressar na operação, recuperar seus resultados e assim por diante, você pode compilar bibliotecas úteis de combinadores que compõem tarefas para compilar padrões maiores.  Conforme discutido na seção anterior, o .NET Framework inclui vários combinadores internos, mas você também pode compilar os seus próprios combinadores. As seções a seguir fornecem vários exemplos de tipos e métodos combinadores potenciais.

### <a name="retryonfault"></a>RetryOnFault
 Em muitas situações, talvez você queira repetir uma operação se uma tentativa anterior falhar.  Para código síncrono, você pode compilar um método auxiliar como `RetryOnFault` no exemplo a seguir para fazer isso:

```csharp
public static T RetryOnFault<T>(
    Func<T> function, int maxTries)
{
    for(int i=0; i<maxTries; i++)
    {
        try { return function(); }
        catch { if (i == maxTries-1) throw; }
    }
    return default(T);
}
```

 Você pode compilar um método auxiliar quase idêntico para operações assíncronas implementadas com TAP e, assim, retornar tarefas:

```csharp
public static async Task<T> RetryOnFault<T>(
    Func<Task<T>> function, int maxTries)
{
    for(int i=0; i<maxTries; i++)
    {
        try { return await function().ConfigureAwait(false); }
        catch { if (i == maxTries-1) throw; }
    }
    return default(T);
}
```

 Você pode usar esse combinador para codificar repetições na lógica do aplicativo; por exemplo:

```csharp
// Download the URL, trying up to three times in case of failure
string pageContents = await RetryOnFault(
    () => DownloadStringAsync(url), 3);
```

 Você pode estender ainda mais a função `RetryOnFault`. Por exemplo, a função poderá aceitar outro `Func<Task>` que será invocado entre as tentativas para determinar quando tentar novamente a operação, por exemplo:

```csharp
public static async Task<T> RetryOnFault<T>(
    Func<Task<T>> function, int maxTries, Func<Task> retryWhen)
{
    for(int i=0; i<maxTries; i++)
    {
        try { return await function().ConfigureAwait(false); }
        catch { if (i == maxTries-1) throw; }
        await retryWhen().ConfigureAwait(false);
    }
    return default(T);
}
```

 Você pode então usar a função da seguinte maneira, para aguardar um segundo antes de repetir a operação:

```csharp
// Download the URL, trying up to three times in case of failure,
// and delaying for a second between retries
string pageContents = await RetryOnFault(
    () => DownloadStringAsync(url), 3, () => Task.Delay(1000));
```

### <a name="needonlyone"></a>NeedOnlyOne
 Às vezes, você pode aproveitar a redundância para melhorar a latência de uma operação e as chances de sucesso.  Considere vários serviços Web que fornecem cotações de ações, mas em diferentes horas do dia, cada serviço pode fornecer diferentes níveis de qualidade e tempos de resposta.  Para lidar com essas flutuações, você poderá emitir solicitações para todos os serviços Web e, assim que você obtiver uma resposta de um deles, cancelar as solicitações restantes.  Você pode implementar uma função auxiliar para facilitar a implementação desse padrão comum de inicialização de várias operações, aguardar uma delas e, em seguida, cancelar o restante. A função `NeedOnlyOne` no exemplo a seguir ilustra esse cenário:

```csharp
public static async Task<T> NeedOnlyOne(
    params Func<CancellationToken,Task<T>> [] functions)
{
    var cts = new CancellationTokenSource();
    var tasks = (from function in functions
                 select function(cts.Token)).ToArray();
    var completed = await Task.WhenAny(tasks).ConfigureAwait(false);
    cts.Cancel();
    foreach(var task in tasks)
    {
        var ignored = task.ContinueWith(
            t => Log(t), TaskContinuationOptions.OnlyOnFaulted);
    }
    return completed;
}
```

 Você pode então usar essa função conforme demonstrado a seguir:

```csharp
double currentPrice = await NeedOnlyOne(
    ct => GetCurrentPriceFromServer1Async("msft", ct),
    ct => GetCurrentPriceFromServer2Async("msft", ct),
    ct => GetCurrentPriceFromServer3Async("msft", ct));
```

### <a name="interleaved-operations"></a>Operações intercaladas
 Há um possível problema de desempenho com o uso do <xref:System.Threading.Tasks.Task.WhenAny%2A> método para dar suporte a um cenário de intercalação quando você estiver trabalhando com grandes conjuntos de tarefas. Todas as chamadas para <xref:System.Threading.Tasks.Task.WhenAny%2A> fazem uma continuação ser registrada com cada tarefa. Para N número de tarefas, isso resulta em o (N<sup>2</sup>) de continuação criadas durante o tempo de vida da operação de intercalação. Se você estiver trabalhando com um grande conjunto de tarefas, poderá usar um combinador ( `Interleaved` no exemplo a seguir) para resolver o problema de desempenho:

```csharp
static IEnumerable<Task<T>> Interleaved<T>(IEnumerable<Task<T>> tasks)
{
    var inputTasks = tasks.ToList();
    var sources = (from _ in Enumerable.Range(0, inputTasks.Count)
                   select new TaskCompletionSource<T>()).ToList();
    int nextTaskIndex = -1;
    foreach (var inputTask in inputTasks)
    {
        inputTask.ContinueWith(completed =>
        {
            var source = sources[Interlocked.Increment(ref nextTaskIndex)];
            if (completed.IsFaulted)
                source.TrySetException(completed.Exception.InnerExceptions);
            else if (completed.IsCanceled)
                source.TrySetCanceled();
            else
                source.TrySetResult(completed.Result);
        }, CancellationToken.None,
           TaskContinuationOptions.ExecuteSynchronously,
           TaskScheduler.Default);
    }
    return from source in sources
           select source.Task;
}
```

 Você poderá usar o combinador para processar os resultados das tarefas conforme elas forem concluídas, por exemplo:

```csharp
IEnumerable<Task<int>> tasks = ...;
foreach(var task in Interleaved(tasks))
{
    int result = await task;
    …
}
```

### <a name="whenallorfirstexception"></a>WhenAllOrFirstException
 Em determinados cenários de dispersão/coleta, convém esperar por todas as tarefas em um conjunto, a menos que uma delas falhe, caso em que você desejará interromper a espera assim que a exceção ocorrer.  Você pode realizar isso com um método combinador tal como `WhenAllOrFirstException` no exemplo a seguir:

```csharp
public static Task<T[]> WhenAllOrFirstException<T>(IEnumerable<Task<T>> tasks)
{
    var inputs = tasks.ToList();
    var ce = new CountdownEvent(inputs.Count);
    var tcs = new TaskCompletionSource<T[]>();

    Action<Task> onCompleted = (Task completed) =>
    {
        if (completed.IsFaulted)
            tcs.TrySetException(completed.Exception.InnerExceptions);
        if (ce.Signal() && !tcs.Task.IsCompleted)
            tcs.TrySetResult(inputs.Select(t => t.Result).ToArray());
    };

    foreach (var t in inputs) t.ContinueWith(onCompleted);
    return tcs.Task;
}
```

## <a name="building-task-based-data-structures"></a>Compilando estruturas de dados com base em tarefa
 Além da capacidade de criar combinadores baseados em tarefas personalizados, ter uma estrutura de dados no <xref:System.Threading.Tasks.Task> e <xref:System.Threading.Tasks.Task%601> que representa os resultados de uma operação assíncrona e a sincronização necessária para ingressar com ela torna um tipo poderoso no qual criar estruturas de dados personalizadas para serem usadas em cenários assíncronos.

### <a name="asynccache"></a>AsyncCache
 Um aspecto importante de uma tarefa é que ela pode ser enviada para vários consumidores, todos os quais podem esperar por ela, registrar continuações com ela, obter seu resultado ou exceções (no caso de <xref:System.Threading.Tasks.Task%601>) e assim por diante.  Isso torna <xref:System.Threading.Tasks.Task> e <xref:System.Threading.Tasks.Task%601> perfeitamente adequado para ser usado em uma infraestrutura de cache assíncrona.  Aqui está um exemplo de um cache assíncrono pequeno, mas poderoso, criado com base em <xref:System.Threading.Tasks.Task%601> :

```csharp
public class AsyncCache<TKey, TValue>
{
    private readonly Func<TKey, Task<TValue>> _valueFactory;
    private readonly ConcurrentDictionary<TKey, Lazy<Task<TValue>>> _map;

    public AsyncCache(Func<TKey, Task<TValue>> valueFactory)
    {
        if (valueFactory == null) throw new ArgumentNullException("loader");
        _valueFactory = valueFactory;
        _map = new ConcurrentDictionary<TKey, Lazy<Task<TValue>>>();
    }

    public Task<TValue> this[TKey key]
    {
        get
        {
            if (key == null) throw new ArgumentNullException("key");
            return _map.GetOrAdd(key, toAdd =>
                new Lazy<Task<TValue>>(() => _valueFactory(toAdd))).Value;
        }
    }
}
```

 A [classe \<TKey,TValue> AsyncCache](https://devblogs.microsoft.com/pfxteam/parallelextensionsextras-tour-12-asynccache/) aceita como um delegado para seu Construtor uma função que usa um `TKey` e retorna um <xref:System.Threading.Tasks.Task%601> .  Quaisquer valores do cache previamente acessados são armazenados no dicionário interno e `AsyncCache` garante que apenas uma tarefa seja gerada por chave, mesmo que o cache seja acessado simultaneamente.

 Por exemplo, você pode compilar um cache para páginas da Web baixadas:

```csharp
private AsyncCache<string,string> m_webPages =
    new AsyncCache<string,string>(DownloadStringAsync);
```

 Você pode em seguida usar esse cache em métodos assíncronos sempre que precisar do conteúdo de uma página da Web. A `AsyncCache` classe garante que você está baixando o mínimo de páginas possível e armazena os resultados em cache.

```csharp
private async void btnDownload_Click(object sender, RoutedEventArgs e)
{
    btnDownload.IsEnabled = false;
    try
    {
        txtContents.Text = await m_webPages["https://www.microsoft.com"];
    }
    finally { btnDownload.IsEnabled = true; }
}
```

### <a name="asyncproducerconsumercollection"></a>AsyncProducerConsumerCollection
 Você também pode usar tarefas para compilar estruturas de dados para coordenar atividades assíncronas.  Considere um dos padrões de design paralelos clássicos: produtor/consumidor.  Nesse padrão, produtores geram dados que são consumidos pelos consumidores e produtores e consumidores podem executar em paralelo. Por exemplo, o consumidor processa o item 1, que anteriormente foi gerado por um produtor que agora está produzindo o item 2.  Para o padrão de produtor/consumidor, você precisa invariavelmente que alguma estrutura de dados armazene o trabalho criado pelos produtores para que os consumidores possam ser notificados sobre novos dados e localizá-los quando disponíveis.

 Aqui está uma estrutura de dados simples, criada com base em tarefas, que permite que métodos assíncronos sejam usados como produtores e consumidores:

```csharp
public class AsyncProducerConsumerCollection<T>
{
    private readonly Queue<T> m_collection = new Queue<T>();
    private readonly Queue<TaskCompletionSource<T>> m_waiting =
        new Queue<TaskCompletionSource<T>>();

    public void Add(T item)
    {
        TaskCompletionSource<T> tcs = null;
        lock (m_collection)
        {
            if (m_waiting.Count > 0) tcs = m_waiting.Dequeue();
            else m_collection.Enqueue(item);
        }
        if (tcs != null) tcs.TrySetResult(item);
    }

    public Task<T> Take()
    {
        lock (m_collection)
        {
            if (m_collection.Count > 0)
            {
                return Task.FromResult(m_collection.Dequeue());
            }
            else
            {
                var tcs = new TaskCompletionSource<T>();
                m_waiting.Enqueue(tcs);
                return tcs.Task;
            }
        }
    }
}
```

 Com essa estrutura de dados estabelecida, você pode escrever código como o seguinte:

```csharp
private static AsyncProducerConsumerCollection<int> m_data = …;
…
private static async Task ConsumerAsync()
{
    while(true)
    {
        int nextItem = await m_data.Take();
        ProcessNextItem(nextItem);
    }
}
…
private static void Produce(int data)
{
    m_data.Add(data);
}
```

O namespace <xref:System.Threading.Tasks.Dataflow> inclui o tipo <xref:System.Threading.Tasks.Dataflow.BufferBlock%601>, que pode ser usado de maneira semelhante, mas sem a necessidade de criar um tipo de coleção personalizado:

```csharp
private static BufferBlock<int> m_data = …;
…
private static async Task ConsumerAsync()
{
    while(true)
    {
        int nextItem = await m_data.ReceiveAsync();
        ProcessNextItem(nextItem);
    }
}
…
private static void Produce(int data)
{
    m_data.Post(data);
}
```

> [!NOTE]
> O namespace <xref:System.Threading.Tasks.Dataflow> está disponível no .NET Framework 4.5 por meio de **NuGet**. Para instalar o assembly que contém o namespace <xref:System.Threading.Tasks.Dataflow>, abra seu projeto no Visual Studio, escolha **Gerenciar Pacotes NuGet** no menu Projeto e procure online pelo pacote Microsoft.Tpl.Dataflow.

## <a name="see-also"></a>Consulte também

- [TAP (Padrão Assíncrono Baseado em Tarefa)](task-based-asynchronous-pattern-tap.md)
- [Implementando o padrão assíncrono baseado em tarefa](implementing-the-task-based-asynchronous-pattern.md)
- [Interoperabilidade com outros tipos e padrões assíncronos](interop-with-other-asynchronous-patterns-and-types.md)
