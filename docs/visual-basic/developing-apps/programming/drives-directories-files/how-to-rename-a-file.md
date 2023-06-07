---
title: 'Como: renomear um arquivo'
ms.date: 07/20/2015
helpviewer_keywords:
- I/O [Visual Basic], renaming files
- files [Visual Basic], renaming
ms.assetid: 0ea7e0c8-2cb2-4bf5-a00d-7b6e3c08a3bc
ms.openlocfilehash: 3de41ee6627315f0e26964b75f564ff98fe472ec
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411584"
---
# <a name="how-to-rename-a-file-in-visual-basic"></a>Como renomear um arquivo no Visual Basic

Use o método `RenameFile` do objeto `My.Computer.FileSystem` para renomear um arquivo, fornecendo o local atual, o nome de arquivo e o novo nome de arquivo. Esse método não pode ser usado para mover um arquivo. Use o método `MoveFile` para mover e renomear o arquivo.  
  
### <a name="to-rename-a-file"></a>Para renomear um arquivo  
  
- Use o método `My.Computer.FileSystem.RenameFile` para renomear um arquivo. Este exemplo renomeia o arquivo chamado `Test.txt` para `SecondTest.txt`.  
  
     [!code-vb[VbVbcnMyFileSystem#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#9)]  
  
 Este exemplo de código também está disponível como um snippet de código do IntelliSense. No selecionador de snippet de código, o snippet está localizado em **Sistema de Arquivos – Processando Unidades, Pastas e Arquivos**. Para obter mais informações, consulte [Snippets de Código](/visualstudio/ide/code-snippets).  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar uma exceção:  
  
- O caminho não é válido por um dos seguintes motivos: é uma cadeia de comprimento zero, contém apenas espaços em branco, contém caracteres inválidos ou é um caminho de dispositivo (começa com \\\\.\\) (<xref:System.ArgumentException>).  
  
- `newName` contém informações de caminho (<xref:System.ArgumentException>).  
  
- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).  
  
- `newName` é `Nothing` ou é uma cadeia de caracteres vazia (<xref:System.ArgumentNullException>).  
  
- O arquivo de origem não é válido ou não existe (<xref:System.IO.FileNotFoundException>).  
  
- Há um arquivo ou diretório existente com o nome especificado em `newName` (<xref:System.IO.IOException>).  
  
- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).  
  
- Um nome de arquivo ou de diretório no caminho contém dois-pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).  
  
- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>).  
  
- O usuário não tem a permissão necessária (<xref:System.UnauthorizedAccessException>).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.RenameFile%2A>
- [Como: mover um arquivo](how-to-move-a-file.md)
- [Criar, excluir e mover arquivos e diretórios](creating-deleting-and-moving-files-and-directories.md)
- [Como: criar uma cópia de um arquivo no mesmo diretório](how-to-create-a-copy-of-a-file-in-the-same-directory.md)
- [Como: criar uma cópia de um arquivo em outro diretório](how-to-create-a-copy-of-a-file-in-a-different-directory.md)
