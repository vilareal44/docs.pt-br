---
title: Cancelar tarefas assíncronas após um período (C#)
description: Use o método CancellationTokenSource. CancelAfter em C# para agendar o cancelamento de quaisquer tarefas associadas não concluídas dentro de um período neste exemplo.
ms.date: 07/20/2015
ms.assetid: 194282c2-399f-46da-a7a6-96674e00b0b3
ms.openlocfilehash: f32af1d893c60ac17648f60fa3aa90adaa0383e8
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925282"
---
# <a name="cancel-async-tasks-after-a-period-of-time-c"></a>Cancelar tarefas assíncronas após um período (C#)

Você pode cancelar uma operação assíncrona após um período de tempo usando o método <xref:System.Threading.CancellationTokenSource.CancelAfter%2A?displayProperty=nameWithType> se você não deseja aguardar a conclusão da operação. Esse método agenda o cancelamento de quaisquer tarefas associadas que não são concluídas dentro do período designado pela expressão `CancelAfter`.

Este exemplo adiciona o código desenvolvido em [Cancelar uma tarefa assíncrona ou uma lista de tarefas (C#)](./cancel-an-async-task-or-a-list-of-tasks.md) para baixar uma lista de sites e para exibir o tamanho dos conteúdos de cada um.

> [!NOTE]
> Para executar os exemplos, você precisa ter o Visual Studio 2012 ou uma versão mais recente e o .NET Framework 4.5 ou posterior instalados em seu computador.

## <a name="download-the-example"></a>Baixar o exemplo

Você pode baixar o projeto completo do WPF (Windows Presentation Foundation) em [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea) e, em seguida, seguir estas etapas.

1. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.

2. Na barra de menus, escolha **arquivo**  >  **abrir**  >  **projeto/solução**.

3. Na caixa de diálogo **Abrir Projeto**, abra a pasta em que está o código de exemplo que você descompactou e, em seguida, abra o arquivo de solução (.sln) de AsyncFineTuningCS.

4. No **Gerenciador de Soluções**, abra o menu de atalho do projeto **CancelAfterTime** e, em seguida, escolha **Definir como Projeto de Inicialização**.

5. Pressione a tecla **F5** para executar o projeto. (Ou pressione **Ctrl** + **F5** para executar o projeto sem depurá-lo).

6. Execute o programa várias vezes para verificar que a saída pode mostrar a saída para todos os sites, nenhum site ou alguns sites.

Se você não quiser baixar o projeto, você poderá examinar o arquivo MainWindow.xaml.cs no final deste tópico.

## <a name="build-the-example"></a>Criar o exemplo

O exemplo neste tópico adiciona ao projeto que é desenvolvido em [Cancelar uma tarefa assíncrona ou uma lista de tarefas (C#)](./cancel-an-async-task-or-a-list-of-tasks.md) para cancelar uma lista de tarefas. O exemplo usa a mesma interface do usuário, embora o botão **Cancelar** não seja explicitamente usado.

Para compilar o exemplo você mesmo, passo a passo, siga as instruções na seção "Baixando o exemplo", mas escolha **CancelAListOfTasks** como o **Projeto de Inicialização**. Adicione as alterações deste tópico ao projeto.

Para especificar um tempo máximo antes que as tarefas sejam marcadas como canceladas, adicione uma chamada para `CancelAfter` a `startButton_Click`, como o exemplo a seguir mostra. A adição é marcada com asteriscos.

```csharp
private async void startButton_Click(object sender, RoutedEventArgs e)
{
    // Instantiate the CancellationTokenSource.
    cts = new CancellationTokenSource();

    resultsTextBox.Clear();

    try
    {
        // ***Set up the CancellationTokenSource to cancel after 2.5 seconds. (You
        // can adjust the time.)
        cts.CancelAfter(2500);

        await AccessTheWebAsync(cts.Token);
        resultsTextBox.Text += "\r\nDownloads succeeded.\r\n";
    }
    catch (OperationCanceledException)
    {
        resultsTextBox.Text += "\r\nDownloads canceled.\r\n";
    }
    catch (Exception)
    {
        resultsTextBox.Text += "\r\nDownloads failed.\r\n";
    }

    cts = null;
}
```

 Execute o programa várias vezes para verificar que a saída pode mostrar a saída para todos os sites, nenhum site ou alguns sites. A saída a seguir é um exemplo.

```output
Length of the downloaded string: 35990.

Length of the downloaded string: 407399.

Length of the downloaded string: 226091.

Downloads canceled.
```

## <a name="complete-example"></a>Exemplo completo

O código a seguir é o texto completo do arquivo MainWindow.xaml.cs para o exemplo. Os asteriscos marcam os elementos que foram adicionados para esse exemplo.

Observe que você deve adicionar uma referência para <xref:System.Net.Http>.

Você pode baixar o projeto de [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea).

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

// Add the following using directive.
using System.Threading;

namespace CancelAfterTime
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
                // ***Set up the CancellationTokenSource to cancel after 2.5 seconds. (You
                // can adjust the time.)
                cts.CancelAfter(2500);

                await AccessTheWebAsync(cts.Token);
                resultsTextBox.Text += "\r\nDownloads succeeded.\r\n";
            }
            catch (OperationCanceledException)
            {
                resultsTextBox.Text += "\r\nDownloads canceled.\r\n";
            }
            catch (Exception)
            {
                resultsTextBox.Text += "\r\nDownloads failed.\r\n";
            }

            cts = null;
        }

        // You can still include a Cancel button if you want to.
        private void cancelButton_Click(object sender, RoutedEventArgs e)
        {
            if (cts != null)
            {
                cts.Cancel();
            }
        }

        async Task AccessTheWebAsync(CancellationToken ct)
        {
            // Declare an HttpClient object.
            HttpClient client = new HttpClient();

            // Make a list of web addresses.
            List<string> urlList = SetUpURLList();

            foreach (var url in urlList)
            {
                // GetAsync returns a Task<HttpResponseMessage>.
                // Argument ct carries the message if the Cancel button is chosen.
                // Note that the Cancel button cancels all remaining downloads.
                HttpResponseMessage response = await client.GetAsync(url, ct);

                // Retrieve the website contents from the HttpResponseMessage.
                byte[] urlContents = await response.Content.ReadAsByteArrayAsync();

                resultsTextBox.Text +=
                    $"\r\nLength of the downloaded string: {urlContents.Length}.\r\n";
            }
        }

        private List<string> SetUpURLList()
        {
            List<string> urls = new List<string>
            {
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/windows/apps/br211380.aspx",
                "https://msdn.microsoft.com/library/hh290136.aspx",
                "https://msdn.microsoft.com/library/ee256749.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            };
            return urls;
        }
    }

    // Sample Output:

    // Length of the downloaded string: 35990.

    // Length of the downloaded string: 407399.

    // Length of the downloaded string: 226091.

    // Downloads canceled.
}
```

## <a name="see-also"></a>Veja também

- [Programação assíncrona com Async e Await (C#)](./index.md)
- [Passo a passo: acessando a Web usando async e await (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Cancelar uma tarefa assíncrona ou uma lista de tarefas (C#)](./cancel-an-async-task-or-a-list-of-tasks.md)
- [Ajuste fino de seu aplicativo assíncrono (C#)](./fine-tuning-your-async-application.md)
- [Exemplo assíncrono: ajuste fino de seu aplicativo](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
