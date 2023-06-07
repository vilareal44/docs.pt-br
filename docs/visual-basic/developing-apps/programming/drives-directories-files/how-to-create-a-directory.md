---
title: 'Como: criar um diretório'
ms.date: 07/20/2015
helpviewer_keywords:
- directories [Visual Basic], creating
- folders [Visual Basic], creating
ms.assetid: 0351a2ca-24d8-43b5-bb39-9b99e6401cff
ms.openlocfilehash: 0da915054a2e38c778f15bc0b472fe9b02521189
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401661"
---
# <a name="how-to-create-a-directory-in-visual-basic"></a>Como criar um diretório no Visual Basic

Use o método `CreateDirectory` do objeto `My.Computer.FileSystem` para criar diretórios.  
  
 Se o diretório já existir, nenhuma exceção será lançada.  
  
### <a name="to-create-a-directory"></a>Criar um diretório  
  
- Use o método `CreateDirectory` especificando o caminho completo do local em que o diretório deverá ser criado. Este exemplo cria o diretório `NewDirectory` em `C:\Documents and Settings\All Users\Documents`.  
  
     [!code-vb[VbVbcnMyFileSystem#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#2)]  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar uma exceção:  
  
- O nome do diretório está malformado. Por exemplo, ele contém caracteres inválidos ou é somente um espaço em branco (<xref:System.ArgumentException>).  
  
- O diretório pai do diretório a ser criado é somente leitura (<xref:System.IO.IOException>).  
  
- O nome do diretório é `Nothing` (<xref:System.ArgumentNullException>).  
  
- O nome do diretório é muito longo (<xref:System.IO.PathTooLongException>).  
  
- O nome do diretório é um sinal de dois pontos ":" (<xref:System.NotSupportedException>).  
  
- O usuário não tem permissão para criar o diretório (<xref:System.UnauthorizedAccessException>).  
  
- O usuário não possui permissões em uma situação de confiança parcial (<xref:System.Security.SecurityException>).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.CreateDirectory%2A>
- [Criar, excluir e mover arquivos e diretórios](creating-deleting-and-moving-files-and-directories.md)
