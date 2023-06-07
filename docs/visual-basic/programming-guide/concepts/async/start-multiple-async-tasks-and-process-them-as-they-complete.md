---
title: Iniciar várias tarefas assíncronas e processá-las na conclusão
ms.date: 07/20/2015
ms.assetid: 57ffb748-af40-4794-bedd-bdb7fea062de
ms.openlocfilehash: e227029928676e21d3ed14450140e92b386bf216
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400791"
---
# <a name="start-multiple-async-tasks-and-process-them-as-they-complete-visual-basic"></a>Iniciar várias tarefas assíncronas e processá-las na conclusão (Visual Basic)
Usando <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>, você pode iniciar várias tarefas ao mesmo tempo e processá-las individualmente conforme elas foram concluídas, em vez de processá-las na ordem em que foram iniciadas.  
  
 O exemplo a seguir usa uma consulta para criar uma coleção de tarefas. Cada tarefa baixa o conteúdo de um site especificado. Em cada iteração de um loop "while", uma chamada esperada para `WhenAny` retorna a tarefa na coleção de tarefas que concluir o download primeiro. Essa tarefa é removida da coleção e processada. O loop é repetido até que a coleção não contenha mais tarefas.  
  
> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio 2012 ou mais recente e o .NET Framework 4.5 ou posterior instalados no seu computador.  
  
## <a name="downloading-the-example"></a>Baixando o Exemplo  
 Você pode baixar o projeto completo do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea) e, em seguida, seguir estas etapas.  
  
1. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.  
  
2. Na barra de menus, escolha **Arquivo**, **Abrir**, **Projeto/Solução**.  
  
3. Na caixa de diálogo **Abrir Projeto**, abra a pasta em que está o código de exemplo que você descompactou e, em seguida, abra o arquivo de solução (.sln) de AsyncFineTuningVB.  
  
4. No **Gerenciador de Soluções**, abra o menu de atalho do projeto **ProcessTasksAsTheyFinish** e escolha **Definir como Projeto de Inicialização**.  
  
5. Escolha a tecla F5 para executar o projeto.  
  
     Escolha as teclas CTRL+F5 para executar o projeto sem depurá-lo.  
  
6. Execute o projeto várias vezes para verificar se os tamanhos baixados não aparecem sempre na mesma ordem.  
  
 Se não quiser baixar o projeto, você poderá examinar o arquivo MainWindow.xaml.vb no final deste tópico.  
  
## <a name="building-the-example"></a>Compilando o Exemplo  
 Este exemplo adiciona ao código que é desenvolvido em [Cancelar tarefas assíncronas restantes após uma conclusão (Visual Basic)](cancel-remaining-async-tasks-after-one-is-complete.md) e usa a mesma interface do usuário.  
  
 Para compilar o exemplo por conta própria, passo a passo, siga as instruções na seção "Baixando o exemplo", mas escolha **CancelAfterOneTask** como o **Projeto de Inicialização**. Adicione as alterações neste tópico ao método `AccessTheWebAsync` naquele projeto. As alterações estão marcadas com asteriscos.  
  
 O projeto **CancelAfterOneTask** já inclui uma consulta que, quando executada, cria uma coleção de tarefas. Cada chamada para `ProcessURLAsync` no código a seguir retorna um <xref:System.Threading.Tasks.Task%601> em que `TResult` é um inteiro.  
  
```vb  
Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =  
    From url In urlList Select ProcessURLAsync(url, client, ct)  
```  
  
 No arquivo MainWindow. XAML. vb do projeto, faça as seguintes alterações no `AccessTheWebAsync` método.  
  
- Execute a consulta aplicando <xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> em vez de <xref:System.Linq.Enumerable.ToArray%2A>.  
  
    ```vb  
    Dim downloadTasks As List(Of Task(Of Integer)) = downloadTasksQuery.ToList()  
    ```  
  
- Adiciona um loop "while" que executa as seguintes etapas para cada tarefa na coleção.  
  
    1. Espera uma chamada para `WhenAny` para identificar a primeira tarefa na coleção a concluir o download.  
  
        ```vb  
        Dim firstFinishedTask As Task(Of Integer) = Await Task.WhenAny(downloadTasks)  
        ```  
  
    2. Remove a tarefa da coleção.  
  
        ```vb  
        downloadTasks.Remove(firstFinishedTask)  
        ```  
  
    3. Espera `firstFinishedTask`, que é retornado por uma chamada para `ProcessURLAsync`. A variável `firstFinishedTask` é uma <xref:System.Threading.Tasks.Task%601> em que `TReturn` é um inteiro. A tarefa já foi concluída, mas você espera para recuperar o tamanho do site baixado, como mostra o exemplo a seguir.  
  
        ```vb  
        Dim length = Await firstFinishedTask  
        resultsTextBox.Text &= String.Format(vbCrLf & "Length of the downloaded website:  {0}" & vbCrLf, length)  
        ```  
  
 Você deve executar o projeto várias vezes para verificar se os tamanhos baixados não aparecem sempre na mesma ordem.  
  
> [!CAUTION]
> Você pode usar `WhenAny` em um loop, conforme descrito no exemplo, para resolver problemas que envolvem um número pequeno de tarefas. No entanto, outras abordagens são mais eficientes se você tiver um número grande de tarefas para processar. Para obter mais informações e exemplos, consulte [processando tarefas conforme elas são concluídas](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/).  
  
## <a name="complete-example"></a>Exemplo completo  
 O código a seguir é o texto completo do arquivo MainWindow.xaml.vb para o exemplo. Os asteriscos marcam os elementos que foram adicionados para esse exemplo.  
  
 Observe que você deve adicionar uma referência para <xref:System.Net.Http>.  
  
 Você pode baixar o projeto de [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea).  
  
```vb  
' Add an Imports directive and a reference for System.Net.Http.  
Imports System.Net.Http  
  
' Add the following Imports directive for System.Threading.  
Imports System.Threading  
  
Class MainWindow  
  
    ' Declare a System.Threading.CancellationTokenSource.  
    Dim cts As CancellationTokenSource  
  
    Private Async Sub startButton_Click(sender As Object, e As RoutedEventArgs)  
  
        ' Instantiate the CancellationTokenSource.  
        cts = New CancellationTokenSource()  
  
        resultsTextBox.Clear()  
  
        Try  
            Await AccessTheWebAsync(cts.Token)  
            resultsTextBox.Text &= vbCrLf & "Downloads complete."  
  
        Catch ex As OperationCanceledException  
            resultsTextBox.Text &= vbCrLf & "Downloads canceled." & vbCrLf  
  
        Catch ex As Exception  
            resultsTextBox.Text &= vbCrLf & "Downloads failed." & vbCrLf  
        End Try  
  
        ' Set the CancellationTokenSource to Nothing when the download is complete.  
        cts = Nothing  
    End Sub  
  
    ' You can still include a Cancel button if you want to.  
    Private Sub cancelButton_Click(sender As Object, e As RoutedEventArgs)  
  
        If cts IsNot Nothing Then  
            cts.Cancel()  
        End If  
    End Sub  
  
    ' Provide a parameter for the CancellationToken.  
    ' Change the return type to Task because the method has no return statement.  
    Async Function AccessTheWebAsync(ct As CancellationToken) As Task  
  
        Dim client As HttpClient = New HttpClient()  
  
        ' Call SetUpURLList to make a list of web addresses.  
        Dim urlList As List(Of String) = SetUpURLList()  
  
        ' ***Create a query that, when executed, returns a collection of tasks.  
        Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =  
            From url In urlList Select ProcessURLAsync(url, client, ct)  
  
        ' ***Use ToList to execute the query and start the download tasks.
        Dim downloadTasks As List(Of Task(Of Integer)) = downloadTasksQuery.ToList()  
  
        ' ***Add a loop to process the tasks one at a time until none remain.  
        While downloadTasks.Count > 0  
            ' ***Identify the first task that completes.  
            Dim firstFinishedTask As Task(Of Integer) = Await Task.WhenAny(downloadTasks)  
  
            ' ***Remove the selected task from the list so that you don't  
            ' process it more than once.  
            downloadTasks.Remove(firstFinishedTask)  
  
            ' ***Await the first completed task and display the results.  
            Dim length = Await firstFinishedTask  
            resultsTextBox.Text &= String.Format(vbCrLf & "Length of the downloaded website:  {0}" & vbCrLf, length)  
        End While  
  
    End Function  
  
    ' Bundle the processing steps for a website into one async method.  
    Async Function ProcessURLAsync(url As String, client As HttpClient, ct As CancellationToken) As Task(Of Integer)  
  
        ' GetAsync returns a Task(Of HttpResponseMessage).
        Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)  
  
        ' Retrieve the website contents from the HttpResponseMessage.  
        Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()  
  
        Return urlContents.Length  
    End Function  
  
    ' Add a method that creates a list of web addresses.  
    Private Function SetUpURLList() As List(Of String)  
  
        Dim urls = New List(Of String) From  
            {  
                "https://msdn.microsoft.com",  
                "https://msdn.microsoft.com/library/hh290138.aspx",  
                "https://msdn.microsoft.com/library/hh290140.aspx",  
                "https://msdn.microsoft.com/library/dd470362.aspx",  
                "https://msdn.microsoft.com/library/aa578028.aspx",  
                "https://msdn.microsoft.com/library/ms404677.aspx",  
                "https://msdn.microsoft.com/library/ff730837.aspx"  
            }  
        Return urls  
    End Function  
  
End Class  
  
' Sample output:  
  
' Length of the download:  226093  
' Length of the download:  412588  
' Length of the download:  175490  
' Length of the download:  204890  
' Length of the download:  158855  
' Length of the download:  145790  
' Length of the download:  44908  
' Downloads complete.  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Threading.Tasks.Task.WhenAny%2A>
- [Ajustando seu aplicativo assíncrono (Visual Basic)](fine-tuning-your-async-application.md)
- [Programação assíncrona com Async e Await (Visual Basic)](index.md)
- [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
