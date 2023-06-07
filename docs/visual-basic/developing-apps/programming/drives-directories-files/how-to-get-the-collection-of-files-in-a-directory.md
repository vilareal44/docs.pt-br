---
title: 'Como: obter a coleção de arquivos em um diretório'
ms.date: 07/20/2015
helpviewer_keywords:
- folders, working with
- files [Visual Basic], accessing
ms.assetid: 6c8ba7e8-dd37-4853-92bf-762b67c98160
ms.openlocfilehash: 055151d4b3e3aba6be9b9b03ac9abffc6eb7b734
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401609"
---
# <a name="how-to-get-the-collection-of-files-in-a-directory-in-visual-basic"></a>Como obter a coleção de arquivos em um diretório no Visual Basic

As sobrecargas do método <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles%2A?displayProperty=nameWithType> retornam uma coleção somente leitura de cadeias de caracteres representando os nomes dos arquivos dentro de um diretório:  
  
- Use a sobrecarga <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles%28System.String%29> para fazer uma busca de arquivo simples em um diretório especificado, sem buscar em subdiretórios.  
  
- Use a sobrecarga <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles(System.String,Microsoft.VisualBasic.FileIO.SearchOption,System.String[])> para especificar opções adicionais para a busca. É possível usar o parâmetro `wildCards` para especificar um padrão de busca. Para incluir subdiretórios na busca, defina o parâmetro `searchType` para <xref:Microsoft.VisualBasic.FileIO.SearchOption.SearchAllSubDirectories?displayProperty=nameWithType>.  
  
 Uma coleção vazia é retornada se nenhum arquivo correspondente ao padrão especificado for encontrado.  
  
### <a name="to-list-files-in-a-directory"></a>Listar arquivos em um diretório  
  
- Use uma das sobrecargas do método <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles%2A?displayProperty=nameWithType>, fornecendo o nome e o caminho do diretório para buscar no parâmetro `directory`. O exemplo a seguir retorna todos os arquivos no diretório e os inclui na `ListBox1`.  
  
     [!code-vb[VbVbcnMyFileSystem#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#32)]  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar uma exceção:  
  
- O caminho não é válido por um dos seguintes motivos: é uma cadeia de comprimento zero, contém apenas espaços em branco, contém caracteres inválidos ou é um caminho de dispositivo (começa com \\\\.\\) (<xref:System.ArgumentException>).  
  
- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).  
  
- `directory` não existe (<xref:System.IO.DirectoryNotFoundException>).  
  
- `directory` aponta para um arquivo existente (<xref:System.IO.IOException>).  
  
- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).  
  
- Um nome de arquivo ou de diretório no caminho contém dois-pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).  
  
- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>).  
  
- O usuário não possui as permissões necessárias (<xref:System.UnauthorizedAccessException>).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles%2A>
- [Como: localizar arquivos com um padrão específico](how-to-find-files-with-a-specific-pattern.md)
- [Como: localizar subdiretórios com um padrão específico](how-to-find-subdirectories-with-a-specific-pattern.md)
