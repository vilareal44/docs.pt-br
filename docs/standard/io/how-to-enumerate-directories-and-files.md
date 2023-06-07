---
title: 'Como: enumerar diretórios e arquivos'
description: Saiba como enumerar diretórios e arquivos usando coleções enumeráveis, que podem fornecer melhor desempenho do que as matrizes no .NET.
ms.date: 12/27/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- I/O [.NET Framework], enumerating directories and files
ms.assetid: 86b69a08-3bfa-4e5f-b4e1-3b7cb8478215
ms.openlocfilehash: 276668f4a3eee89610a81b1256820770d1f72dc3
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662570"
---
# <a name="how-to-enumerate-directories-and-files"></a>Como: enumerar diretórios e arquivos
Coleções enumeráveis fornecem um desempenho melhor do que matrizes ao trabalhar com coleções grandes de arquivos e diretórios. Para enumerar diretórios e arquivos, use métodos que retornam uma coleção enumerável de nomes de diretório ou arquivo ou seus objetos <xref:System.IO.DirectoryInfo>, <xref:System.IO.FileInfo> ou <xref:System.IO.FileSystemInfo>.  
  
Caso deseje pesquisar e retornar somente os nomes de diretórios ou arquivos, use os métodos de enumeração da classe <xref:System.IO.Directory>. Caso deseje pesquisar e retornar outras propriedades de diretórios ou arquivos, use as classes <xref:System.IO.DirectoryInfo> e <xref:System.IO.FileSystemInfo>.  
  
Use coleções enumeráveis desses métodos como o parâmetro <xref:System.Collections.Generic.IEnumerable%601> para construtores de classes de coleção como <xref:System.Collections.Generic.List%601>.  
  
A seguinte tabela resume os métodos que retornam coleções enumeráveis de arquivos e diretórios:  
  
|Para pesquisar e retornar|Use o método|  
|------------------|-------------------------------------|-------------------|  
|Nomes de diretório|<xref:System.IO.Directory.EnumerateDirectories%2A?displayProperty=nameWithType>|  
|Informações de diretório (<xref:System.IO.DirectoryInfo>)|<xref:System.IO.DirectoryInfo.EnumerateDirectories%2A?displayProperty=nameWithType>|  
|Nomes de arquivo|<xref:System.IO.Directory.EnumerateFiles%2A?displayProperty=nameWithType>|  
|Informações do arquivo (<xref:System.IO.FileInfo>)|<xref:System.IO.DirectoryInfo.EnumerateFiles%2A?displayProperty=nameWithType>|  
|Nomes de entrada do sistema de arquivos|<xref:System.IO.Directory.EnumerateFileSystemEntries%2A?displayProperty=nameWithType>|  
|Informações de entrada do sistema de arquivos (<xref:System.IO.FileSystemInfo>)|<xref:System.IO.DirectoryInfo.EnumerateFileSystemInfos%2A?displayProperty=nameWithType>|  
|Nomes de diretório e arquivo |<xref:System.IO.Directory.EnumerateFileSystemEntries%2A?displayProperty=nameWithType>|  

> [!NOTE]
> Embora você possa enumerar imediatamente todos os arquivos nos subdiretórios de um diretório pai usando a opção <xref:System.IO.SearchOption.AllDirectories> da enumeração <xref:System.IO.SearchOption> opcional, os erros <xref:System.UnauthorizedAccessException> podem tornar a enumeração incompleta. Capture essas exceções enumerando primeiro os diretórios e, em seguida, os arquivos.  
  
## <a name="examples-use-the-directory-class"></a>Exemplos: usar a classe de diretório  
  
O exemplo a seguir usa o método <xref:System.IO.Directory.EnumerateDirectories%28System.String%29?displayProperty=nameWithType> para obter uma lista dos nomes de diretório de nível superior em um caminho especificado.  

[!code-csharp[System.IO.EnumDirs1#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.enumdirs1/cs/program.cs#1)]
[!code-vb[System.IO.EnumDirs1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.enumdirs1/vb/program.vb#1)]  

O exemplo a seguir usa o método <xref:System.IO.Directory.EnumerateFiles%28System.String%2CSystem.String%2CSystem.IO.SearchOption%29?displayProperty=nameWithType> para enumerar recursivamente todos os nomes de arquivo em um diretório e subdiretórios que correspondam a determinado padrão. Em seguida, ele lê cada linha de cada arquivo e exibe as linhas que contêm uma cadeia de caracteres especificada, com seus nomes de arquivo e caminhos.

[!code-csharp[System.IO.Directory.EnumerateFiles#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.directory.enumeratefiles/cs/program.cs#1)]
[!code-vb[System.IO.Directory.EnumerateFiles#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.directory.enumeratefiles/vb/program.vb#1)]  
  
## <a name="examples-use-the-directoryinfo-class"></a>Exemplos: usar a classe DirectoryInfo  
  
O exemplo a seguir usa o método <xref:System.IO.DirectoryInfo.EnumerateDirectories%2A?displayProperty=nameWithType> para listar uma coleção de diretórios de nível superior cuja <xref:System.IO.FileSystemInfo.CreationTimeUtc> é anterior a determinado valor <xref:System.DateTime>.  

[!code-csharp[System.IO.DirectoryInfo.EnumDirs#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.directoryinfo.enumdirs/cs/program.cs)]
[!code-vb[System.IO.DirectoryInfo.EnumDirs#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.directoryinfo.enumdirs/vb/module1.vb)]  
  
O exemplo a seguir usa o método <xref:System.IO.DirectoryInfo.EnumerateFiles%2A?displayProperty=nameWithType> para listar todos os arquivos cujo <xref:System.IO.FileInfo.Length> excede 10 MB. Este exemplo enumera primeiro os diretórios de nível superior para capturar possíveis exceções de acesso não autorizado e, em seguida, enumera os arquivos.  

[!code-csharp[System.IO.DirectoryInfo.EnumerateDirectories#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.directoryinfo.enumeratedirectories/cs/program.cs#1)]
[!code-vb[System.IO.DirectoryInfo.EnumerateDirectories#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.directoryinfo.enumeratedirectories/vb/program.vb#1)]  
  
## <a name="see-also"></a>Confira também

- [E/S de arquivo e de fluxo](index.md)
