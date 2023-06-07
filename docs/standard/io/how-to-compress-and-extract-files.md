---
title: Como compactar e extrair arquivos
description: Compacte & extrair arquivos usando System. IO. compactação. Veja exemplos usando ZipFile, ZipArchive, ZipArchiveEntry, DeflateStream, & GZipStream.
ms.date: 01/14/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- I/O [.NET Framework], compression
- compression
- compress files
ms.assetid: e9876165-3c60-4c84-a272-513e47acf579
ms.openlocfilehash: c13f464432aa6f67136d3a844bdeda256e7ab9b6
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769230"
---
# <a name="how-to-compress-and-extract-files"></a>Como compactar e extrair arquivos

O namespace <xref:System.IO.Compression> contém os seguintes tipos para compactar e descompactar arquivos e fluxos. Use também esses tipos para ler e modificar o conteúdo de um arquivo compactado.

- <xref:System.IO.Compression.ZipFile>
- <xref:System.IO.Compression.ZipArchive>
- <xref:System.IO.Compression.ZipArchiveEntry>
- <xref:System.IO.Compression.DeflateStream>
- <xref:System.IO.Compression.GZipStream>

Os exemplos a seguir mostram algumas das operações que você pode executar com arquivos compactados. Estes exemplos exigem que os seguintes pacotes NuGet sejam adicionados ao seu projeto:

- [System.IO.Compression](https://www.nuget.org/packages/System.IO.Compression)
- [System.IO.Compression.ZipFile](https://www.nuget.org/packages/System.IO.Compression.ZipFile)

Se você estiver usando .NET Framework, adicione referências a essas duas bibliotecas ao seu projeto:

- `System.IO.Compression`
- `System.IO.Compression.FileSystem`

## <a name="example-1-create-and-extract-a-zip-file"></a>Exemplo 1: criar e extrair um arquivo. zip

O exemplo a seguir mostra como criar e extrair um arquivo *.zip* compactado usando a classe <xref:System.IO.Compression.ZipFile>. O exemplo compacta o conteúdo de uma pasta em um novo arquivo *.zip* e extrai o zip para uma nova pasta.

Para executar a amostra, crie uma pasta *Iniciar* na pasta do programa e popule-a com arquivos a serem zipados.

[!code-csharp[System.IO.Compression.ZipFile#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.compression.zipfile/cs/program1.cs#1)]
[!code-vb[System.IO.Compression.ZipFile#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.compression.zipfile/vb/program1.vb#1)]

## <a name="example-2-extract-specific-file-extensions"></a>Exemplo 2: extrair extensões de arquivo específicas

O próximo exemplo itera pelo conteúdo de um arquivo *.zip* existente e extrai os arquivos que têm uma extensão *.txt*. Ele usa a classe <xref:System.IO.Compression.ZipArchive> para acessar o zip e a classe <xref:System.IO.Compression.ZipArchiveEntry> para inspecionar as entradas individuais. O método de extensão <xref:System.IO.Compression.ZipFileExtensions.ExtractToFile%2A> para o objeto <xref:System.IO.Compression.ZipArchiveEntry> está disponível na classe <xref:System.IO.Compression.ZipFileExtensions?displayProperty=nameWithType>.

Para executar a amostra, coloque um arquivo *.zip* chamado *result.zip* na pasta do programa. Quando solicitado, forneça um nome de pasta na qual extrair.

> [!IMPORTANT]
> Ao descompactar os arquivos, você precisa procurar caminhos de arquivos maliciosos que possam escapar do diretório no qual deseja descompactar. Isso é conhecido como um ataque de passagem de caminho. O exemplo a seguir demonstra como verificar caminhos de arquivos maliciosos e fornece uma maneira segura de descompactação.

[!code-csharp[System.IO.Compression.ZipArchive#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.compression.ziparchive/cs/program1.cs#1)]
[!code-vb[System.IO.Compression.ZipArchive#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.compression.ziparchive/vb/program1.vb#1)]

## <a name="example-3-add-a-file-to-an-existing-zip"></a>Exemplo 3: adicionar um arquivo a um zip existente

O exemplo a seguir usa a classe <xref:System.IO.Compression.ZipArchive> para acessar um arquivo *.zip* existente e adiciona um arquivo a ele. O novo arquivo é compactado quando você o adiciona ao zip existente.

[!code-csharp[System.IO.Compression.ZipArchiveMode#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.compression.ziparchivemode/cs/program1.cs#1)]
[!code-vb[System.IO.Compression.ZipArchiveMode#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.compression.ziparchivemode/vb/program1.vb#1)]

## <a name="example-4-compress-and-decompress-gz-files"></a>Exemplo 4: compactar e descompactar arquivos. gz

Você também pode usar as classes <xref:System.IO.Compression.GZipStream> e <xref:System.IO.Compression.DeflateStream> para compactar e descompactar dados. Elas usam o mesmo algoritmo de compactação. Você pode descompactar objetos <xref:System.IO.Compression.GZipStream> que são gravados em um arquivo *.gz* usando muitas ferramentas comuns. O exemplo a seguir mostra como compactar e descompactar um diretório de arquivos usando a classe <xref:System.IO.Compression.GZipStream>:

[!code-csharp[IO.Compression.GZip1#1](../../../samples/snippets/csharp/VS_Snippets_CLR/IO.Compression.GZip1/CS/gziptest.cs#1)]
[!code-vb[IO.Compression.GZip1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/IO.Compression.GZip1/VB/gziptest.vb#1)]

## <a name="see-also"></a>Veja também

- <xref:System.IO.Compression.ZipArchive>  
- <xref:System.IO.Compression.ZipFile>  
- <xref:System.IO.Compression.ZipArchiveEntry>  
- <xref:System.IO.Compression.DeflateStream>  
- <xref:System.IO.Compression.GZipStream>  
- [E/S de arquivo e de fluxo](index.md)
