---
title: 'Como: excluir um arquivo'
ms.date: 07/20/2015
helpviewer_keywords:
- Delete method [Visual Basic]
- files [Visual Basic], deleting
- files [Visual Basic], manipulating
- File object
ms.assetid: 4b721769-3e45-4be7-b7fe-b08dc4141b44
ms.openlocfilehash: 0c8213786b8073d784f1f3ea51417741d5ad4cba
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401648"
---
# <a name="how-to-delete-a-file-in-visual-basic"></a>Como excluir um arquivo no Visual Basic

O método `DeleteFile` do objeto `My.Computer.FileSystem` permite a exclusão de um arquivo. As opções oferecidas são: enviar um arquivo excluído para a **Lixeira**, solicitar do usuário uma confirmação de que o arquivo deve ser excluído e o que fazer quando o usuário cancela a operação.  
  
### <a name="to-delete-a-text-file"></a>Excluir um arquivo de texto  
  
- Use o método `DeleteFile` para excluir o arquivo. O código a seguir demonstra como excluir o arquivo com o nome `test.txt`.  
  
     [!code-vb[VbVbcnMyFileSystem#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#22)]  
  
### <a name="to-delete-a-text-file-and-ask-the-user-to-confirm-that-the-file-should-be-deleted"></a>Excluir um arquivo de texto e solicitar do usuário uma confirmação de que o arquivo deve ser excluído  
  
- Use o método `DeleteFile` para excluir o arquivo, configurando `showUI` para `AllDialogs`. O código a seguir demonstra como excluir o arquivo com o nome `test.txt` e permitir que o usuário confirme se o arquivo deve ser excluído.  
  
     [!code-vb[VbFileIOMisc#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#9)]  
  
### <a name="to-delete-a-text-file-and-send-it-to-the-recycle-bin"></a>Excluir um arquivo de texto e enviá-lo para a Lixeira  
  
- Use o método `DeleteFile` para excluir o arquivo, especificando `SendToRecycleBin` para o parâmetro `recycle`. O código a seguir demonstra como excluir o arquivo com o nome `test.txt` e enviá-lo para a **Lixeira**.  
  
     [!code-vb[VbFileIOMisc#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#10)]  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar uma exceção:  
  
- O caminho não é válido por um dos seguintes motivos: é uma cadeia de comprimento zero, contém apenas espaços em branco, contém caracteres inválidos ou é um caminho de dispositivo (começa com \\\\.\\) (<xref:System.ArgumentException>).  
  
- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).  
  
- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).  
  
- Um nome de pasta no caminho contém dois pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).  
  
- O arquivo está sendo usado (<xref:System.IO.IOException>).  
  
- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>).  
  
- O arquivo não existe (<xref:System.IO.FileNotFoundException>).  
  
- O usuário não tem permissão para excluir o arquivo ou o arquivo é somente leitura (<xref:System.UnauthorizedAccessException>).  
  
- Há uma situação de confiança parcial na qual o usuário não tem permissões suficientes (<xref:System.Security.SecurityException>).  
  
- O usuário cancelou a operação e `onUserCancel` está definido como `ThrowException` (<xref:System.OperationCanceledException>).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.UICancelOption>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.UIOption>
- <xref:Microsoft.VisualBasic.FileIO.RecycleOption>
- [Como: obter a coleção de arquivos em um diretório](how-to-get-the-collection-of-files-in-a-directory.md)
