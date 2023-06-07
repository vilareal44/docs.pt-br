---
title: 'Como: criar uma cópia de um arquivo no mesmo diretório'
ms.date: 07/20/2015
f1_keywords:
- File.Copy
helpviewer_keywords:
- My.Computer.FileSystem.CopyFile method, copying files [Visual Basic]
- files [Visual Basic], copying
- CopyFile method [Visual Basic], copying files in Visual Basic
- I/O [Visual Basic], copying files
ms.assetid: b2fdda86-e666-42c2-9706-9527e9fa68ff
ms.openlocfilehash: 741f0c80ba268369ebdd598460e9d5fa13d09571
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401674"
---
# <a name="how-to-create-a-copy-of-a-file-in-the-same-directory-in-visual-basic"></a>Como criar uma cópia de um arquivo no mesmo diretório no Visual Basic

Use o método `My.Computer.FileSystem.CopyFile` para copiar arquivos. Os parâmetros permitem substituir os arquivos existentes, renomear o arquivo, mostrar o progresso da operação e permitir que o usuário cancele a operação.  
  
### <a name="to-create-a-copy-of-a-file-in-the-same-folder"></a>Para criar uma cópia de um arquivo na mesma pasta  
  
- Use o método `CopyFile`, fornecendo o arquivo e o local de destino. O exemplo a seguir cria uma cópia do `test.txt` chamada `test2.txt`.  
  
     [!code-vb[VbVbcnMyFileSystem#51](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#51)]  
  
### <a name="to-create-a-copy-of-a-file-in-the-same-folder-overwriting-existing-files"></a>Para criar uma cópia de um arquivo na mesma pasta, sobrescrevendo arquivos existentes  
  
- Use o método `CopyFile`, fornecendo o arquivo e o local de destino e configurando `overwrite` como `True`. O exemplo a seguir cria uma cópia do `test.txt` com o nome `test2.txt` e substitui os arquivos existentes com este nome.  
  
     [!code-vb[VbVbcnMyFileSystem#52](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#52)]  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar o lançamento de uma exceção:  
  
- O caminho não é válido por um dos seguintes motivos: é uma cadeia de comprimento zero, contém apenas espaços em branco, contém caracteres inválidos ou é um caminho de dispositivo (começa com \\\\.\\) (<xref:System.ArgumentException>).  
  
- O sistema não conseguiu recuperar o caminho absoluto (<xref:System.ArgumentException>).  
  
- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).  
  
- O arquivo de origem não é válido ou não existe (<xref:System.IO.FileNotFoundException>).  
  
- O caminho combinado aponta para um diretório existente (<xref:System.IO.IOException>).  
  
- O arquivo de destino já existe e `overwrite` está definido como `False` (<xref:System.IO.IOException>).  
  
- O usuário não tem permissões suficientes para acessar o arquivo (<xref:System.IO.IOException>).  
  
- Um arquivo na pasta de destino com o mesmo nome está sendo usado (<xref:System.IO.IOException>).  
  
- Um nome de pasta no caminho contém dois pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).  
  
- `ShowUI` está definido como `True`, `onUserCancel` está definido como `ThrowException` e o usuário cancelou a operação (<xref:System.OperationCanceledException>).  
  
- `ShowUI` está definido como `True`, `onUserCancel` está definido como `ThrowException` e ocorreu um erro de E/S não especificado (<xref:System.OperationCanceledException>).  
  
- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).  
  
- O usuário não tem a permissão necessária (<xref:System.UnauthorizedAccessException>).  
  
- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyFile%2A>
- <xref:Microsoft.VisualBasic.FileIO.UICancelOption>
- [Como: copiar arquivos com um padrão específico para um diretório](how-to-copy-files-with-a-specific-pattern-to-a-directory.md)
- [Como: criar uma cópia de um arquivo em outro diretório](how-to-create-a-copy-of-a-file-in-a-different-directory.md)
- [Como: copiar um diretório para outro diretório](how-to-copy-a-directory-to-another-directory.md)
- [Como: renomear um arquivo](how-to-rename-a-file.md)
