---
title: 'Como: analisar caminhos de arquivo'
ms.date: 07/20/2015
helpviewer_keywords:
- file names [Visual Basic], parsing [Visual Basic]
- parsing, file paths [Visual Basic]
ms.assetid: c1bd99c9-8160-456a-b5ab-60a49139b923
ms.openlocfilehash: eb7714a8594c0bce344eb2e48ebc5053dc3bfbb4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359936"
---
# <a name="how-to-parse-file-paths-in-visual-basic"></a>Como analisar demarcadores de arquivo no Visual Basic

O objeto <xref:Microsoft.VisualBasic.FileIO.FileSystem> oferece uma série de métodos úteis ao analisar os caminhos de arquivo.  
  
- O método <xref:Microsoft.VisualBasic.FileIO.FileSystem.CombinePath%2A> usa dois caminhos e retorna um caminho combinado formatado corretamente.  
  
- O método <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetParentPath%2A> retorna o caminho absoluto do pai do caminho fornecido.  
  
- O método <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFileInfo%2A> retorna um objeto <xref:System.IO.FileInfo> que pode ser consultado para determinar as propriedades do arquivo, como seu nome e caminho.  
  
 Não tome decisões sobre os conteúdos do arquivo com base na extensão de nome de arquivo. Por exemplo, o arquivo Form1.vb pode não ser um arquivo de código-fonte do Visual Basic.  
  
### <a name="to-determine-a-files-name-and-path"></a>Determinar o nome e o caminho de um arquivo  
  
- Use as propriedades <xref:System.IO.FileInfo.DirectoryName%2A> e <xref:System.IO.FileInfo.Name%2A> do objeto <xref:System.IO.FileInfo> para determinar o nome e o caminho do arquivo. Este exemplo determina o nome e o caminho e os exibe.  
  
     [!code-vb[VbVbcnMyFileSystem#54](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#54)]  
  
### <a name="to-combine-a-files-name-and-directory-to-create-the-full-path"></a>Combinar o nome e o diretório de um arquivo para criar o caminho completo  
  
- Use o método `CombinePath`, fornecendo o diretório e o nome. Este exemplo usa as cadeias de caracteres `folderPath` e `fileName`, criadas no exemplo anterior, as combina e exibe o resultado.  
  
     [!code-vb[VbVbcnMyFileSystem#55](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#55)]  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.CombinePath%2A>
- <xref:System.IO.FileInfo>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFileInfo%2A>
- [Como: obter a coleção de arquivos em um diretório](how-to-get-the-collection-of-files-in-a-directory.md)
