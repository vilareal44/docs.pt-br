---
title: 'Como: localizar arquivos com um padrão específico'
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], finding
- pattern matching
- patterns, matching
ms.assetid: 25e3b71d-b844-4293-9e4e-f06c5836b5cc
ms.openlocfilehash: 71073672ed14cb2d5df5b5365266b718c59cb18f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401635"
---
# <a name="how-to-find-files-with-a-specific-pattern-in-visual-basic"></a>Como localizar arquivos com um padrão específico no Visual Basic

O método <xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.GetFiles%2A> retorna uma coleção somente leitura de cadeias de caracteres que representam os nomes de caminho para os arquivos. É possível usar o parâmetro `wildCards` para especificar um padrão específico. Para incluir subdiretórios na pesquisa, configure o parâmetro `searchType` para `SearchOption.SearchAllSubDirectories`.  
  
 Uma coleção vazia é retornada se nenhum arquivo correspondente ao padrão especificado for encontrado.  
  
> [!NOTE]
> Para saber mais sobre como retornar uma lista de arquivos usando a classe `DirectoryInfo` do namespace `System.IO`, confira <xref:System.IO.DirectoryInfo.GetFiles%2A>.  
  
### <a name="to-find-files-with-a-specified-pattern"></a>Localizar arquivos com um padrão específico  
  
- Use o método `GetFiles`, fornecendo o nome e o caminho do diretório a ser pesquisado e especificando o padrão. O exemplo a seguir retorna todos os arquivos com a extensão `.dll` no diretório e os adiciona a `ListBox1`.  
  
     [!code-vb[VbFileIOMisc#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#4)]  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  

 As seguintes condições podem causar uma exceção:  
  
- O caminho não é válido por um dos seguintes motivos: é uma cadeia de comprimento zero, contém apenas espaços em branco, contém caracteres inválidos ou é um caminho de dispositivo (começa com \\\\.\\) (<xref:System.ArgumentException>).  
  
- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).  
  
- `directory` não existe (<xref:System.IO.DirectoryNotFoundException>).  
  
- `directory` aponta para um arquivo existente (<xref:System.IO.IOException>).  
  
- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).  
  
- Um nome de pasta no caminho contém dois pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).  
  
- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>).  
  
- O usuário não possui as permissões necessárias (<xref:System.UnauthorizedAccessException>).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.GetFiles%2A>
- [Como: localizar subdiretórios com um padrão específico](how-to-find-subdirectories-with-a-specific-pattern.md)
- [Solução de problemas: ler e gravar em arquivos de texto](troubleshooting-reading-from-and-writing-to-text-files.md)
- [Como: obter a coleção de arquivos em um diretório](how-to-get-the-collection-of-files-in-a-directory.md)
