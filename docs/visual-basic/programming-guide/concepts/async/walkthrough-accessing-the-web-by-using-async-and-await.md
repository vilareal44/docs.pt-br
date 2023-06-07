---
title: 'Instruções passo a passo: acessando a Web e usando Async e Await'
ms.date: 07/20/2015
ms.assetid: 84fd047f-fab8-4d89-8ced-104fb7310a91
ms.openlocfilehash: 41ededd4d4335b78b8d7a33e8fe387c7d632cbee
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400739"
---
# <a name="walkthrough-accessing-the-web-by-using-async-and-await-visual-basic"></a>Instruções passo a passo: acessando a Web usando Async e Await (Visual Basic)

É possível escrever programas assíncronos de forma mais fácil e intuitiva usando funcionalidades async/await. Você pode escrever código assíncrono que se parece com código síncrono e deixar que o compilador trate das complicadas continuações e funções de retorno de chamada que um código assíncrono normalmente envolve.

Para obter mais informações sobre o recurso assíncrono, consulte [programação assíncrona com Async e Await (Visual Basic)](index.md).

Este passo a passo começa com um aplicativo WPF (Windows Presentation Foundation) síncrono que soma o número de bytes em uma lista de sites. Em seguida, converte o aplicativo em uma solução assíncrona usando os novos recursos.

Se não quiser compilar os aplicativos, você poderá baixar "Exemplo de assincronia: acessando o passo a passo da Web (C# e Visual Basic)" nas [Amostras de código do desenvolvedor](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f).

Neste passo a passo, você realizará as seguintes tarefas:

> [!div class="checklist"]
>
> - [Criar um aplicativo WPF](#create-a-wpf-application)
> - [Projetar um MainWindow WPF simples](#design-a-simple-wpf-mainwindow)
> - [Adicionar uma referência](#add-a-reference)
> - [Adicionar instruções de importações necessárias](#add-necessary-imports-statements)
> - [Criar um aplicativo síncrono](#create-a-synchronous-application)
> - [Testar a solução síncrona](#test-the-synchronous-solution)
> - [Converter GetURLContents em um método assíncrono](#convert-geturlcontents-to-an-asynchronous-method)
> - [Converter SumPageSizes em um método assíncrono](#convert-sumpagesizes-to-an-asynchronous-method)
> - [Converter startButton_Click em um método assíncrono](#convert-startbutton_click-to-an-asynchronous-method)
> - [Testar a solução assíncrona](#test-the-asynchronous-solution)
> - [Substitua o método GetURLContentsAsync por um método .NET Framework](#replace-the-geturlcontentsasync-method-with-a-net-framework-method)

Consulte a seção de [exemplo](#example) para obter o exemplo assíncrono completo.

## <a name="prerequisites"></a>Pré-requisitos

O Visual Studio 2012 ou posterior deve estar instalado em seu computador. Para obter mais informações, consulte a página de [downloads](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) do Visual Studio.

## <a name="create-a-wpf-application"></a>Criar um aplicativo WPF

1. Inicie o Visual Studio.

2. Na barra de menus, escolha **Arquivo**, **Novo**, **Projeto**.

    A caixa de diálogo **Novo Projeto** será aberta.

3. No painel **modelos instalados** , escolha Visual Basic e, em seguida, escolha **aplicativo WPF** na lista de tipos de projeto.

4. Na caixa de texto **Nome**, insira `AsyncExampleWPF` e, em seguida, escolha o botão **OK**.

    O novo projeto aparece no **Gerenciador de Soluções**.

## <a name="design-a-simple-wpf-mainwindow"></a>Projetar um MainWindow WPF simples

1. No Editor do Visual Studio Code, escolha a guia **MainWindow.xaml**.

2. Se a janela **Caixa de Ferramentas** não estiver visível, abra o menu **Exibir** e, em seguida, escolha **Caixa de Ferramentas**.

3. Adicione um controle **Botão** e um controle **Caixa de Texto** à janela **MainWindow**.

4. Realce o controle **Caixa de Texto** e, na janela **Propriedades**, defina os seguintes valores:

    - Defina a propriedade **Nome** como `resultsTextBox`.

    - Defina a propriedade **Altura** como 250.

    - Defina a propriedade **Largura** como 500.

    - Na guia **Texto**, especifique uma fonte com espaçamento uniforme, como Lucida Console ou Global Monospace.

5. Realce o controle **Botão** e, na janela **Propriedades**, defina os seguintes valores:

    - Defina a propriedade **Nome** como `startButton`.

    - Altere o valor da propriedade **Conteúdo** de **Botão** para **Iniciar**.

6. Posicione a caixa de texto e o botão de modo que ambos sejam exibidos na janela **MainWindow**.

    Para obter mais informações sobre o Designer XAML do WPF, consulte [Criando uma interface do usuário usando o Designer XAML](/visualstudio/xaml-tools/creating-a-ui-by-using-xaml-designer-in-visual-studio).

## <a name="add-a-reference"></a>Adicionar uma referência

1. No **Gerenciador de Soluções**, realce o nome do projeto.

2. Na barra de menus, escolha **Projeto**, **Adicionar Referência**.

    A caixa de diálogo **Gerenciador de Referências** é exibida.

3. Na parte superior da caixa de diálogo, verifique se seu projeto é voltado para o .NET Framework 4.5 ou posterior.

4. Na área **Assemblies**, escolha **Framework** se ele ainda não estiver escolhido.

5. Na lista de nomes, marque a caixa de seleção **System.Net.Http**.

6. Escolha o botão **OK** para fechar a caixa de diálogo.

## <a name="add-necessary-imports-statements"></a>Adicionar instruções de importações necessárias

1. No **Gerenciador de soluções**, abra o menu de atalho para MainWindow. XAML. vb e escolha **Exibir código**.

2. Adicione as instruções a seguir `Imports` na parte superior do arquivo de código se elas ainda não estiverem presentes.

    ```vb
    Imports System.Net.Http
    Imports System.Net
    Imports System.IO
    ```

## <a name="create-a-synchronous-application"></a>Criar um aplicativo síncrono

1. Na janela de design, MainWindow. XAML, clique duas vezes no botão **Iniciar** para criar o `startButton_Click` manipulador de eventos em MainWindow. XAML. vb.

2. Em MainWindow. XAML. vb, copie o seguinte código no corpo de `startButton_Click` :

    ```vb
    resultsTextBox.Clear()
    SumPageSizes()
    resultsTextBox.Text &= vbCrLf & "Control returned to startButton_Click."
    ```

    O código chama o método que aciona o aplicativo, `SumPageSizes` e exibe uma mensagem quando o controle retorna para `startButton_Click`.

3. O código para a solução síncrona contém os quatro métodos a seguir:

    - `SumPageSizes`, que obtém uma lista de URLs de página da Web de `SetUpURLList` e chama `GetURLContents` e `DisplayResults` para processar cada URL.

    - `SetUpURLList`, que cria e retorna uma lista de endereços web.

    - `GetURLContents`, que baixa o conteúdo de cada site e retorna o conteúdo como uma matriz de bytes.

    - `DisplayResults`, que exibe o número de bytes na matriz de bytes de cada URL.

    Copie os quatro métodos a seguir e cole-os no `startButton_Click` manipulador de eventos em MainWindow. XAML. vb:

    ```vb
    Private Sub SumPageSizes()

        ' Make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        Dim total = 0
        For Each url In urlList
            ' GetURLContents returns the contents of url as a byte array.
            Dim urlContents As Byte() = GetURLContents(url)

            DisplayResults(url, urlContents)

            ' Update the total.
            total += urlContents.Length
        Next

        ' Display the total count for all of the web addresses.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf & "Total bytes returned:  {0}" & vbCrLf, total)
    End Sub

    Private Function SetUpURLList() As List(Of String)

        Dim urls = New List(Of String) From
            {
                "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
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

    Private Function GetURLContents(url As String) As Byte()

        ' The downloaded resource ends up in the variable named content.
        Dim content = New MemoryStream()

        ' Initialize an HttpWebRequest for the current URL.
        Dim webReq = CType(WebRequest.Create(url), HttpWebRequest)

        ' Send the request to the Internet resource and wait for
        ' the response.
        ' Note: you can't use HttpWebRequest.GetResponse in a Windows Store app.
        Using response As WebResponse = webReq.GetResponse()
            ' Get the data stream that is associated with the specified URL.
            Using responseStream As Stream = response.GetResponseStream()
                ' Read the bytes in responseStream and copy them to content.
                responseStream.CopyTo(content)
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
    ```

## <a name="test-the-synchronous-solution"></a>Testar a solução síncrona

1. Escolha a tecla F5 para executar o programa e, em seguida, o botão **Iniciar**.

    Uma saída semelhante à lista a seguir deve aparecer:

    ```console
    msdn.microsoft.com/library/windows/apps/br211380.aspx        383832
    msdn.microsoft.com                                            33964
    msdn.microsoft.com/library/hh290136.aspx               225793
    msdn.microsoft.com/library/ee256749.aspx               143577
    msdn.microsoft.com/library/hh290138.aspx               237372
    msdn.microsoft.com/library/hh290140.aspx               128279
    msdn.microsoft.com/library/dd470362.aspx               157649
    msdn.microsoft.com/library/aa578028.aspx               204457
    msdn.microsoft.com/library/ms404677.aspx               176405
    msdn.microsoft.com/library/ff730837.aspx               143474

    Total bytes returned:  1834802

    Control returned to startButton_Click.
    ```

    Observe que são necessários alguns segundos para exibir as contagens. Durante esse tempo, o thread da interface do usuário é bloqueado enquanto espera que os recursos solicitados sejam baixados. Como resultado, você não pode mover, maximizar, minimizar ou até mesmo fechar a janela de exibição após escolher o botão **Iniciar**. Esses esforços falham até que as contagens de bytes comecem a aparecer. Se um site não estiver respondendo, você não terá nenhuma indicação de qual site falhou. É difícil até mesmo parar de esperar e fechar o programa.

## <a name="convert-geturlcontents-to-an-asynchronous-method"></a>Converter GetURLContents em um método assíncrono

1. Para converter a solução síncrona em uma solução assíncrona, o melhor lugar para começar é no, `GetURLContents` pois as chamadas para o <xref:System.Net.HttpWebRequest.GetResponse%2A?displayProperty=nameWithType> método e para o <xref:System.IO.Stream.CopyTo%2A?displayProperty=nameWithType> método são onde o aplicativo acessa a Web. O .NET Framework facilita a conversão fornecendo versões assíncronas dos dois métodos.

    Para obter mais informações sobre os métodos usados em `GetURLContents`, consulte <xref:System.Net.WebRequest>.

    > [!NOTE]
    > Conforme você seguir as etapas neste passo a passo, vários erros de compilador serão exibidos. Você pode ignorá-los e continuar com o passo a passo.

    Altere o método chamado na terceira linha de `GetURLContents` de `GetResponse` para o método <xref:System.Net.WebRequest.GetResponseAsync%2A> assíncrono, baseado em tarefas.

    ```vb
    Using response As WebResponse = webReq.GetResponseAsync()
    ```

2. `GetResponseAsync` retorna um <xref:System.Threading.Tasks.Task%601>. Nesse caso, a *variável de retorno da tarefa*, `TResult` , tem o tipo <xref:System.Net.WebResponse> . A tarefa é uma promessa de produzir um objeto `WebResponse` verdadeiro após os dados solicitados terem sido baixados e a tarefa ter sido executada até a conclusão.

    Para recuperar o `WebResponse` valor da tarefa, aplique um operador [Await](../../../language-reference/operators/await-operator.md) à chamada para `GetResponseAsync` , como mostra o código a seguir.

    ```vb
    Using response As WebResponse = Await webReq.GetResponseAsync()
    ```

    O operador `Await` suspende a execução do método atual, `GetURLContents`, até que a tarefa aguardada seja concluída. Enquanto isso, o controle retorna para o chamador do método atual. Neste exemplo, o método atual é `GetURLContents` e o chamador é `SumPageSizes`. Quando a tarefa é concluída, o objeto `WebResponse` prometido é produzido como o valor da tarefa aguardada e é atribuído à variável `response`.

    A instrução anterior pode ser separada em duas instruções a seguir para esclarecer o que acontece.

    ```vb
    Dim responseTask As Task(Of WebResponse) = webReq.GetResponseAsync()
    Using response As WebResponse = Await responseTask
    ```

    A chamada para `webReq.GetResponseAsync` retorna um `Task(Of WebResponse)` ou `Task<WebResponse>`. Em seguida, um `Await` operador é aplicado à tarefa para recuperar o `WebResponse` valor.

    Se o método Async tiver trabalho para fazer isso não depende da conclusão da tarefa, o método poderá continuar com esse trabalho entre essas duas instruções, após a chamada para o método Async e antes que o operador Await seja aplicado. Para obter exemplos, consulte [como: fazer várias solicitações da Web em paralelo usando Async e Await (Visual Basic)](how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md) e [como: estender o tutorial assíncrono usando Task. WhenAll (Visual Basic)](how-to-extend-the-async-walkthrough-by-using-task-whenall.md).

3. Como você adicionou o operador `Await` na etapa anterior, um erro do compilador ocorre. O operador só pode ser usado em métodos que são marcados com o modificador [Async](../../../language-reference/modifiers/async.md) . Ignore o erro enquanto você repetir as etapas de conversão para substituir a chamada para `CopyTo` por uma chamada para `CopyToAsync`.

    - Altere o nome do método que é chamado para <xref:System.IO.Stream.CopyToAsync%2A>.

    - O método `CopyTo` ou `CopyToAsync` copia bytes para seu argumento, `content` e não retorna um valor significativo. Na versão síncrona, a chamada para `CopyTo` é uma instrução simples que não retorna um valor. A versão assíncrona, `CopyToAsync`, retorna um <xref:System.Threading.Tasks.Task>. A tarefa funciona como "Task(void)" e permite que o método seja aguardado. Aplique `Await` ou `await` à chamada para `CopyToAsync`, como mostra o código a seguir.

        ```vb
        Await responseStream.CopyToAsync(content)
        ```

         A instrução anterior abrevia as duas linhas de código a seguir.

        ```vb
        ' CopyToAsync returns a Task, not a Task<T>.
        Dim copyTask As Task = responseStream.CopyToAsync(content)

        ' When copyTask is completed, content contains a copy of
        ' responseStream.
        Await copyTask
        ```

4. Tudo o que resta fazer em `GetURLContents` é ajustar a assinatura do método. Você pode usar o `Await` operador somente em métodos que são marcados com o modificador [assíncrono](../../../language-reference/modifiers/async.md) . Adicione o modificador para marcar o método como um *método assíncrono*, como mostra o código a seguir.

    ```vb
    Private Async Function GetURLContents(url As String) As Byte()
    ```

5. O tipo de retorno de um método assíncrono só pode ser <xref:System.Threading.Tasks.Task> , <xref:System.Threading.Tasks.Task%601> . Em Visual Basic, o método deve ser um `Function` que retorna um `Task` ou um `Task(Of T)` , ou o método deve ser um `Sub` . Normalmente, um `Sub` método é usado apenas em um manipulador de eventos assíncrono, onde `Sub` é necessário. Em outros casos, você usará `Task(T)` se o método Completed tiver uma instrução [Return](../../../language-reference/statements/return-statement.md) que retorna um valor do tipo T, e você usará `Task` se o método Completed não retornar um valor significativo.

    Para obter mais informações, consulte [tipos de retorno assíncronos (Visual Basic)](async-return-types.md).

    O método `GetURLContents` tem uma instrução de retorno e a instrução retorna uma matriz de bytes. Portanto, o tipo de retorno da versão assíncrona é Task(T), em que T é uma matriz de bytes. Faça as seguintes alterações na assinatura do método:

    - Altere o tipo de retorno para `Task(Of Byte())`.

    - Por convenção, métodos assíncronos têm nomes que terminam em "Async", então renomeie o método `GetURLContentsAsync`.

    O código a seguir mostra essas alterações.

    ```vb
    Private Async Function GetURLContentsAsync(url As String) As Task(Of Byte())
    ```

    Com essas poucas alterações, a conversão de `GetURLContents` em um método assíncrono é concluída.

## <a name="convert-sumpagesizes-to-an-asynchronous-method"></a>Converter SumPageSizes em um método assíncrono

1. Repita as etapas do procedimento anterior para `SumPageSizes`. Primeiro, altere a chamada para `GetURLContents` para uma chamada assíncrona.

    - Altere o nome do método que é chamado de `GetURLContents` para `GetURLContentsAsync`, se ainda não tiver feito isso.

    - Aplique `Await` à tarefa que `GetURLContentsAsync` retorna para obter o valor da matriz de bytes.

    O código a seguir mostra essas alterações.

    ```vb
    Dim urlContents As Byte() = Await GetURLContentsAsync(url)
    ```

    A atribuição anterior abrevia as duas linhas de código a seguir.

    ```vb
    ' GetURLContentsAsync returns a task. At completion, the task
    ' produces a byte array.
    Dim getContentsTask As Task(Of Byte()) = GetURLContentsAsync(url)
    Dim urlContents As Byte() = Await getContentsTask
    ```

2. Faça as seguintes alterações na assinatura do método:

    - Marque o método com o modificador `Async`.

    - Acrescente "Async" ao nome do método.

    - Não há nenhuma variável de retorno de tarefa, T, desta vez porque `SumPageSizesAsync` o não retorna um valor para T. (o método não tem `Return` instrução.) No entanto, o método deve retornar um `Task` para ser awaitable. Portanto, altere o tipo de método de `Sub` para `Function` . O tipo de retorno da função é `Task` .

    O código a seguir mostra essas alterações.

    ```vb
    Private Async Function SumPageSizesAsync() As Task
    ```

    A conversão de `SumPageSizes` em `SumPageSizesAsync` está concluída.

## <a name="convert-startbutton_click-to-an-asynchronous-method"></a>Converter startButton_Click em um método assíncrono

1. No manipulador de eventos, altere o nome do método chamado de `SumPageSizes` para `SumPageSizesAsync`, se ainda não tiver feito isso.

2. Como `SumPageSizesAsync` é um método assíncrono, altere o código no manipulador de eventos para aguardar o resultado.

    A chamada para `SumPageSizesAsync` espelha a chamada para `CopyToAsync` em `GetURLContentsAsync`. A chamada retorna um `Task` e não um `Task(T)`.

    Assim como nos procedimentos anteriores, você pode converter a chamada usando uma instrução ou duas instruções. O código a seguir mostra essas alterações.

    ```vb
    ' One-step async call.
    Await SumPageSizesAsync()

    ' Two-step async call.
    Dim sumTask As Task = SumPageSizesAsync()
    Await sumTask
    ```

3. Para evitar inserir novamente a operação por acidente, adicione a seguinte instrução à parte superior de `startButton_Click` para desabilitar o botão **Iniciar**.

    ```vb
    ' Disable the button until the operation is complete.
    startButton.IsEnabled = False
    ```

    É possível reabilitar o botão no final do manipulador de eventos.

    ```vb
    ' Reenable the button in case you want to run the operation again.
    startButton.IsEnabled = True
    ```

    Para obter mais informações sobre a reentrância, consulte [lidando com a reentrância em aplicativos assíncronos (Visual Basic)](handling-reentrancy-in-async-apps.md).

4. Por fim, adicione o modificador `Async` à declaração de modo que o manipulador de eventos pode esperar `SumPagSizesAsync`.

    ```vb
    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click
    ```

    Normalmente, os nomes dos manipuladores de eventos não são alterados. O tipo de retorno não é alterado para `Task` porque os manipuladores de eventos devem ser `Sub` procedimentos em Visual Basic.

    A conversão do projeto de um processamento síncrono em um assíncrono está concluída.

## <a name="test-the-asynchronous-solution"></a>Testar a solução assíncrona

1. Escolha a tecla F5 para executar o programa e, em seguida, o botão **Iniciar**.

2. Uma saída semelhante à saída da solução síncrona deve aparecer. No entanto, observe as diferenças a seguir.

    - Os resultados não ocorrem todos ao mesmo tempo, após a conclusão do processamento. Por exemplo, os dois programas contêm uma linha em `startButton_Click` que desmarca a caixa de texto. A intenção é desmarcar a caixa de texto entre as execuções se você escolher o botão **Iniciar** pela segunda vez, após um conjunto de resultados ter aparecido. Na versão síncrona, a caixa de texto é desmarcada logo antes das contagens aparecerem pela segunda vez, quando os downloads são concluídos e o thread da interface do usuário fica livre para executar outras tarefas. Na versão assíncrona, a caixa de texto é limpa imediatamente após você escolher o botão **Iniciar**.

    - Mais importante, o thread da interface do usuário não é bloqueado durante os downloads. Você pode mover ou redimensionar a janela enquanto os recursos da Web estão sendo baixados, contados e exibido. Se um dos sites estiver lento ou não estiver respondendo, você poderá cancelar a operação escolhendo o botão **Fechar** (o x no campo vermelho no canto superior direito).

## <a name="replace-the-geturlcontentsasync-method-with-a-net-framework-method"></a>Substitua o método GetURLContentsAsync por um método .NET Framework

1. O .NET Framework fornece muitos métodos assíncronos que você pode usar. Um deles, o <xref:System.Net.Http.HttpClient.GetByteArrayAsync%28System.String%29?displayProperty=nameWithType> método, faz exatamente o que você precisa para este passo a passos. Você pode usá-lo em vez do método `GetURLContentsAsync` que criou em um procedimento anterior.

    A primeira etapa é criar um <xref:System.Net.Http.HttpClient> objeto no `SumPageSizesAsync` método. Adicione a declaração a seguir ao início do método.

    ```vb
    ' Declare an HttpClient object and increase the buffer size. The
    ' default buffer size is 65,536.
    Dim client As HttpClient =
        New HttpClient() With {.MaxResponseContentBufferSize = 1000000}
    ```

2. Em `SumPageSizesAsync,`, substitua a chamada para seu método `GetURLContentsAsync` por uma chamada para o método `HttpClient`.

    ```vb
    Dim urlContents As Byte() = Await client.GetByteArrayAsync(url)
    ```

3. Remova ou comente o método `GetURLContentsAsync` que você escreveu.

4. Escolha a tecla F5 para executar o programa e, em seguida, o botão **Iniciar**.

    O comportamento desta versão do projeto deve corresponder ao comportamento que o procedimento "Testar a solução assíncrona" descreve, mas com ainda menos esforço para você.

## <a name="example"></a>Exemplo

Veja a seguir o exemplo completo da solução assíncrona convertida que usa o `GetURLContentsAsync` método assíncrono. Observe que ele se assemelha muito à solução síncrona original.

```vb
' Add the following Imports statements, and add a reference for System.Net.Http.
Imports System.Net.Http
Imports System.Net
Imports System.IO

Class MainWindow

    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click

        ' Disable the button until the operation is complete.
        startButton.IsEnabled = False

        resultsTextBox.Clear()

        '' One-step async call.
        Await SumPageSizesAsync()

        ' Two-step async call.
        'Dim sumTask As Task = SumPageSizesAsync()
        'Await sumTask

        resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."

        ' Reenable the button in case you want to run the operation again.
        startButton.IsEnabled = True
    End Sub

    Private Async Function SumPageSizesAsync() As Task

        ' Make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        Dim total = 0
        For Each url In urlList
            Dim urlContents As Byte() = Await GetURLContentsAsync(url)

            ' The previous line abbreviates the following two assignment statements.

            '//<snippet21>
            ' GetURLContentsAsync returns a task. At completion, the task
            ' produces a byte array.
            'Dim getContentsTask As Task(Of Byte()) = GetURLContentsAsync(url)
            'Dim urlContents As Byte() = Await getContentsTask

            DisplayResults(url, urlContents)

            ' Update the total.
            total += urlContents.Length
        Next

        ' Display the total count for all of the websites.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function

    Private Function SetUpURLList() As List(Of String)

        Dim urls = New List(Of String) From
            {
                "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
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

    Private Async Function GetURLContentsAsync(url As String) As Task(Of Byte())

        ' The downloaded resource ends up in the variable named content.
        Dim content = New MemoryStream()

        ' Initialize an HttpWebRequest for the current URL.
        Dim webReq = CType(WebRequest.Create(url), HttpWebRequest)

        ' Send the request to the Internet resource and wait for
        ' the response.
        Using response As WebResponse = Await webReq.GetResponseAsync()

            ' The previous statement abbreviates the following two statements.

            'Dim responseTask As Task(Of WebResponse) = webReq.GetResponseAsync()
            'Using response As WebResponse = Await responseTask

            ' Get the data stream that is associated with the specified URL.
            Using responseStream As Stream = response.GetResponseStream()
                ' Read the bytes in responseStream and copy them to content.
                Await responseStream.CopyToAsync(content)

                ' The previous statement abbreviates the following two statements.

                ' CopyToAsync returns a Task, not a Task<T>.
                'Dim copyTask As Task = responseStream.CopyToAsync(content)

                ' When copyTask is completed, content contains a copy of
                ' responseStream.
                'Await copyTask
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

O código a seguir contém o exemplo completo da solução que usa o método `HttpClient`, `GetByteArrayAsync`.

```vb
' Add the following Imports statements, and add a reference for System.Net.Http.
Imports System.Net.Http
Imports System.Net
Imports System.IO

Class MainWindow

    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click

        resultsTextBox.Clear()

        ' Disable the button until the operation is complete.
        startButton.IsEnabled = False

        ' One-step async call.
        Await SumPageSizesAsync()

        ' Two-step async call.
        'Dim sumTask As Task = SumPageSizesAsync()
        'Await sumTask

        resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."

        ' Reenable the button in case you want to run the operation again.
        startButton.IsEnabled = True
    End Sub

    Private Async Function SumPageSizesAsync() As Task

        ' Declare an HttpClient object and increase the buffer size. The
        ' default buffer size is 65,536.
        Dim client As HttpClient =
            New HttpClient() With {.MaxResponseContentBufferSize = 1000000}

        ' Make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        Dim total = 0
        For Each url In urlList
            ' GetByteArrayAsync returns a task. At completion, the task
            ' produces a byte array.
            Dim urlContents As Byte() = Await client.GetByteArrayAsync(url)

            ' The following two lines can replace the previous assignment statement.
            'Dim getContentsTask As Task(Of Byte()) = client.GetByteArrayAsync(url)
            'Dim urlContents As Byte() = Await getContentsTask

            DisplayResults(url, urlContents)

            ' Update the total.
            total += urlContents.Length
        Next

        ' Display the total count for all of the websites.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function

    Private Function SetUpURLList() As List(Of String)

        Dim urls = New List(Of String) From
            {
                "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
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

- [Exemplo de assincronia: acessando o passo a passo da Web (C# e Visual Basic)](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f)
- [Operador Await](../../../language-reference/operators/await-operator.md)
- [Async](../../../language-reference/modifiers/async.md)
- [Programação assíncrona com Async e Await (Visual Basic)](index.md)
- [Tipos de retorno assíncronos (Visual Basic)](async-return-types.md)
- [TAP (programação assíncrona baseada em tarefas)](https://www.microsoft.com/download/details.aspx?id=19957)
- [Como estender as instruções passo a passo assíncronas usando Task.WhenAll (Visual Basic)](how-to-extend-the-async-walkthrough-by-using-task-whenall.md)
- [Como fazer várias solicitações da Web em paralelo usando Async e Await (Visual Basic)](how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md)
