---
title: 'Instruções passo a passo: fazer várias solicitações da Web em paralelo e usando Async e Await'
ms.date: 07/20/2015
ms.assetid: a894b99b-7cfd-4a38-adfb-20d24f986730
ms.openlocfilehash: 40bab392af94ba941c2562e885a8d2e08aeea5b9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396578"
---
# <a name="how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await-visual-basic"></a>Como fazer várias solicitações da Web em paralelo usando Async e Await (Visual Basic)

Em um método assíncrono, as tarefas são iniciadas quando elas são criadas. O operador [Await](../../../language-reference/operators/await-operator.md) é aplicado à tarefa no ponto do método em que o processamento não pode continuar até que a tarefa seja concluída. Geralmente, uma tarefa é aguardada assim que ela é criada, como mostrado no exemplo a seguir.

```vb
Dim result = Await someWebAccessMethodAsync(url)
```

No entanto, você pode separar a criação da tarefa da espera da tarefa se o programa tiver outro trabalho a realizar, que não depende da conclusão da tarefa.

```vb
' The following line creates and starts the task.
Dim myTask = someWebAccessMethodAsync(url)

' While the task is running, you can do other work that does not depend
' on the results of the task.
' . . . . .

' The application of Await suspends the rest of this method until the task is
' complete.
Dim result = Await myTask
```

Entre o início de uma tarefa e a espera por ela, você pode iniciar outras tarefas. As tarefas adicionais são executadas implicitamente em paralelo, mas não são criados threads adicionais.

O programa a seguir inicia três downloads assíncronos na Web e, em seguida, os aguarda na ordem em que foram chamados. Observe ao executar o programa, que as tarefas nem sempre são concluídas na ordem em que foram criadas e aguardadas. Eles começam a ser executadas quando são criadas e uma ou mais tarefas podem terminar antes que o método alcance as expressões await.

> [!NOTE]
> Para concluir esse projeto, você precisa ter o Visual Studio 2012 ou posterior e o .NET Framework 4.5 ou posterior instalados no seu computador.

Para outro exemplo que inicia várias tarefas ao mesmo tempo, consulte [como: estender o passo a assíncrona usando Task. WhenAll (Visual Basic)](how-to-extend-the-async-walkthrough-by-using-task-whenall.md).

Você pode baixar o código deste exemplo de [Exemplos de código para desenvolvedores](https://code.msdn.microsoft.com/Async-Make-Multiple-Web-49adb82e).

### <a name="to-set-up-the-project"></a>Para configurar o projeto

1. Para configurar um aplicativo WPF, complete as etapas a seguir. Você pode encontrar instruções detalhadas para essas etapas no [passo a passos: acessando a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md).

    - Crie um aplicativo WPF que contenha uma caixa de texto e um botão. Dê o nome `startButton` para o botão e `resultsTextBox`, para a caixa de texto.

    - Adicione uma referência para <xref:System.Net.Http>.

    - No arquivo MainWindow. XAML. vb, adicione uma `Imports` instrução para `System.Net.Http` .

### <a name="to-add-the-code"></a>Para adicionar o código

1. Na janela de design, MainWindow. XAML, clique duas vezes no botão para criar o `startButton_Click` manipulador de eventos em MainWindow. XAML. vb.

2. Copie o código a seguir e cole-o no corpo de `startButton_Click` MainWindow. XAML. vb.

    ```vb
    resultsTextBox.Clear()
    Await CreateMultipleTasksAsync()
    resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."
    ```

     O código chama um método assíncrono, `CreateMultipleTasksAsync`, que controla o aplicativo.

3. Adicione os seguintes métodos de suporte ao projeto:

    - O `ProcessURLAsync` usa um método <xref:System.Net.Http.HttpClient> para baixar o conteúdo de um site como uma matriz de bytes. Em seguida, o método de suporte `ProcessURLAsync` exibe e retorna o comprimento da matriz.

    - O `DisplayResults` exibe o número de bytes na matriz de bytes para cada URL. Essa exibição mostra quando cada tarefa termina o download.

     Copie os seguintes métodos e cole-os após o `startButton_Click` manipulador de eventos em MainWindow. XAML. vb.

    ```vb
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
    ```

4. Finalmente, defina o método `CreateMultipleTasksAsync`, que executa as seguintes etapas.

    - O método declara um objeto `HttpClient`, que você precisa para acessar o método <xref:System.Net.Http.HttpClient.GetByteArrayAsync%2A> em `ProcessURLAsync`.

    - O método cria e inicia três tarefas do tipo <xref:System.Threading.Tasks.Task%601>, em que `TResult` é um inteiro. Conforme cada tarefa termina, `DisplayResults` exibe a URL da tarefa e o comprimento do conteúdo baixado. Como as tarefas estão em execução de maneira assíncrona, a ordem na qual os resultados são exibidos pode diferir da ordem na qual elas foram declaradas.

    - O método aguarda a conclusão de cada tarefa. Cada operador `Await` suspende a execução de `CreateMultipleTasksAsync` até que a tarefa aguardada seja concluída. O operador também recupera o valor retornado da chamada ao `ProcessURLAsync` de cada tarefa concluída.

    - Quando as tarefas forem concluídas e os valores inteiros forem recuperados, o método somará os comprimentos dos sites e exibirá o resultado.

     Copie o seguinte método e cole-o em sua solução.

    ```vb
    Private Async Function CreateMultipleTasksAsync() As Task

        ' Declare an HttpClient object, and increase the buffer size. The
        ' default buffer size is 65,536.
        Dim client As HttpClient =
            New HttpClient() With {.MaxResponseContentBufferSize = 1000000}

        ' Create and start the tasks. As each task finishes, DisplayResults
        ' displays its length.
        Dim download1 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com", client)
        Dim download2 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/hh156528(VS.110).aspx", client)
        Dim download3 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/67w7t67f.aspx", client)

        ' Await each task.
        Dim length1 As Integer = Await download1
        Dim length2 As Integer = Await download2
        Dim length3 As Integer = Await download3

        Dim total As Integer = length1 + length2 + length3

        ' Display the total count for all of the websites.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function
    ```

5. Escolha a tecla F5 para executar o programa e, em seguida, o botão **Iniciar**.

     Execute o programa várias vezes para ver que as três tarefas nem sempre são concluídas na mesma ordem e que a ordem em que elas são concluídas não é, necessariamente, a ordem em que elas foram criadas e aguardadas.

## <a name="example"></a>Exemplo

O código a seguir contem o exemplo completo.

```vb
' Add the following Imports statements, and add a reference for System.Net.Http.
Imports System.Net.Http

Class MainWindow

    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click
        resultsTextBox.Clear()
        Await CreateMultipleTasksAsync()
        resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."
    End Sub

    Private Async Function CreateMultipleTasksAsync() As Task

        ' Declare an HttpClient object, and increase the buffer size. The
        ' default buffer size is 65,536.
        Dim client As HttpClient =
            New HttpClient() With {.MaxResponseContentBufferSize = 1000000}

        ' Create and start the tasks. As each task finishes, DisplayResults
        ' displays its length.
        Dim download1 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com", client)
        Dim download2 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/hh156528(VS.110).aspx", client)
        Dim download3 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/67w7t67f.aspx", client)

        ' Await each task.
        Dim length1 As Integer = Await download1
        Dim length2 As Integer = Await download2
        Dim length3 As Integer = Await download3

        Dim total As Integer = length1 + length2 + length3

        ' Display the total count for all of the websites.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
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

- [Instruções passo a passo: acessando a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Programação assíncrona com Async e Await (Visual Basic)](index.md)
- [Como estender as instruções passo a passo assíncronas usando Task.WhenAll (Visual Basic)](how-to-extend-the-async-walkthrough-by-using-task-whenall.md)
