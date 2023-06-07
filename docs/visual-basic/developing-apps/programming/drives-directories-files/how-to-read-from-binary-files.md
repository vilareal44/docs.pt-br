---
title: 'Como: ler de arquivos binários'
ms.date: 07/20/2015
helpviewer_keywords:
- binary files [Visual Basic], reading from
- I/O [Visual Basic], reading from binary files
- ReadAllBytes method [Visual Basic], reading from binary files
- My.Computer.FileSystem object, reading from binary files
ms.assetid: d2b1269e-24b6-42e0-9414-ae708db282d8
ms.openlocfilehash: 3b4108034b86d99143fff6943e68ca0077ebd21b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359923"
---
# <a name="how-to-read-from-binary-files-in-visual-basic"></a>Como ler a partir de arquivos binários no Visual Basic

O objeto `My.Computer.FileSystem` fornece o método `ReadAllBytes` para ler arquivos binários.  
  
### <a name="to-read-from-a-binary-file"></a>Para ler em um arquivo binário  
  
- Use o método `ReadAllBytes`, que retorna o conteúdo de um arquivo como uma matriz de bytes. Este exemplo lê do arquivo `C:/Documents and Settings/selfportrait.jpg`.  
  
     [!code-vb[VbVbcnMyFileSystem#78](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#78)]  
  
- Para arquivos binários grandes, é possível usar o método <xref:System.IO.FileStream.Read%2A> do objeto <xref:System.IO.FileStream> para ler apenas uma quantidade especificada do arquivo por vez. Assim, é possível limitar quanto do arquivo é carregado na memória para cada operação de leitura. O exemplo de código a seguir copia um arquivo e permite que o chamador especifique quanto do arquivo é lido na memória por operação de leitura.  
  
     [!code-vb[VbVbcnMyFileSystem#91](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#91)]  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar o lançamento de uma exceção:  
  
- O caminho não é válido por um destes motivos: é uma cadeia de caracteres de comprimento zero, contém somente espaço em branco, contém caracteres inválidos ou é um caminho de dispositivo (<xref:System.ArgumentException>).  
  
- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).  
  
- O arquivo não existe (<xref:System.IO.FileNotFoundException>).  
  
- O arquivo está sendo usado por outro processo, ou ocorre um erro de E/S (<xref:System.IO.IOException>).  
  
- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).  
  
- Um nome de arquivo ou de diretório no caminho contém dois-pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).  
  
- Não há memória suficiente para gravar a cadeia de caracteres no buffer (<xref:System.OutOfMemoryException>).  
  
- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>).  
  
 Não tome decisões sobre o conteúdo do arquivo com base no nome do arquivo. Por exemplo, o arquivo Form1.vb pode não ser um arquivo de código-fonte do Visual Basic.  
  
 Verifique todas as entradas antes de usar os dados no seu aplicativo. O conteúdo do arquivo pode não ser esperado, e os métodos para ler o arquivo podem falhar.  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.ReadAllBytes%2A>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllBytes%2A>
- [Ler arquivos](reading-from-files.md)
- [Como: ler de arquivos de texto com vários formatos](how-to-read-from-text-files-with-multiple-formats.md)
- [Armazenar dados e ler da área de transferência](../computer-resources/storing-data-to-and-reading-from-the-clipboard.md)
