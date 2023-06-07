---
title: Usando o Async para acessar arquivos
ms.date: 07/20/2015
ms.assetid: c989305f-08e3-4687-95c3-948465cda202
ms.openlocfilehash: 2ee1efa69f4b13224be65fe802ebf5f834c941aa
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400765"
---
# <a name="using-async-for-file-access-visual-basic"></a>Usando o Async para acessar arquivos (Visual Basic)
Você pode usar o recurso assíncrono para acessar arquivos. Usando o recurso Async, você pode chamar os métodos assíncronos sem usar retornos de chamada ou dividir seu código em vários métodos ou expressões lambda. Para tornar síncrono um código assíncrono, basta chamar um método assíncrono em vez de um método síncrono e adicionar algumas palavras-chave ao código.  
  
 Você pode considerar seguintes motivos para adicionar a assincronia às chamadas de acesso ao arquivo:  
  
- A assincronia torna os aplicativos de interface do usuário mais responsivos porque o thread de interface do usuário que inicia a operação pode executar outro trabalho. Se o thread de interface do usuário precisar executar o código que leva muito tempo (por exemplo, mais de 50 milissegundos), a interface do usuário poderá congelar até que a E/S seja concluída e o thread da interface do usuário possa processar entradas do mouse e do teclado e outros eventos.  
  
- A assincronia melhora a escalabilidade do ASP.NET e outros aplicativos baseados em servidor reduzindo a necessidade de threads. Se o aplicativo usar um thread dedicado por resposta e mil solicitações forem tratadas simultaneamente, serão necessários mil threads. As operações assíncronas normalmente não precisam usar um thread durante a espera. Elas podem usar o thread de conclusão de E/S existente rapidamente no final.  
  
- A latência de uma operação de acesso de arquivo pode ser muito baixa nas condições atuais, mas a latência pode aumentar consideravelmente no futuro. Por exemplo, um arquivo pode ser movido para um servidor que está do outro lado do mundo.  
  
- A sobrecarga adicional de usar o recurso async é pequena.  
  
- As tarefas assíncronas podem facilmente ser executadas em paralelo.  
  
## <a name="running-the-examples"></a>Executando os exemplos  
 Para executar os exemplos neste tópico, você pode criar um **Aplicativo WPF** ou um **Aplicativo do Windows Forms** e, em seguida, adicionar um **Botão**. No evento `Click` do botão, adicione uma chamada para o primeiro método em cada exemplo.  
  
 Nos exemplos a seguir, inclua as seguintes instruções `Imports`.  
  
```vb  
Imports System  
Imports System.Collections.Generic  
Imports System.Diagnostics  
Imports System.IO  
Imports System.Text  
Imports System.Threading.Tasks  
```  
  
## <a name="use-of-the-filestream-class"></a>Uso da classe FileStream  
 Os exemplos neste tópico usam a classe <xref:System.IO.FileStream>, que tem uma opção que faz com que a E/S assíncrona ocorra no nível do sistema operacional. Usando essa opção, você pode evitar o bloqueio de um thread de ThreadPool em muitos casos. Para habilitar essa opção, você deve especificar o argumento `useAsync=true` ou `options=FileOptions.Asynchronous` na chamada do construtor.  
  
 Você não pode usar essa opção com <xref:System.IO.StreamReader> e <xref:System.IO.StreamWriter> se você os abrir diretamente especificando um caminho de arquivo. No entanto, você poderá usar essa opção se fornecer um <xref:System.IO.Stream> que a classe <xref:System.IO.FileStream> abriu. Observe que as chamadas assíncronas serão mais rápidas em aplicativos de interface do usuário mesmo se um thread de ThreadPool estiver bloqueado, porque o thread de interface do usuário não é bloqueado durante a espera.  
  
## <a name="writing-text"></a>Gravando texto  
 O exemplo a seguir grava um texto em um arquivo. A cada instrução await, o método sai imediatamente. Quando o arquivo de E/S for concluído, o método continuará na instrução após a instrução await. Observe que o modificador async é a definição de métodos que usam a instrução await.  
  
```vb  
Public Async Sub ProcessWrite()  
    Dim filePath = "temp2.txt"  
    Dim text = "Hello World" & ControlChars.CrLf  
  
    Await WriteTextAsync(filePath, text)  
End Sub  
  
Private Async Function WriteTextAsync(filePath As String, text As String) As Task  
    Dim encodedText As Byte() = Encoding.Unicode.GetBytes(text)  
  
    Using sourceStream As New FileStream(filePath,  
        FileMode.Append, FileAccess.Write, FileShare.None,  
        bufferSize:=4096, useAsync:=True)  
  
        Await sourceStream.WriteAsync(encodedText, 0, encodedText.Length)  
    End Using  
End Function  
```  
  
 O exemplo original tem a instrução `Await sourceStream.WriteAsync(encodedText, 0, encodedText.Length)`, que é uma contração das duas instruções a seguir:  
  
```vb  
Dim theTask As Task = sourceStream.WriteAsync(encodedText, 0, encodedText.Length)  
Await theTask  
```  
  
 A primeira instrução retorna uma tarefa e faz com que o processamento do arquivo seja iniciado. A segunda instrução com o await faz com que o método saia imediatamente e retorne uma tarefa diferente. Quando o processamento do arquivo é concluído posteriormente, a execução retorna para a instrução após a await. Para obter mais informações, consulte [fluxo de controle em programas assíncronos (Visual Basic)](control-flow-in-async-programs.md).  
  
## <a name="reading-text"></a>Lendo texto  
 O exemplo a seguir lê o texto de um arquivo. O texto é armazenado em buffer e, nesse caso, colocado em um <xref:System.Text.StringBuilder>. Diferentemente do exemplo anterior, a avaliação de await produz um valor. O método <xref:System.IO.Stream.ReadAsync%2A> retorna um <xref:System.Threading.Tasks.Task>\<<xref:System.Int32>>, portanto, a avaliação do await produz um valor `Int32` (`numRead`) após a conclusão da operação. Para obter mais informações, consulte [tipos de retorno assíncronos (Visual Basic)](async-return-types.md).  
  
```vb  
Public Async Sub ProcessRead()  
    Dim filePath = "temp2.txt"  
  
    If File.Exists(filePath) = False Then  
        Debug.WriteLine("file not found: " & filePath)  
    Else  
        Try  
            Dim text As String = Await ReadTextAsync(filePath)  
            Debug.WriteLine(text)  
        Catch ex As Exception  
            Debug.WriteLine(ex.Message)  
        End Try  
    End If  
End Sub  
  
Private Async Function ReadTextAsync(filePath As String) As Task(Of String)  
  
    Using sourceStream As New FileStream(filePath,  
        FileMode.Open, FileAccess.Read, FileShare.Read,  
        bufferSize:=4096, useAsync:=True)  
  
        Dim sb As New StringBuilder  
  
        Dim buffer As Byte() = New Byte(&H1000) {}  
        Dim numRead As Integer  
        numRead = Await sourceStream.ReadAsync(buffer, 0, buffer.Length)  
        While numRead <> 0  
            Dim text As String = Encoding.Unicode.GetString(buffer, 0, numRead)  
            sb.Append(text)  
  
            numRead = Await sourceStream.ReadAsync(buffer, 0, buffer.Length)  
        End While  
  
        Return sb.ToString  
    End Using  
End Function  
```  
  
## <a name="parallel-asynchronous-io"></a>E/S assíncrona paralela  
 O exemplo a seguir demonstra o processamento paralelo escrevendo dez arquivos de texto. Para cada arquivo, o método <xref:System.IO.Stream.WriteAsync%2A> retorna uma tarefa que é então adicionada a uma lista de tarefas. A instrução `Await Task.WhenAll(tasks)` sai do método e retoma no método quando o processamento do arquivo é concluído para todas as tarefas.  
  
 O exemplo fecha todas as instâncias de <xref:System.IO.FileStream> em um bloco `Finally` após as tarefas serem concluídas. Se cada `FileStream` foi criado em uma instrução `Imports`, o `FileStream` pode ter sido descartado antes de a tarefa ter sido concluída.  
  
 Observe que qualquer aumento de desempenho é quase que totalmente do processamento paralelo e não o processamento assíncrono. As vantagens da assincronia são que ela não bloqueia vários threads e que ela não bloqueia o thread da interface do usuário.  
  
```vb  
Public Async Sub ProcessWriteMult()  
    Dim folder = "tempfolder\"  
    Dim tasks As New List(Of Task)  
    Dim sourceStreams As New List(Of FileStream)  
  
    Try  
        For index = 1 To 10  
            Dim text = "In file " & index.ToString & ControlChars.CrLf  
  
            Dim fileName = "thefile" & index.ToString("00") & ".txt"  
            Dim filePath = folder & fileName  
  
            Dim encodedText As Byte() = Encoding.Unicode.GetBytes(text)  
  
            Dim sourceStream As New FileStream(filePath,  
                FileMode.Append, FileAccess.Write, FileShare.None,  
                bufferSize:=4096, useAsync:=True)  
  
            Dim theTask As Task = sourceStream.WriteAsync(encodedText, 0, encodedText.Length)  
            sourceStreams.Add(sourceStream)  
  
            tasks.Add(theTask)  
        Next  
  
        Await Task.WhenAll(tasks)  
    Finally  
        For Each sourceStream As FileStream In sourceStreams  
            sourceStream.Close()  
        Next  
    End Try  
End Sub  
```  
  
 Ao usar os métodos <xref:System.IO.Stream.WriteAsync%2A> e <xref:System.IO.Stream.ReadAsync%2A>, você pode especificar um <xref:System.Threading.CancellationToken>, que pode ser usado para cancelar o fluxo intermediário da operação. Para obter mais informações, consulte ajustar [seu aplicativo assíncrono (Visual Basic)](fine-tuning-your-async-application.md) e o [cancelamento em threads gerenciados](../../../../standard/threading/cancellation-in-managed-threads.md).  
  
## <a name="see-also"></a>Confira também

- [Programação assíncrona com Async e Await (Visual Basic)](index.md)
- [Tipos de retorno assíncronos (Visual Basic)](async-return-types.md)
- [Fluxo de controle em programas assíncronos (Visual Basic)](control-flow-in-async-programs.md)
