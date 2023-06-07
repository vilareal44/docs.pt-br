---
title: Usando o Async para acessar arquivos (C#)
description: Saiba como usar o recurso assíncrono para acessar arquivos em C#. Você pode chamar métodos assíncronos sem usar retornos de chamada ou dividir seu código entre os métodos.
ms.date: 07/20/2015
ms.assetid: bb018fea-5313-4c80-ab3f-7c24b2145bd9
ms.openlocfilehash: eb67bd408fe37b99e6c5ffdc2550e8f95110d7eb
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925118"
---
# <a name="using-async-for-file-access-c"></a>Usando o Async para acessar arquivos (C#)
Você pode usar o recurso async para acessar arquivos. Usando o recurso async, você pode chamar os métodos assíncronos sem usar retornos de chamada ou dividir seu código em vários métodos ou expressões lambda. Para tornar síncrono um código assíncrono, basta chamar um método assíncrono em vez de um método síncrono e adicionar algumas palavras-chave ao código.  
  
 Você pode considerar seguintes motivos para adicionar a assincronia às chamadas de acesso ao arquivo:  
  
- A assincronia torna os aplicativos de interface do usuário mais responsivos porque o thread de interface do usuário que inicia a operação pode executar outro trabalho. Se o thread de interface do usuário precisar executar o código que leva muito tempo (por exemplo, mais de 50 milissegundos), a interface do usuário poderá congelar até que a E/S seja concluída e o thread da interface do usuário possa processar entradas do mouse e do teclado e outros eventos.  
  
- A assincronia melhora a escalabilidade do ASP.NET e outros aplicativos baseados em servidor reduzindo a necessidade de threads. Se o aplicativo usar um thread dedicado por resposta e mil solicitações forem tratadas simultaneamente, serão necessários mil threads. As operações assíncronas normalmente não precisam usar um thread durante a espera. Elas podem usar o thread de conclusão de E/S existente rapidamente no final.  
  
- A latência de uma operação de acesso de arquivo pode ser muito baixa nas condições atuais, mas a latência pode aumentar consideravelmente no futuro. Por exemplo, um arquivo pode ser movido para um servidor que está do outro lado do mundo.  
  
- A sobrecarga adicional de usar o recurso async é pequena.  
  
- As tarefas assíncronas podem facilmente ser executadas em paralelo.  
  
## <a name="running-the-examples"></a>Executando os exemplos  
 Para executar os exemplos neste tópico, você pode criar um **Aplicativo WPF** ou um **Aplicativo do Windows Forms** e, em seguida, adicionar um **Botão**. No evento `Click` do botão, adicione uma chamada para o primeiro método em cada exemplo.  
  
 Nos exemplos a seguir, inclua as seguintes `using` diretivas.  
  
```csharp  
using System;  
using System.Collections.Generic;  
using System.Diagnostics;  
using System.IO;  
using System.Text;  
using System.Threading.Tasks;  
```  
  
## <a name="use-of-the-filestream-class"></a>Uso da classe FileStream  
 Os exemplos neste tópico usam a classe <xref:System.IO.FileStream>, que tem uma opção que faz com que a E/S assíncrona ocorra no nível do sistema operacional. Usando essa opção, você pode evitar o bloqueio de um thread de ThreadPool em muitos casos. Para habilitar essa opção, você deve especificar o argumento `useAsync=true` ou `options=FileOptions.Asynchronous` na chamada do construtor.  
  
 Você não pode usar essa opção com <xref:System.IO.StreamReader> e <xref:System.IO.StreamWriter> se você os abrir diretamente especificando um caminho de arquivo. No entanto, você poderá usar essa opção se fornecer um <xref:System.IO.Stream> que a classe <xref:System.IO.FileStream> abriu. Observe que as chamadas assíncronas serão mais rápidas em aplicativos de interface do usuário mesmo se um thread de ThreadPool estiver bloqueado, porque o thread de interface do usuário não é bloqueado durante a espera.  
  
## <a name="writing-text"></a>Gravando texto  
 O exemplo a seguir grava um texto em um arquivo. A cada instrução await, o método sai imediatamente. Quando o arquivo de E/S for concluído, o método continuará na instrução após a instrução await. Observe que o modificador async é a definição de métodos que usam a instrução await.  
  
```csharp  
public async Task ProcessWriteAsync()  
{  
    string filePath = @"temp2.txt";  
    string text = "Hello World\r\n";  
  
    await WriteTextAsync(filePath, text);  
}  
  
private async Task WriteTextAsync(string filePath, string text)  
{  
    byte[] encodedText = Encoding.Unicode.GetBytes(text);  
  
    using (FileStream sourceStream = new FileStream(filePath,  
        FileMode.Append, FileAccess.Write, FileShare.None,  
        bufferSize: 4096, useAsync: true))  
    {  
        await sourceStream.WriteAsync(encodedText, 0, encodedText.Length);  
    };  
}  
```  
  
 O exemplo original tem a instrução `await sourceStream.WriteAsync(encodedText, 0, encodedText.Length);`, que é uma contração das duas instruções a seguir:  
  
```csharp  
Task theTask = sourceStream.WriteAsync(encodedText, 0, encodedText.Length);  
await theTask;  
```  
  
 A primeira instrução retorna uma tarefa e faz com que o processamento do arquivo seja iniciado. A segunda instrução com o await faz com que o método saia imediatamente e retorne uma tarefa diferente. Quando o processamento do arquivo é concluído posteriormente, a execução retorna para a instrução após a await. Para obter mais informações, consulte [Fluxo de controle em programas assíncronos (C#)](./control-flow-in-async-programs.md).  
  
## <a name="reading-text"></a>Lendo texto  
 O exemplo a seguir lê o texto de um arquivo. O texto é armazenado em buffer e, nesse caso, colocado em um <xref:System.Text.StringBuilder>. Diferentemente do exemplo anterior, a avaliação de await produz um valor. O método <xref:System.IO.Stream.ReadAsync%2A> retorna um <xref:System.Threading.Tasks.Task>\<<xref:System.Int32>>, portanto, a avaliação do await produz um valor `Int32` (`numRead`) após a conclusão da operação. Para obter mais informações, consulte [Tipos de retorno assíncronos (C#)](./async-return-types.md).  
  
```csharp  
public async Task ProcessReadAsync()  
{  
    string filePath = @"temp2.txt";  
  
    if (File.Exists(filePath) == false)  
    {  
        Debug.WriteLine("file not found: " + filePath);  
    }  
    else  
    {  
        try  
        {  
            string text = await ReadTextAsync(filePath);  
            Debug.WriteLine(text);  
        }  
        catch (Exception ex)  
        {  
            Debug.WriteLine(ex.Message);  
        }  
    }  
}  
  
private async Task<string> ReadTextAsync(string filePath)  
{  
    using (FileStream sourceStream = new FileStream(filePath,  
        FileMode.Open, FileAccess.Read, FileShare.Read,  
        bufferSize: 4096, useAsync: true))  
    {  
        StringBuilder sb = new StringBuilder();  
  
        byte[] buffer = new byte[0x1000];  
        int numRead;  
        while ((numRead = await sourceStream.ReadAsync(buffer, 0, buffer.Length)) != 0)  
        {  
            string text = Encoding.Unicode.GetString(buffer, 0, numRead);  
            sb.Append(text);  
        }  
  
        return sb.ToString();  
    }  
}  
```  
  
## <a name="parallel-asynchronous-io"></a>E/S assíncrona paralela  
 O exemplo a seguir demonstra o processamento paralelo escrevendo dez arquivos de texto. Para cada arquivo, o método <xref:System.IO.Stream.WriteAsync%2A> retorna uma tarefa que é então adicionada a uma lista de tarefas. A instrução `await Task.WhenAll(tasks);` sai do método e retoma no método quando o processamento do arquivo é concluído para todas as tarefas.  
  
 O exemplo fecha todas as instâncias de <xref:System.IO.FileStream> em um bloco `finally` após as tarefas serem concluídas. Se cada `FileStream` foi criado em uma instrução `using`, o `FileStream` pode ter sido descartado antes de a tarefa ter sido concluída.  
  
 Observe que qualquer aumento de desempenho é quase que totalmente do processamento paralelo e não o processamento assíncrono. As vantagens da assincronia são que ela não bloqueia vários threads e que ela não bloqueia o thread da interface do usuário.  
  
```csharp  
public async Task ProcessWriteMultAsync()  
{  
    string folder = @"tempfolder\";  
    List<Task> tasks = new List<Task>();  
    List<FileStream> sourceStreams = new List<FileStream>();  
  
    try  
    {  
        for (int index = 1; index <= 10; index++)  
        {  
            string text = "In file " + index.ToString() + "\r\n";  
  
            string fileName = "thefile" + index.ToString("00") + ".txt";  
            string filePath = folder + fileName;  
  
            byte[] encodedText = Encoding.Unicode.GetBytes(text);  
  
            FileStream sourceStream = new FileStream(filePath,  
                FileMode.Append, FileAccess.Write, FileShare.None,  
                bufferSize: 4096, useAsync: true);  
  
            Task theTask = sourceStream.WriteAsync(encodedText, 0, encodedText.Length);  
            sourceStreams.Add(sourceStream);  
  
            tasks.Add(theTask);  
        }  
  
        await Task.WhenAll(tasks);  
    }  
  
    finally  
    {  
        foreach (FileStream sourceStream in sourceStreams)  
        {  
            sourceStream.Close();  
        }  
    }  
}  
```  
  
 Ao usar os métodos <xref:System.IO.Stream.WriteAsync%2A> e <xref:System.IO.Stream.ReadAsync%2A>, você pode especificar um <xref:System.Threading.CancellationToken>, que pode ser usado para cancelar o fluxo intermediário da operação. Para obter mais informações, consulte [Ajuste fino de seu aplicativo assíncrono (C#)](./fine-tuning-your-async-application.md) e [Cancelamento em threads gerenciados](../../../../standard/threading/cancellation-in-managed-threads.md).  
  
## <a name="see-also"></a>Veja também

- [Programação assíncrona com Async e Await (C#)](./index.md)
- [Tipos de retorno assíncronos (C#)](./async-return-types.md)
- [Fluxo de controle em programas assíncronos (C#)](./control-flow-in-async-programs.md)
