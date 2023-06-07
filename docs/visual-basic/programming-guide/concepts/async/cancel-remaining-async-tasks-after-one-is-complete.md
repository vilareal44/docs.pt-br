---
title: Cancelar tarefas assíncronas restantes após a conclusão de uma delas
ms.date: 07/20/2015
ms.assetid: c928b5a1-622f-4441-8baf-adca1dde197f
ms.openlocfilehash: be716e98263c865adad3c197236467b2f48d7740
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396669"
---
# <a name="cancel-remaining-async-tasks-after-one-is-complete-visual-basic"></a>Cancelar as demais tarefas assíncronas depois que uma delas estiver concluída (Visual Basic)

Usando o método <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType> juntamente com um <xref:System.Threading.CancellationToken>, você pode cancelar todas as tarefas restantes quando uma tarefa é concluída. O método `WhenAny` leva um argumento que é uma coleção de tarefas. O método inicia todas as tarefas e retorna uma única tarefa. A tarefa única será concluída quando qualquer tarefa na coleção for concluída.

Este exemplo demonstra como usar um token de cancelamento em conjunto com `WhenAny` para aguardar na primeira tarefa que for finalizada na coleção de tarefas e para cancelar as tarefas restantes. Cada tarefa baixa o conteúdo de um site. O exemplo exibe o comprimento do conteúdo do primeiro download que é concluído e cancela os outros downloads.

> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio 2012 ou uma versão mais recente e o .NET Framework 4.5 ou posterior instalados em seu computador.

## <a name="downloading-the-example"></a>Baixando o Exemplo

Você pode baixar o projeto completo do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea) e, em seguida, seguir estas etapas.

1. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.

2. Na barra de menus, escolha **Arquivo**, **Abrir**, **Projeto/Solução**.

3. Na caixa de diálogo **Abrir Projeto**, abra a pasta em que está o código de exemplo que você descompactou e, em seguida, abra o arquivo de solução (.sln) de AsyncFineTuningVB.

4. No **Gerenciador de Soluções**, abra o menu de atalho do projeto **CancelAfterOneTask** e, em seguida, escolha **Definir como Projeto de Inicialização**.

5. Escolha a tecla F5 para executar o projeto.

    Escolha as teclas CTRL+F5 para executar o projeto sem depurá-lo.

6. Execute o programa várias vezes para verificar que diferentes downloads são concluídos em primeiro.

Se não quiser baixar o projeto, você poderá examinar o arquivo MainWindow.xaml.vb no final deste tópico.

## <a name="building-the-example"></a>Compilando o Exemplo

O exemplo neste tópico adiciona ao projeto que é desenvolvido em [cancelar uma tarefa assíncrona ou uma lista de tarefas](cancel-an-async-task-or-a-list-of-tasks.md) para cancelar uma lista de tarefas. O exemplo usa a mesma interface do usuário, embora o botão **Cancelar** não seja explicitamente usado.

Para compilar o exemplo você mesmo, passo a passo, siga as instruções na seção "Baixando o exemplo", mas escolha **CancelAListOfTasks** como o **Projeto de Inicialização**. Adicione as alterações deste tópico ao projeto.

No arquivo MainWindow. XAML. vb do projeto **CancelAListOfTasks** , inicie a transição movendo as etapas de processamento para cada site do loop `AccessTheWebAsync` para o seguinte método assíncrono.

```vb
' ***Bundle the processing steps for a website into one async method.
Async Function ProcessURLAsync(url As String, client As HttpClient, ct As CancellationToken) As Task(Of Integer)

    ' GetAsync returns a Task(Of HttpResponseMessage).
    Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

    ' Retrieve the website contents from the HttpResponseMessage.
    Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

    Return urlContents.Length
End Function
```

Em `AccessTheWebAsync`, este exemplo usa uma consulta, o método <xref:System.Linq.Enumerable.ToArray%2A> e o método `WhenAny` para criar e iniciar uma matriz de tarefas. A aplicação de `WhenAny` à matriz retorna uma única tarefa que, quando colocada em espera, resulta na primeira tarefa a alcançar a conclusão na matriz de tarefas.

Faça as seguintes alterações em `AccessTheWebAsync`. Os asteriscos marcam as alterações no arquivo de código.

1. Comente ou exclua o loop.

2. Crie uma consulta que, quando executada, produz uma coleção de tarefas genéricas. Cada chamada para `ProcessURLAsync` retorna um <xref:System.Threading.Tasks.Task%601> em que `TResult` é um inteiro.

    ```vb
    ' ***Create a query that, when executed, returns a collection of tasks.
    Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =
        From url In urlList Select ProcessURLAsync(url, client, ct)
    ```

3. Chame `ToArray` para executar a consulta e iniciar as tarefas. A aplicação do método `WhenAny` na próxima etapa executaria a consulta e iniciaria as tarefas sem usar `ToArray`, mas outros métodos podem não fazê-lo. A prática mais segura é forçar a execução da consulta explicitamente.

    ```vb
    ' ***Use ToArray to execute the query and start the download tasks.
    Dim downloadTasks As Task(Of Integer)() = downloadTasksQuery.ToArray()
    ```

4. Chame `WhenAny` na coleção de tarefas. `WhenAny` retorna um `Task(Of Task(Of Integer))` ou `Task<Task<int>>`.  Ou seja, `WhenAny` retorna uma tarefa que resulta em uma única `Task(Of Integer)` ou `Task<int>` quando é esperada. Essa tarefa única é a primeira tarefa na coleção a ser concluída. A tarefa que foi concluída em primeiro é atribuída a `firstFinishedTask`. O tipo de `firstFinishedTask` é <xref:System.Threading.Tasks.Task%601>, em que `TResult` é um inteiro, porque esse é o tipo de retorno de `ProcessURLAsync`.

    ```vb
    ' ***Call WhenAny and then await the result. The task that finishes
    ' first is assigned to firstFinishedTask.
    Dim firstFinishedTask As Task(Of Integer) = Await Task.WhenAny(downloadTasks)
    ```

5. Neste exemplo, você está interessado apenas na tarefa que termina primeiro. Portanto, use <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> para cancelar as tarefas restantes.

    ```vb
    ' ***Cancel the rest of the downloads. You just want the first one.
    cts.Cancel()
    ```

6. Por fim, aguarde que `firstFinishedTask` recupere o comprimento do conteúdo baixado.

    ```vb
    Dim length = Await firstFinishedTask
    resultsTextBox.Text &= vbCrLf & $"Length of the downloaded website:  {length}" & vbCrLf
    ```

Execute o programa várias vezes para verificar que diferentes downloads são concluídos em primeiro.

## <a name="complete-example"></a>Exemplo completo

O código a seguir é o arquivo MainWindow. XAML. vb ou MainWindow.xaml.cs completo para o exemplo. Os asteriscos marcam os elementos que foram adicionados para esse exemplo.

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
            resultsTextBox.Text &= vbCrLf & "Download complete."

        Catch ex As OperationCanceledException
            resultsTextBox.Text &= vbCrLf & "Download canceled." & vbCrLf

        Catch ex As Exception
            resultsTextBox.Text &= vbCrLf & "Download failed." & vbCrLf
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

        '' Comment out or delete the loop.
        ''For Each url In urlList
        ''    ' GetAsync returns a Task(Of HttpResponseMessage).
        ''    ' Argument ct carries the message if the Cancel button is chosen.
        ''    ' Note that the Cancel button can cancel all remaining downloads.
        ''    Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

        ''    ' Retrieve the website contents from the HttpResponseMessage.
        ''    Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

        ''    resultsTextBox.Text &=
        ''        vbCrLf & $"Length of the downloaded string: {urlContents.Length}." & vbCrLf
        ''Next

        ' ***Create a query that, when executed, returns a collection of tasks.
        Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =
            From url In urlList Select ProcessURLAsync(url, client, ct)

        ' ***Use ToArray to execute the query and start the download tasks.
        Dim downloadTasks As Task(Of Integer)() = downloadTasksQuery.ToArray()

        ' ***Call WhenAny and then await the result. The task that finishes
        ' first is assigned to firstFinishedTask.
        Dim firstFinishedTask As Task(Of Integer) = Await Task.WhenAny(downloadTasks)

        ' ***Cancel the rest of the downloads. You just want the first one.
        cts.Cancel()

        ' ***Await the first completed task and display the results
        ' Run the program several times to demonstrate that different
        ' websites can finish first.
        Dim length = Await firstFinishedTask
        resultsTextBox.Text &= vbCrLf & $"Length of the downloaded website:  {length}" & vbCrLf
    End Function

    ' ***Bundle the processing steps for a website into one async method.
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

' Length of the downloaded website:  158856

' Download complete.
```

## <a name="see-also"></a>Confira também

- <xref:System.Threading.Tasks.Task.WhenAny%2A>
- [Ajustando seu aplicativo assíncrono (Visual Basic)](fine-tuning-your-async-application.md)
- [Programação assíncrona com Async e Await (Visual Basic)](index.md)
- [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
