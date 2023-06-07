---
title: 'Como: ler caracteres de uma cadeia de caracteres'
description: Saiba como ler caracteres de uma cadeia de caracteres no .NET. Veja exemplos de leitura de caracteres de forma síncrona e assíncrona.
ms.date: 01/21/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reading characters from strings
- data streams, reading characters from string
- I/O [.NET Framework], reading characters from strings
- reading data, strings
- streams, reading characters from string
ms.assetid: 27ea5e52-6db8-42d8-980a-50bcfc7fd270
ms.openlocfilehash: 40ff144e0899f3560805a31fa76f391afaeccd67
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594836"
---
# <a name="how-to-read-characters-from-a-string"></a>Como: ler caracteres de uma cadeia de caracteres
Os exemplos de código a seguir mostram como ler caracteres de forma síncrona ou assíncrona de uma cadeia de caracteres.  
  
## <a name="example-read-characters-synchronously"></a>Exemplo: ler caracteres de forma síncrona
 Este exemplo lê 13 caracteres de forma síncrona de uma cadeia de caracteres, armazena-os em uma matriz e exibe-os. Em seguida, o exemplo lê os caracteres restantes na cadeia de caracteres, armazena-os na matriz começando pelo sexto elemento e exibe o conteúdo da matriz.  
  
 [!code-csharp[Conceptual.StringReader#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.stringreader/cs/source.cs#1)]
 [!code-vb[Conceptual.StringReader#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.stringreader/vb/source.vb#1)]  
  
## <a name="example-read-characters-asynchronously"></a>Exemplo: ler caracteres de forma assíncrona  
 O próximo exemplo é o código por trás de um aplicativo WPF. Na carga de janela, o exemplo lê todos os caracteres de forma assíncrona de um controle <xref:System.Windows.Controls.TextBox> e os armazena em uma matriz. Em seguida, ele grava cada letra ou caractere de espaço em branco de forma assíncrona em uma linha separada de um controle <xref:System.Windows.Controls.TextBlock>.  
  
 [!code-csharp[Conceptual.StringReader#2](../../../samples/snippets/csharp/VS_Snippets_Wpf/StringReaderWriter/MainWindow.xaml.cs)]
 [!code-vb[Conceptual.StringReader#2](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/StringReaderWriter/MainWindow.xaml.vb)]  
  
## <a name="see-also"></a>Consulte também

- <xref:System.IO.StringReader>  
- <xref:System.IO.StringReader.Read%2A?displayProperty=nameWithType>  
- [E/S de arquivo assíncrona](asynchronous-file-i-o.md)  
- [Como: criar uma listagem de diretório](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/5cf8zcfh(v=vs.100))  
- [Como: ler e gravar em um arquivo de dados recém-criado](how-to-read-and-write-to-a-newly-created-data-file.md)  
- [Como: abrir e anexar a um arquivo de log](how-to-open-and-append-to-a-log-file.md)  
- [Como: ler texto de um arquivo](how-to-read-text-from-a-file.md)  
- [Como: gravar texto em um arquivo](how-to-write-text-to-a-file.md)  
- [Como: gravar caracteres em uma cadeia de caracteres](how-to-write-characters-to-a-string.md)  
- [E/S de arquivo e de fluxo](index.md)
