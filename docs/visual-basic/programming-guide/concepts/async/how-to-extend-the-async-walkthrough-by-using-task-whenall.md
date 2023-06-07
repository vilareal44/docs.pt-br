---
title: Como estender as instruções passo a passo assíncronas usando Task.WhenAll
ms.date: 07/20/2015
ms.assetid: c06d386d-e996-4da9-bf3d-05a3b6c0a258
ms.openlocfilehash: fb323852c83b1edf51396a0b800c2d54a833d0c0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396617"
---
# <a name="how-to-extend-the-async-walkthrough-by-using-taskwhenall-visual-basic"></a>Como estender as instruções passo a passo assíncronas usando Task.WhenAll (Visual Basic)

Você pode melhorar o desempenho da solução assíncrona em [passo a passo: acessando a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md) usando o <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> método. Esse método aguarda de maneira assíncrona várias operações assíncronas, que são representadas como uma coleção de tarefas.

Você deve ter notado no passo a passo que os sites fazem o download em taxas diferentes. Às vezes, um dos sites está muito lento e isso atrasa todos os downloads restantes. Ao executar as soluções assíncronas que você compilou no passo a passo, você poderá finalizar o programa facilmente se não quiser esperar, mas uma opção melhor seria iniciar todos os downloads ao mesmo tempo e permitir que os downloads mais rápidos continuem, sem aguardar o que está atrasado.

Você aplica o método `Task.WhenAll` a uma coleção de tarefas. A aplicação de `WhenAll` retorna uma única tarefa que não será concluída até a conclusão de cada tarefa na coleção. As tarefas parecem ser executadas em paralelo, mas não são criados threads adicionais. As tarefas podem ser concluídas em qualquer ordem.

> [!IMPORTANT]
> Os procedimentos a seguir descrevem extensões para os aplicativos assíncronos que são desenvolvidos em [passo a passos: acessando a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md). Você pode desenvolver os aplicativos concluindo o passo a passo ou baixando o código em [Exemplos de código para desenvolvedores](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f).
>
> Para executar o exemplo, você deve ter o Visual Studio 2012 ou mais recente instalado no seu computador.

### <a name="to-add-taskwhenall-to-your-geturlcontentsasync-solution"></a>Para adicionar o Task.WhenAll à sua solução GetURLContentsAsync

1. Adicione o `ProcessURLAsync` método ao primeiro aplicativo que é desenvolvido em [passo a passos: acessando a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md).

    - Se você baixou o código de [exemplos de código do desenvolvedor](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f), abra o projeto AsyncWalkthrough e, em seguida, adicione `ProcessURLAsync` ao arquivo MainWindow. XAML. vb.

    - Se você desenvolveu o código ao concluir o passo a passo, adicione `ProcessURLAsync` ao aplicativo que inclui o método `GetURLContentsAsync`. O arquivo MainWindow. XAML. vb para esse aplicativo é o primeiro exemplo na seção "exemplos de código completos da explicação passo a passo".

     O método `ProcessURLAsync` consolida as ações no corpo do loop `For Each` em `SumPageSizesAsync` no passo a passo original. O método baixa de maneira assíncrona o conteúdo de um site especificado como uma matriz de bytes e, em seguida, exibe e retorna o comprimento da matriz de bytes.

    ```vb
    Private Async Function ProcessURLAsync(url As String) As Task(Of Integer)

        Dim byteArray = Await GetURLContentsAsync(url)
        DisplayResults(url, byteArray)
        Return byteArray.Length
    End Function
    ```

2. Comente ou exclua o loop `For Each` em `SumPageSizesAsync`, como mostrado no código a seguir.

    ```vb
    'Dim total = 0
    'For Each url In urlList

    '    Dim urlContents As Byte() = Await GetURLContentsAsync(url)

    '    ' The previous line abbreviates the following two assignment statements.

    '    ' GetURLContentsAsync returns a task. At completion, the task
    '    ' produces a byte array.
    '    'Dim getContentsTask As Task(Of Byte()) = GetURLContentsAsync(url)
    '    'Dim urlContents As Byte() = Await getContentsTask

    '    DisplayResults(url, urlContents)

    '    ' Update the total.
    '    total += urlContents.Length
    'Next
    ```

3. Crie uma coleção de tarefas. O código a seguir define uma [consulta](../linq/index.md) que, quando executada pelo método <xref:System.Linq.Enumerable.ToArray%2A>, cria uma coleção de tarefas que baixa o conteúdo de cada site. As tarefas são iniciadas quando a consulta é avaliada.

     Adicione o seguinte código ao método `SumPageSizesAsync` depois da declaração da `urlList`.

    ```vb
    ' Create a query.
    Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =
        From url In urlList Select ProcessURLAsync(url)

    ' Use ToArray to execute the query and start the download tasks.
    Dim downloadTasks As Task(Of Integer)() = downloadTasksQuery.ToArray()
    ```

4. Aplique `Task.WhenAll` à coleção de tarefas `downloadTasks`. O `Task.WhenAll` retorna uma única tarefa que será terminada quando todas as tarefas na coleção de tarefas forem concluídas.

     No exemplo a seguir, a expressão `Await` aguarda a conclusão da única tarefa que o `WhenAll` retorna. A expressão é avaliada para uma matriz de inteiros, em que cada inteiro é o comprimento de um site baixado. Adicione o seguinte código ao `SumPageSizesAsync`, logo após o código que você adicionou na etapa anterior.

    ```vb
    ' Await the completion of all the running tasks.
    Dim lengths As Integer() = Await Task.WhenAll(downloadTasks)

    '' The previous line is equivalent to the following two statements.
    'Dim whenAllTask As Task(Of Integer()) = Task.WhenAll(downloadTasks)
    'Dim lengths As Integer() = Await whenAllTask
    ```

5. Por fim, use o método <xref:System.Linq.Enumerable.Sum%2A> para calcular a soma dos comprimentos de todos os sites. Adicione a seguinte linha ao `SumPageSizesAsync`.

    ```vb
    Dim total = lengths.Sum()
    ```

### <a name="to-add-taskwhenall-to-the-httpclientgetbytearrayasync-solution"></a>Para adicionar o Task.WhenAll à solução HttpClient.GetByteArrayAsync

1. Adicione a seguinte versão do `ProcessURLAsync` ao segundo aplicativo que é desenvolvido em [passo a passos: acessando a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md).

    - Se você baixou o código de [exemplos de código do desenvolvedor](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f), abra o projeto AsyncWalkthrough_HttpClient e, em seguida, adicione `ProcessURLAsync` ao arquivo MainWindow. XAML. vb.

    - Se você desenvolveu o código ao concluir o passo a passo, adicione `ProcessURLAsync` ao aplicativo que usa o método `HttpClient.GetByteArrayAsync`. O arquivo MainWindow. XAML. vb para esse aplicativo é o segundo exemplo na seção "exemplos de código completos da explicação passo a passo".

     O método `ProcessURLAsync` consolida as ações no corpo do loop `For Each` em `SumPageSizesAsync` no passo a passo original. O método baixa de maneira assíncrona o conteúdo de um site especificado como uma matriz de bytes e, em seguida, exibe e retorna o comprimento da matriz de bytes.

     A única diferença do método `ProcessURLAsync` no procedimento anterior é o uso da instância <xref:System.Net.Http.HttpClient>, a `client`.

    ```vb
    Private Async Function ProcessURLAsync(url As String, client As HttpClient) As Task(Of Integer)

        Dim byteArray = Await client.GetByteArrayAsync(url)
        DisplayResults(url, byteArray)
        Return byteArray.Length
    End Function
    ```

2. Comente ou exclua o loop `For Each` em `SumPageSizesAsync`, como mostrado no código a seguir.

    ```vb
    'Dim total = 0
    'For Each url In urlList
    '    ' GetByteArrayAsync returns a task. At completion, the task
    '    ' produces a byte array.
    '    Dim urlContents As Byte() = Await client.GetByteArrayAsync(url)

    '    ' The following two lines can replace the previous assignment statement.
    '    'Dim getContentsTask As Task(Of Byte()) = client.GetByteArrayAsync(url)
    '    'Dim urlContents As Byte() = Await getContentsTask

    '    DisplayResults(url, urlContents)

    '    ' Update the total.
    '    total += urlContents.Length
    'Next
    ```

3. Defina uma [consulta](../linq/index.md) que, quando executada pelo método <xref:System.Linq.Enumerable.ToArray%2A>, cria uma coleção de tarefas que baixa o conteúdo de cada site. As tarefas são iniciadas quando a consulta é avaliada.

     Adicione o seguinte código ao método `SumPageSizesAsync` depois da declaração de `client` e `urlList`.

    ```vb
    ' Create a query.
    Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =
        From url In urlList Select ProcessURLAsync(url, client)

    ' Use ToArray to execute the query and start the download tasks.
    Dim downloadTasks As Task(Of Integer)() = downloadTasksQuery.ToArray()
    ```

4. Em seguida, aplique `Task.WhenAll` à coleção de tarefas `downloadTasks`. O `Task.WhenAll` retorna uma única tarefa que será terminada quando todas as tarefas na coleção de tarefas forem concluídas.

     No exemplo a seguir, a expressão `Await` aguarda a conclusão da única tarefa que o `WhenAll` retorna. Quando concluída, a expressão `Await` é avaliada para uma matriz de inteiros, em que cada inteiro é o comprimento de um site baixado. Adicione o seguinte código ao `SumPageSizesAsync`, logo após o código que você adicionou na etapa anterior.

    ```vb
    ' Await the completion of all the running tasks.
    Dim lengths As Integer() = Await Task.WhenAll(downloadTasks)

    '' The previous line is equivalent to the following two statements.
    'Dim whenAllTask As Task(Of Integer()) = Task.WhenAll(downloadTasks)
    'Dim lengths As Integer() = Await whenAllTask
    ```

5. Por fim, use o método <xref:System.Linq.Enumerable.Sum%2A> para obter a soma dos comprimentos de todos os sites. Adicione a seguinte linha ao `SumPageSizesAsync`.

    ```vb
    Dim total = lengths.Sum()
    ```

### <a name="to-test-the-taskwhenall-solutions"></a>Para testar as soluções Task.WhenAll

Para qualquer uma das soluções, escolha a tecla F5 para executar o programa e, em seguida, escolha o botão **Iniciar**. A saída deve se parecer com a saída das soluções assíncronas em [passo a passos: acessar a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md). No entanto, observe que os sites aparecem em uma ordem diferente a cada vez.

## <a name="example"></a>Exemplo

O código a seguir mostra as extensões para o projeto que usa o método `GetURLContentsAsync` para baixar conteúdo da Web.

```vb
' Add the following Imports statements, and add a reference for System.Net.Http.
Imports System.Net.Http
Imports System.Net
Imports System.IO

Class MainWindow

    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click

        resultsTextBox.Clear()

        ' One-step async call.
        Await SumPageSizesAsync()

        '' Two-step async call.
        'Dim sumTask As Task = SumPageSizesAsync()
        'Await sumTask

        resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."
    End Sub

    Private Async Function SumPageSizesAsync() As Task

        ' Make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        ' Create a query.
        Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =
            From url In urlList Select ProcessURLAsync(url)

        ' Use ToArray to execute the query and start the download tasks.
        Dim downloadTasks As Task(Of Integer)() = downloadTasksQuery.ToArray()

        ' You can do other work here before awaiting.

        ' Await the completion of all the running tasks.
        Dim lengths As Integer() = Await Task.WhenAll(downloadTasks)

        '' The previous line is equivalent to the following two statements.
        'Dim whenAllTask As Task(Of Integer()) = Task.WhenAll(downloadTasks)
        'Dim lengths As Integer() = Await whenAllTask

        Dim total = lengths.Sum()

        'Dim total = 0
        'For Each url In urlList

        '    Dim urlContents As Byte() = Await GetURLContentsAsync(url)

        '    ' The previous line abbreviates the following two assignment statements.

        '    ' GetURLContentsAsync returns a task. At completion, the task
        '    ' produces a byte array.
        '    'Dim getContentsTask As Task(Of Byte()) = GetURLContentsAsync(url)
        '    'Dim urlContents As Byte() = Await getContentsTask

        '    DisplayResults(url, urlContents)

        '    ' Update the total.
        '    total += urlContents.Length
        'NextNext

        ' Display the total count for all of the web addresses.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function

    Private Function SetUpURLList() As List(Of String)

        Dim urls = New List(Of String) From
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
            }
        Return urls
    End Function

    ' The actions from the foreach loop are moved to this async method.
    Private Async Function ProcessURLAsync(url As String) As Task(Of Integer)

        Dim byteArray = Await GetURLContentsAsync(url)
        DisplayResults(url, byteArray)
        Return byteArray.Length
    End Function

    Private Async Function GetURLContentsAsync(url As String) As Task(Of Byte())

        ' The downloaded resource ends up in the variable named content.
        Dim content = New MemoryStream()

        ' Initialize an HttpWebRequest for the current URL.
        Dim webReq = CType(WebRequest.Create(url), HttpWebRequest)

        ' Send the request to the Internet resource and wait for
        ' the response.
        Using response As WebResponse = Await webReq.GetResponseAsync()
            ' Get the data stream that is associated with the specified URL.
            Using responseStream As Stream = response.GetResponseStream()
                ' Read the bytes in responseStream and copy them to content.
                ' CopyToAsync returns a Task, not a Task<T>.
                Await responseStream.CopyToAsync(content)
            End Using
        End Using

        ' Return the result as a byte array.
        Return content.ToArray()
    End Function

    Private Sub DisplayResults(url As String, content As Byte())

        ' Display the length of each website. The string format
        ' is designed to be used with a monospaced font, such as
        ' Lucida Console or Global Monospace.
        Dim bytes = content.Length
        ' Strip off the "https://".
        Dim displayURL = url.Replace("https://", "")
        resultsTextBox.Text &= String.Format(vbCrLf & "{0,-58} {1,8}", displayURL, bytes)
    End Sub

End Class
```

## <a name="example"></a>Exemplo

O código a seguir mostra as extensões para o projeto que usa o método `HttpClient.GetByteArrayAsync` para baixar conteúdo da Web.

```vb
' Add the following Imports statements, and add a reference for System.Net.Http.
Imports System.Net.Http
Imports System.Net
Imports System.IO

Class MainWindow

    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click

        resultsTextBox.Clear()

        '' One-step async call.
        Await SumPageSizesAsync()

        '' Two-step async call.
        'Dim sumTask As Task = SumPageSizesAsync()
        'Await sumTask

        resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."
    End Sub

    Private Async Function SumPageSizesAsync() As Task

        ' Declare an HttpClient object and increase the buffer size. The
        ' default buffer size is 65,536.
        Dim client As HttpClient =
            New HttpClient() With {.MaxResponseContentBufferSize = 1000000}

        ' Make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        ' Create a query.
        Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =
            From url In urlList Select ProcessURLAsync(url, client)

        ' Use ToArray to execute the query and start the download tasks.
        Dim downloadTasks As Task(Of Integer)() = downloadTasksQuery.ToArray()

        ' You can do other work here before awaiting.

        ' Await the completion of all the running tasks.
        Dim lengths As Integer() = Await Task.WhenAll(downloadTasks)

        '' The previous line is equivalent to the following two statements.
        'Dim whenAllTask As Task(Of Integer()) = Task.WhenAll(downloadTasks)
        'Dim lengths As Integer() = Await whenAllTask

        Dim total = lengths.Sum()

        ''<snippet7>
        'Dim total = 0
        'For Each url In urlList
        '    ' GetByteArrayAsync returns a task. At completion, the task
        '    ' produces a byte array.
        '    '<snippet31>
        '    Dim urlContents As Byte() = Await client.GetByteArrayAsync(url)
        '    '</snippet31>

        '    ' The following two lines can replace the previous assignment statement.
        '    'Dim getContentsTask As Task(Of Byte()) = client.GetByteArrayAsync(url)
        '    'Dim urlContents As Byte() = Await getContentsTask

        '    DisplayResults(url, urlContents)

        '    ' Update the total.
        '    total += urlContents.Length
        'NextNext

        ' Display the total count for all of the web addresses.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function

    Private Function SetUpURLList() As List(Of String)

        Dim urls = New List(Of String) From
            {
                "https://www.msdn.com",
                "https://msdn.microsoft.com/library/hh290136.aspx",
                "https://msdn.microsoft.com/library/ee256749.aspx",
                "https://msdn.microsoft.com/library/hh290138.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/dd470362.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            }
        Return urls
    End Function

    Private Async Function ProcessURLAsync(url As String, client As HttpClient) As Task(Of Integer)

        Dim byteArray = Await client.GetByteArrayAsync(url)
        DisplayResults(url, byteArray)
        Return byteArray.Length
    End Function

    Private Sub DisplayResults(url As String, content As Byte())

        ' Display the length of each website. The string format
        ' is designed to be used with a monospaced font, such as
        ' Lucida Console or Global Monospace.
        Dim bytes = content.Length
        ' Strip off the "https://".
        Dim displayURL = url.Replace("https://", "")
        resultsTextBox.Text &= String.Format(vbCrLf & "{0,-58} {1,8}", displayURL, bytes)
    End Sub

End Class
```

## <a name="see-also"></a>Confira também

- <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType>
- [Instruções passo a passo: acessando a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md)
