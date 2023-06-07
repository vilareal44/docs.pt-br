---
title: Cancelar tarefas assíncronas após um período
ms.date: 07/20/2015
ms.assetid: a48045a3-6a99-42af-b824-af340f0b9a5d
ms.openlocfilehash: 048d4c19d459905ea579ede96c69230e718d55aa
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396682"
---
# <a name="cancel-async-tasks-after-a-period-of-time-visual-basic"></a>Cancelar tarefas assíncronas após um período (Visual Basic)

Você pode cancelar uma operação assíncrona após um período de tempo usando o método <xref:System.Threading.CancellationTokenSource.CancelAfter%2A?displayProperty=nameWithType> se você não deseja aguardar a conclusão da operação. Esse método agenda o cancelamento de quaisquer tarefas associadas que não são concluídas dentro do período designado pela expressão `CancelAfter`.

Este exemplo adiciona o código desenvolvido em [Cancelar uma tarefa assíncrona ou uma lista de tarefas (Visual Basic)](cancel-an-async-task-or-a-list-of-tasks.md) para baixar uma lista de sites e para exibir o tamanho dos conteúdos de cada um.

> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio 2012 ou posterior e o .NET Framework 4.5 ou posterior instalados no seu computador.

## <a name="downloading-the-example"></a>Baixando o Exemplo

Você pode baixar o projeto completo do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea) e, em seguida, seguir estas etapas.

1. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.

2. Na barra de menus, escolha **Arquivo**, **Abrir**, **Projeto/Solução**.

3. Na caixa de diálogo **Abrir Projeto**, abra a pasta em que está o código de exemplo que você descompactou e, em seguida, abra o arquivo de solução (.sln) de AsyncFineTuningVB.

4. No **Gerenciador de Soluções**, abra o menu de atalho do projeto **CancelAfterTime** e, em seguida, escolha **Definir como Projeto de Inicialização**.

5. Escolha a tecla F5 para executar o projeto.

     Escolha as teclas CTRL+F5 para executar o projeto sem depurá-lo.

6. Execute o programa várias vezes para verificar que a saída pode mostrar a saída para todos os sites, nenhum site ou alguns sites.

 Se não quiser baixar o projeto, você poderá examinar o arquivo MainWindow.xaml.vb no final deste tópico.

## <a name="building-the-example"></a>Compilando o Exemplo

O exemplo neste tópico é adicionado ao projeto desenvolvido em [Cancelar uma tarefa assíncrona ou uma lista de tarefas (Visual Basic)](cancel-an-async-task-or-a-list-of-tasks.md) para cancelar uma lista de tarefas. O exemplo usa a mesma interface do usuário, embora o botão **Cancelar** não seja explicitamente usado.

Para compilar o exemplo você mesmo, passo a passo, siga as instruções na seção "Baixando o exemplo", mas escolha **CancelAListOfTasks** como o **Projeto de Inicialização**. Adicione as alterações deste tópico ao projeto.

Para especificar um tempo máximo antes que as tarefas sejam marcadas como canceladas, adicione uma chamada para `CancelAfter` a `startButton_Click`, como o exemplo a seguir mostra. A adição é marcada com asteriscos.

```vb
Private Async Sub startButton_Click(sender As Object, e As RoutedEventArgs)

    ' Instantiate the CancellationTokenSource.
    cts = New CancellationTokenSource()

    resultsTextBox.Clear()

    Try
        ' ***Set up the CancellationTokenSource to cancel after 2.5 seconds. (You
        ' can adjust the time.)
        cts.CancelAfter(2500)

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
```

Execute o programa várias vezes para verificar que a saída pode mostrar a saída para todos os sites, nenhum site ou alguns sites. A saída a seguir é um exemplo:

```console
Length of the downloaded string: 35990.

Length of the downloaded string: 407399.

Length of the downloaded string: 226091.

Downloads canceled.
```

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
            ' ***Set up the CancellationTokenSource to cancel after 2.5 seconds. (You
            ' can adjust the time.)
            cts.CancelAfter(2500)

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

        ' Process each element in the list of web addresses.
        For Each url In urlList
            ' GetAsync returns a Task(Of HttpResponseMessage).
            ' Argument ct carries the message if the Cancel button is chosen.
            ' Note that the Cancel button can cancel all remaining downloads.
            Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

            ' Retrieve the website contents from the HttpResponseMessage.
            Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

            resultsTextBox.Text &=
                vbCrLf & $"Length of the downloaded string: {urlContents.Length}." & vbCrLf
        Next
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

' Length of the downloaded string: 35990.

' Length of the downloaded string: 407399.

' Length of the downloaded string: 226091.

' Downloads canceled.
```

## <a name="see-also"></a>Confira também

- [Programação assíncrona com Async e Await (Visual Basic)](index.md)
- [Instruções passo a passo: acessando a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Cancelar uma tarefa assíncrona ou uma lista de tarefas (Visual Basic)](cancel-an-async-task-or-a-list-of-tasks.md)
- [Ajustando seu aplicativo assíncrono (Visual Basic)](fine-tuning-your-async-application.md)
- [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
