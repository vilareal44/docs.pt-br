---
title: 'Como: Excluir arquivos e diretórios no armazenamento isolado'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data storage using isolated storage, deleting files and directories
- directories [.NET Framework], isolated storage
- files [.NET Framework], isolated storage
- isolated storage, deleting files and directories
- data stores, deleting files and directories
- stores, creating files and directories
- deleting files within isolated stage file
- storing data using isolated storage, deleting files and directories
- deleting directories within isolated stage file
ms.assetid: 8fcc0dea-435b-4d40-ba4d-ba056265c202
ms.openlocfilehash: dc84fefbde1177993b17e9ec687a1ef759b74735
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291897"
---
# <a name="how-to-delete-files-and-directories-in-isolated-storage"></a>Como: Excluir arquivos e diretórios no armazenamento isolado
Você pode excluir pastas e arquivos em um arquivo de armazenamento isolado. Em um repositório, nomes de arquivo e diretório são dependentes do sistema operacional e são especificados como relativos à raiz do sistema de arquivos virtual. Eles não diferenciam maiúsculas de minúsculas em sistemas operacionais Windows.  
  
 A classe <xref:System.IO.IsolatedStorage.IsolatedStorageFile?displayProperty=nameWithType> fornece dois métodos para excluir arquivos e diretórios: <xref:System.IO.IsolatedStorage.IsolatedStorageFile.DeleteDirectory%2A> e <xref:System.IO.IsolatedStorage.IsolatedStorageFile.DeleteFile%2A>. Uma exceção <xref:System.IO.IsolatedStorage.IsolatedStorageException> será gerada se você tentar excluir um arquivo ou um diretório que não exista. Se você incluir um caractere curinga no nome, <xref:System.IO.IsolatedStorage.IsolatedStorageFile.DeleteDirectory%2A> gerará uma exceção <xref:System.IO.IsolatedStorage.IsolatedStorageException> e <xref:System.IO.IsolatedStorage.IsolatedStorageFile.DeleteFile%2A> gerará uma exceção <xref:System.ArgumentException>.  
  
 O método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.DeleteDirectory%2A> falhará se o diretório contiver arquivos ou subpastas. Você pode usar os métodos <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A> e <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A> para recuperar os arquivos e diretórios existentes. Para saber mais sobre o sistema de arquivos virtual de um repositório de pesquisa, confira [Como localizar arquivos e diretórios existentes no armazenamento isolado](how-to-find-existing-files-and-directories-in-isolated-storage.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir cria e exclui vários diretórios e arquivos.  
  
 [!code-cpp[Conceptual.IsolatedStorage#4](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source4.cpp#4)]
 [!code-csharp[Conceptual.IsolatedStorage#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source4.cs#4)]
 [!code-vb[Conceptual.IsolatedStorage#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source4.vb#4)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile?displayProperty=nameWithType>
- [Armazenamento isolado](isolated-storage.md)
