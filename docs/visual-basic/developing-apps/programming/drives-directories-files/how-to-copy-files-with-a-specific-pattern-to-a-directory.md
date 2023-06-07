---
title: 'Como: copiar arquivos com um padrão específico para um diretório'
ms.date: 07/20/2015
helpviewer_keywords:
- My.Computer.FileSystem.CopyFile method, copying files [Visual Basic]
- files [Visual Basic], copying
- CopyFile method [Visual Basic], copying files in Visual Basic
- I/O [Visual Basic], copying files
ms.assetid: f205d2ad-bbe5-4d55-8a40-acda21aa82dd
ms.openlocfilehash: 7e263e2c9035db54dbb58c6c78c0647d5442504e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401700"
---
# <a name="how-to-copy-files-with-a-specific-pattern-to-a-directory-in-visual-basic"></a>Como copiar arquivos com um padrão específico para um diretório no Visual Basic

O método <xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.GetFiles%2A> retorna uma coleção somente leitura de cadeias de caracteres que representam os nomes de caminho para os arquivos. É possível usar o parâmetro `wildCards` para especificar um padrão específico.  
  
 Se nenhum arquivo correspondente for encontrado, uma coleção vazia será retornada.  
  
 Você pode usar o método <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyFile%2A> para copiar os arquivos em um diretório.  
  
### <a name="to-copy-files-with-a-specific-pattern-to-a-directory"></a>Copiar arquivos com um padrão específico para um diretório  
  
1. Use o método `GetFiles` para retornar a lista de arquivos. Este exemplo retorna todos os arquivos .rtf do diretório especificado.  
  
     [!code-vb[VbFileIOMisc#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#36)]  
  
2. Use o método `CopyFile` para copiar os arquivos. Este exemplo copia os arquivos para o diretório de nome `testdirectory`.  
  
     [!code-vb[VbVbcnMyFileSystem#88](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#88)]  
  
3. Feche a instrução `For` com uma instrução `Next`.  
  
     [!code-vb[VbVbcnMyFileSystem#89](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#89)]  
  
## <a name="example"></a>Exemplo  

 O exemplo a seguir, que apresenta os snippets acima em sua forma completa, copia todos os arquivos .rtf do diretório especificado para o diretório de nome `testdirectory`.  
  
 [!code-vb[VbFileIOMisc#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#37)]  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  

 As seguintes condições podem causar uma exceção:  
  
- O caminho não é válido por um dos seguintes motivos: é uma cadeia de comprimento zero, contém apenas espaços em branco, contém caracteres inválidos ou é um caminho de dispositivo (começa com \\\\.\\) (<xref:System.ArgumentException>).  
  
- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).  
  
- O diretório não existe (<xref:System.IO.DirectoryNotFoundException>).  
  
- O diretório aponta para um arquivo existente (<xref:System.IO.IOException>).  
  
- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).  
  
- Um nome de arquivo ou de diretório no caminho contém dois-pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).  
  
- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>). O usuário não possui as permissões necessárias (<xref:System.UnauthorizedAccessException>).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyFile%2A>
- <xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.GetFiles%2A>
- [Como: localizar subdiretórios com um padrão específico](how-to-find-subdirectories-with-a-specific-pattern.md)
- [Solução de problemas: ler e gravar em arquivos de texto](troubleshooting-reading-from-and-writing-to-text-files.md)
- [Como: obter a coleção de arquivos em um diretório](how-to-get-the-collection-of-files-in-a-directory.md)
