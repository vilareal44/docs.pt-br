---
title: Cancelar uma tarefa assíncrona ou uma lista de tarefas (C#)
description: Use estes exemplos para adicionar um botão que cancela um aplicativo assíncrono antes de ser concluído. Esse aplicativo C# baixa o conteúdo de um ou mais sites.
ms.date: 07/20/2015
ms.assetid: eec32dbb-70ea-4c88-bd27-fa2e34546914
ms.openlocfilehash: 21bdbc3bc7c3b752fab160429d71356fb87d9976
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925340"
---
# <a name="cancel-an-async-task-or-a-list-of-tasks-c"></a>Cancelar uma tarefa assíncrona ou uma lista de tarefas (C#)

Você pode configurar um botão que pode ser usado para cancelar um aplicativo assíncrono se não desejar aguardar sua conclusão. Seguindo os exemplos neste tópico, você pode adicionar um botão de cancelamento a um aplicativo que baixa o conteúdo de um site ou uma lista de sites.

Os exemplos usam a interface do usuário que [Ajuste fino de seu aplicativo assíncrono (C#)](./fine-tuning-your-async-application.md) descreve.

> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio 2012 ou uma versão mais recente e o .NET Framework 4.5 ou posterior instalados em seu computador.

## <a name="cancel-a-task"></a>Cancelar uma tarefa

O primeiro exemplo associa o botão **Cancelar** a uma única tarefa de download. Se você escolher o botão enquanto o aplicativo está baixando conteúdo, o download será cancelado.

### <a name="download-the-example"></a>Baixar o exemplo

Você pode baixar o projeto completo do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea) e, em seguida, seguir estas etapas.

1. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.

2. Na barra de menus, escolha **arquivo**  >  **abrir**  >  **projeto/solução**.

3. Na caixa de diálogo **Abrir Projeto**, abra a pasta em que está o código de exemplo que você descompactou e, em seguida, abra o arquivo de solução (.sln) de AsyncFineTuningCS.

4. No **Gerenciador de Soluções**, abra o menu de atalho do projeto **CancelATask** e, em seguida, escolha **Definir como Projeto de Inicialização**.

5. Escolha a tecla **F5** para executar o projeto (ou pressione **Ctrl**+**F5** para executar o projeto sem depurá-lo).

> [!TIP]
> Se você não quiser baixar o projeto, você poderá examinar o arquivo MainWindow.xaml.cs no final deste tópico.

### <a name="build-the-example"></a>Criar o exemplo
 As alterações a seguir adicionam um botão **Cancelar** a um aplicativo que baixa um site. Se não desejar baixar ou compilar o exemplo, você poderá examinar o produto final na seção "Exemplos completos" no final deste tópico. Os asteriscos marcam as alterações no código.

 Para compilar o exemplo você mesmo, passo a passo, siga as instruções na seção “Baixando o exemplo”, mas escolha **StarterCode** como o **Projeto de Inicialização** em vez de **CancelATask**.

 Em seguida, adicione as seguintes alterações ao arquivo MainWindow.xaml.cs desse projeto.

1. Declare uma variável `CancellationTokenSource`, `cts`, que está no escopo para todos os métodos a acessarem.

    ```csharp
    public partial class MainWindow : Window
    {
        // ***Declare a System.Threading.CancellationTokenSource.
        CancellationTokenSource cts;
    ```

2. Adicione o seguinte manipulador de eventos para o botão **Cancelar**. O manipulador de eventos usa o método <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=nameWithType> para notificar `cts` quando o usuário solicita o cancelamento.

    ```csharp
    // ***Add an event handler for the Cancel button.
    private void cancelButton_Click(object sender, RoutedEventArgs e)
    {
        if (cts != null)
        {
            cts.Cancel();
        }
    }
    ```

3. Faça as seguintes alterações no manipulador de eventos para o botão **Iniciar**, `startButton_Click`.

    - Crie a instância de `CancellationTokenSource`, `cts`.

        ```csharp
        // ***Instantiate the CancellationTokenSource.
        cts = new CancellationTokenSource();
        ```

    - Na chamada para `AccessTheWebAsync`, que baixa o conteúdo de um site especificado, envie a propriedade <xref:System.Threading.CancellationTokenSource.Token%2A?displayProperty=nameWithType> de `cts` como um argumento. A propriedade `Token` propaga a mensagem se o cancelamento é solicitado. Adicione um bloco catch que exibe uma mensagem se o usuário optar por cancelar a operação de download. O código a seguir mostra as alterações.

        ```csharp
        try
        {
            // ***Send a token to carry the message if cancellation is requested.
            int contentLength = await AccessTheWebAsync(cts.Token);
            resultsTextBox.Text += $"\r\nLength of the downloaded string: {contentLength}.\r\n";
        }
        // *** If cancellation is requested, an OperationCanceledException results.
        catch (OperationCanceledException)
        {
            resultsTextBox.Text += "\r\nDownload canceled.\r\n";
        }
        catch (Exception)
        {
            resultsTextBox.Text += "\r\nDownload failed.\r\n";
        }
        ```

4. No `AccessTheWebAsync`, use a sobrecarga <xref:System.Net.Http.HttpClient.GetAsync%28System.String%2CSystem.Threading.CancellationToken%29?displayProperty=nameWithType> do método `GetAsync` no tipo <xref:System.Net.Http.HttpClient> para baixar o conteúdo de um site. Passe `ct`, o parâmetro <xref:System.Threading.CancellationToken> de `AccessTheWebAsync`, como o segundo argumento. O token executa a mensagem se o usuário escolhe o botão **Cancelar**.

     O código a seguir mostra as alterações em `AccessTheWebAsync`.

    ```csharp
    // ***Provide a parameter for the CancellationToken.
    async Task<int> AccessTheWebAsync(CancellationToken ct)
    {
        HttpClient client = new HttpClient();

        resultsTextBox.Text += "\r\nReady to download.\r\n";

        // You might need to slow things down to have a chance to cancel.
        await Task.Delay(250);

        // GetAsync returns a Task<HttpResponseMessage>.
        // ***The ct argument carries the message if the Cancel button is chosen.
        HttpResponseMessage response = await client.GetAsync("https://msdn.microsoft.com/library/dd470362.aspx", ct);

        // Retrieve the website contents from the HttpResponseMessage.
        byte[] urlContents = await response.Content.ReadAsByteArrayAsync();

        // The result of the method is the length of the downloaded website.
        return urlContents.Length;
    }
    ```

5. Se você não cancelar o programa, ele produzirá a saída a seguir.

    ```text
    Ready to download.
    Length of the downloaded string: 158125.
    ```

     Se você escolher o botão **Cancelar** antes de o programa terminar de baixar o conteúdo, o programa produzirá a saída a seguir.

    ```text
    Ready to download.
    Download canceled.
    ```

## <a name="cancel-a-list-of-tasks"></a>Cancelar uma lista de tarefas

Você pode estender o exemplo anterior para cancelar muitas tarefas associando a mesma instância `CancellationTokenSource` a cada tarefa. Se você escolher o botão **Cancelar**, cancela todas as tarefas que ainda não estão concluídas.

### <a name="download-the-example"></a>Baixar o exemplo

Você pode baixar o projeto completo do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea) e, em seguida, seguir estas etapas.

1. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.

2. Na barra de menus, escolha **arquivo**  >  **abrir**  >  **projeto/solução**.

3. Na caixa de diálogo **Abrir Projeto**, abra a pasta em que está o código de exemplo que você descompactou e, em seguida, abra o arquivo de solução (.sln) de AsyncFineTuningCS.

4. No **Gerenciador de Soluções**, abra o menu de atalho do projeto **CancelAListOfTasks** e, em seguida, escolha **Definir como Projeto de Inicialização**.

5. Pressione a tecla **F5** para executar o projeto.

     Escolha as teclas **Ctrl** + **F5** para executar o projeto sem depurá-lo.

Se você não quiser baixar o projeto, você poderá examinar o arquivo MainWindow.xaml.cs no final deste tópico.

### <a name="build-the-example"></a>Criar o exemplo

Para estender o exemplo você mesmo, passo a passo, siga as instruções na seção "Baixando o exemplo", mas escolha **CancelATask** como o **Projeto de Inicialização**. Adicione as seguintes alterações ao projeto. Os asteriscos marcam as alterações no programa.

1. Adicione um método para criar uma lista de endereços Web.

    ```csharp
    // ***Add a method that creates a list of web addresses.
    private List<string> SetUpURLList()
    {
        List<string> urls = new List<string>
        {
            "https://msdn.microsoft.com",
            "https://msdn.microsoft.com/library/hh290138.aspx",
            "https://msdn.microsoft.com/library/hh290140.aspx",
            "https://msdn.microsoft.com/library/dd470362.aspx",
            "https://msdn.microsoft.com/library/aa578028.aspx",
            "https://msdn.microsoft.com/library/ms404677.aspx",
            "https://msdn.microsoft.com/library/ff730837.aspx"
        };
        return urls;
    }
    ```

2. Chame o método em `AccessTheWebAsync`.

    ```csharp
    // ***Call SetUpURLList to make a list of web addresses.
    List<string> urlList = SetUpURLList();
    ```

3. Adicione o seguinte loop em `AccessTheWebAsync` para processar cada endereço web na lista.

    ```csharp
    // ***Add a loop to process the list of web addresses.
    foreach (var url in urlList)
    {
        // GetAsync returns a Task<HttpResponseMessage>.
        // Argument ct carries the message if the Cancel button is chosen.
        // ***Note that the Cancel button can cancel all remaining downloads.
        HttpResponseMessage response = await client.GetAsync(url, ct);

        // Retrieve the website contents from the HttpResponseMessage.
        byte[] urlContents = await response.Content.ReadAsByteArrayAsync();

        resultsTextBox.Text +=
            $"\r\nLength of the downloaded string: {urlContents.Length}.\r\n";
    }
    ```

4. Como `AccessTheWebAsync` exibe os comprimentos, o método não precisa retornar nada. Remova a instrução de retorno e altere o tipo de retorno do método para <xref:System.Threading.Tasks.Task> em vez de <xref:System.Threading.Tasks.Task%601>.

    ```csharp
    async Task AccessTheWebAsync(CancellationToken ct)
    ```

     Chame o método de `startButton_Click` usando uma instrução em vez de uma expressão.

    ```csharp
    await AccessTheWebAsync(cts.Token);
    ```

5. Se você não cancelar o programa, ele produzirá a saída a seguir.

    ```text
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

    ```text
    Length of the downloaded string: 35939.

    Length of the downloaded string: 237682.

    Length of the downloaded string: 128607.

    Downloads canceled.
    ```

## <a name="complete-examples"></a>Exemplos completos

As seções a seguir contêm o código para cada um dos exemplos anteriores. Observe que você deve adicionar uma referência para <xref:System.Net.Http>.

Você pode baixar os projetos de [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea).

### <a name="example---cancel-a-task"></a>Exemplo – cancelar uma tarefa

O código a seguir é o arquivo MainWindow.xaml.cs completo do exemplo que cancela uma única tarefa.

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

// Add a using directive and a reference for System.Net.Http.
using System.Net.Http;

// Add the following using directive for System.Threading.

using System.Threading;
namespace CancelATask
{
    public partial class MainWindow : Window
    {
        // ***Declare a System.Threading.CancellationTokenSource.
        CancellationTokenSource cts;

        public MainWindow()
        {
            InitializeComponent();
        }

        private async void startButton_Click(object sender, RoutedEventArgs e)
        {
            // ***Instantiate the CancellationTokenSource.
            cts = new CancellationTokenSource();

            resultsTextBox.Clear();

            try
            {
                // ***Send a token to carry the message if cancellation is requested.
                int contentLength = await AccessTheWebAsync(cts.Token);
                resultsTextBox.Text +=
                    $"\r\nLength of the downloaded string: {contentLength}.\r\n";
            }
            // *** If cancellation is requested, an OperationCanceledException results.
            catch (OperationCanceledException)
            {
                resultsTextBox.Text += "\r\nDownload canceled.\r\n";
            }
            catch (Exception)
            {
                resultsTextBox.Text += "\r\nDownload failed.\r\n";
            }

            // ***Set the CancellationTokenSource to null when the download is complete.
            cts = null;
        }

        // ***Add an event handler for the Cancel button.
        private void cancelButton_Click(object sender, RoutedEventArgs e)
        {
            if (cts != null)
            {
                cts.Cancel();
            }
        }

        // ***Provide a parameter for the CancellationToken.
        async Task<int> AccessTheWebAsync(CancellationToken ct)
        {
            HttpClient client = new HttpClient();

            resultsTextBox.Text += "\r\nReady to download.\r\n";

            // You might need to slow things down to have a chance to cancel.
            await Task.Delay(250);

            // GetAsync returns a Task<HttpResponseMessage>.
            // ***The ct argument carries the message if the Cancel button is chosen.
            HttpResponseMessage response = await client.GetAsync("https://msdn.microsoft.com/library/dd470362.aspx", ct);

            // Retrieve the website contents from the HttpResponseMessage.
            byte[] urlContents = await response.Content.ReadAsByteArrayAsync();

            // The result of the method is the length of the downloaded website.
            return urlContents.Length;
        }
    }

    // Output for a successful download:

    // Ready to download.

    // Length of the downloaded string: 158125.

    // Or, if you cancel:

    // Ready to download.

    // Download canceled.
}
```

### <a name="example---cancel-a-list-of-tasks"></a>Exemplo – cancelar uma lista de tarefas

O código a seguir é o arquivo MainWindow.xaml.cs completo do exemplo que cancela uma lista de tarefas.

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

// Add a using directive and a reference for System.Net.Http.
using System.Net.Http;

// Add the following using directive for System.Threading.
using System.Threading;

namespace CancelAListOfTasks
{
    public partial class MainWindow : Window
    {
        // Declare a System.Threading.CancellationTokenSource.
        CancellationTokenSource cts;

        public MainWindow()
        {
            InitializeComponent();
        }

        private async void startButton_Click(object sender, RoutedEventArgs e)
        {
            // Instantiate the CancellationTokenSource.
            cts = new CancellationTokenSource();

            resultsTextBox.Clear();

            try
            {
                await AccessTheWebAsync(cts.Token);
                // ***Small change in the display lines.
                resultsTextBox.Text += "\r\nDownloads complete.";
            }
            catch (OperationCanceledException)
            {
                resultsTextBox.Text += "\r\nDownloads canceled.";
            }
            catch (Exception)
            {
                resultsTextBox.Text += "\r\nDownloads failed.";
            }

            // Set the CancellationTokenSource to null when the download is complete.
            cts = null;
        }

        // Add an event handler for the Cancel button.
        private void cancelButton_Click(object sender, RoutedEventArgs e)
        {
            if (cts != null)
            {
                cts.Cancel();
            }
        }

        // Provide a parameter for the CancellationToken.
        // ***Change the return type to Task because the method has no return statement.
        async Task AccessTheWebAsync(CancellationToken ct)
        {
            // Declare an HttpClient object.
            HttpClient client = new HttpClient();

            // ***Call SetUpURLList to make a list of web addresses.
            List<string> urlList = SetUpURLList();

            // ***Add a loop to process the list of web addresses.
            foreach (var url in urlList)
            {
                // GetAsync returns a Task<HttpResponseMessage>.
                // Argument ct carries the message if the Cancel button is chosen.
                // ***Note that the Cancel button can cancel all remaining downloads.
                HttpResponseMessage response = await client.GetAsync(url, ct);

                // Retrieve the website contents from the HttpResponseMessage.
                byte[] urlContents = await response.Content.ReadAsByteArrayAsync();

                resultsTextBox.Text +=
                    $"\r\nLength of the downloaded string: {urlContents.Length}.\r\n";
            }
        }

        // ***Add a method that creates a list of web addresses.
        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
            {
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/hh290138.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/dd470362.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            };
            return urls;
        }
    }

    // Output if you do not choose to cancel:

    //Length of the downloaded string: 35939.

    //Length of the downloaded string: 237682.

    //Length of the downloaded string: 128607.

    //Length of the downloaded string: 158124.

    //Length of the downloaded string: 204890.

    //Length of the downloaded string: 175488.

    //Length of the downloaded string: 145790.

    //Downloads complete.

    // Sample output if you choose to cancel:

    //Length of the downloaded string: 35939.

    //Length of the downloaded string: 237682.

    //Length of the downloaded string: 128607.

    //Downloads canceled.
}
```

## <a name="see-also"></a>Veja também

- <xref:System.Threading.CancellationTokenSource>
- <xref:System.Threading.CancellationToken>
- [Programação assíncrona com Async e Await (C#)](./index.md)
- [Ajuste fino de seu aplicativo assíncrono (C#)](./fine-tuning-your-async-application.md)
- [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
