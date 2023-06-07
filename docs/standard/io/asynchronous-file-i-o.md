---
title: E/S de arquivo assíncrona
description: Leia sobre e/s de arquivo assíncrono no .NET. Aprenda métodos assíncronos para simplificar operações assíncronas, como ReadAsync, WriteAsync e muito mais.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- streams, synchronous streams
- asynchronous I/O
- synchronous I/O
- streams, asynchronous streams
- I/O [.NET Framework], asynchronous I/O
- Stream class, synchronous I/O
- data streams, asynchronous streams
- Stream class, asynchronous I/O
- multiple I/O requests
- data streams, synchronous streams
ms.assetid: dbdd55e7-d6b9-4f9e-8abb-ab0edd4457f7
ms.openlocfilehash: 9506a366b6f1e363ec13550e5ed68c7176dd4d0a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598613"
---
# <a name="asynchronous-file-io"></a>E/S de arquivo assíncrona

Operações assíncronas permitem que você execute operações de E/S que consomem muitos recursos sem bloquear o thread principal. Essa consideração de desempenho é particularmente importante em um aplicativo de armazenamento do Windows 8. x ou aplicativo de área de trabalho em que uma operação de fluxo demorado pode bloquear o thread da interface do usuário e fazer seu aplicativo aparecer como se não estiver funcionando.

Do .NET Framework 4.5 em diante, os tipos de E/S incluem métodos assíncronos para simplificar operações assíncronas. Um método assíncrono contém `Async` em seu nome, como <xref:System.IO.Stream.ReadAsync%2A>, <xref:System.IO.Stream.WriteAsync%2A>, <xref:System.IO.Stream.CopyToAsync%2A>, <xref:System.IO.Stream.FlushAsync%2A>, <xref:System.IO.TextReader.ReadLineAsync%2A> e <xref:System.IO.TextReader.ReadToEndAsync%2A>. Esses métodos assíncronos são implementados em classes de fluxo, como <xref:System.IO.Stream>, <xref:System.IO.FileStream> e <xref:System.IO.MemoryStream>, e nas classes que são usadas para leitura ou gravação em fluxos, como <xref:System.IO.TextReader> e <xref:System.IO.TextWriter>.

No .NET Framework 4, e em versões anteriores, você precisa usar métodos como <xref:System.IO.Stream.BeginRead%2A> e <xref:System.IO.Stream.EndRead%2A> para implementar operações de E/S assíncronas. Esses métodos ainda estão disponíveis no .NET Framework 4.5 para dar suporte ao código herdado; no entanto, os métodos assíncronos ajudarão você a implementar operações de E/S assíncronas com mais facilidade.

C# e Visual Basic têm duas palavras-chave cada para a programação assíncrona:

- `Async` (Visual Basic) ou modificador `async` (C#), que é usado para marcar um método que contém uma operação assíncrona.

- `Await` (Visual Basic) ou operador `await` (C#), que é aplicado ao resultado de um método assíncrono.

Para implementar operações de E/S assíncronas, use essas palavras-chave em conjunto com os métodos assíncronos, conforme mostrado nos exemplos a seguir. Para obter mais informações, confira [Programação assíncrona com async e await (C#)](../../csharp/programming-guide/concepts/async/index.md) ou [Programação assíncrona com Async e Await (Visual Basic)](../../visual-basic/programming-guide/concepts/async/index.md).

O exemplo a seguir demonstra como usar dois objetos <xref:System.IO.FileStream> para copiar arquivos de forma assíncrona de um diretório para outro. Observe que o manipulador de eventos <xref:System.Web.UI.WebControls.Button.Click> para o controle <xref:System.Windows.Controls.Button> está marcado com o modificador `async`, pois ele chama um método assíncrono.

[!code-csharp[Asynchronous_File_IO_async#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Asynchronous_File_IO_async/cs/example.cs#1)]
[!code-vb[Asynchronous_File_IO_async#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Asynchronous_File_IO_async/vb/example.vb#1)]

O exemplo a seguir é semelhante ao anterior, mas usa objetos <xref:System.IO.StreamReader> e <xref:System.IO.StreamWriter> para ler e gravar o conteúdo de um arquivo de texto de forma assíncrona.

[!code-csharp[Asynchronous_File_IO_async#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Asynchronous_File_IO_async/cs/example2.cs#2)]
[!code-vb[Asynchronous_File_IO_async#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Asynchronous_File_IO_async/vb/example2.vb#2)]

O exemplo a seguir mostra o arquivo code-behind e o arquivo XAML que são usados para abrir um arquivo como um <xref:System.IO.Stream> em um aplicativo da loja do Windows 8. x e ler seu conteúdo usando uma instância da <xref:System.IO.StreamReader> classe. Ele usa os métodos assíncronos para abrir o arquivo como um fluxo e ler seu conteúdo.

[!code-csharp[System.IO.WindowsRuntimeStorageExtensions#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.windowsruntimestorageextensions/cs/blankpage.xaml.cs#2)]
[!code-vb[System.IO.WindowsRuntimeStorageExtensions#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.windowsruntimestorageextensions/vb/blankpage.xaml.vb#2)]

[!code-xaml[System.IO.WindowsRuntimeStorageExtensions#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.windowsruntimestorageextensions/cs/blankpage.xaml#1)]

## <a name="see-also"></a>Consulte também

- <xref:System.IO.Stream>
- [Arquivo e e/s de fluxo](index.md)
- [Programação assíncrona com async e await (C#)](../../csharp/programming-guide/concepts/async/index.md)
- [Programação assíncrona com Async e Await (Visual Basic)](../../visual-basic/programming-guide/concepts/async/index.md)
