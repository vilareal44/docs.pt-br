---
title: Fazer chamadas thread-safe para controles
ms.date: 02/19/2019
description: Saiba como implementar multithreading em seu aplicativo chamando controles de thread cruzado de forma segura para thread.
dev_langs:
- csharp
- vb
f1_keywords:
- EHInvalidOperation.WinForms.IllegalCrossThreadCall
helpviewer_keywords:
- thread safety [Windows Forms], calling controls [Windows Forms]
- calling controls [Windows Forms], thread safety [Windows Forms]
- CheckForIllegalCrossThreadCalls property [Windows Forms]
- Windows Forms controls [Windows Forms], multithreading
- BackgroundWorker class [Windows Forms], examples
- threading [Windows Forms], cross-thread calls
- controls [Windows Forms], multithreading
ms.assetid: 138f38b6-1099-4fd5-910c-390b41cbad35
ms.openlocfilehash: b5f4de7bd3d8d89de98dbe27e2dbf360763670d0
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84904176"
---
# <a name="how-to-make-thread-safe-calls-to-windows-forms-controls"></a>Como: fazer chamadas de thread-safe para Windows Forms controles

O multithreading pode melhorar o desempenho de aplicativos Windows Forms, mas o acesso aos controles Windows Forms não é inerentemente seguro ao thread. O multithreading pode expor seu código a bugs muito graves e complexos. Dois ou mais threads manipulando um controle podem forçar o controle em um estado inconsistente e levar a condições de corrida, deadlocks e congelamento ou travamentos. Se você implementar multithreading em seu aplicativo, certifique-se de chamar controles entre threads de forma segura para thread. Para obter mais informações, consulte [práticas recomendadas de Threading gerenciadas](../../../standard/threading/managed-threading-best-practices.md).

Há duas maneiras de chamar com segurança um controle de Windows Forms de um thread que não criou esse controle. Você pode usar o <xref:System.Windows.Forms.Control.Invoke%2A?displayProperty=fullName> método para chamar um delegado criado no thread principal que, por sua vez, chama o controle. Ou, você pode implementar um <xref:System.ComponentModel.BackgroundWorker?displayProperty=nameWithType> , que usa um modelo controlado por eventos para separar o trabalho feito no thread em segundo plano de relatórios sobre os resultados.

## <a name="unsafe-cross-thread-calls"></a>Chamadas de thread cruzado não seguras

Não é seguro chamar um controle diretamente de um thread que não o criou. O trecho de código a seguir ilustra uma chamada não segura para o <xref:System.Windows.Forms.TextBox?displayProperty=nameWithType> controle. O `Button1_Click` manipulador de eventos cria um novo `WriteTextUnsafe` thread, que define a propriedade do thread principal <xref:System.Windows.Forms.TextBox.Text%2A?displayProperty=nameWithType> diretamente.

```csharp
private void Button1_Click(object sender, EventArgs e)
{
    thread2 = new Thread(new ThreadStart(WriteTextUnsafe));
    thread2.Start();
}
private void WriteTextUnsafe()
{
    textBox1.Text = "This text was set unsafely.";
}
```

```vb
Private Sub Button1_Click(ByVal sender As Object, e As EventArgs) Handles Button1.Click
    Thread2 = New Thread(New ThreadStart(AddressOf WriteTextUnsafe))
    Thread2.Start()
End Sub

Private Sub WriteTextUnsafe()
    TextBox1.Text = "This text was set unsafely."
End Sub
```

O depurador do Visual Studio detecta essas chamadas de thread não seguras gerando uma <xref:System.InvalidOperationException> com a mensagem, **operação entre threads não válida. Controle "" acessado de um thread que não seja o thread em que foi criado.** O <xref:System.InvalidOperationException> sempre ocorre para chamadas de thread cruzado não seguras durante a depuração do Visual Studio e pode ocorrer no tempo de execução do aplicativo. Você deve corrigir o problema, mas pode desabilitar a exceção definindo a <xref:System.Windows.Forms.Control.CheckForIllegalCrossThreadCalls%2A?displayProperty=nameWithType> propriedade como `false` .

## <a name="safe-cross-thread-calls"></a>Chamadas de thread cruzado seguras

Os exemplos de código a seguir demonstram duas maneiras de chamar com segurança um controle de Windows Forms de um thread que não o criou:

1. O <xref:System.Windows.Forms.Control.Invoke%2A?displayProperty=fullName> método, que chama um delegado do thread principal para chamar o controle.
2. Um <xref:System.ComponentModel.BackgroundWorker?displayProperty=nameWithType> componente, que oferece um modelo controlado por eventos.

Em ambos os exemplos, o thread em segundo plano é suspenso por um segundo para simular o trabalho que está sendo feito nesse thread.

Você pode criar e executar esses exemplos como .NET Framework aplicativos da linha de comando C# ou Visual Basic. Para obter mais informações, consulte [criação de linha de comando com csc.exe](../../../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md) ou [Build na linha de comando (Visual Basic)](../../../visual-basic/reference/command-line-compiler/building-from-the-command-line.md).

A partir do .NET Core 3,0, você também pode criar e executar os exemplos como aplicativos do Windows .NET Core de uma pasta que tem um arquivo de projeto Windows Forms * \<folder name> . csproj* do .NET Core.

## <a name="example-use-the-invoke-method-with-a-delegate"></a>Exemplo: usar o método Invoke com um delegate

O exemplo a seguir demonstra um padrão para garantir chamadas de thread-safe para um controle Windows Forms. Ele consulta a <xref:System.Windows.Forms.Control.InvokeRequired%2A?displayProperty=fullName> propriedade, que compara a ID de thread de criação do controle com a ID de thread de chamada. Se as IDs de thread forem as mesmas, ele chamará o controle diretamente. Se as IDs de thread forem diferentes, ele chamará o <xref:System.Windows.Forms.Control.Invoke%2A?displayProperty=nameWithType> método com um delegado do thread principal, que faz a chamada real para o controle.

O `SafeCallDelegate` habilita a definição da <xref:System.Windows.Forms.TextBox> Propriedade do controle <xref:System.Windows.Forms.TextBox.Text%2A> . O `WriteTextSafe` método consulta <xref:System.Windows.Forms.Control.InvokeRequired%2A> . Se <xref:System.Windows.Forms.Control.InvokeRequired%2A> retorna `true` , `WriteTextSafe` passa o `SafeCallDelegate` para o <xref:System.Windows.Forms.Control.Invoke%2A> método para fazer a chamada real para o controle. Se <xref:System.Windows.Forms.Control.InvokeRequired%2A> retorna `false` , `WriteTextSafe` define o <xref:System.Windows.Forms.TextBox.Text%2A?displayProperty=nameWithType> diretamente. O `Button1_Click` manipulador de eventos cria o novo thread e executa o `WriteTextSafe` método.

 [!code-csharp[ThreadSafeCalls#1](~/samples/snippets/winforms/thread-safe/example1/cs/Form1.cs)]
 [!code-vb[ThreadSafeCalls#1](~/samples/snippets/winforms/thread-safe/example1/vb/Form1.vb)]  

## <a name="example-use-a-backgroundworker-event-handler"></a>Exemplo: usar um manipulador de eventos de BackgroundWorker

Uma maneira fácil de implementar multithreading é com o <xref:System.ComponentModel.BackgroundWorker?displayProperty=nameWithType> componente, que usa um modelo controlado por eventos. O thread em segundo plano executa o <xref:System.ComponentModel.BackgroundWorker.DoWork?displayProperty=nameWithType> evento, que não interage com o thread principal. O thread principal executa os <xref:System.ComponentModel.BackgroundWorker.ProgressChanged?displayProperty=nameWithType> <xref:System.ComponentModel.BackgroundWorker.RunWorkerCompleted?displayProperty=nameWithType> manipuladores de eventos e, que podem chamar os controles do thread principal.

Para fazer uma chamada thread-safe usando <xref:System.ComponentModel.BackgroundWorker> , crie um método no thread em segundo plano para fazer o trabalho e associá-lo ao <xref:System.ComponentModel.BackgroundWorker.DoWork> evento. Crie outro método no thread principal para relatar os resultados do trabalho em segundo plano e associá-los ao <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> evento ou <xref:System.ComponentModel.BackgroundWorker.RunWorkerCompleted> . Para iniciar o thread em segundo plano, chame <xref:System.ComponentModel.BackgroundWorker.RunWorkerAsync%2A?displayProperty=nameWithType> .

O exemplo usa o <xref:System.ComponentModel.BackgroundWorker.RunWorkerCompleted> manipulador de eventos para definir a <xref:System.Windows.Forms.TextBox> Propriedade do controle <xref:System.Windows.Forms.TextBox.Text%2A> . Para obter um exemplo usando o <xref:System.ComponentModel.BackgroundWorker.ProgressChanged> evento, consulte <xref:System.ComponentModel.BackgroundWorker> .

 [!code-csharp[ThreadSafeCalls#2](~/samples/snippets/winforms/thread-safe/example2/cs/Form1.cs)]
 [!code-vb[ThreadSafeCalls#2](~/samples/snippets/winforms/thread-safe/example2/vb/Form1.vb)]  

## <a name="see-also"></a>Veja também

- <xref:System.ComponentModel.BackgroundWorker>
- [Como executar uma operação em segundo plano](how-to-run-an-operation-in-the-background.md)
- [Como: implementar um formulário que usa uma operação em segundo plano](how-to-implement-a-form-that-uses-a-background-operation.md)
- [Desenvolva controles de Windows Forms personalizados com o .NET Framework](developing-custom-windows-forms-controls.md)
