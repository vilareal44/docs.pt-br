---
title: 'Passo a passo: acessando a Web usando async e await (C#)'
description: Este tutorial converte um aplicativo síncrono em uma solução assíncrona em C# que usa os recursos Async e Await.
ms.date: 07/20/2015
ms.assetid: c95d8d71-5a98-4bf0-aaf4-45fed2ebbacd
ms.openlocfilehash: d643793bfcdeaaeff56dd252c510d197a45442f9
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925105"
---
# <a name="walkthrough-accessing-the-web-by-using-async-and-await-c"></a>Passo a passo: acessando a Web usando async e await (C#)

É possível escrever programas assíncronos de forma mais fácil e intuitiva usando funcionalidades async/await. Você pode escrever código assíncrono que se parece com código síncrono e deixar que o compilador trate das complicadas continuações e funções de retorno de chamada que um código assíncrono normalmente envolve.

Para obter mais informações sobre o recurso Assíncrono, consulte [Programação assíncrona com async e await (C#)](./index.md).

Este passo a passo começa com um aplicativo WPF (Windows Presentation Foundation) síncrono que soma o número de bytes em uma lista de sites. Em seguida, converte o aplicativo em uma solução assíncrona usando os novos recursos.

Se não quiser compilar os aplicativos, você poderá baixar o [Exemplo de assincronia: acessando o passo a passo da Web (C# e Visual Basic)](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f).

> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio 2012 ou uma versão mais recente e o .NET Framework 4.5 ou posterior instalados em seu computador.

## <a name="create-a-wpf-application"></a>Criar um aplicativo WPF

1. Inicie o Visual Studio.

2. Na barra de menus, escolha **arquivo**  >  **novo**  >  **projeto**.

     A caixa de diálogo **Novo Projeto** será aberta.

3. No painel **Modelos Instalados**, escolha Visual C# e, em seguida, escolha **Aplicativo WPF** na lista de tipos de projeto.

4. Na caixa de texto **Nome**, insira `AsyncExampleWPF` e, em seguida, escolha o botão **OK**.

     O novo projeto aparece no **Gerenciador de Soluções**.

## <a name="design-a-simple-wpf-mainwindow"></a>Projetar um MainWindow WPF simples

1. No Editor do Visual Studio Code, escolha a guia **MainWindow.xaml**.

2. Se a janela **caixa de ferramentas** não estiver visível, abra o menu **Exibir** e escolha caixa de **ferramentas**.

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

2. Na barra de menus, escolha **projeto**  >  **Adicionar referência**.

     A caixa de diálogo **Gerenciador de Referências** é exibida.

3. Na parte superior da caixa de diálogo, verifique se seu projeto é voltado para o .NET Framework 4.5 ou posterior.

4. Na categoria **assemblies** , escolha **estrutura** se ela ainda não estiver escolhida.

5. Na lista de nomes, marque a caixa de seleção **System.Net.Http**.

6. Escolha o botão **OK** para fechar a caixa de diálogo.

## <a name="add-necessary-using-directives"></a>Adicionar as diretivas using necessárias

1. No **Gerenciador de Soluções**, abra o menu de atalho de MainWindow.xaml.cs e, em seguida, escolha **Exibir Código**.

2. Adicione as seguintes `using` diretivas na parte superior do arquivo de código se elas ainda não estiverem presentes.

    ```csharp
    using System.Net.Http;
    using System.Net;
    using System.IO;
    ```

## <a name="create-a-synchronous-app"></a>Criar um aplicativo síncrono

1. Na janela de design, MainWindow.xaml, clique duas vezes no botão **Iniciar** para criar o manipulador de eventos `startButton_Click` em MainWindow.xaml.cs.

2. Em MainWindow.xaml.cs, copie o seguinte código para o corpo de `startButton_Click`:

    ```csharp
    resultsTextBox.Clear();
    SumPageSizes();
    resultsTextBox.Text += "\r\nControl returned to startButton_Click.";
    ```

    O código chama o método que aciona o aplicativo, `SumPageSizes` e exibe uma mensagem quando o controle retorna para `startButton_Click`.

3. O código para a solução síncrona contém os quatro métodos a seguir:

    - `SumPageSizes`, que obtém uma lista de URLs de página da Web de `SetUpURLList` e chama `GetURLContents` e `DisplayResults` para processar cada URL.

    - `SetUpURLList`, que cria e retorna uma lista de endereços web.

    - `GetURLContents`, que baixa o conteúdo de cada site e retorna o conteúdo como uma matriz de bytes.

    - `DisplayResults`, que exibe o número de bytes na matriz de bytes de cada URL.

    Copie os quatro métodos a seguir e cole-os no manipulador de eventos `startButton_Click` em MainWindow.xaml.cs:

    ```csharp
    private void SumPageSizes()
    {
        // Make a list of web addresses.
        List<string> urlList = SetUpURLList();

        var total = 0;
        foreach (var url in urlList)
        {
            // GetURLContents returns the contents of url as a byte array.
            byte[] urlContents = GetURLContents(url);

            DisplayResults(url, urlContents);

            // Update the total.
            total += urlContents.Length;
        }

        // Display the total count for all of the web addresses.
        resultsTextBox.Text += $"\r\n\r\nTotal bytes returned:  {total}\r\n";
    }

    private List<string> SetUpURLList()
    {
        var urls = new List<string>
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
        };
        return urls;
    }

    private byte[] GetURLContents(string url)
    {
        // The downloaded resource ends up in the variable named content.
        var content = new MemoryStream();

        // Initialize an HttpWebRequest for the current URL.
        var webReq = (HttpWebRequest)WebRequest.Create(url);

        // Send the request to the Internet resource and wait for
        // the response.
        // Note: you can't use HttpWebRequest.GetResponse in a Windows Store app.
        using (WebResponse response = webReq.GetResponse())
        {
            // Get the data stream that is associated with the specified URL.
            using (Stream responseStream = response.GetResponseStream())
            {
                // Read the bytes in responseStream and copy them to content.
                responseStream.CopyTo(content);
            }
        }

        // Return the result as a byte array.
        return content.ToArray();
    }

    private void DisplayResults(string url, byte[] content)
    {
        // Display the length of each website. The string format
        // is designed to be used with a monospaced font, such as
        // Lucida Console or Global Monospace.
        var bytes = content.Length;
        // Strip off the "https://".
        var displayURL = url.Replace("https://", "");
        resultsTextBox.Text += $"\n{displayURL,-58} {bytes,8}";
    }
    ```

## <a name="test-the-synchronous-solution"></a>Testar a solução síncrona

Escolha a tecla **F5** para executar o programa e, em seguida, o botão **Iniciar**.

Uma saída semelhante à lista a seguir deve aparecer:

```text
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

1. Para converter a solução síncrona em uma solução assíncrona, o melhor lugar para começar é em `GetURLContents`, porque é pelas chamadas para o método <xref:System.Net.HttpWebRequest.GetResponse%2A> de <xref:System.Net.HttpWebRequest> e para o método <xref:System.IO.Stream.CopyTo%2A> de <xref:System.IO.Stream> que o aplicativo acessa a Web. .NET Framework facilita a conversão fornecendo versões assíncronas de ambos os métodos.

     Para obter mais informações sobre os métodos usados em `GetURLContents`, consulte <xref:System.Net.WebRequest>.

    > [!NOTE]
    > Conforme você seguir as etapas neste passo a passo, vários erros de compilador serão exibidos. Você pode ignorá-los e continuar com o passo a passo.

     Altere o método chamado na terceira linha de `GetURLContents` de `GetResponse` para o método <xref:System.Net.WebRequest.GetResponseAsync%2A> assíncrono, baseado em tarefas.

    ```csharp
    using (WebResponse response = webReq.GetResponseAsync())
    ```

2. `GetResponseAsync` retorna um <xref:System.Threading.Tasks.Task%601>. Nesse caso, a *variável de retorno da tarefa*, `TResult` , tem o tipo <xref:System.Net.WebResponse> . A tarefa é uma promessa de produzir um objeto `WebResponse` verdadeiro após os dados solicitados terem sido baixados e a tarefa ter sido executada até a conclusão.

     Para recuperar o valor `WebResponse` da tarefa, aplique um operador [await](../../../language-reference/operators/await.md) à chamada para `GetResponseAsync`, como mostra o código a seguir.

    ```csharp
    using (WebResponse response = await webReq.GetResponseAsync())
    ```

     O operador `await` suspende a execução do método atual, `GetURLContents`, até que a tarefa aguardada seja concluída. Enquanto isso, o controle retorna para o chamador do método atual. Neste exemplo, o método atual é `GetURLContents` e o chamador é `SumPageSizes`. Quando a tarefa é concluída, o objeto `WebResponse` prometido é produzido como o valor da tarefa aguardada e é atribuído à variável `response`.

     A instrução anterior pode ser separada em duas instruções a seguir para esclarecer o que acontece.

    ```csharp
    //Task<WebResponse> responseTask = webReq.GetResponseAsync();
    //using (WebResponse response = await responseTask)
    ```

     A chamada para `webReq.GetResponseAsync` retorna um `Task(Of WebResponse)` ou `Task<WebResponse>`. Em seguida, um operador await é aplicado à tarefa para recuperar o valor `WebResponse`.

     Se o método Async tiver trabalho para fazer isso não depende da conclusão da tarefa, o método poderá continuar com esse trabalho entre essas duas instruções, após a chamada para o método Async e antes da aplicação do `await` operador. Para obter exemplos, consulte [como fazer várias solicitações da Web em paralelo usando Async e Await (c#)](./how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md) e [como estender a explicação em modo assíncrono usando Task. WhenAll (c#)](./how-to-extend-the-async-walkthrough-by-using-task-whenall.md).

3. Como você adicionou o operador `await` na etapa anterior, um erro do compilador ocorre. O operador pode ser usado apenas em métodos que são marcados com o modificador [async](../../../language-reference/keywords/async.md). Ignore o erro enquanto você repetir as etapas de conversão para substituir a chamada para `CopyTo` por uma chamada para `CopyToAsync`.

    - Altere o nome do método chamado para <xref:System.IO.Stream.CopyToAsync%2A> .

    - O `CopyTo` `CopyToAsync` método ou copia bytes para seu argumento, `content` e não retorna um valor significativo. Na versão síncrona, a chamada para `CopyTo` é uma instrução simples que não retorna um valor. A versão assíncrona, `CopyToAsync`, retorna um <xref:System.Threading.Tasks.Task>. A tarefa funciona como "Task(void)" e permite que o método seja aguardado. Aplique `Await` ou `await` à chamada para `CopyToAsync`, como mostra o código a seguir.

        ```csharp
        await responseStream.CopyToAsync(content);
        ```

         A instrução anterior abrevia as duas linhas de código a seguir.

        ```csharp
        // CopyToAsync returns a Task, not a Task<T>.
        //Task copyTask = responseStream.CopyToAsync(content);

        // When copyTask is completed, content contains a copy of
        // responseStream.
        //await copyTask;
        ```

4. Tudo o que resta fazer em `GetURLContents` é ajustar a assinatura do método. Você pode usar o operador `await` apenas em métodos que são marcados com o modificador [async](../../../language-reference/keywords/async.md). Adicione o modificador para marcar o método como um *método assíncrono*, como mostra o código a seguir.

    ```csharp
    private async byte[] GetURLContents(string url)
    ```

5. O tipo de retorno de um método assíncrono só pode ser <xref:System.Threading.Tasks.Task>, <xref:System.Threading.Tasks.Task%601> ou `void` em C#. Normalmente, um tipo de retorno de `void` é usado somente em um manipulador de eventos assíncrono, em que `void` é necessário. Em outros casos, você usará `Task(T)` se o método Completed tiver uma instrução [Return](../../../language-reference/keywords/return.md) que retorna um valor do tipo T, e você usará `Task` se o método Completed não retornar um valor significativo. Você pode considerar que o tipo de retorno `Task` significa "Task(void)".

     Para obter mais informações, consulte [Tipos de retorno assíncronos (C#)](./async-return-types.md).

     O método `GetURLContents` tem uma instrução de retorno e a instrução retorna uma matriz de bytes. Portanto, o tipo de retorno da versão assíncrona é Task(T), em que T é uma matriz de bytes. Faça as seguintes alterações na assinatura do método:

    - Altere o tipo de retorno para `Task<byte[]>`.

    - Por convenção, métodos assíncronos têm nomes que terminam em "Async", então renomeie o método `GetURLContentsAsync`.

     O código a seguir mostra essas alterações.

    ```csharp
    private async Task<byte[]> GetURLContentsAsync(string url)
    ```

     Com essas poucas alterações, a conversão de `GetURLContents` em um método assíncrono é concluída.

## <a name="convert-sumpagesizes-to-an-asynchronous-method"></a>Converter SumPageSizes em um método assíncrono

1. Repita as etapas do procedimento anterior para `SumPageSizes`. Primeiro, altere a chamada para `GetURLContents` para uma chamada assíncrona.

    - Altere o nome do método que é chamado de `GetURLContents` para `GetURLContentsAsync` , se você ainda não tiver feito isso.

    - Aplique `await` à tarefa que `GetURLContentsAsync` retorna para obter o valor da matriz de bytes.

     O código a seguir mostra essas alterações.

    ```csharp
    byte[] urlContents = await GetURLContentsAsync(url);
    ```

     A atribuição anterior abrevia as duas linhas de código a seguir.

    ```csharp
    // GetURLContentsAsync returns a Task<T>. At completion, the task
    // produces a byte array.
    //Task<byte[]> getContentsTask = GetURLContentsAsync(url);
    //byte[] urlContents = await getContentsTask;
    ```

2. Faça as seguintes alterações na assinatura do método:

    - Marque o método com o modificador `async`.

    - Acrescente "Async" ao nome do método.

    - Não há nenhuma variável de retorno de tarefa, T, desta vez porque `SumPageSizesAsync` o não retorna um valor para T. (o método não tem `return` instrução.) No entanto, o método deve retornar um `Task` para ser awaitable. Portanto, altere o tipo de retorno do método de `void` para `Task`.

    O código a seguir mostra essas alterações.

    ```csharp
    private async Task SumPageSizesAsync()
    ```

     A conversão de `SumPageSizes` em `SumPageSizesAsync` está concluída.

## <a name="convert-startbutton_click-to-an-asynchronous-method"></a>Converter startButton_Click em um método assíncrono

1. No manipulador de eventos, altere o nome do método chamado de `SumPageSizes` para `SumPageSizesAsync` , se você ainda não tiver feito isso.

2. Como `SumPageSizesAsync` é um método assíncrono, altere o código no manipulador de eventos para aguardar o resultado.

     A chamada para `SumPageSizesAsync` espelha a chamada para `CopyToAsync` em `GetURLContentsAsync`. A chamada retorna um `Task` e não um `Task(T)`.

     Assim como nos procedimentos anteriores, você pode converter a chamada usando uma instrução ou duas instruções. O código a seguir mostra essas alterações.

    ```csharp
    // One-step async call.
    await SumPageSizesAsync();

    // Two-step async call.
    //Task sumTask = SumPageSizesAsync();
    //await sumTask;
    ```

3. Para evitar inserir novamente a operação por acidente, adicione a seguinte instrução à parte superior de `startButton_Click` para desabilitar o botão **Iniciar**.

    ```csharp
    // Disable the button until the operation is complete.
    startButton.IsEnabled = false;
    ```

     É possível reabilitar o botão no final do manipulador de eventos.

    ```csharp
    // Reenable the button in case you want to run the operation again.
    startButton.IsEnabled = true;
    ```

     Para obter mais informações sobre a reentrada, consulte [Tratando a reentrada em aplicativos assíncronos (C#)](./handling-reentrancy-in-async-apps.md).

4. Por fim, adicione o modificador `async` à declaração de modo que o manipulador de eventos pode esperar `SumPagSizesAsync`.

    ```csharp
    private async void startButton_Click(object sender, RoutedEventArgs e)
    ```

     Normalmente, os nomes dos manipuladores de eventos não são alterados. O tipo de retorno não é alterado para `Task` porque os manipuladores de eventos devem retornar `void` .

     A conversão do projeto de um processamento síncrono em um assíncrono está concluída.

## <a name="test-the-asynchronous-solution"></a>Testar a solução assíncrona

1. Escolha a tecla **F5** para executar o programa e, em seguida, o botão **Iniciar**.

2. Uma saída semelhante à saída da solução síncrona deve aparecer. No entanto, observe as diferenças a seguir.

    - Os resultados não ocorrem ao mesmo tempo, após a conclusão do processamento. Por exemplo, os dois programas contêm uma linha em `startButton_Click` que desmarca a caixa de texto. A intenção é desmarcar a caixa de texto entre as execuções se você escolher o botão **Iniciar** pela segunda vez, após um conjunto de resultados ter aparecido. Na versão síncrona, a caixa de texto é desmarcada logo antes das contagens aparecerem pela segunda vez, quando os downloads são concluídos e o thread da interface do usuário fica livre para executar outras tarefas. Na versão assíncrona, a caixa de texto é limpa imediatamente após você escolher o botão **Iniciar**.

    - O mais importante é que o thread da interface do usuário não é bloqueado durante os downloads. Você pode mover ou redimensionar a janela enquanto os recursos da Web estão sendo baixados, contados e exibido. Se um dos sites estiver lento ou não estiver respondendo, você poderá cancelar a operação escolhendo o botão **Fechar** (o x no campo vermelho no canto superior direito).

## <a name="replace-method-geturlcontentsasync-with-a-net-method"></a>Substituir o método GetURLContentsAsync por um método .NET

1. .NET Framework 4,5 e versões posteriores fornecem muitos métodos assíncronos que você pode usar. Um deles, o método <xref:System.Net.Http.HttpClient.GetByteArrayAsync%28System.String%29> de <xref:System.Net.Http.HttpClient>, faz exatamente o que é necessário para este passo a passo. Você pode usá-lo em vez do método `GetURLContentsAsync` que criou em um procedimento anterior.

     A primeira etapa é criar um objeto `HttpClient` no método `SumPageSizesAsync`. Adicione a declaração a seguir ao início do método.

    ```csharp
    // Declare an HttpClient object and increase the buffer size. The
    // default buffer size is 65,536.
    HttpClient client =
        new HttpClient() { MaxResponseContentBufferSize = 1000000 };
    ```

2. Em `SumPageSizesAsync,`, substitua a chamada para seu método `GetURLContentsAsync` por uma chamada para o método `HttpClient`.

    ```csharp
    byte[] urlContents = await client.GetByteArrayAsync(url);
    ```

3. Remova ou comente o método `GetURLContentsAsync` que você escreveu.

4. Escolha a tecla **F5** para executar o programa e, em seguida, o botão **Iniciar**.

     O comportamento desta versão do projeto deve corresponder ao comportamento que o procedimento "Testar a solução assíncrona" descreve, mas com ainda menos esforço para você.

## <a name="example-code"></a>Código de exemplo

O código a seguir contém o exemplo completo da conversão de uma solução síncrona em uma solução assíncrona usando o método `GetURLContentsAsync` assíncrono que você escreveu. Observe que ele se assemelha muito à solução síncrona original.

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
using System.IO;
using System.Net;

namespace AsyncExampleWPF
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private async void startButton_Click(object sender, RoutedEventArgs e)
        {
            // Disable the button until the operation is complete.
            startButton.IsEnabled = false;

            resultsTextBox.Clear();

            // One-step async call.
            await SumPageSizesAsync();

            // Two-step async call.
            //Task sumTask = SumPageSizesAsync();
            //await sumTask;

            resultsTextBox.Text += "\r\nControl returned to startButton_Click.\r\n";

            // Reenable the button in case you want to run the operation again.
            startButton.IsEnabled = true;
        }

        private async Task SumPageSizesAsync()
        {
            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            var total = 0;

            foreach (var url in urlList)
            {
                byte[] urlContents = await GetURLContentsAsync(url);

                // The previous line abbreviates the following two assignment statements.

                // GetURLContentsAsync returns a Task<T>. At completion, the task
                // produces a byte array.
                //Task<byte[]> getContentsTask = GetURLContentsAsync(url);
                //byte[] urlContents = await getContentsTask;

                DisplayResults(url, urlContents);

                // Update the total.
                total += urlContents.Length;
            }
            // Display the total count for all of the websites.
            resultsTextBox.Text +=
                $"\r\n\r\nTotal bytes returned:  {total}\r\n";
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
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
            };
            return urls;
        }

        private async Task<byte[]> GetURLContentsAsync(string url)
        {
            // The downloaded resource ends up in the variable named content.
            var content = new MemoryStream();

            // Initialize an HttpWebRequest for the current URL.
            var webReq = (HttpWebRequest)WebRequest.Create(url);

            // Send the request to the Internet resource and wait for
            // the response.
            using (WebResponse response = await webReq.GetResponseAsync())

            // The previous statement abbreviates the following two statements.

            //Task<WebResponse> responseTask = webReq.GetResponseAsync();
            //using (WebResponse response = await responseTask)
            {
                // Get the data stream that is associated with the specified url.
                using (Stream responseStream = response.GetResponseStream())
                {
                    // Read the bytes in responseStream and copy them to content.
                    await responseStream.CopyToAsync(content);

                    // The previous statement abbreviates the following two statements.

                    // CopyToAsync returns a Task, not a Task<T>.
                    //Task copyTask = responseStream.CopyToAsync(content);

                    // When copyTask is completed, content contains a copy of
                    // responseStream.
                    //await copyTask;
                }
            }
            // Return the result as a byte array.
            return content.ToArray();
        }

        private void DisplayResults(string url, byte[] content)
        {
            // Display the length of each website. The string format
            // is designed to be used with a monospaced font, such as
            // Lucida Console or Global Monospace.
            var bytes = content.Length;
            // Strip off the "https://".
            var displayURL = url.Replace("https://", "");
            resultsTextBox.Text += $"\n{displayURL,-58} {bytes,8}";
        }
    }
}
```

O código a seguir contém o exemplo completo da solução que usa o método `HttpClient`, `GetByteArrayAsync`.

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
using System.IO;
using System.Net;

namespace AsyncExampleWPF
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        private async void startButton_Click(object sender, RoutedEventArgs e)
        {
            resultsTextBox.Clear();

            // Disable the button until the operation is complete.
            startButton.IsEnabled = false;

            // One-step async call.
            await SumPageSizesAsync();

            //// Two-step async call.
            //Task sumTask = SumPageSizesAsync();
            //await sumTask;

            resultsTextBox.Text += "\r\nControl returned to startButton_Click.\r\n";

            // Reenable the button in case you want to run the operation again.
            startButton.IsEnabled = true;
        }

        private async Task SumPageSizesAsync()
        {
            // Declare an HttpClient object and increase the buffer size. The
            // default buffer size is 65,536.
            HttpClient client =
                new HttpClient() { MaxResponseContentBufferSize = 1000000 };

            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            var total = 0;

            foreach (var url in urlList)
            {
                // GetByteArrayAsync returns a task. At completion, the task
                // produces a byte array.
                byte[] urlContents = await client.GetByteArrayAsync(url);

                // The following two lines can replace the previous assignment statement.
                //Task<byte[]> getContentsTask = client.GetByteArrayAsync(url);
                //byte[] urlContents = await getContentsTask;

                DisplayResults(url, urlContents);

                // Update the total.
                total += urlContents.Length;
            }

            // Display the total count for all of the websites.
            resultsTextBox.Text +=
                $"\r\n\r\nTotal bytes returned:  {total}\r\n";
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
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
            };
            return urls;
        }

        private void DisplayResults(string url, byte[] content)
        {
            // Display the length of each website. The string format
            // is designed to be used with a monospaced font, such as
            // Lucida Console or Global Monospace.
            var bytes = content.Length;
            // Strip off the "https://".
            var displayURL = url.Replace("https://", "");
            resultsTextBox.Text += $"\n{displayURL,-58} {bytes,8}";
        }
    }
}
```

## <a name="see-also"></a>Veja também

- [Exemplo de assincronia: acessando o passo a passo da Web (C# e Visual Basic)](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2012/hh300224(v=vs.110))
- [async](../../../language-reference/keywords/async.md)
- [await](../../../language-reference/operators/await.md)
- [Programação assíncrona com Async e Await (C#)](./index.md)
- [Tipos de retorno assíncronos (C#)](./async-return-types.md)
- [TAP (programação assíncrona baseada em tarefas)](https://www.microsoft.com/download/details.aspx?id=19957)
- [Como estender a instrução assíncrona usando Task. WhenAll (C#)](./how-to-extend-the-async-walkthrough-by-using-task-whenall.md)
- [Como fazer várias solicitações da Web em paralelo usando Async e Await (C#)](./how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md)
