---
title: Cancelar as demais tarefas assíncronas depois que uma delas estiver concluída (C#)
description: Use o método Task. WhenAny junto com um CancellationToken em C# para cancelar todas as tarefas restantes quando uma tarefa for concluída neste exemplo.
ms.date: 07/20/2015
ms.assetid: d3cebc74-c392-497b-b1e6-62a262eabe05
ms.openlocfilehash: 6de60c8faa93752961e3703a042885a71972cc4a
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925274"
---
# <a name="cancel-remaining-async-tasks-after-one-is-complete-c"></a>Cancelar as demais tarefas assíncronas depois que uma delas estiver concluída (C#)
Usando o método <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> juntamente com um <xref:System.Threading.CancellationToken>, você pode cancelar todas as tarefas restantes quando uma tarefa é concluída. O método `WhenAny` leva um argumento que é uma coleção de tarefas. O método inicia todas as tarefas e retorna uma única tarefa. A tarefa única será concluída quando qualquer tarefa na coleção for concluída.  
  
 Este exemplo demonstra como usar um token de cancelamento em conjunto com `WhenAny` para aguardar na primeira tarefa que for finalizada na coleção de tarefas e para cancelar as tarefas restantes. Cada tarefa baixa o conteúdo de um site. O exemplo exibe o comprimento do conteúdo do primeiro download que é concluído e cancela os outros downloads.  
  
> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio 2012 ou uma versão mais recente e o .NET Framework 4.5 ou posterior instalados em seu computador.  
  
## <a name="downloading-the-example"></a>Baixando o Exemplo  
 Você pode baixar o projeto completo do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea) e, em seguida, seguir estas etapas.  
  
1. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.  
  
2. Na barra de menus, escolha **Arquivo**, **Abrir**, **Projeto/Solução**.  
  
3. Na caixa de diálogo **Abrir Projeto**, abra a pasta em que está o código de exemplo que você descompactou e, em seguida, abra o arquivo de solução (.sln) de AsyncFineTuningCS.  
  
4. No **Gerenciador de Soluções**, abra o menu de atalho do projeto **CancelAfterOneTask** e, em seguida, escolha **Definir como Projeto de Inicialização**.  
  
5. Escolha a tecla F5 para executar o projeto.  
  
     Escolha as teclas CTRL+F5 para executar o projeto sem depurá-lo.  
  
6. Execute o programa várias vezes para verificar que diferentes downloads são concluídos em primeiro.  
  
 Se você não quiser baixar o projeto, você poderá examinar o arquivo MainWindow.xaml.cs no final deste tópico.  
  
## <a name="building-the-example"></a>Compilando o Exemplo  
 O exemplo neste tópico adiciona ao projeto que é desenvolvido em [Cancelar uma tarefa assíncrona ou uma lista de tarefas (C#)](./cancel-an-async-task-or-a-list-of-tasks.md) para cancelar uma lista de tarefas. O exemplo usa a mesma interface do usuário, embora o botão **Cancelar** não seja explicitamente usado.  
  
 Para compilar o exemplo você mesmo, passo a passo, siga as instruções na seção "Baixando o exemplo", mas escolha **CancelAListOfTasks** como o **Projeto de Inicialização**. Adicione as alterações deste tópico ao projeto.  
  
 No arquivo MainWindow.xaml.cs do projeto **CancelAListOfTasks**, inicie a transição, movendo as etapas de processamento de cada site do loop em `AccessTheWebAsync` para o seguinte método assíncrono.  
  
```csharp  
// ***Bundle the processing steps for a website into one async method.  
async Task<int> ProcessURLAsync(string url, HttpClient client, CancellationToken ct)  
{  
    // GetAsync returns a Task<HttpResponseMessage>.
    HttpResponseMessage response = await client.GetAsync(url, ct);  
  
    // Retrieve the website contents from the HttpResponseMessage.  
    byte[] urlContents = await response.Content.ReadAsByteArrayAsync();  
  
    return urlContents.Length;  
}  
```  
  
 Em `AccessTheWebAsync`, este exemplo usa uma consulta, o método <xref:System.Linq.Enumerable.ToArray%2A> e o método `WhenAny` para criar e iniciar uma matriz de tarefas. A aplicação de `WhenAny` à matriz retorna uma única tarefa que, quando colocada em espera, resulta na primeira tarefa a alcançar a conclusão na matriz de tarefas.  
  
 Faça as seguintes alterações em `AccessTheWebAsync`. Os asteriscos marcam as alterações no arquivo de código.  
  
1. Comente ou exclua o loop.  
  
2. Crie uma consulta que, quando executada, produz uma coleção de tarefas genéricas. Cada chamada para `ProcessURLAsync` retorna um <xref:System.Threading.Tasks.Task%601> em que `TResult` é um inteiro.  
  
    ```csharp  
    // ***Create a query that, when executed, returns a collection of tasks.  
    IEnumerable<Task<int>> downloadTasksQuery =  
        from url in urlList select ProcessURLAsync(url, client, ct);  
    ```  
  
3. Chame `ToArray` para executar a consulta e iniciar as tarefas. A aplicação do método `WhenAny` na próxima etapa executaria a consulta e iniciaria as tarefas sem usar `ToArray`, mas outros métodos podem não fazê-lo. A prática mais segura é forçar a execução da consulta explicitamente.  
  
    ```csharp  
    // ***Use ToArray to execute the query and start the download tasks.
    Task<int>[] downloadTasks = downloadTasksQuery.ToArray();  
    ```  
  
4. Chame `WhenAny` na coleção de tarefas. `WhenAny` retorna um `Task(Of Task(Of Integer))` ou `Task<Task<int>>`.  Ou seja, `WhenAny` retorna uma tarefa que resulta em uma única `Task(Of Integer)` ou `Task<int>` quando é esperada. Essa tarefa única é a primeira tarefa na coleção a ser concluída. A tarefa que foi concluída em primeiro é atribuída a `firstFinishedTask`. O tipo de `firstFinishedTask` é <xref:System.Threading.Tasks.Task%601>, em que `TResult` é um inteiro, porque esse é o tipo de retorno de `ProcessURLAsync`.  
  
    ```csharp  
    // ***Call WhenAny and then await the result. The task that finishes
    // first is assigned to firstFinishedTask.  
    Task<int> firstFinishedTask = await Task.WhenAny(downloadTasks);  
    ```  
  
5. Neste exemplo, você está interessado apenas na tarefa que termina primeiro. Portanto, use <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> para cancelar as tarefas restantes.  
  
    ```csharp  
    // ***Cancel the rest of the downloads. You just want the first one.  
    cts.Cancel();  
    ```  
  
6. Por fim, aguarde que `firstFinishedTask` recupere o comprimento do conteúdo baixado.  
  
    ```csharp  
    var length = await firstFinishedTask;  
    resultsTextBox.Text += $"\r\nLength of the downloaded website:  {length}\r\n";
    ```  
  
 Execute o programa várias vezes para verificar que diferentes downloads são concluídos em primeiro.  
  
## <a name="complete-example"></a>Exemplo completo  
 O código a seguir é o arquivo MainWindow.xaml.cs completo para o exemplo. Os asteriscos marcam os elementos que foram adicionados para esse exemplo.  
  
 Observe que você deve adicionar uma referência para <xref:System.Net.Http>.  
  
 Você pode baixar o projeto de [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea).  
  
```csharp  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Threading.Tasks;  
using System.Windows;  
using System.Windows.Controls;  
using System.Windows.Data;  
using System.Windows.Documents;  
using System.Windows.Input;  
using System.Windows.Media;  
using System.Windows.Media.Imaging;  
using System.Windows.Navigation;  
using System.Windows.Shapes;  
  
// Add a using directive and a reference for System.Net.Http.  
using System.Net.Http;  
  
// Add the following using directive.  
using System.Threading;  
  
namespace CancelAfterOneTask  
{  
    public partial class MainWindow : Window  
    {  
        // Declare a System.Threading.CancellationTokenSource.  
        CancellationTokenSource cts;  
  
        public MainWindow()  
        {  
            InitializeComponent();  
        }  
  
        private async void startButton_Click(object sender, RoutedEventArgs e)  
        {  
            // Instantiate the CancellationTokenSource.  
            cts = new CancellationTokenSource();  
  
            resultsTextBox.Clear();  
  
            try  
            {  
                await AccessTheWebAsync(cts.Token);  
                resultsTextBox.Text += "\r\nDownload complete.";  
            }  
            catch (OperationCanceledException)  
            {  
                resultsTextBox.Text += "\r\nDownload canceled.";  
            }  
            catch (Exception)  
            {  
                resultsTextBox.Text += "\r\nDownload failed.";  
            }  
  
            // Set the CancellationTokenSource to null when the download is complete.  
            cts = null;  
        }  
  
        // You can still include a Cancel button if you want to.  
        private void cancelButton_Click(object sender, RoutedEventArgs e)  
        {  
            if (cts != null)  
            {  
                cts.Cancel();  
            }  
        }  
  
        // Provide a parameter for the CancellationToken.  
        async Task AccessTheWebAsync(CancellationToken ct)  
        {  
            HttpClient client = new HttpClient();  
  
            // Call SetUpURLList to make a list of web addresses.  
            List<string> urlList = SetUpURLList();  
  
            // ***Comment out or delete the loop.  
            //foreach (var url in urlList)  
            //{  
            //    // GetAsync returns a Task<HttpResponseMessage>.
            //    // Argument ct carries the message if the Cancel button is chosen.
            //    // ***Note that the Cancel button can cancel all remaining downloads.  
            //    HttpResponseMessage response = await client.GetAsync(url, ct);  
  
            //    // Retrieve the website contents from the HttpResponseMessage.  
            //    byte[] urlContents = await response.Content.ReadAsByteArrayAsync();  
  
            //    resultsTextBox.Text +=  
            //        $"\r\nLength of the downloaded string: {urlContents.Length}.\r\n";
            //}  
  
            // ***Create a query that, when executed, returns a collection of tasks.  
            IEnumerable<Task<int>> downloadTasksQuery =  
                from url in urlList select ProcessURLAsync(url, client, ct);  
  
            // ***Use ToArray to execute the query and start the download tasks.
            Task<int>[] downloadTasks = downloadTasksQuery.ToArray();  
  
            // ***Call WhenAny and then await the result. The task that finishes
            // first is assigned to firstFinishedTask.  
            Task<int> firstFinishedTask = await Task.WhenAny(downloadTasks);  
  
            // ***Cancel the rest of the downloads. You just want the first one.  
            cts.Cancel();  
  
            // ***Await the first completed task and display the results.
            // Run the program several times to demonstrate that different  
            // websites can finish first.  
            var length = await firstFinishedTask;  
            resultsTextBox.Text += $"\r\nLength of the downloaded website:  {length}\r\n";
        }  
  
        // ***Bundle the processing steps for a website into one async method.  
        async Task<int> ProcessURLAsync(string url, HttpClient client, CancellationToken ct)  
        {  
            // GetAsync returns a Task<HttpResponseMessage>.
            HttpResponseMessage response = await client.GetAsync(url, ct);  
  
            // Retrieve the website contents from the HttpResponseMessage.  
            byte[] urlContents = await response.Content.ReadAsByteArrayAsync();  
  
            return urlContents.Length;  
        }  
  
        // Add a method that creates a list of web addresses.  
        private List<string> SetUpURLList()  
        {  
            List<string> urls = new List<string>
            {
                "https://msdn.microsoft.com",  
                "https://msdn.microsoft.com/library/hh290138.aspx",  
                "https://msdn.microsoft.com/library/hh290140.aspx",  
                "https://msdn.microsoft.com/library/dd470362.aspx",  
                "https://msdn.microsoft.com/library/aa578028.aspx",  
                "https://msdn.microsoft.com/library/ms404677.aspx",  
                "https://msdn.microsoft.com/library/ff730837.aspx"  
            };  
            return urls;  
        }  
    }  
    // Sample output:  
  
    // Length of the downloaded website:  158856  
  
    // Download complete.  
}  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.Threading.Tasks.Task.WhenAny%2A>
- [Ajuste fino de seu aplicativo assíncrono (C#)](./fine-tuning-your-async-application.md)
- [Programação assíncrona com Async e Await (C#)](./index.md)
- [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
