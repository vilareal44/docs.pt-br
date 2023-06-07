---
title: 'Como: Localizar arquivos e diretórios existentes no armazenamento isolado'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- stores, finding files and directories
- locating files in isolated storage file
- directories [.NET Framework], isolated storage
- isolated storage, finding files and directories
- data storage using isolated storage, finding files and directories
- files [.NET Framework], isolated storage
- data stores, finding files and directories
- locating directories in isolated storage file
- storing data using isolated storage, finding files and directories
ms.assetid: eb28458a-6161-4e7a-9ada-30ef93761b5c
ms.openlocfilehash: ec1d1fbdefdad3ec0dd2c07fbc1278e4d1d217c6
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291845"
---
# <a name="how-to-find-existing-files-and-directories-in-isolated-storage"></a>Como: Localizar arquivos e diretórios existentes no armazenamento isolado

Para procurar um diretório no armazenamento isolado, use o método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A?displayProperty=nameWithType>. Esse método usa uma cadeia de caracteres que representa um padrão de pesquisa. Você pode usar o caracteres curinga de caractere único (?) e caracteres múltiplos (\*) no padrão de pesquisa, mas os caracteres curinga deverão aparecer na parte final do nome. Por exemplo, `directory1/*ect*` é uma cadeia de caracteres de pesquisa válida, mas `*ect*/directory2` não é.  
  
 Para procurar um arquivo, use o método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A?displayProperty=nameWithType>. A restrição para caracteres curinga em cadeias de caracteres de pesquisa que se aplica a <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A> também se aplica a <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A>.  
  
 Nenhum desses métodos é recursivo; a classe <xref:System.IO.IsolatedStorage.IsolatedStorageFile> fornece métodos para listar todos os diretórios ou arquivos no repositório. No entanto, os métodos recursivos são mostrados no exemplo de código a seguir.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir ilustra como criar arquivos e diretórios em um repositório isolado. Primeiro, um repositório isolado para usuário, domínio e assembly é recuperado e colocado na variável `isoStore`. O método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.CreateDirectory%2A> é usado para configurar algumas pastas diferentes, e o construtor <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream.%23ctor%28System.String%2CSystem.IO.FileMode%2CSystem.IO.IsolatedStorage.IsolatedStorageFile%29> cria alguns arquivos nesses diretórios. O código executa um loop através dos resultados do método `GetAllDirectories`. Esse método usa <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A> para localizar todos os nomes de diretório no diretório atual. Esses nomes são armazenados em uma matriz, e `GetAllDirectories` chama a si mesmo, passando cada diretório encontrado. Como resultado, todos os nomes de diretório são retornados em uma matriz. Em seguida, o código chama o método `GetAllFiles`. Este método chama `GetAllDirectories` para descobrir os nomes de todos os diretórios e verifica cada diretório de arquivos usando o método <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A>. O resultado é retornado em uma matriz para exibição.  
  
 [!code-cpp[Conceptual.IsolatedStorage#9](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source8.cpp#9)]
 [!code-csharp[Conceptual.IsolatedStorage#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source8.cs#9)]
 [!code-vb[Conceptual.IsolatedStorage#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source8.vb#9)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- [Armazenamento isolado](isolated-storage.md)
