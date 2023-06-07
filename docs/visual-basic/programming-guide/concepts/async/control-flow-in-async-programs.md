---
title: Fluxo de controle em programas assíncronos
ms.date: 07/20/2015
ms.assetid: b0443af7-c586-4cb0-b476-742ae4098a96
ms.openlocfilehash: 0c479b9dd2a691b1b353fac54ee3320a895b1c7f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396656"
---
# <a name="control-flow-in-async-programs-visual-basic"></a>Fluxo de controle em programas assíncronos (Visual Basic)

Você pode escrever e manter programas assíncronos mais facilmente usando as palavras-chave `Async` e `Await`. No entanto, os resultados podem surpreendê-lo se você não entender o funcionamento do seu programa. Este tópico rastreia o fluxo de controle por meio de um programa assíncrono simples para mostrar quando o controle se move de um método para o outro e quais informações são transferidas a cada vez.

> [!NOTE]
> As palavras-chave `Async` e `Await` foram introduzidas no Visual Studio 2012.

Em geral, você marca os métodos que contêm código assíncrono com o modificador [assíncrono](../../../language-reference/modifiers/async.md) . Em um método marcado com um modificador Async, você pode usar um operador [Await (Visual Basic)](../../../language-reference/operators/await-operator.md) para especificar onde o método pausa para aguardar a conclusão de um processo assíncrono chamado. Para obter mais informações, consulte [programação assíncrona com Async e Await (Visual Basic)](index.md).

O exemplo a seguir usa os métodos assíncronos para baixar o conteúdo de um site especificado como uma cadeia de caracteres e exibir o comprimento da cadeia de caracteres. O exemplo contém os dois métodos a seguir.

- `startButton_Click`, que chama `AccessTheWebAsync` e exibe o resultado.

- `AccessTheWebAsync`, que baixa o conteúdo de um site na forma de uma cadeia de caracteres e retorna o comprimento da cadeia de caracteres. `AccessTheWebAsync` usa um método <xref:System.Net.Http.HttpClient> assíncrono, <xref:System.Net.Http.HttpClient.GetStringAsync%28System.String%29>, para baixar o conteúdo.

Linhas numeradas de exibição aparecem em pontos estratégicos em todo o programa para ajudá-lo a entender como o programa é executado e explicar o que acontece em cada ponto marcado. As linhas de exibição são rotuladas como "UM"a "SEIS". Os rótulos representam a ordem na qual o programa alcança essas linhas de código.

O código a seguir mostra uma estrutura de tópicos do programa.

```vb
Class MainWindow

    Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs) Handles StartButton.Click

        ' ONE
        Dim getLengthTask As Task(Of Integer) = AccessTheWebAsync()

        ' FOUR
        Dim contentLength As Integer = Await getLengthTask

        ' SIX
        ResultsTextBox.Text &=
            vbCrLf & $"Length of the downloaded string: {contentLength}." & vbCrLf

    End Sub

    Async Function AccessTheWebAsync() As Task(Of Integer)

        ' TWO
        Dim client As HttpClient = New HttpClient()
        Dim getStringTask As Task(Of String) =
            client.GetStringAsync("https://msdn.microsoft.com")

        ' THREE
        Dim urlContents As String = Await getStringTask

        ' FIVE
        Return urlContents.Length
    End Function

End Class
```

Cada um dos locais rotulados, "UM"a "SEIS," exibe informações sobre o estado atual do programa. A saída a seguir será produzida:

```console
ONE:   Entering startButton_Click.
           Calling AccessTheWebAsync.

TWO:   Entering AccessTheWebAsync.
           Calling HttpClient.GetStringAsync.

THREE: Back in AccessTheWebAsync.
           Task getStringTask is started.
           About to await getStringTask & return a Task<int> to startButton_Click.

FOUR:  Back in startButton_Click.
           Task getLengthTask is started.
           About to await getLengthTask -- no caller to return to.

FIVE:  Back in AccessTheWebAsync.
           Task getStringTask is complete.
           Processing the return statement.
           Exiting from AccessTheWebAsync.

SIX:   Back in startButton_Click.
           Task getLengthTask is finished.
           Result from AccessTheWebAsync is stored in contentLength.
           About to display contentLength and exit.

Length of the downloaded string: 33946.
```

## <a name="set-up-the-program"></a>Configurar o Programa

Você pode baixar o código usado nesse tópico no MSDN ou você mesmo pode criá-lo.

> [!NOTE]
> Para executar o exemplo, você deve ter o Visual Studio 2012 ou mais recente e o .NET Framework 4,5 ou mais recente instalados no seu computador.

### <a name="download-the-program"></a>Baixar o Programa

Você pode baixar o aplicativo deste tópico em [Exemplo assíncrono: controlar fluxo em programas assíncronos](https://code.msdn.microsoft.com/Async-Sample-Control-Flow-5c804fc0). As etapas a seguir abrem e executam o programa.

1. Descompacte o arquivo baixado e, em seguida, inicie o Visual Studio.

2. Na barra de menus, escolha **Arquivo**, **Abrir**, **Projeto/Solução**.

3. Navegue até a pasta que contém o código de exemplo descompactado, abra o arquivo da solução (.sln) e, em seguida, escolha a tecla F5 para compilar e executar o projeto.

### <a name="build-the-program-yourself"></a>Compilar o Programa Sozinho

O projeto WPF (Windows Presentation Foundation) a seguir contém o exemplo de código deste tópico.

Para executar o projeto, realize as seguintes etapas:

1. Inicie o Visual Studio.

2. Na barra de menus, escolha **Arquivo**, **Novo**, **Projeto**.

    A caixa de diálogo **Novo Projeto** será aberta.

3. No painel **modelos instalados** , escolha **Visual Basic**e, em seguida, escolha **aplicativo WPF** na lista de tipos de projeto.

4. Digite `AsyncTracer` como o nome do projeto e, em seguida, escolha o botão **OK**.

    O novo projeto aparece no **Gerenciador de Soluções**.

5. No Editor do Visual Studio Code, escolha a guia **MainWindow.xaml**.

    Se a guia não estiver visível, abra o menu de atalho para MainWindow.xaml no **Gerenciador de Soluções** e, em seguida, escolha **Exibir Código**.

6. Na exibição **XAML** de MainWindow.xaml, substitua o código pelo código a seguir.

    ```vb
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008" xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" mc:Ignorable="d" x:Class="MainWindow"
        Title="Control Flow Trace" Height="350" Width="525">
        <Grid>
            <Button x:Name="StartButton" Content="Start" HorizontalAlignment="Left" Margin="221,10,0,0" VerticalAlignment="Top" Width="75"/>
            <TextBox x:Name="ResultsTextBox" HorizontalAlignment="Left" TextWrapping="Wrap" VerticalAlignment="Bottom" Width="510" Height="265" FontFamily="Lucida Console" FontSize="10" VerticalScrollBarVisibility="Visible" d:LayoutOverrides="HorizontalMargin"/>

        </Grid>
    </Window>
    ```

    Uma janela simples, contendo uma caixa de texto e um botão, aparecerá no modo de exibição de **Design** de MainWindow.xaml.

7. Adicione uma referência para <xref:System.Net.Http>.

8. No **Gerenciador de soluções**, abra o menu de atalho para MainWindow. XAML. vb e escolha **Exibir código**.

9. Em MainWindow. XAML. vb, substitua o código pelo código a seguir.

    ```vb
    ' Add an Imports statement and a reference for System.Net.Http.
    Imports System.Net.Http

    Class MainWindow

        Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs) Handles StartButton.Click

            ' The display lines in the example lead you through the control shifts.
            ResultsTextBox.Text &= "ONE:   Entering StartButton_Click." & vbCrLf &
                "           Calling AccessTheWebAsync." & vbCrLf

            Dim getLengthTask As Task(Of Integer) = AccessTheWebAsync()

            ResultsTextBox.Text &= vbCrLf & "FOUR:  Back in StartButton_Click." & vbCrLf &
                "           Task getLengthTask is started." & vbCrLf &
                "           About to await getLengthTask -- no caller to return to." & vbCrLf

            Dim contentLength As Integer = Await getLengthTask

            ResultsTextBox.Text &= vbCrLf & "SIX:   Back in StartButton_Click." & vbCrLf &
                "           Task getLengthTask is finished." & vbCrLf &
                "           Result from AccessTheWebAsync is stored in contentLength." & vbCrLf &
                "           About to display contentLength and exit." & vbCrLf

            ResultsTextBox.Text &=
                String.Format(vbCrLf & "Length of the downloaded string: {0}." & vbCrLf, contentLength)
        End Sub

        Async Function AccessTheWebAsync() As Task(Of Integer)

            ResultsTextBox.Text &= vbCrLf & "TWO:   Entering AccessTheWebAsync."

            ' Declare an HttpClient object.
            Dim client As HttpClient = New HttpClient()

            ResultsTextBox.Text &= vbCrLf & "           Calling HttpClient.GetStringAsync." & vbCrLf

            ' GetStringAsync returns a Task(Of String).
            Dim getStringTask As Task(Of String) = client.GetStringAsync("https://msdn.microsoft.com")

            ResultsTextBox.Text &= vbCrLf & "THREE: Back in AccessTheWebAsync." & vbCrLf &
                "           Task getStringTask is started."

            ' AccessTheWebAsync can continue to work until getStringTask is awaited.

            ResultsTextBox.Text &=
                vbCrLf & "           About to await getStringTask & return a Task(Of Integer) to StartButton_Click." & vbCrLf

            ' Retrieve the website contents when task is complete.
            Dim urlContents As String = Await getStringTask

            ResultsTextBox.Text &= vbCrLf & "FIVE:  Back in AccessTheWebAsync." &
                vbCrLf & "           Task getStringTask is complete." &
                vbCrLf & "           Processing the return statement." &
                vbCrLf & "           Exiting from AccessTheWebAsync." & vbCrLf

            Return urlContents.Length
        End Function

    End Class
    ```

10. Escolha a tecla F5 para executar o programa e, em seguida, o botão **Iniciar**.

    A saída a seguir deve aparecer:

    ```console
    ONE:   Entering startButton_Click.
               Calling AccessTheWebAsync.

    TWO:   Entering AccessTheWebAsync.
               Calling HttpClient.GetStringAsync.

    THREE: Back in AccessTheWebAsync.
               Task getStringTask is started.
               About to await getStringTask & return a Task<int> to startButton_Click.

    FOUR:  Back in startButton_Click.
               Task getLengthTask is started.
               About to await getLengthTask -- no caller to return to.

    FIVE:  Back in AccessTheWebAsync.
               Task getStringTask is complete.
               Processing the return statement.
               Exiting from AccessTheWebAsync.

    SIX:   Back in startButton_Click.
               Task getLengthTask is finished.
               Result from AccessTheWebAsync is stored in contentLength.
               About to display contentLength and exit.

    Length of the downloaded string: 33946.
    ```

## <a name="trace-the-program"></a>Rastrear o Programa

### <a name="steps-one-and-two"></a>Etapas UM e DOIS

As duas primeiras linhas de exibição rastreiam o caminho conforme `startButton_Click` chama `AccessTheWebAsync` e `AccessTheWebAsync` chama o método <xref:System.Net.Http.HttpClient> assíncrono <xref:System.Net.Http.HttpClient.GetStringAsync%28System.String%29>. A imagem a seguir delineia as chamadas de método a método.

![Etapas UM e DOIS](../../../../csharp/programming-guide/concepts/async/media/asynctrace-onetwo.png "AsyncTrace-ONETWO")

O tipo de retorno de ambos `AccessTheWebAsync` e `client.GetStringAsync` é <xref:System.Threading.Tasks.Task%601>. Para `AccessTheWebAsync`, TResult é um inteiro. Para `GetStringAsync`, TResult é uma cadeia de caracteres. Para obter mais informações sobre tipos de retorno de método assíncrono, consulte [tipos de retorno assíncrono (Visual Basic)](async-return-types.md).

Um método assíncrono de retorno de tarefas retorna uma instância de tarefa, quando o controle volta para o chamador. O controle retorna de um método assíncrono para seu chamador quando um operador `Await` é encontrado no método chamado ou quando o método chamado termina. As linhas de exibição rotuladas como "TRÊS"até "SEIS" rastreiam essa parte do processo.

### <a name="step-three"></a>Etapa TRÊS

Em `AccessTheWebAsync`, o método assíncrono <xref:System.Net.Http.HttpClient.GetStringAsync%28System.String%29> é chamado para baixar o conteúdo de página da Web de destino. O controle retorna de `client.GetStringAsync` para `AccessTheWebAsync` quando `client.GetStringAsync` retornar.

O método `client.GetStringAsync` retorna uma tarefa de cadeia de caracteres que é atribuída à variável `getStringTask` em `AccessTheWebAsync`. A linha do programa de exemplo a seguir mostra a chamada à `client.GetStringAsync` e a atribuição.

```vb
Dim getStringTask As Task(Of String) = client.GetStringAsync("https://msdn.microsoft.com")
```

Você pode imaginar a tarefa como uma promessa de `client.GetStringAsync` para eventualmente produzir uma cadeia de caracteres real. Enquanto isso, se `AccessTheWebAsync` tiver trabalho a fazer, que não dependa da cadeia de caracteres prometida de `client.GetStringAsync`, o trabalho poderá continuar enquanto `client.GetStringAsync` aguarda. No exemplo, as linhas de saída seguintes, que são rotuladas como "TRÊS", representam a oportunidade para fazer o trabalho independente

```console
THREE: Back in AccessTheWebAsync.
           Task getStringTask is started.
           About to await getStringTask & return a Task<int> to startButton_Click.
```

 A instrução a seguir suspende o andamento em `AccessTheWebAsync` quando `getStringTask` é aguardado.

```vb
Dim urlContents As String = Await getStringTask
```

A imagem a seguir mostra o fluxo de controle de `client.GetStringAsync` para a atribuição de e para a `getStringTask` criação de `getStringTask` para o aplicativo de um operador Await.

![Etapa três](../../../../csharp/programming-guide/concepts/async/media/asynctrace-three.png "AsyncTrace-três")

A expressão await suspende `AccessTheWebAsync` até que `client.GetStringAsync` retorne. Enquanto isso, o controle retorna para o chamador de `AccessTheWebAsync`, `startButton_Click`.

> [!NOTE]
> Normalmente, você aguarda a chamada a um método assíncrono imediatamente. Por exemplo, a atribuição a seguir poderia substituir o código anterior que cria e, em seguida, aguarda `getStringTask`: `Dim urlContents As String = Await client.GetStringAsync("https://msdn.microsoft.com")`
>
> Neste tópico, o operador await é aplicado posteriormente para acomodar as linhas de saída que marcam o fluxo de controle em todo o programa.

### <a name="step-four"></a>Etapa QUATRO

O tipo de retorno declarado de `AccessTheWebAsync` é `Task(Of Integer)`. Portanto, quando `AccessTheWebAsync` for suspenso, ele retornará uma tarefa de inteiro para `startButton_Click`. Você deve compreender que a tarefa retornada não é `getStringTask`. A tarefa retornada é uma nova tarefa de um inteiro que representa o que ainda precisa ser feito no método suspenso `AccessTheWebAsync`. A tarefa é uma promessa de `AccessTheWebAsync` para produzir um inteiro quando a tarefa for concluída.

A instrução a seguir atribui essa tarefa à variável `getLengthTask`.

```vb
Dim getLengthTask As Task(Of Integer) = AccessTheWebAsync()
```

Como em `AccessTheWebAsync`, `startButton_Click` pode continuar com o trabalho que não dependa dos resultados da tarefa assíncrona (`getLengthTask`) até que a tarefa seja aguardada. As seguintes linhas de saída representam o trabalho:

```console
FOUR:  Back in startButton_Click.
           Task getLengthTask is started.
           About to await getLengthTask -- no caller to return to.
```

O avanço em `startButton_Click` será suspenso quando `getLengthTask` for aguardada. A seguinte instrução de atribuição suspende `startButton_Click` até que `AccessTheWebAsync` seja concluída.

```vb
Dim contentLength As Integer = Await getLengthTask
```

Na ilustração a seguir, as setas mostram o fluxo de controle da expressão await em `AccessTheWebAsync` para a atribuição de um valor para `getLengthTask`, seguido pelo processamento normal de `startButton_Click` até que `getLengthTask` seja aguardada.

![Etapa quatro](../../../../csharp/programming-guide/concepts/async/media/asynctrace-four.png "AsyncTrace-quatro")

### <a name="step-five"></a>Etapa CINCO

Quando `client.GetStringAsync` sinaliza que está concluído, o processamento em `AccessTheWebAsync` é liberado da suspensão e pode continuar após a instrução await. As seguintes linhas de saída representam a retomada do processamento:

```console
FIVE:  Back in AccessTheWebAsync.
           Task getStringTask is complete.
           Processing the return statement.
           Exiting from AccessTheWebAsync.
```

O operando da instrução return, `urlContents.Length`, é armazenado na tarefa que `AccessTheWebAsync` retorna. A expressão await recupera esse valor de `getLengthTask` em `startButton_Click`.

A imagem a seguir mostra a transferência de controle após `client.GetStringAsync` (e `getStringTask`) estarem concluídas.

![Etapa CINCO](../../../../csharp/programming-guide/concepts/async/media/asynctrace-five.png "AsyncTrace-cinco")

`AccessTheWebAsync` é executado até a conclusão e o controle retorna ao `startButton_Click`, que está aguardando a conclusão.

### <a name="step-six"></a>Etapa SEIS

Quando `AccessTheWebAsync` sinaliza que está concluído, o processamento pode continuar após a instrução await em `startButton_Async`. Na verdade, o programa não tem mais nada para fazer.

As seguintes linhas de saída representam a retomada do processamento em `startButton_Async`:

```console
SIX:   Back in startButton_Click.
           Task getLengthTask is finished.
           Result from AccessTheWebAsync is stored in contentLength.
           About to display contentLength and exit.
```

A expressão await recupera de `getLengthTask` o valor inteiro, que é o operando da instrução return em `AccessTheWebAsync`. A instrução a seguir atribui esse valor à variável `contentLength`.

```vb
Dim contentLength As Integer = Await getLengthTask
```

A imagem a seguir mostra o retorno do controle de `AccessTheWebAsync` para `startButton_Click`.

![Etapa SEIS](../../../../csharp/programming-guide/concepts/async/media/asynctrace-six.png "AsyncTrace-seis")

## <a name="see-also"></a>Confira também

- [Programação assíncrona com Async e Await (Visual Basic)](index.md)
- [Tipos de retorno assíncronos (Visual Basic)](async-return-types.md)
- [Instruções passo a passo: acessando a Web usando Async e Await (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Exemplo de assíncrono: fluxo de controle em programas assíncronos (C# e Visual Basic)](https://code.msdn.microsoft.com/Async-Sample-Control-Flow-5c804fc0)
