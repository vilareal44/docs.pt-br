---
title: Cancelar uma tarefa assíncrona ou uma lista de tarefas
ms.date: 07/20/2015
ms.assetid: a9ee1b71-5bec-4736-a1e9-448042dd7215
ms.openlocfilehash: 932bf46f1e3aee220d0412f1688e961faaef3459
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396695"
---
# <a name="cancel-an-async-task-or-a-list-of-tasks-visual-basic"></a>Cancelar uma tarefa assíncrona ou uma lista de tarefas (Visual Basic)

Você pode configurar um botão que pode ser usado para cancelar um aplicativo assíncrono se não desejar aguardar sua conclusão. Seguindo os exemplos neste tópico, você pode adicionar um botão de cancelamento a um aplicativo que baixa o conteúdo de um site ou uma lista de sites.

Os exemplos usam a interface do usuário que descreve o [ajuste do seu aplicativo assíncrono (Visual Basic)](fine-tuning-your-async-application.md) .

> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio 2012 ou uma versão mais recente e o .NET Framework 4.5 ou posterior instalados em seu computador.

## <a name="cancel-a-task"></a><a name="BKMK_CancelaTask"></a>Cancelar uma tarefa

O primeiro exemplo associa o botão **Cancelar** a uma única tarefa de download. Se você escolher o botão enquanto o aplicativo está baixando conteúdo, o download será cancelado.

### <a name="downloading-the-example"></a>Baixando o Exemplo

Você pode baixar o projeto completo do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea) e, em seguida, seguir estas etapas.

1. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.

2. Na barra de menus, escolha **Arquivo**, **Abrir**, **Projeto/Solução**.

3. Na caixa de diálogo **Abrir Projeto**, abra a pasta em que está o código de exemplo que você descompactou e, em seguida, abra o arquivo de solução (.sln) de AsyncFineTuningVB.

4. No **Gerenciador de Soluções**, abra o menu de atalho do projeto **CancelATask** e, em seguida, escolha **Definir como Projeto de Inicialização**.

5. Escolha a tecla F5 para executar o projeto.

     Escolha as teclas CTRL+F5 para executar o projeto sem depurá-lo.

 Se você não quiser baixar o projeto, poderá examinar os arquivos MainWindow. XAML. vb no final deste tópico.

### <a name="building-the-example"></a>Compilando o Exemplo

As alterações a seguir adicionam um botão **Cancelar** a um aplicativo que baixa um site. Se não desejar baixar ou compilar o exemplo, você poderá examinar o produto final na seção "Exemplos completos" no final deste tópico. Os asteriscos marcam as alterações no código.

Para compilar o exemplo você mesmo, passo a passo, siga as instruções na seção “Baixando o exemplo”, mas escolha **StarterCode** como o **Projeto de Inicialização** em vez de **CancelATask**.

Em seguida, adicione as seguintes alterações ao arquivo MainWindow. XAML. vb desse projeto.

1. Declare uma variável `CancellationTokenSource`, `cts`, que está no escopo para todos os métodos a acessarem.

    ```vb
    Class MainWindow

        ' ***Declare a System.Threading.CancellationTokenSource.
        Dim cts As CancellationTokenSource
    ```

2. Adicione o seguinte manipulador de eventos para o botão **Cancelar**. O manipulador de eventos usa o método <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> para notificar `cts` quando o usuário solicita o cancelamento.

    ```vb
    ' ***Add an event handler for the Cancel button.
    Private Sub cancelButton_Click(sender As Object, e As RoutedEventArgs)

        If cts IsNot Nothing Then
            cts.Cancel()
        End If
    End Sub
    ```

3. Faça as seguintes alterações no manipulador de eventos para o botão **Iniciar**, `startButton_Click`.

    - Crie a instância de `CancellationTokenSource`, `cts`.

      ```vb
      ' ***Instantiate the CancellationTokenSource.
      cts = New CancellationTokenSource()
      ```

    - Na chamada para `AccessTheWebAsync`, que baixa o conteúdo de um site especificado, envie a propriedade <xref:System.Threading.CancellationTokenSource.Token%2A?displayProperty=nameWithType> de `cts` como um argumento. A propriedade `Token` propaga a mensagem se o cancelamento é solicitado. Adicione um bloco catch que exibe uma mensagem se o usuário optar por cancelar a operação de download. O código a seguir mostra as alterações.

      ```vb
      Try
          ' ***Send a token to carry the message if cancellation is requested.
          Dim contentLength As Integer = Await AccessTheWebAsync(cts.Token)

          resultsTextBox.Text &=
              vbCrLf & $"Length of the downloaded string: {contentLength}." & vbCrLf

          ' *** If cancellation is requested, an OperationCanceledException results.
      Catch ex As OperationCanceledException
          resultsTextBox.Text &= vbCrLf & "Download canceled." & vbCrLf

      Catch ex As Exception
          resultsTextBox.Text &= vbCrLf & "Download failed." & vbCrLf
      End Try
      ```

4. No `AccessTheWebAsync`, use a sobrecarga <xref:System.Net.Http.HttpClient.GetAsync%28System.String%2CSystem.Threading.CancellationToken%29?displayProperty=nameWithType> do método `GetAsync` no tipo <xref:System.Net.Http.HttpClient> para baixar o conteúdo de um site. Passe `ct`, o parâmetro <xref:System.Threading.CancellationToken> de `AccessTheWebAsync`, como o segundo argumento. O token executa a mensagem se o usuário escolhe o botão **Cancelar**.

    O código a seguir mostra as alterações em `AccessTheWebAsync`.

    ```vb
    ' ***Provide a parameter for the CancellationToken.
    Async Function AccessTheWebAsync(ct As CancellationToken) As Task(Of Integer)

        Dim client As HttpClient = New HttpClient()

        resultsTextBox.Text &= vbCrLf & "Ready to download." & vbCrLf

        ' You might need to slow things down to have a chance to cancel.
        Await Task.Delay(250)

        ' GetAsync returns a Task(Of HttpResponseMessage).
        ' ***The ct argument carries the message if the Cancel button is chosen.
        Dim response As HttpResponseMessage = Await client.GetAsync("https://msdn.microsoft.com/library/dd470362.aspx", ct)

        ' Retrieve the website contents from the HttpResponseMessage.
        Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

        ' The result of the method is the length of the downloaded website.
        Return urlContents.Length
    End Function
    ```

5. Se você não cancelar o programa, ele produzirá a seguinte saída:

    ```console
    Ready to download.
    Length of the downloaded string: 158125.
    ```

    Se você escolher o botão **Cancelar** antes de o programa concluir o download do conteúdo, o programa produzirá a seguinte saída:

    ```console
    Ready to download.
    Download canceled.
    ```

## <a name="cancel-a-list-of-tasks"></a><a name="BKMK_CancelaListofTasks"></a>Cancelar uma lista de tarefas

Você pode estender o exemplo anterior para cancelar muitas tarefas associando a mesma instância `CancellationTokenSource` a cada tarefa. Se você escolher o botão **Cancelar**, cancela todas as tarefas que ainda não estão concluídas.

### <a name="downloading-the-example"></a>Baixando o Exemplo

Você pode baixar o projeto completo do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea) e, em seguida, seguir estas etapas.

1. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.

2. Na barra de menus, escolha **Arquivo**, **Abrir**, **Projeto/Solução**.

3. Na caixa de diálogo **Abrir Projeto**, abra a pasta em que está o código de exemplo que você descompactou e, em seguida, abra o arquivo de solução (.sln) de AsyncFineTuningVB.

4. No **Gerenciador de Soluções**, abra o menu de atalho do projeto **CancelAListOfTasks** e, em seguida, escolha **Definir como Projeto de Inicialização**.

5. Escolha a tecla F5 para executar o projeto.

     Escolha as teclas CTRL+F5 para executar o projeto sem depurá-lo.

 Se você não quiser baixar o projeto, poderá examinar os arquivos MainWindow. XAML. vb no final deste tópico.

### <a name="building-the-example"></a>Compilando o Exemplo

Para estender o exemplo você mesmo, passo a passo, siga as instruções na seção "Baixando o exemplo", mas escolha **CancelATask** como o **Projeto de Inicialização**. Adicione as seguintes alterações ao projeto. Os asteriscos marcam as alterações no programa.

1. Adicione um método para criar uma lista de endereços Web.

    ```vb
    ' ***Add a method that creates a list of web addresses.
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
    ```

2. Chame o método em `AccessTheWebAsync`.

    ```vb
    ' ***Call SetUpURLList to make a list of web addresses.
    Dim urlList As List(Of String) = SetUpURLList()
    ```

3. Adicione o seguinte loop em `AccessTheWebAsync` para processar cada endereço web na lista.

    ```vb
    ' ***Add a loop to process the list of web addresses.
    For Each url In urlList
        ' GetAsync returns a Task(Of HttpResponseMessage).
        ' Argument ct carries the message if the Cancel button is chosen.
        ' ***Note that the Cancel button can cancel all remaining downloads.
        Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

        ' Retrieve the website contents from the HttpResponseMessage.
        Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

        resultsTextBox.Text &=
            vbCrLf & $"Length of the downloaded string: {urlContents.Length}." & vbCrLf
    Next
    ```

4. Como `AccessTheWebAsync` exibe os comprimentos, o método não precisa retornar nada. Remova a instrução de retorno e altere o tipo de retorno do método para <xref:System.Threading.Tasks.Task> em vez de <xref:System.Threading.Tasks.Task%601>.

    ```vb
    Async Function AccessTheWebAsync(ct As CancellationToken) As Task
    ```

    Chame o método de `startButton_Click` usando uma instrução em vez de uma expressão.

    ```vb
    Await AccessTheWebAsync(cts.Token)
    ```

5. Se você não cancelar o programa, ele produzirá a seguinte saída:

    ```console
    Length of the downloaded string: 35939.

    Length of the downloaded string: 237682.

    Length of the downloaded string: 128607.

    Length of the downloaded string: 158124.

    Length of the downloaded string: 204890.

    Length of the downloaded string: 175488.

    Length of the downloaded string: 145790.

    Downloads complete.
    ```

    Se você escolher o botão **Cancelar** antes de os downloads serem concluídos, a saída conterá os tamanhos dos downloads que foram concluídos antes do cancelamento.

    ```console
    Length of the downloaded string: 35939.

    Length of the downloaded string: 237682.

    Length of the downloaded string: 128607.

    Downloads canceled.
    ```

## <a name="complete-examples"></a><a name="BKMK_CompleteExamples"></a>Exemplos completos

As seções a seguir contêm o código para cada um dos exemplos anteriores. Observe que você deve adicionar uma referência para <xref:System.Net.Http>.

Você pode baixar os projetos de [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea).

### <a name="cancel-a-task-example"></a>Exemplo de cancelar uma tarefa

O código a seguir é o arquivo MainWindow. XAML. vb completo para o exemplo que cancela uma única tarefa.

```vb
' Add an Imports directive and a reference for System.Net.Http.
Imports System.Net.Http

' Add the following Imports directive for System.Threading.
Imports System.Threading

Class MainWindow

    ' ***Declare a System.Threading.CancellationTokenSource.
    Dim cts As CancellationTokenSource

    Private Async Sub startButton_Click(sender As Object, e As RoutedEventArgs)
        ' ***Instantiate the CancellationTokenSource.
        cts = New CancellationTokenSource()

        resultsTextBox.Clear()

        Try
            ' ***Send a token to carry the message if cancellation is requested.
            Dim contentLength As Integer = Await AccessTheWebAsync(cts.Token)

            resultsTextBox.Text &=
                vbCrLf & $"Length of the downloaded string: {contentLength}." & vbCrLf

            ' *** If cancellation is requested, an OperationCanceledException results.
        Catch ex As OperationCanceledException
            resultsTextBox.Text &= vbCrLf & "Download canceled." & vbCrLf

        Catch ex As Exception
            resultsTextBox.Text &= vbCrLf & "Download failed." & vbCrLf
        End Try

        ' ***Set the CancellationTokenSource to Nothing when the download is complete.
        cts = Nothing
    End Sub

    ' ***Add an event handler for the Cancel button.
    Private Sub cancelButton_Click(sender As Object, e As RoutedEventArgs)

        If cts IsNot Nothing Then
            cts.Cancel()
        End If
    End Sub

    ' ***Provide a parameter for the CancellationToken.
    Async Function AccessTheWebAsync(ct As CancellationToken) As Task(Of Integer)

        Dim client As HttpClient = New HttpClient()

        resultsTextBox.Text &=
            vbCrLf & "Ready to download." & vbCrLf

        ' You might need to slow things down to have a chance to cancel.
        Await Task.Delay(250)

        ' GetAsync returns a Task(Of HttpResponseMessage).
        ' ***The ct argument carries the message if the Cancel button is chosen.
        Dim response As HttpResponseMessage = Await client.GetAsync("https://msdn.microsoft.com/library/dd470362.aspx", ct)

        ' Retrieve the website contents from the HttpResponseMessage.
        Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

        ' The result of the method is the length of the downloaded website.
        Return urlContents.Length
    End Function
End Class

' Output for a successful download:

' Ready to download.

' Length of the downloaded string: 158125.

' Or, if you cancel:

' Ready to download.

' Download canceled.
```

### <a name="cancel-a-list-of-tasks-example"></a>Exemplo de cancelar uma lista de tarefas

O código a seguir é o arquivo MainWindow. XAML. vb completo para o exemplo que cancela uma lista de tarefas.

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
            ' ***AccessTheWebAsync returns a Task, not a Task(Of Integer).
            Await AccessTheWebAsync(cts.Token)
            '  ***Small change in the display lines.
            resultsTextBox.Text &= vbCrLf & "Downloads complete."

        Catch ex As OperationCanceledException
            resultsTextBox.Text &= vbCrLf & "Downloads canceled." & vbCrLf

        Catch ex As Exception
            resultsTextBox.Text &= vbCrLf & "Downloads failed." & vbCrLf
        End Try

        ' Set the CancellationTokenSource to Nothing when the download is complete.
        cts = Nothing
    End Sub

    ' Add an event handler for the Cancel button.
    Private Sub cancelButton_Click(sender As Object, e As RoutedEventArgs)

        If cts IsNot Nothing Then
            cts.Cancel()
        End If
    End Sub

    ' Provide a parameter for the CancellationToken.
    ' ***Change the return type to Task because the method has no return statement.
    Async Function AccessTheWebAsync(ct As CancellationToken) As Task

        Dim client As HttpClient = New HttpClient()

        ' ***Call SetUpURLList to make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        ' ***Add a loop to process the list of web addresses.
        For Each url In urlList
            ' GetAsync returns a Task(Of HttpResponseMessage).
            ' Argument ct carries the message if the Cancel button is chosen.
            ' ***Note that the Cancel button can cancel all remaining downloads.
            Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

            ' Retrieve the website contents from the HttpResponseMessage.
            Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

            resultsTextBox.Text &=
                vbCrLf & $"Length of the downloaded string: {urlContents.Length}." & vbCrLf
        Next
    End Function

    ' ***Add a method that creates a list of web addresses.
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

' Output if you do not choose to cancel:

' Length of the downloaded string: 35939.

' Length of the downloaded string: 237682.

' Length of the downloaded string: 128607.

' Length of the downloaded string: 158124.

' Length of the downloaded string: 204890.

' Length of the downloaded string: 175488.

' Length of the downloaded string: 145790.

' Downloads complete.

'  Sample output if you choose to cancel:

' Length of the downloaded string: 35939.

' Length of the downloaded string: 237682.

' Length of the downloaded string: 128607.

' Downloads canceled.
```

## <a name="see-also"></a>Confira também

- <xref:System.Threading.CancellationTokenSource>
- <xref:System.Threading.CancellationToken>
- [Programação assíncrona com Async e Await (Visual Basic)](index.md)
- [Ajustando seu aplicativo assíncrono (Visual Basic)](fine-tuning-your-async-application.md)
- [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
