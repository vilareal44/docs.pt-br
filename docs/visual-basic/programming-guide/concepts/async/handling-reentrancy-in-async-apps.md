---
title: Tratar a reentrada em aplicativos assíncronos
ms.date: 07/20/2015
ms.assetid: ef3dc73d-13fb-4c5f-a686-6b84148bbffe
ms.openlocfilehash: 2d1af14016f82b5de875d3638b132e14c7d2280d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396630"
---
# <a name="handling-reentrancy-in-async-apps-visual-basic"></a>Tratando a reentrada em aplicativos assíncronos (Visual Basic)

Ao incluir código assíncrono em seu aplicativo, você deve considerar e, possivelmente, evitar a reentrância, que se refere à reinserção de uma operação assíncrona antes de ela ser concluída. Se você não identificar e tratar as possibilidades de reentrância, isso poderá causar resultados inesperados.

> [!NOTE]
> Para executar o exemplo, você deve ter o Visual Studio 2012 ou mais recente e o .NET Framework 4.5 ou posterior instalados no seu computador.

> [!NOTE]
> O protocolo TLS versão 1,2 agora é a versão mínima a ser usada no desenvolvimento de seu aplicativo. Se seu aplicativo tiver como alvo uma versão .NET Framework anterior a 4,7, consulte o artigo a seguir para obter as [práticas recomendadas de TLS (Transport Layer Security) com o .NET Framework](../../../../framework/network-programming/tls.md).

## <a name="recognizing-reentrancy"></a><a name="BKMK_RecognizingReentrancy"></a>Reconhecendo a reentrância

No exemplo deste tópico, os usuários escolhem um botão **Iniciar** para iniciar um aplicativo assíncrono que baixa uma série de sites e calcula o número total de bytes baixados. Uma versão síncrona do exemplo responderia da mesma forma, independentemente de quantas vezes um usuário escolhesse o botão porque, após a primeira vez, o thread da interface do usuário ignora esses eventos até que o aplicativo conclua a execução. Em um aplicativo assíncrono, no entanto, o thread da interface do usuário continua a responder e você pode reinserir a operação assíncrona antes que ele seja concluído.

O exemplo a seguir mostra a saída esperada, caso o usuário escolha o botão **Iniciar** apenas uma vez. É exibida uma lista dos sites baixados com o tamanho, em bytes, de cada site. O número total de bytes é exibido no final.

```console
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

No entanto, se o usuário escolhe o botão mais de uma vez, o manipulador de eventos é invocado repetidamente e o processo de download é reinserido a cada vez. Como resultado, várias operações assíncronas estarão em execução ao mesmo tempo, a saída intercalará os resultados e o número total de bytes será confuso.

```console
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
6. msdn.microsoft.com/library/ms404677.aspx               197325
3. msdn.microsoft.com/library/jj155761.aspx                29019
7. msdn.microsoft.com                                            42972
4. msdn.microsoft.com/library/hh290140.aspx               117152
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591

5. msdn.microsoft.com/library/hh524395.aspx                68959
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
6. msdn.microsoft.com/library/ms404677.aspx               197325
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
7. msdn.microsoft.com                                            42972
5. msdn.microsoft.com/library/hh524395.aspx                68959
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591

6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

Você pode examinar o código que produz esta saída fazendo a rolagem até o final deste tópico. Você pode fazer experimentos com o código baixando a solução em seu computador local e, em seguida, executar o projeto WebsiteDownload ou usar o código no final deste tópico para criar seu próprio projeto. Para obter mais informações e instruções, consulte [Examinar e executar o aplicativo de exemplo](#BKMD_SettingUpTheExample).

## <a name="handling-reentrancy"></a><a name="BKMK_HandlingReentrancy"></a> Tratando a reentrância

É possível tratar a reentrância de várias maneiras, dependendo do que você deseja que seu aplicativo faça. Este tópico apresenta os exemplos a seguir:

- [Desabilitar o botão Iniciar](#BKMK_DisableTheStartButton)

  Desabilitar o botão **Iniciar** enquanto a operação estiver em execução para que o usuário não possa interrompê-la.

- [Cancelar e reiniciar a operação](#BKMK_CancelAndRestart)

  Cancelar qualquer operação que ainda estiver em execução quando o usuário escolher o botão **Iniciar** novamente e, em seguida, permitir que a operação solicitada mais recentemente continue.

- [Executar várias operações e colocar a saída na fila](#BKMK_RunMultipleOperations)

  Permitir que todas as operações solicitadas sejam executadas de forma assíncrona, mas coordenar a exibição da saída, de forma que os resultados de cada operação apareçam juntos e em ordem.

### <a name="disable-the-start-button"></a><a name="BKMK_DisableTheStartButton"></a>Desabilitar o botão iniciar

Você pode bloquear o botão **Iniciar** enquanto uma operação estiver em execução, desabilitando o botão na parte superior do manipulador de eventos `StartButton_Click`. Você pode reativar o botão de dentro um bloco `Finally` quando a operação for concluída para que os usuários possam executar o aplicativo novamente.

O código a seguir mostra essas alterações, que estão marcadas com asteriscos. Você pode adicionar as alterações ao código no final deste tópico ou você pode baixar o aplicativo concluído em [Exemplos assíncronos: reentrância em aplicativos de área de trabalho do .NET](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06). O nome do projeto é DisableStartButton.

```vb
Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)
    ' This line is commented out to make the results clearer in the output.
    'ResultsTextBox.Text = ""

    ' ***Disable the Start button until the downloads are complete.
    StartButton.IsEnabled = False

    Try
        Await AccessTheWebAsync()

    Catch ex As Exception
        ResultsTextBox.Text &= vbCrLf & "Downloads failed."
    ' ***Enable the Start button in case you want to run the program again.
    Finally
        StartButton.IsEnabled = True

    End Try
End Sub
```

Como resultado das alterações, o botão não responderá enquanto `AccessTheWebAsync` estiver baixando os sites, para que o processo não possa ser reinserido.

### <a name="cancel-and-restart-the-operation"></a><a name="BKMK_CancelAndRestart"></a> Cancelar e reiniciar a operação

Em vez de desabilitar o botão **Iniciar**, você pode manter o botão ativo, mas, se o usuário escolher esse botão novamente, cancele a operação que já está em execução e permita que a operação iniciada mais recentemente continue.

Para obter mais informações sobre cancelamento, consulte ajustar [seu aplicativo assíncrono (Visual Basic)](fine-tuning-your-async-application.md).

Para configurar esse cenário, faça as seguintes alterações no código básico que é fornecido em [Examinar e executar o aplicativo de exemplo](#BKMD_SettingUpTheExample). Você também pode baixar o aplicativo concluído de [exemplos assíncronos: reentrância em aplicativos da área de trabalho .net](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06). O nome do projeto é CancelAndRestart.

1. Declare uma variável <xref:System.Threading.CancellationTokenSource>, `cts`, que está no escopo para todos os métodos.

    ```vb
    Class MainWindow // Or Class MainPage

        ' *** Declare a System.Threading.CancellationTokenSource.
        Dim cts As CancellationTokenSource
    ```

2. Em `StartButton_Click`, determine se uma operação já está em andamento. Se o valor de `cts` for `Nothing` , nenhuma operação já estará ativa. Se o valor não for `Nothing` , a operação que já está em execução será cancelada.

    ```vb
    ' *** If a download process is already underway, cancel it.
    If cts IsNot Nothing Then
        cts.Cancel()
    End If
    ```

3. Defina `cts` para um valor diferente que represente o processo atual.

    ```vb
    ' *** Now set cts to cancel the current process if the button is chosen again.
    Dim newCTS As CancellationTokenSource = New CancellationTokenSource()
    cts = newCTS
    ```

4. No final do `StartButton_Click` , o processo atual é concluído, portanto, defina o valor de de `cts` volta para `Nothing` .

    ```vb
    ' *** When the process completes, signal that another process can proceed.
    If cts Is newCTS Then
        cts = Nothing
    End If
    ```

O código a seguir mostra todas as alterações em `StartButton_Click`. As adições estão marcadas com asteriscos.

```vb
Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)

    ' This line is commented out to make the results clearer.
    'ResultsTextBox.Text = ""

    ' *** If a download process is underway, cancel it.
    If cts IsNot Nothing Then
        cts.Cancel()
    End If

    ' *** Now set cts to cancel the current process if the button is chosen again.
    Dim newCTS As CancellationTokenSource = New CancellationTokenSource()
    cts = newCTS

    Try
        ' *** Send a token to carry the message if the operation is canceled.
        Await AccessTheWebAsync(cts.Token)

    Catch ex As OperationCanceledException
        ResultsTextBox.Text &= vbCrLf & "Download canceled." & vbCrLf

    Catch ex As Exception
        ResultsTextBox.Text &= vbCrLf & "Downloads failed."
    End Try

    ' *** When the process is complete, signal that another process can proceed.
    If cts Is newCTS Then
        cts = Nothing
    End If
End Sub
```

Em `AccessTheWebAsync`, faça as seguintes alterações.

- Adicione um parâmetro para aceitar o token de cancelamento de `StartButton_Click`.

- Use o método <xref:System.Net.Http.HttpClient.GetAsync%2A> para baixar os sites porque `GetAsync` aceita um argumento <xref:System.Threading.CancellationToken>.

- Antes de chamar `DisplayResults` para exibir os resultados para cada site baixado, verifique `ct` para ver se a operação atual não foi cancelada.

 O código a seguir mostra essas alterações, que estão marcadas com asteriscos.

```vb
' *** Provide a parameter for the CancellationToken from StartButton_Click.
Private Async Function AccessTheWebAsync(ct As CancellationToken) As Task

    ' Declare an HttpClient object.
    Dim client = New HttpClient()

    ' Make a list of web addresses.
    Dim urlList As List(Of String) = SetUpURLList()

    Dim total = 0
    Dim position = 0

    For Each url In urlList
        ' *** Use the HttpClient.GetAsync method because it accepts a
        ' cancellation token.
        Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

        ' *** Retrieve the website contents from the HttpResponseMessage.
        Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

        ' *** Check for cancellations before displaying information about the
        ' latest site.
        ct.ThrowIfCancellationRequested()

        position += 1
        DisplayResults(url, urlContents, position)

        ' Update the total.
        total += urlContents.Length
    Next

    ' Display the total count for all of the websites.
    ResultsTextBox.Text &=
        String.Format(vbCrLf & vbCrLf & "TOTAL bytes returned:  " & total & vbCrLf)
End Function
```

Se você escolher o botão **Iniciar** várias vezes enquanto esse aplicativo estiver em execução, ele deverá produzir resultados que se assemelham à seguinte saída:

```console
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               122505
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
Download canceled.

1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
Download canceled.

1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

Para eliminar as listas parciais, remova a marca de comentário da primeira linha de código em `StartButton_Click`, para limpar a caixa de texto sempre que o usuário reiniciar a operação.

### <a name="run-multiple-operations-and-queue-the-output"></a><a name="BKMK_RunMultipleOperations"></a>Executar várias operações e enfileirar a saída

Este terceiro exemplo é mais complicado pois o aplicativo iniciará outra operação assíncrona a cada vez que o usuário escolher o botão **Iniciar** e todas as operações serão executadas até a conclusão. Todas as operações solicitadas baixam sites da lista de maneira assíncrona, mas a saída das operações será apresentada sequencialmente. Ou seja, a atividade de download real é intercalada, conforme mostrado na saída em [Reconhecendo a reentrância](#BKMK_RecognizingReentrancy), mas a lista de resultados para cada grupo é apresentada separadamente.

As operações compartilham uma <xref:System.Threading.Tasks.Task> global, `pendingWork`, que serve como um gatekeeper para o processo de exibição.

Você pode executar este exemplo colando as alterações no código na [criação do aplicativo](#BKMK_BuildingTheApp)ou pode seguir as instruções em [baixando o aplicativo](#BKMK_DownloadingTheApp) para baixar o exemplo e, em seguida, executar o projeto QueueResults.

A saída a seguir mostra o resultado, caso o usuário escolha o botão **Iniciar** apenas uma vez. O rótulo de letra A indica que o resultado é da primeira vez que o botão **Iniciar** foi escolhido. Os números mostram a ordem das URLs na lista de destinos de download.

```console
#Starting group A.
#Task assigned for group A.

A-1. msdn.microsoft.com/library/hh191443.aspx                87389
A-2. msdn.microsoft.com/library/aa578028.aspx               209858
A-3. msdn.microsoft.com/library/jj155761.aspx                30870
A-4. msdn.microsoft.com/library/hh290140.aspx               119027
A-5. msdn.microsoft.com/library/hh524395.aspx                71260
A-6. msdn.microsoft.com/library/ms404677.aspx               199186
A-7. msdn.microsoft.com                                            53266
A-8. msdn.microsoft.com/library/ff730837.aspx               148020

TOTAL bytes returned:  918876

#Group A is complete.
```

Se o usuário escolher o botão **Iniciar** três vezes, o aplicativo produzirá uma saída semelhante à das linhas a seguir. As linhas de informações que começam com um sinal de jogo da velha (#) rastreiam o progresso do aplicativo.

```console
#Starting group A.
#Task assigned for group A.

A-1. msdn.microsoft.com/library/hh191443.aspx                87389
A-2. msdn.microsoft.com/library/aa578028.aspx               207089
A-3. msdn.microsoft.com/library/jj155761.aspx                30870
A-4. msdn.microsoft.com/library/hh290140.aspx               119027
A-5. msdn.microsoft.com/library/hh524395.aspx                71259
A-6. msdn.microsoft.com/library/ms404677.aspx               199185

#Starting group B.
#Task assigned for group B.

A-7. msdn.microsoft.com                                            53266

#Starting group C.
#Task assigned for group C.

A-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  916095

B-1. msdn.microsoft.com/library/hh191443.aspx                87389
B-2. msdn.microsoft.com/library/aa578028.aspx               207089
B-3. msdn.microsoft.com/library/jj155761.aspx                30870
B-4. msdn.microsoft.com/library/hh290140.aspx               119027
B-5. msdn.microsoft.com/library/hh524395.aspx                71260
B-6. msdn.microsoft.com/library/ms404677.aspx               199186

#Group A is complete.

B-7. msdn.microsoft.com                                            53266
B-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  916097

C-1. msdn.microsoft.com/library/hh191443.aspx                87389
C-2. msdn.microsoft.com/library/aa578028.aspx               207089

#Group B is complete.

C-3. msdn.microsoft.com/library/jj155761.aspx                30870
C-4. msdn.microsoft.com/library/hh290140.aspx               119027
C-5. msdn.microsoft.com/library/hh524395.aspx                72765
C-6. msdn.microsoft.com/library/ms404677.aspx               199186
C-7. msdn.microsoft.com                                            56190
C-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  920526

#Group C is complete.
```

Os grupos B e C iniciam antes da conclusão do grupo A, mas a saída para cada grupo é exibida separadamente. Toda a saída para o grupo A aparece em primeiro, seguida por toda a saída para o grupo B e, em seguida, toda a saída para o grupo C. O aplicativo sempre exibe os grupos em ordem e, para cada grupo, sempre exibe as informações sobre os sites individuais na ordem em que as URLs aparecem na lista de URLs.

No entanto, não é possível prever a ordem na qual os downloads realmente acontecem. Depois que vários grupos tiverem sido iniciados, as tarefas de download que eles geram estarão todos ativas. Você não pode presumir que A-1 será baixado antes de B-1 e não pode presumir que A-1 será baixado antes de A-2.

#### <a name="global-definitions"></a>Definições Globais

O código de exemplo contém as duas declarações globais a seguir, que estão visíveis em todos os métodos.

```vb
Class MainWindow    ' Class MainPage in Windows Store app.

    ' ***Declare the following variables where all methods can access them.
    Private pendingWork As Task = Nothing
    Private group As Char = ChrW(AscW("A") - 1)
```

A variável `Task`, `pendingWork`, supervisiona o processo de exibição e impede que qualquer grupo interrompa a operação de exibição do outro grupo. A variável de caractere, `group`, rotula a saída de grupos diferentes para verificar se os resultados aparecem na ordem esperada.

#### <a name="the-click-event-handler"></a>O manipulador de eventos Click

O manipulador de eventos `StartButton_Click`, incrementa a letra de grupo sempre que o usuário escolhe o botão **Iniciar**. Em seguida, o manipulador chama `AccessTheWebAsync` para executar a operação de download.

```vb
Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)
    ' ***Verify that each group's results are displayed together, and that
    ' the groups display in order, by marking each group with a letter.
    group = ChrW(AscW(group) + 1)
    ResultsTextBox.Text &= String.Format(vbCrLf & vbCrLf & "#Starting group {0}.", group)

    Try
        ' *** Pass the group value to AccessTheWebAsync.
        Dim finishedGroup As Char = Await AccessTheWebAsync(group)

        ' The following line verifies a successful return from the download and
        ' display procedures.
        ResultsTextBox.Text &= String.Format(vbCrLf & vbCrLf & "#Group {0} is complete." & vbCrLf, finishedGroup)

    Catch ex As Exception
        ResultsTextBox.Text &= vbCrLf & "Downloads failed."

    End Try
End Sub
```

#### <a name="the-accessthewebasync-method"></a>O método AccessTheWebAsync

Este exemplo divide o `AccessTheWebAsync` em dois métodos. O primeiro método, `AccessTheWebAsync`, inicia todas as tarefas de download de um grupo e configura `pendingWork` para controlar o processo de exibição. O método usa uma LINQ (consulta integrada à linguagem) e um <xref:System.Linq.Enumerable.ToArray%2A> para iniciar todas as tarefas de download ao mesmo tempo.

Em seguida, o `AccessTheWebAsync` chama `FinishOneGroupAsync` para aguardar a conclusão de cada download e exibir seu comprimento.

O `FinishOneGroupAsync` retorna uma tarefa que é atribuída a `pendingWork` em `AccessTheWebAsync`. Esse valor impede a interrupção por outra operação antes que a tarefa seja concluída.

```vb
Private Async Function AccessTheWebAsync(grp As Char) As Task(Of Char)

    Dim client = New HttpClient()

    ' Make a list of the web addresses to download.
    Dim urlList As List(Of String) = SetUpURLList()

    ' ***Kick off the downloads. The application of ToArray activates all the download tasks.
    Dim getContentTasks As Task(Of Byte())() =
        urlList.Select(Function(addr) client.GetByteArrayAsync(addr)).ToArray()

    ' ***Call the method that awaits the downloads and displays the results.
    ' Assign the Task that FinishOneGroupAsync returns to the gatekeeper task, pendingWork.
    pendingWork = FinishOneGroupAsync(urlList, getContentTasks, grp)

    ResultsTextBox.Text &=
        String.Format(vbCrLf & "#Task assigned for group {0}. Download tasks are active." & vbCrLf, grp)

    ' ***This task is complete when a group has finished downloading and displaying.
    Await pendingWork

    ' You can do other work here or just return.
    Return grp
End Function
```

#### <a name="the-finishonegroupasync-method"></a>O método FinishOneGroupAsync

Esse método percorre as tarefas de download em um grupo, aguardando cada uma delas, exibindo o comprimento do site baixado e adicionando o comprimento ao total.

A primeira instrução em `FinishOneGroupAsync` usa `pendingWork` para verificar se o método de inserção não interfere com uma operação que já está no processo de exibição ou que já está aguardando. Se houver uma operação dessas em andamento, a operação de inserção deverá esperar por sua vez.

```vb
Private Async Function FinishOneGroupAsync(urls As List(Of String), contentTasks As Task(Of Byte())(), grp As Char) As Task

    ' Wait for the previous group to finish displaying results.
    If pendingWork IsNot Nothing Then
        Await pendingWork
    End If

    Dim total = 0

    ' contentTasks is the array of Tasks that was created in AccessTheWebAsync.
    For i As Integer = 0 To contentTasks.Length - 1
        ' Await the download of a particular URL, and then display the URL and
        ' its length.
        Dim content As Byte() = Await contentTasks(i)
        DisplayResults(urls(i), content, i, grp)
        total += content.Length
    Next

    ' Display the total count for all of the websites.
    ResultsTextBox.Text &=
        String.Format(vbCrLf & vbCrLf & "TOTAL bytes returned:  " & total & vbCrLf)
End Function
```

Você pode executar esse exemplo, colando as alterações no código em [Compilando o aplicativo](#BKMK_BuildingTheApp) ou pode seguir as instruções em [Baixando o aplicativo](#BKMK_DownloadingTheApp) para baixar o exemplo e executar o projeto QueueResults.

#### <a name="points-of-interest"></a>Pontos de Interesse

As linhas de informações que começam com um sinal de jogo da velha (#) na saída esclarecem o funcionamento deste exemplo.

A saída mostra os padrões a seguir.

- Um grupo pode ser iniciado enquanto um grupo anterior estiver exibindo a saída, mas a exibição da saída do grupo anterior não será interrompida.

  ```console
  #Starting group A.
  #Task assigned for group A. Download tasks are active.

  A-1. msdn.microsoft.com/library/hh191443.aspx                87389
  A-2. msdn.microsoft.com/library/aa578028.aspx               207089
  A-3. msdn.microsoft.com/library/jj155761.aspx                30870
  A-4. msdn.microsoft.com/library/hh290140.aspx               119037
  A-5. msdn.microsoft.com/library/hh524395.aspx                71260

  #Starting group B.
  #Task assigned for group B. Download tasks are active.

  A-6. msdn.microsoft.com/library/ms404677.aspx               199186
  A-7. msdn.microsoft.com                                            53078
  A-8. msdn.microsoft.com/library/ff730837.aspx               148010

  TOTAL bytes returned:  915919

  B-1. msdn.microsoft.com/library/hh191443.aspx                87388
  B-2. msdn.microsoft.com/library/aa578028.aspx               207089
  B-3. msdn.microsoft.com/library/jj155761.aspx                30870

  #Group A is complete.

  B-4. msdn.microsoft.com/library/hh290140.aspx               119027
  B-5. msdn.microsoft.com/library/hh524395.aspx                71260
  B-6. msdn.microsoft.com/library/ms404677.aspx               199186
  B-7. msdn.microsoft.com                                            53078
  B-8. msdn.microsoft.com/library/ff730837.aspx               148010

  TOTAL bytes returned:  915908
  ```

- A `pendingWork` tarefa está `Nothing` no início de `FinishOneGroupAsync` apenas para o grupo A, que foi iniciado primeiro. O grupo A ainda não concluiu uma expressão await quando alcança o `FinishOneGroupAsync`. Portanto, o controle não foi retornado ao `AccessTheWebAsync` e a primeira atribuição para `pendingWork` não ocorreu.

- As duas linhas a seguir sempre aparecem juntas na saída. O código nunca é interrompido entre o início de uma operação de um grupo em `StartButton_Click` e a atribuição de uma tarefa para o grupo para `pendingWork`.

  ```console
  #Starting group B.
  #Task assigned for group B. Download tasks are active.
  ```

  Depois que um grupo insere o `StartButton_Click`, a operação não conclui uma expressão await até que a operação insira `FinishOneGroupAsync`. Portanto, nenhuma outra operação pode obter controle durante esse segmento de código.

## <a name="reviewing-and-running-the-example-app"></a><a name="BKMD_SettingUpTheExample"></a>Revisando e executando o aplicativo de exemplo

Para entender melhor o aplicativo de exemplo, você pode baixá-lo, compilá-lo ou examinar o código ao final deste tópico sem implementar o aplicativo.

> [!NOTE]
> Para executar o exemplo como um aplicativo da área de trabalho do WPF (Windows Presentation Foundation), você deve ter o Visual Studio 2012 ou mais recente e o .NET Framework 4.5 ou posterior instalados no seu computador.

### <a name="downloading-the-app"></a><a name="BKMK_DownloadingTheApp"></a> Baixar o aplicativo

1. Baixe o aplicativo compactado em [Exemplos assíncronos: reentrância em aplicativos de área de trabalho do .NET](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06).

2. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.

3. Na barra de menus, escolha **Arquivo**, **Abrir**, **Projeto/Solução**.

4. Navegue até a pasta que contém o código de exemplo descompactado e, em seguida, abra o arquivo de solução (.sln).

5. No **Gerenciador de Soluções**, abra o menu de atalho do projeto que você deseja executar e, em seguida, escolha **Definir como Projeto de Inicialização**.

6. Escolha as teclas CTRL+F5 para compilar e executar o projeto.

### <a name="building-the-app"></a><a name="BKMK_BuildingTheApp"></a> Compilando o aplicativo

A seção a seguir fornece o código para compilar o exemplo como um aplicativo do WPF.

##### <a name="to-build-a-wpf-app"></a>Para compilar um aplicativo do WPF

1. Inicie o Visual Studio.

2. Na barra de menus, escolha **Arquivo**, **Novo**, **Projeto**.

     A caixa de diálogo **Novo Projeto** será aberta.

3. No painel **modelos instalados** , expanda **Visual Basic**e, em seguida, expanda **Windows**.

4. Na lista de tipos de projeto, escolha **Aplicativo WPF**.

5. Nomeie o projeto `WebsiteDownloadWPF` , escolha .NET Framework versão 4,6 ou superior e, em seguida, clique no botão **OK** .

     O novo projeto aparece no **Gerenciador de Soluções**.

6. No Editor do Visual Studio Code, escolha a guia **MainWindow.xaml**.

     Se a guia não estiver visível, abra o menu de atalho para MainWindow.xaml no **Gerenciador de Soluções** e, em seguida, escolha **Exibir Código**.

7. Na exibição **XAML** de MainWindow.xaml, substitua o código pelo código a seguir.

    ```xaml
    <Window x:Class="MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:WebsiteDownloadWPF"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid Width="517" Height="360">
            <Button x:Name="StartButton" Content="Start" HorizontalAlignment="Left" Margin="-1,0,0,0" VerticalAlignment="Top" Click="StartButton_Click" Height="53" Background="#FFA89B9B" FontSize="36" Width="518"  />
            <TextBox x:Name="ResultsTextBox" HorizontalAlignment="Left" Margin="-1,53,0,-36" TextWrapping="Wrap" VerticalAlignment="Top" Height="343" FontSize="10" ScrollViewer.VerticalScrollBarVisibility="Visible" Width="518" FontFamily="Lucida Console" />
        </Grid>
    </Window>
    ```

     Uma janela simples, contendo uma caixa de texto e um botão, aparecerá no modo de exibição de **Design** de MainWindow.xaml.

8. Em **Gerenciador de soluções**, clique com o botão direito do mouse em **referências** e selecione **Adicionar referência**.

     Adicione uma referência para <xref:System.Net.Http> , se ela ainda não estiver selecionada.

9. No **Gerenciador de soluções**, abra o menu de atalho para MainWindow. XAML. vb e escolha **Exibir código**.

10. Em MainWindow. XAML. vb, substitua o código pelo código a seguir.

    ```vb
    ' Add the following Imports statements, and add a reference for System.Net.Http.
    Imports System.Net.Http
    Imports System.Threading

    Class MainWindow

        Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)
            System.Net.ServicePointManager.SecurityProtocol = System.Net.ServicePointManager.SecurityProtocol Or System.Net.SecurityProtocolType.Tls12

            ' This line is commented out to make the results clearer in the output.
            'ResultsTextBox.Text = ""

            Try
                Await AccessTheWebAsync()

            Catch ex As Exception
                ResultsTextBox.Text &= vbCrLf & "Downloads failed."

            End Try
        End Sub

        Private Async Function AccessTheWebAsync() As Task

            ' Declare an HttpClient object.
            Dim client = New HttpClient()

            ' Make a list of web addresses.
            Dim urlList As List(Of String) = SetUpURLList()

            Dim total = 0
            Dim position = 0

            For Each url In urlList
                ' GetByteArrayAsync returns a task. At completion, the task
                ' produces a byte array.
                Dim urlContents As Byte() = Await client.GetByteArrayAsync(url)

                position += 1
                DisplayResults(url, urlContents, position)

                ' Update the total.
                total += urlContents.Length
            Next

            ' Display the total count for all of the websites.
            ResultsTextBox.Text &=
                String.Format(vbCrLf & vbCrLf & "TOTAL bytes returned:  " & total & vbCrLf)
        End Function

        Private Function SetUpURLList() As List(Of String)
            Dim urls = New List(Of String) From
            {
                "https://msdn.microsoft.com/library/hh191443.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/jj155761.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/hh524395.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            }
            Return urls
        End Function

        Private Sub DisplayResults(url As String, content As Byte(), pos As Integer)
            ' Display the length of each website. The string format is designed
            ' to be used with a monospaced font, such as Lucida Console or
            ' Global Monospace.

            ' Strip off the "http:'".
            Dim displayURL = url.Replace("https://", "")
            ' Display position in the URL list, the URL, and the number of bytes.
            ResultsTextBox.Text &= String.Format(vbCrLf & "{0}. {1,-58} {2,8}", pos, displayURL, content.Length)
        End Sub
    End Class
    ```

11. Escolha a tecla CTRL+F5 para executar o programa e, em seguida, escolha o botão **Iniciar** várias vezes.

12. Faça as alterações de [Desabilitar o botão Iniciar](#BKMK_DisableTheStartButton), [Cancelar e reiniciar a operação](#BKMK_CancelAndRestart) ou [Executar várias operações e colocar a saída em fila](#BKMK_RunMultipleOperations) para tratar a reentrância.

## <a name="see-also"></a>Confira também

- [Instruções passo a passo: acessando a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Programação assíncrona com Async e Await (Visual Basic)](index.md)
