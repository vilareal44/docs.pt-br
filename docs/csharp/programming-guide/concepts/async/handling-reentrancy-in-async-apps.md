---
title: Tratando a reentrada em aplicativos assíncronos (C#)
description: Saiba como lidar com a reentrância em aplicativos assíncronos em C#, que se refere à reinserção de uma operação assíncrona antes que ela seja concluída com possíveis resultados inesperados.
ms.date: 07/20/2015
ms.assetid: 47c5075e-c448-45ce-9155-ed4e7e98c677
ms.openlocfilehash: fdd440d167b95268a5ae6de0e57a32f0fad66b7c
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925235"
---
# <a name="handling-reentrancy-in-async-apps-c"></a>Tratando a reentrada em aplicativos assíncronos (C#)

Ao incluir código assíncrono em seu aplicativo, você deve considerar e, possivelmente, evitar a reentrância, que se refere à reinserção de uma operação assíncrona antes de ela ser concluída. Se você não identificar e tratar as possibilidades de reentrância, isso poderá causar resultados inesperados.

**Neste tópico**

- [Reconhecendo a reentrância](#BKMK_RecognizingReentrancy)

- [Manipulando a reentrância](#BKMK_HandlingReentrancy)

  - [Desabilitar o botão Iniciar](#BKMK_DisableTheStartButton)

  - [Cancelar e reiniciar a operação](#BKMK_CancelAndRestart)

  - [Executar várias operações e colocar a saída na fila](#BKMK_RunMultipleOperations)

- [Examinar e executar o aplicativo de exemplo](#BKMD_SettingUpTheExample)

> [!NOTE]
> Para executar o exemplo, você deve ter o Visual Studio 2012 ou mais recente e .NET Framework 4,5 ou mais recente instalados no seu computador.

> [!NOTE]
> O protocolo TLS versão 1,2 agora é a versão mínima a ser usada no desenvolvimento de seu aplicativo. Se seu aplicativo for destinado a uma versão de .NET Framework anterior a 4,7, consulte o artigo a seguir para [práticas recomendadas de TLS (Transport Layer Security) com .NET Framework](../../../../framework/network-programming/tls.md).

## <a name="recognizing-reentrancy"></a><a name="BKMK_RecognizingReentrancy"></a>Reconhecendo a reentrância

No exemplo deste tópico, os usuários escolhem um botão **Iniciar** para iniciar um aplicativo assíncrono que baixa uma série de sites e calcula o número total de bytes baixados. Uma versão síncrona do exemplo responderia da mesma forma, independentemente de quantas vezes um usuário escolhesse o botão porque, após a primeira vez, o thread da interface do usuário ignora esses eventos até que o aplicativo conclua a execução. Em um aplicativo assíncrono, no entanto, o thread da interface do usuário continua a responder e você pode reinserir a operação assíncrona antes que ele seja concluído.

O exemplo a seguir mostra a saída esperada, caso o usuário escolha o botão **Iniciar** apenas uma vez. É exibida uma lista dos sites baixados com o tamanho, em bytes, de cada site. O número total de bytes é exibido no final.

```output
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

```output
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

Você pode bloquear o botão **Iniciar** enquanto uma operação estiver em execução, desabilitando o botão na parte superior do manipulador de eventos `StartButton_Click`. Você pode reativar o botão de dentro um bloco `finally` quando a operação for concluída para que os usuários possam executar o aplicativo novamente.

Para configurar esse cenário, faça as seguintes alterações no código básico que é fornecido em [Examinar e executar o aplicativo de exemplo](#BKMD_SettingUpTheExample). Você também pode baixar o aplicativo concluído de [exemplos assíncronos: reentrância em aplicativos da área de trabalho .net](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06). O nome do projeto é DisableStartButton.

```csharp
private async void StartButton_Click(object sender, RoutedEventArgs e)
{
    // This line is commented out to make the results clearer in the output.
    //ResultsTextBox.Text = "";

    // ***Disable the Start button until the downloads are complete.
    StartButton.IsEnabled = false;

    try
    {
        await AccessTheWebAsync();
    }
    catch (Exception)
    {
        ResultsTextBox.Text += "\r\nDownloads failed.";
    }
    // ***Enable the Start button in case you want to run the program again.
    finally
    {
        StartButton.IsEnabled = true;
    }
}
```

Como resultado das alterações, o botão não responde enquanto `AccessTheWebAsync` está baixando os sites, portanto, o processo não pode ser reinserido.

### <a name="cancel-and-restart-the-operation"></a><a name="BKMK_CancelAndRestart"></a> Cancelar e reiniciar a operação

Em vez de desabilitar o botão **Iniciar**, você pode manter o botão ativo, mas, se o usuário escolher esse botão novamente, cancele a operação que já está em execução e permita que a operação iniciada mais recentemente continue.

Para obter mais informações sobre o cancelamento, consulte [Ajuste fino de seu aplicativo assíncrono (C#)](./fine-tuning-your-async-application.md).

Para configurar esse cenário, faça as seguintes alterações no código básico que é fornecido em [Examinar e executar o aplicativo de exemplo](#BKMD_SettingUpTheExample). Você também pode baixar o aplicativo concluído de [exemplos assíncronos: reentrância em aplicativos da área de trabalho .net](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06). O nome do projeto é CancelAndRestart.

1. Declare uma <xref:System.Threading.CancellationTokenSource> variável, `cts` , que está no escopo de todos os métodos.

    ```csharp
    public partial class MainWindow : Window   // Or class MainPage
    {
        // *** Declare a System.Threading.CancellationTokenSource.
        CancellationTokenSource cts;
    ```

2. Em `StartButton_Click`, determine se uma operação já está em andamento. Se o valor de `cts` for nulo, nenhuma operação estará ativa. Se o valor não for nulo, a operação que já está em execução será cancelada.

    ```csharp
    // *** If a download process is already underway, cancel it.
    if (cts != null)
    {
        cts.Cancel();
    }
    ```

3. Defina `cts` para um valor diferente que represente o processo atual.

    ```csharp
    // *** Now set cts to a new value that you can use to cancel the current process
    // if the button is chosen again.
    CancellationTokenSource newCTS = new CancellationTokenSource();
    cts = newCTS;
    ```

4. Ao final de `StartButton_Click`, o processo atual estará concluído, então, defina o valor de `cts` novamente como nulo.

    ```csharp
    // *** When the process is complete, signal that another process can begin.
    if (cts == newCTS)
        cts = null;
    ```

O código a seguir mostra todas as alterações em `StartButton_Click`. As adições estão marcadas com asteriscos.

```csharp
private async void StartButton_Click(object sender, RoutedEventArgs e)
{
    // This line is commented out to make the results clearer in the output.
    //ResultsTextBox.Clear();

    // *** If a download process is already underway, cancel it.
    if (cts != null)
    {
        cts.Cancel();
    }

    // *** Now set cts to cancel the current process if the button is chosen again.
    CancellationTokenSource newCTS = new CancellationTokenSource();
    cts = newCTS;

    try
    {
        // ***Send cts.Token to carry the message if there is a cancellation request.
        await AccessTheWebAsync(cts.Token);

    }
    // *** Catch cancellations separately.
    catch (OperationCanceledException)
    {
        ResultsTextBox.Text += "\r\nDownloads canceled.\r\n";
    }
    catch (Exception)
    {
        ResultsTextBox.Text += "\r\nDownloads failed.\r\n";
    }
    // *** When the process is complete, signal that another process can proceed.
    if (cts == newCTS)
        cts = null;
}
```

Em `AccessTheWebAsync`, faça as seguintes alterações.

- Adicione um parâmetro para aceitar o token de cancelamento de `StartButton_Click`.

- Use o método <xref:System.Net.Http.HttpClient.GetAsync%2A> para baixar os sites porque `GetAsync` aceita um argumento <xref:System.Threading.CancellationToken>.

- Antes `DisplayResults` de chamar para exibir os resultados de cada site baixado, verifique se `ct` a operação atual não foi cancelada.

O código a seguir mostra essas alterações, que estão marcadas com asteriscos.

```csharp
// *** Provide a parameter for the CancellationToken from StartButton_Click.
async Task AccessTheWebAsync(CancellationToken ct)
{
    // Declare an HttpClient object.
    HttpClient client = new HttpClient();

    // Make a list of web addresses.
    List<string> urlList = SetUpURLList();

    var total = 0;
    var position = 0;

    foreach (var url in urlList)
    {
        // *** Use the HttpClient.GetAsync method because it accepts a
        // cancellation token.
        HttpResponseMessage response = await client.GetAsync(url, ct);

        // *** Retrieve the website contents from the HttpResponseMessage.
        byte[] urlContents = await response.Content.ReadAsByteArrayAsync();

        // *** Check for cancellations before displaying information about the
        // latest site.
        ct.ThrowIfCancellationRequested();

        DisplayResults(url, urlContents, ++position);

        // Update the total.
        total += urlContents.Length;
    }

    // Display the total count for all of the websites.
    ResultsTextBox.Text +=
        $"\r\n\r\nTOTAL bytes returned:  {total}\r\n";
}
```

Se você escolher o botão **Iniciar** várias vezes enquanto este aplicativo estiver em execução, ele deverá produzir resultados semelhantes à saída a seguir.

```output
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

Para configurar esse cenário, faça as seguintes alterações no código básico que é fornecido em [Examinar e executar o aplicativo de exemplo](#BKMD_SettingUpTheExample). Você também pode baixar o aplicativo concluído de [exemplos assíncronos: reentrância em aplicativos da área de trabalho .net](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06). O nome do projeto é QueueResults.

A saída a seguir mostra o resultado, caso o usuário escolha o botão **Iniciar** apenas uma vez. O rótulo de letra A indica que o resultado é da primeira vez que o botão **Iniciar** foi escolhido. Os números mostram a ordem das URLs na lista de destinos de download.

```output
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

```output
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

```csharp
public partial class MainWindow : Window  // Class MainPage in Windows Store app.
{
    // ***Declare the following variables where all methods can access them.
    private Task pendingWork = null;
    private char group = (char)('A' - 1);
```

A variável `Task`, `pendingWork`, supervisiona o processo de exibição e impede que qualquer grupo interrompa a operação de exibição do outro grupo. A variável de caractere, `group`, rotula a saída de grupos diferentes para verificar se os resultados aparecem na ordem esperada.

#### <a name="the-click-event-handler"></a>O manipulador de eventos Click

O manipulador de eventos `StartButton_Click`, incrementa a letra de grupo sempre que o usuário escolhe o botão **Iniciar**. Em seguida, o manipulador chama `AccessTheWebAsync` para executar a operação de download.

```csharp
private async void StartButton_Click(object sender, RoutedEventArgs e)
{
    // ***Verify that each group's results are displayed together, and that
    // the groups display in order, by marking each group with a letter.
    group = (char)(group + 1);
    ResultsTextBox.Text += $"\r\n\r\n#Starting group {group}.";

    try
    {
        // *** Pass the group value to AccessTheWebAsync.
        char finishedGroup = await AccessTheWebAsync(group);

        // The following line verifies a successful return from the download and
        // display procedures.
        ResultsTextBox.Text += $"\r\n\r\n#Group {finishedGroup} is complete.\r\n";
    }
    catch (Exception)
    {
        ResultsTextBox.Text += "\r\nDownloads failed.";
    }
}
```

#### <a name="the-accessthewebasync-method"></a>O método AccessTheWebAsync

Este exemplo divide o `AccessTheWebAsync` em dois métodos. O primeiro método, `AccessTheWebAsync`, inicia todas as tarefas de download de um grupo e configura `pendingWork` para controlar o processo de exibição. O método usa uma LINQ (consulta integrada à linguagem) e um <xref:System.Linq.Enumerable.ToArray%2A> para iniciar todas as tarefas de download ao mesmo tempo.

Em seguida, o `AccessTheWebAsync` chama `FinishOneGroupAsync` para aguardar a conclusão de cada download e exibir seu comprimento.

O `FinishOneGroupAsync` retorna uma tarefa que é atribuída a `pendingWork` em `AccessTheWebAsync`. Esse valor impede a interrupção por outra operação antes que a tarefa seja concluída.

```csharp
private async Task<char> AccessTheWebAsync(char grp)
{
    HttpClient client = new HttpClient();

    // Make a list of the web addresses to download.
    List<string> urlList = SetUpURLList();

    // ***Kick off the downloads. The application of ToArray activates all the download tasks.
    Task<byte[]>[] getContentTasks = urlList.Select(url => client.GetByteArrayAsync(url)).ToArray();

    // ***Call the method that awaits the downloads and displays the results.
    // Assign the Task that FinishOneGroupAsync returns to the gatekeeper task, pendingWork.
    pendingWork = FinishOneGroupAsync(urlList, getContentTasks, grp);

    ResultsTextBox.Text += $"\r\n#Task assigned for group {grp}. Download tasks are active.\r\n";

    // ***This task is complete when a group has finished downloading and displaying.
    await pendingWork;

    // You can do other work here or just return.
    return grp;
}
```

#### <a name="the-finishonegroupasync-method"></a>O método FinishOneGroupAsync

Esse método percorre as tarefas de download em um grupo, aguardando cada uma delas, exibindo o comprimento do site baixado e adicionando o comprimento ao total.

A primeira instrução em `FinishOneGroupAsync` usa `pendingWork` para verificar se o método de inserção não interfere com uma operação que já está no processo de exibição ou que já está aguardando. Se houver uma operação dessas em andamento, a operação de inserção deverá esperar por sua vez.

```csharp
private async Task FinishOneGroupAsync(List<string> urls, Task<byte[]>[] contentTasks, char grp)
{
    // ***Wait for the previous group to finish displaying results.
    if (pendingWork != null) await pendingWork;

    int total = 0;

    // contentTasks is the array of Tasks that was created in AccessTheWebAsync.
    for (int i = 0; i < contentTasks.Length; i++)
    {
        // Await the download of a particular URL, and then display the URL and
        // its length.
        byte[] content = await contentTasks[i];
        DisplayResults(urls[i], content, i, grp);
        total += content.Length;
    }

    // Display the total count for all of the websites.
    ResultsTextBox.Text +=
        $"\r\n\r\nTOTAL bytes returned:  {total}\r\n";
}
```

#### <a name="points-of-interest"></a>Pontos de Interesse

As linhas de informações que começam com um sinal de jogo da velha (#) na saída esclarecem o funcionamento deste exemplo.

A saída mostra os padrões a seguir.

- Um grupo pode ser iniciado enquanto um grupo anterior estiver exibindo a saída, mas a exibição da saída do grupo anterior não será interrompida.

    ```output
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

- O tarefa `pendingWork` é nula no início de `FinishOneGroupAsync` somente para o grupo A, que foi iniciado primeiro. O grupo A ainda não concluiu uma expressão Await quando ela chega `FinishOneGroupAsync` . Portanto, o controle não foi retornado ao `AccessTheWebAsync` e a primeira atribuição para `pendingWork` não ocorreu.

- As duas linhas a seguir sempre aparecem juntas na saída. O código nunca é interrompido entre o início de uma operação de um grupo em `StartButton_Click` e a atribuição de uma tarefa para o grupo para `pendingWork`.

    ```output
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

3. No painel **Modelos Instalados**, expanda **Visual C#** e, em seguida, expanda **Windows**.

4. Na lista de tipos de projeto, escolha **Aplicativo WPF**.

5. Nomeie o projeto `WebsiteDownloadWPF` , escolha .NET Framework versão 4,6 ou superior e, em seguida, clique no botão **OK** .

     O novo projeto aparece no **Gerenciador de Soluções**.

6. No Editor do Visual Studio Code, escolha a guia **MainWindow.xaml**.

     Se a guia não estiver visível, abra o menu de atalho para MainWindow. XAML em **Gerenciador de soluções**e escolha **Exibir código**.

7. Na exibição **XAML** de MainWindow.xaml, substitua o código pelo código a seguir.

    ```csharp
    <Window x:Class="WebsiteDownloadWPF.MainWindow"
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

9. No **Gerenciador de Soluções**, abra o menu de atalho de MainWindow.xaml.cs e, em seguida, escolha **Exibir Código**.

10. Em MainWindow.xaml.cs, substitua o código pelo código a seguir.

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

    // Add the following using directives, and add a reference for System.Net.Http.
    using System.Net.Http;
    using System.Threading;

    namespace WebsiteDownloadWPF
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                System.Net.ServicePointManager.SecurityProtocol |= System.Net.SecurityProtocolType.Tls12;
                InitializeComponent();
            }

            private async void StartButton_Click(object sender, RoutedEventArgs e)
            {
                // This line is commented out to make the results clearer in the output.
                //ResultsTextBox.Text = "";

                try
                {
                    await AccessTheWebAsync();
                }
                catch (Exception)
                {
                    ResultsTextBox.Text += "\r\nDownloads failed.";
                }
            }

            private async Task AccessTheWebAsync()
            {
                // Declare an HttpClient object.
                HttpClient client = new HttpClient();

                // Make a list of web addresses.
                List<string> urlList = SetUpURLList();

                var total = 0;
                var position = 0;

                foreach (var url in urlList)
                {
                    // GetByteArrayAsync returns a task. At completion, the task
                    // produces a byte array.
                    byte[] urlContents = await client.GetByteArrayAsync(url);

                    DisplayResults(url, urlContents, ++position);

                    // Update the total.
                    total += urlContents.Length;
                }

                // Display the total count for all of the websites.
                ResultsTextBox.Text +=
                    $"\r\n\r\nTOTAL bytes returned:  {total}\r\n";
            }

            private List<string> SetUpURLList()
            {
                List<string> urls = new List<string>
                {
                    "https://msdn.microsoft.com/library/hh191443.aspx",
                    "https://msdn.microsoft.com/library/aa578028.aspx",
                    "https://msdn.microsoft.com/library/jj155761.aspx",
                    "https://msdn.microsoft.com/library/hh290140.aspx",
                    "https://msdn.microsoft.com/library/hh524395.aspx",
                    "https://msdn.microsoft.com/library/ms404677.aspx",
                    "https://msdn.microsoft.com",
                    "https://msdn.microsoft.com/library/ff730837.aspx"
                };
                return urls;
            }

            private void DisplayResults(string url, byte[] content, int pos)
            {
                // Display the length of each website. The string format is designed
                // to be used with a monospaced font, such as Lucida Console or
                // Global Monospace.

                // Strip off the "https://".
                var displayURL = url.Replace("https://", "");
                // Display position in the URL list, the URL, and the number of bytes.
                ResultsTextBox.Text += $"\n{pos}. {displayURL,-58} {content.Length,8}";
            }
        }
    }
    ```

11. Escolha a tecla CTRL+F5 para executar o programa e, em seguida, escolha o botão **Iniciar** várias vezes.

12. Faça as alterações de [Desabilitar o botão Iniciar](#BKMK_DisableTheStartButton), [Cancelar e reiniciar a operação](#BKMK_CancelAndRestart) ou [Executar várias operações e colocar a saída em fila](#BKMK_RunMultipleOperations) para tratar a reentrância.

## <a name="see-also"></a>Veja também

- [Passo a passo: acessando a Web usando async e await (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Programação assíncrona com Async e Await (C#)](./index.md)
