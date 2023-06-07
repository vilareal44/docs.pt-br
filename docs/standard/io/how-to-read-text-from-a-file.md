---
title: 'Como: ler texto de um arquivo'
description: Neste artigo, consulte exemplos de como ler texto de forma síncrona ou assíncrona de um arquivo de texto, usando a classe StreamReader no .NET para aplicativos da área de trabalho.
ms.date: 01/03/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- streams, reading text from files
- reading text files
- reading data, text files
- data streams, reading text from files
- I/O [.NET Framework], reading text from files
ms.assetid: ed180baa-dfc6-4c69-a725-46e87edafb27
ms.openlocfilehash: 0d8dfae67ede779a611204fb333a19defcaee8e6
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87382120"
---
# <a name="how-to-read-text-from-a-file"></a>Como: ler texto de um arquivo
Os exemplos a seguir mostram como ler de forma síncrona e assíncrona o texto de um arquivo de texto usando o .NET para aplicativos de área de trabalho. Nos dois exemplos, ao criar uma instância da classe <xref:System.IO.StreamReader>, você fornece o caminho relativo ou absoluto para o arquivo.
  
> [!NOTE]
> Esses exemplos de código não se aplicam ao desenvolvimento para aplicativos UWP (Universal do Windows) porque o Windows Runtime fornece diferentes tipos de fluxos para leitura e gravação em arquivos. Para obter um exemplo que mostra como ler o texto de um arquivo em um aplicativo UWP, consulte [início rápido: lendo e gravando arquivos](https://docs.microsoft.com/previous-versions/windows/apps/hh758325(v=win.10)). Para obter exemplos que mostram como converter entre fluxos de .NET Framework e Windows Runtime fluxos, consulte [como converter entre fluxos de .NET Framework e fluxos de Windows Runtime](how-to-convert-between-dotnet-streams-and-winrt-streams.md).  
  
## <a name="example-synchronous-read-in-a-console-app"></a>Exemplo: leitura síncrona em um aplicativo de console  
O exemplo a seguir mostra uma operação de leitura síncrona em um aplicativo de console. Esse exemplo abre o arquivo de texto usando um leitor de fluxo, copia o conteúdo para uma cadeia de caracteres e gera a cadeia de caracteres no console.  
  
> [!IMPORTANT]
> O exemplo pressupõe que um arquivo chamado *TestFile.txt* já exista na mesma pasta do aplicativo.  

:::code language="csharp" source="snippets/how-to-read-text-from-a-file/csharp/sync-console/Program.cs":::
:::code language="vb" source="snippets/how-to-read-text-from-a-file/vb/sync-console/Program.vb":::
  
## <a name="example-asynchronous-read-in-a-wpf-app"></a>Exemplo: leitura assíncrona em um aplicativo WPF
 O exemplo a seguir mostra uma operação de leitura assíncrona em um aplicativo WPF (Windows Presentation Foundation).  
  
> [!IMPORTANT]
> O exemplo pressupõe que um arquivo chamado *TestFile.txt* já exista na mesma pasta do aplicativo.  

:::code language="csharp" source="snippets/how-to-read-text-from-a-file/csharp/async-wpf/MainWindow.xaml.cs":::
:::code language="vb" source="snippets/how-to-read-text-from-a-file/vb/async-wpf/MainWindow.xaml.vb":::
  
## <a name="see-also"></a>Veja também

- <xref:System.IO.StreamReader>  
- <xref:System.IO.File.OpenText%2A?displayProperty=nameWithType>  
- <xref:System.IO.StreamReader.ReadLine%2A?displayProperty=nameWithType>  
- [E/S de arquivo assíncrona](asynchronous-file-i-o.md)  
- [Como: criar uma listagem de diretório](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/5cf8zcfh(v=vs.100))  
- [Início rápido: ler e gravar arquivos](https://docs.microsoft.com/previous-versions/windows/apps/hh758325%28v=win.10%29)  
- [Como converter entre fluxos de .NET Framework e fluxos de Windows Runtime](how-to-convert-between-dotnet-streams-and-winrt-streams.md)  
- [Como: ler e gravar em um arquivo de dados recém-criado](how-to-read-and-write-to-a-newly-created-data-file.md)  
- [Como: abrir e anexar a um arquivo de log](how-to-open-and-append-to-a-log-file.md)  
- [Como: gravar texto em um arquivo](how-to-write-text-to-a-file.md)  
- [Como: ler caracteres de uma cadeia de caracteres](how-to-read-characters-from-a-string.md)  
- [Como: gravar caracteres em uma cadeia de caracteres](how-to-write-characters-to-a-string.md)  
- [E/S de arquivo e de fluxo](index.md)
