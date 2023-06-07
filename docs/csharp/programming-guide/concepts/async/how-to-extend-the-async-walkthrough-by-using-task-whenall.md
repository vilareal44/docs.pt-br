---
title: Como estender a instrução assíncrona usando Task. WhenAll (C#)
description: Saiba como melhorar o desempenho da solução assíncrona em C# usando Task. WhenAll. Esse método aguarda assincronamente várias operações assíncronas.
ms.date: 07/20/2015
ms.assetid: f6927ef2-dc6c-43f8-bc82-bbeac42de423
ms.openlocfilehash: 275f4aeca854f8938642dbea40bf7fd9dc5362d9
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925209"
---
# <a name="how-to-extend-the-async-walkthrough-by-using-taskwhenall-c"></a>Como estender a instrução assíncrona usando Task. WhenAll (C#)

Você pode melhorar o desempenho da solução assíncrona em [Passo a passo: acessando a Web usando async e await (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md), usando o método <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>. Esse método aguarda de maneira assíncrona várias operações assíncronas, que são representadas como uma coleção de tarefas.

Você deve ter notado no passo a passo que os sites fazem o download em taxas diferentes. Às vezes, um dos sites está muito lento e isso atrasa todos os downloads restantes. Ao executar as soluções assíncronas que você compilou no passo a passo, você poderá finalizar o programa facilmente se não quiser esperar, mas uma opção melhor seria iniciar todos os downloads ao mesmo tempo e permitir que os downloads mais rápidos continuem, sem aguardar o que está atrasado.

Você aplica o método `Task.WhenAll` a uma coleção de tarefas. A aplicação de `WhenAll` retorna uma única tarefa que não será concluída até a conclusão de cada tarefa na coleção. As tarefas parecem ser executadas em paralelo, mas não são criados threads adicionais. As tarefas podem ser concluídas em qualquer ordem.

> [!IMPORTANT]
> Os procedimentos a seguir descrevem as extensões para os aplicativos assíncronos que são desenvolvidos no [Passo a passo: acessando a Web usando async e await (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md). Você pode desenvolver os aplicativos concluindo o passo a passo ou baixando o código em [Exemplos de código para desenvolvedores](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f).
>
> Para executar o exemplo, você deve ter o Visual Studio 2012 ou mais recente instalado no seu computador.

### <a name="to-add-taskwhenall-to-your-geturlcontentsasync-solution"></a>Para adicionar o Task.WhenAll à sua solução GetURLContentsAsync

1. Adicione o método `ProcessURLAsync` ao primeiro aplicativo que é desenvolvido no [Passo a passo: acessando a Web usando async e await (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md).

    - Se você baixou o código de [Exemplos de código para desenvolvedores](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f), abra o projeto AsyncWalkthrough e, em seguida, adicione `ProcessURLAsync` ao arquivo MainWindow.xaml.cs.

    - Se você desenvolveu o código ao concluir o passo a passo, adicione `ProcessURLAsync` ao aplicativo que inclui o método `GetURLContentsAsync`. O arquivo MainWindow.xaml.cs para este aplicativo é o primeiro exemplo da seção "Exemplos de código completos do passo a passo".

    O método `ProcessURLAsync` consolida as ações no corpo do loop `foreach` em `SumPageSizesAsync` no passo a passo original. O método baixa de maneira assíncrona o conteúdo de um site especificado como uma matriz de bytes e, em seguida, exibe e retorna o comprimento da matriz de bytes.

    ```csharp
    private async Task<int> ProcessURLAsync(string url)
    {
        var byteArray = await GetURLContentsAsync(url);
        DisplayResults(url, byteArray);
        return byteArray.Length;
    }
    ```

2. Comente ou exclua o loop `foreach` em `SumPageSizesAsync`, como mostrado no código a seguir.

    ```csharp
    //var total = 0;
    //foreach (var url in urlList)
    //{
    //    byte[] urlContents = await GetURLContentsAsync(url);

    //    // The previous line abbreviates the following two assignment statements.
    //    // GetURLContentsAsync returns a Task<T>. At completion, the task
    //    // produces a byte array.
    //    //Task<byte[]> getContentsTask = GetURLContentsAsync(url);
    //    //byte[] urlContents = await getContentsTask;

    //    DisplayResults(url, urlContents);

    //    // Update the total.
    //    total += urlContents.Length;
    //}
    ```

3. Crie uma coleção de tarefas. O código a seguir define uma [consulta](../linq/index.md) que, quando executada pelo método <xref:System.Linq.Enumerable.ToArray%2A>, cria uma coleção de tarefas que baixa o conteúdo de cada site. As tarefas são iniciadas quando a consulta é avaliada.

    Adicione o seguinte código ao método `SumPageSizesAsync` depois da declaração da `urlList`.

    ```csharp
    // Create a query.
    IEnumerable<Task<int>> downloadTasksQuery =
        from url in urlList select ProcessURLAsync(url);

    // Use ToArray to execute the query and start the download tasks.
    Task<int>[] downloadTasks = downloadTasksQuery.ToArray();
    ```

4. Aplique `Task.WhenAll` à coleção de tarefas `downloadTasks`. O `Task.WhenAll` retorna uma única tarefa que será terminada quando todas as tarefas na coleção de tarefas forem concluídas.

    No exemplo a seguir, a expressão `await` aguarda a conclusão da única tarefa que o `WhenAll` retorna. A expressão é avaliada para uma matriz de inteiros, em que cada inteiro é o comprimento de um site baixado. Adicione o seguinte código ao `SumPageSizesAsync`, logo após o código que você adicionou na etapa anterior.

    ```csharp
    // Await the completion of all the running tasks.
    int[] lengths = await Task.WhenAll(downloadTasks);

    //// The previous line is equivalent to the following two statements.
    //Task<int[]> whenAllTask = Task.WhenAll(downloadTasks);
    //int[] lengths = await whenAllTask;
    ```

5. Por fim, use o método <xref:System.Linq.Enumerable.Sum%2A> para calcular a soma dos comprimentos de todos os sites. Adicione a seguinte linha ao `SumPageSizesAsync`.

    ```csharp
    int total = lengths.Sum();
    ```

### <a name="to-add-taskwhenall-to-the-httpclientgetbytearrayasync-solution"></a>Para adicionar o Task.WhenAll à solução HttpClient.GetByteArrayAsync

1. Adicione a seguinte versão de `ProcessURLAsync` ao segundo aplicativo que é desenvolvido no [Passo a passo: acessando a Web usando async e await (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md).

    - Se você baixou o código de [Exemplos de código para desenvolvedores](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f), abra o projeto AsyncWalkthrough_HttpClient e, em seguida, adicione `ProcessURLAsync` ao arquivo MainWindow.xaml.cs.

    - Se você desenvolveu o código ao concluir o passo a passo, adicione `ProcessURLAsync` ao aplicativo que usa o método `HttpClient.GetByteArrayAsync`. O arquivo MainWindow.xaml.cs para este aplicativo é o segundo exemplo da seção "Exemplos de código completos do passo a passo".

    O método `ProcessURLAsync` consolida as ações no corpo do loop `foreach` em `SumPageSizesAsync` no passo a passo original. O método baixa de maneira assíncrona o conteúdo de um site especificado como uma matriz de bytes e, em seguida, exibe e retorna o comprimento da matriz de bytes.

    A única diferença do método `ProcessURLAsync` no procedimento anterior é o uso da instância <xref:System.Net.Http.HttpClient>, a `client`.

    ```csharp
    async Task<int> ProcessURLAsync(string url, HttpClient client)
    {
        byte[] byteArray = await client.GetByteArrayAsync(url);
        DisplayResults(url, byteArray);
        return byteArray.Length;
    }
    ```

2. Comente ou exclua o `For Each` ou o loop `foreach` em `SumPageSizesAsync`, como mostrado no código a seguir.

    ```csharp
    //var total = 0;
    //foreach (var url in urlList)
    //{
    //    // GetByteArrayAsync returns a Task<T>. At completion, the task
    //    // produces a byte array.
    //    byte[] urlContent = await client.GetByteArrayAsync(url);

    //    // The previous line abbreviates the following two assignment
    //    // statements.
    //    Task<byte[]> getContentTask = client.GetByteArrayAsync(url);
    //    byte[] urlContent = await getContentTask;

    //    DisplayResults(url, urlContent);

    //    // Update the total.
    //    total += urlContent.Length;
    //}
    ```

3. Defina uma [consulta](../linq/index.md) que, quando executada pelo método <xref:System.Linq.Enumerable.ToArray%2A>, cria uma coleção de tarefas que baixa o conteúdo de cada site. As tarefas são iniciadas quando a consulta é avaliada.

    Adicione o seguinte código ao método `SumPageSizesAsync` depois da declaração de `client` e `urlList`.

    ```csharp
    // Create a query.
    IEnumerable<Task<int>> downloadTasksQuery =
        from url in urlList select ProcessURLAsync(url, client);

    // Use ToArray to execute the query and start the download tasks.
    Task<int>[] downloadTasks = downloadTasksQuery.ToArray();
    ```

4. Em seguida, aplique `Task.WhenAll` à coleção de tarefas `downloadTasks`. O `Task.WhenAll` retorna uma única tarefa que será terminada quando todas as tarefas na coleção de tarefas forem concluídas.

    No exemplo a seguir, a expressão `await` aguarda a conclusão da única tarefa que o `WhenAll` retorna. Quando concluída, a expressão `await` é avaliada para uma matriz de inteiros, em que cada inteiro é o comprimento de um site baixado. Adicione o seguinte código ao `SumPageSizesAsync`, logo após o código que você adicionou na etapa anterior.

    ```csharp
    // Await the completion of all the running tasks.
    int[] lengths = await Task.WhenAll(downloadTasks);

    //// The previous line is equivalent to the following two statements.
    //Task<int[]> whenAllTask = Task.WhenAll(downloadTasks);
    //int[] lengths = await whenAllTask;
    ```

5. Por fim, use o método <xref:System.Linq.Enumerable.Sum%2A> para obter a soma dos comprimentos de todos os sites. Adicione a seguinte linha ao `SumPageSizesAsync`.

    ```csharp
    int total = lengths.Sum();
    ```

### <a name="to-test-the-taskwhenall-solutions"></a>Para testar as soluções Task.WhenAll

- Para qualquer uma das soluções, escolha a tecla F5 para executar o programa e, em seguida, escolha o botão **Iniciar**. A saída deve ser parecida com a saída das soluções assíncronas em [Passo a passo: acessando a Web usando async e await (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md). No entanto, observe que os sites aparecem em uma ordem diferente a cada vez.

## <a name="example"></a>Exemplo

O código a seguir mostra as extensões para o projeto que usa o método `GetURLContentsAsync` para baixar conteúdo da Web.

```csharp
// Add the following using directives, and add a reference for System.Net.Http.
using System.Net.Http;
using System.IO;
using System.Net;

namespace AsyncExampleWPF_WhenAll
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private async void startButton_Click(object sender, RoutedEventArgs e)
        {
            resultsTextBox.Clear();

            // Two-step async call.
            Task sumTask = SumPageSizesAsync();
            await sumTask;

            // One-step async call.
            //await SumPageSizesAsync();

            resultsTextBox.Text += "\r\nControl returned to startButton_Click.\r\n";
        }

        private async Task SumPageSizesAsync()
        {
            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            // Create a query.
            IEnumerable<Task<int>> downloadTasksQuery =
                from url in urlList select ProcessURLAsync(url);

            // Use ToArray to execute the query and start the download tasks.
            Task<int>[] downloadTasks = downloadTasksQuery.ToArray();

            // You can do other work here before awaiting.

            // Await the completion of all the running tasks.
            int[] lengths = await Task.WhenAll(downloadTasks);

            //// The previous line is equivalent to the following two statements.
            //Task<int[]> whenAllTask = Task.WhenAll(downloadTasks);
            //int[] lengths = await whenAllTask;

            int total = lengths.Sum();

            //var total = 0;
            //foreach (var url in urlList)
            //{
            //    byte[] urlContents = await GetURLContentsAsync(url);

            //    // The previous line abbreviates the following two assignment statements.
            //    // GetURLContentsAsync returns a Task<T>. At completion, the task
            //    // produces a byte array.
            //    //Task<byte[]> getContentsTask = GetURLContentsAsync(url);
            //    //byte[] urlContents = await getContentsTask;

            //    DisplayResults(url, urlContents);

            //    // Update the total.
            //    total += urlContents.Length;
            //}

            // Display the total count for all of the websites.
            resultsTextBox.Text +=
                $"\r\n\r\nTotal bytes returned:  {total}\r\n";
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
            {
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
                "https://msdn.microsoft.com/library/hh290136.aspx",
                "https://msdn.microsoft.com/library/ee256749.aspx",
                "https://msdn.microsoft.com/library/hh290138.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/dd470362.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            };
            return urls;
        }

        // The actions from the foreach loop are moved to this async method.
        private async Task<int> ProcessURLAsync(string url)
        {
            var byteArray = await GetURLContentsAsync(url);
            DisplayResults(url, byteArray);
            return byteArray.Length;
        }

        private async Task<byte[]> GetURLContentsAsync(string url)
        {
            // The downloaded resource ends up in the variable named content.
            var content = new MemoryStream();

            // Initialize an HttpWebRequest for the current URL.
            var webReq = (HttpWebRequest)WebRequest.Create(url);

            // Send the request to the Internet resource and wait for
            // the response.
            using (WebResponse response = await webReq.GetResponseAsync())
            {
                // Get the data stream that is associated with the specified url.
                using (Stream responseStream = response.GetResponseStream())
                {
                    await responseStream.CopyToAsync(content);
                }
            }

            // Return the result as a byte array.
            return content.ToArray();

        }

        private void DisplayResults(string url, byte[] content)
        {
            // Display the length of each website. The string format
            // is designed to be used with a monospaced font, such as
            // Lucida Console or Global Monospace.
            var bytes = content.Length;
            // Strip off the "https://".
            var displayURL = url.Replace("https://", "");
            resultsTextBox.Text += $"\n{displayURL,-58} {bytes,8}";
        }
    }
}
```

## <a name="example"></a>Exemplo

O código a seguir mostra as extensões para o projeto que usa o método `HttpClient.GetByteArrayAsync` para baixar conteúdo da Web.

```csharp
// Add the following using directives, and add a reference for System.Net.Http.
using System.Net.Http;
using System.IO;
using System.Net;

namespace AsyncExampleWPF_HttpClient_WhenAll
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private async void startButton_Click(object sender, RoutedEventArgs e)
        {
            resultsTextBox.Clear();

            // One-step async call.
            await SumPageSizesAsync();

            // Two-step async call.
            //Task sumTask = SumPageSizesAsync();
            //await sumTask;

            resultsTextBox.Text += "\r\nControl returned to startButton_Click.\r\n";
        }

        private async Task SumPageSizesAsync()
        {
            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            // Declare an HttpClient object and increase the buffer size. The
            // default buffer size is 65,536.
            HttpClient client = new HttpClient() { MaxResponseContentBufferSize = 1000000 };

            // Create a query.
            IEnumerable<Task<int>> downloadTasksQuery =
                from url in urlList select ProcessURLAsync(url, client);

            // Use ToArray to execute the query and start the download tasks.
            Task<int>[] downloadTasks = downloadTasksQuery.ToArray();

            // You can do other work here before awaiting.

            // Await the completion of all the running tasks.
            int[] lengths = await Task.WhenAll(downloadTasks);

            //// The previous line is equivalent to the following two statements.
            //Task<int[]> whenAllTask = Task.WhenAll(downloadTasks);
            //int[] lengths = await whenAllTask;

            int total = lengths.Sum();

            //var total = 0;
            //foreach (var url in urlList)
            //{
            //    // GetByteArrayAsync returns a Task<T>. At completion, the task
            //    // produces a byte array.
            //    byte[] urlContent = await client.GetByteArrayAsync(url);

            //    // The previous line abbreviates the following two assignment
            //    // statements.
            //    Task<byte[]> getContentTask = client.GetByteArrayAsync(url);
            //    byte[] urlContent = await getContentTask;

            //    DisplayResults(url, urlContent);

            //    // Update the total.
            //    total += urlContent.Length;
            //}

            // Display the total count for all of the web addresses.
            resultsTextBox.Text +=
                $"\r\n\r\nTotal bytes returned:  {total}\r\n";
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
            {
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/hh290136.aspx",
                "https://msdn.microsoft.com/library/ee256749.aspx",
                "https://msdn.microsoft.com/library/hh290138.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/dd470362.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            };
            return urls;
        }

        // The actions from the foreach loop are moved to this async method.
        async Task<int> ProcessURLAsync(string url, HttpClient client)
        {
            byte[] byteArray = await client.GetByteArrayAsync(url);
            DisplayResults(url, byteArray);
            return byteArray.Length;
        }

        private void DisplayResults(string url, byte[] content)
        {
            // Display the length of each web site. The string format
            // is designed to be used with a monospaced font, such as
            // Lucida Console or Global Monospace.
            var bytes = content.Length;
            // Strip off the "https://".
            var displayURL = url.Replace("https://", "");
            resultsTextBox.Text += $"\n{displayURL,-58} {bytes,8}";
        }
    }
}
```

## <a name="see-also"></a>Veja também

- <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>
- [Passo a passo: acessando a Web usando async e await (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)
