---
title: 'Como: acrescentar a arquivos de texto'
ms.date: 07/20/2015
helpviewer_keywords:
- I/O [Visual Basic], appending to files
- I/O [Visual Basic], My.Computer.FileSystem.WriteAllText method
- I/O [Visual Basic], WriteAllText method
ms.assetid: bbbd7fb5-f169-41a9-b53f-520ea9613913
ms.openlocfilehash: 53b2e4c9030e9d1dd81a4121ad3f92aad796e3e1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359949"
---
# <a name="how-to-append-to-text-files-in-visual-basic"></a>Como anexar a arquivos de texto no Visual Basic

O método <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A> pode ser usado para anexar a um arquivo de texto especificando que o parâmetro `append` seja definido como `True`.  
  
### <a name="to-append-to-a-text-file"></a>Para acrescentar um arquivo de texto  
  
- Use o método `WriteAllText`, especificando o arquivo de destino e a cadeia de caracteres a ser acrescentada e configuração o parâmetro `append` como `True`.  
  
     Este exemplo grava cadeia de caracteres `"This is a test string."` no nomeado `Testfile.txt`.  
  
     [!code-vb[VbFileIOWrite#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#6)]  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar uma exceção:  
  
- O caminho não é válido por um dos seguintes motivos: é uma cadeia de comprimento zero, contém apenas espaços em branco, contém caracteres inválidos ou é um caminho de dispositivo (começa com \\\\.\\) (<xref:System.ArgumentException>).  
  
- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).  
  
- `File` aponta para um caminho que não existe (<xref:System.IO.FileNotFoundException> ou <xref:System.IO.DirectoryNotFoundException>).  
  
- O arquivo está sendo usado por outro processo, ou ocorre um erro de E/S (<xref:System.IO.IOException>).  
  
- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).  
  
- Um nome de arquivo ou de diretório no caminho contém dois-pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).  
  
- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- [Gravar em arquivos](writing-to-files.md)
