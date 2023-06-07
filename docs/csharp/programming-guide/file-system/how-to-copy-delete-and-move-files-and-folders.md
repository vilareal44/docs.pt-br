---
title: Como copiar, excluir e mover arquivos e pastas-guia de programação em C#
description: Saiba como copiar, excluir e mover arquivos e pastas usando as classes File, Directory, FileInfo e DirectoryInfo.
ms.date: 07/20/2015
helpviewer_keywords:
- I/O [C#]
ms.assetid: 62e52cd7-9597-4e4a-acf9-1315f5cdbf05
ms.openlocfilehash: 208502651080f4fd614e34d1bf5b088dfb1207a6
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303355"
---
# <a name="how-to-copy-delete-and-move-files-and-folders-c-programming-guide"></a>Como copiar, excluir e mover arquivos e pastas (guia de programação C#)
Os exemplos a seguir mostram como copiar, mover e excluir arquivos e pastas de maneira síncrona usando as classes <xref:System.IO.File?displayProperty=nameWithType>, <xref:System.IO.Directory?displayProperty=nameWithType>, <xref:System.IO.FileInfo?displayProperty=nameWithType> e <xref:System.IO.DirectoryInfo?displayProperty=nameWithType> do namespace <xref:System.IO?displayProperty=nameWithType>. Esses exemplos não fornecem uma barra de progresso ou qualquer outra interface do usuário. Se você quiser fornecer uma caixa de diálogo de progresso padrão, consulte [como fornecer uma caixa de diálogo de progresso para operações de arquivo](how-to-provide-a-progress-dialog-box-for-file-operations.md).  
  
 Use <xref:System.IO.FileSystemWatcher?displayProperty=nameWithType> para fornecer eventos que permitirão que você calcule o progresso ao operar em vários arquivos. Outra abordagem é usar a invocação de plataforma para invocar os métodos relacionados ao arquivo relevantes no Shell do Windows. Para obter informações sobre como executar essas operações de arquivo de forma assíncrona, consulte [E/S de arquivo assíncrona](../../../standard/io/asynchronous-file-i-o.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como copiar arquivos e diretórios.  
  
 [!code-csharp[csFilesandFolders#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#7)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como mover arquivos e diretórios.  
  
 [!code-csharp[csFilesandFolders#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#8)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como excluir arquivos e diretórios.  
  
 [!code-csharp[csFilesandFolders#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#9)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.IO?displayProperty=nameWithType>
- [Guia de programação C#](../index.md)
- [Sistema de arquivos e o Registro (Guia de Programação em C#)](index.md)
- [Como fornecer uma caixa de diálogo de progresso para operações de arquivo](how-to-provide-a-progress-dialog-box-for-file-operations.md)
- [Arquivo e e/s de fluxo](../../../standard/io/index.md)
- [Tarefas comuns de e/s](../../../standard/io/common-i-o-tasks.md)
