---
title: 'Como: criar uma cópia de um arquivo em outro diretório'
ms.date: 07/20/2015
helpviewer_keywords:
- My.Computer.FileSystem.CopyFile method, copying files [Visual Basic]
- files [Visual Basic], copying
- CopyFile method [Visual Basic], copying files in Visual Basic
- I/O [Visual Basic], copying files
ms.assetid: 88e2145c-d414-45a5-ad03-6f5d58ecca26
ms.openlocfilehash: 3b4b41e105d6bb6a17fbb688aa4718b33c23b9d6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401687"
---
# <a name="how-to-create-a-copy-of-a-file-in-a-different-directory-in-visual-basic"></a>Como criar uma cópia de um arquivo em um diretório diferente no Visual Basic

O método `My.Computer.FileSystem.CopyFile` permite a cópia de arquivos. Os parâmetros desse método oferecem a capacidade de substituir os arquivos existentes, renomear o arquivo, mostrar o progresso da operação e permitir que o usuário cancele a operação.  
  
### <a name="to-copy-a-text-file-to-another-folder"></a>Copiar um arquivo de texto para outra pasta  
  
- Use o método `CopyFile` para copiar um arquivo, especificando o arquivo de origem e o diretório de destino. O parâmetro `overwrite` permite especificar se os arquivos existentes devem ser sobrescritos ou não. Os exemplos de código a seguir demonstram como usar `CopyFile`.  
  
     [!code-vb[VbFileIOMisc#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#24)]  
  
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
- [Como: criar uma cópia de um arquivo no mesmo diretório](how-to-create-a-copy-of-a-file-in-the-same-directory.md)
- [Como: copiar um diretório para outro diretório](how-to-copy-a-directory-to-another-directory.md)
- [Como: renomear um arquivo](how-to-rename-a-file.md)
