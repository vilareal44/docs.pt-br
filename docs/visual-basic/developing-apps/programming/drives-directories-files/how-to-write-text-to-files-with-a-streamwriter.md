---
title: 'Como: gravar texto em arquivos com um StreamWriter'
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], writing to
- text, writing to files
- writing to files [Visual Basic], StreamWriter
ms.assetid: 99762e57-ef46-4dcc-8959-a8f79c22f067
ms.openlocfilehash: 373f3bd07ea61263ddd81037d8cbbb06f789e599
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411571"
---
# <a name="how-to-write-text-to-files-with-a-streamwriter-in-visual-basic"></a>Como gravar texto em arquivos com um StreamWriter no Visual Basic

Este exemplo abre um objeto <xref:System.IO.StreamWriter> com o método `My.Computer.FileSystem.OpenTextFileWriter` e o usa para gravar uma cadeia de caracteres em um arquivo de texto com o método <xref:System.IO.TextWriter.WriteLine%2A> da classe <xref:System.IO.StreamWriter>.  
  
## <a name="example"></a>Exemplo  

 [!code-vb[VbFileIOWrite#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#5)]  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar uma exceção:  
  
- O arquivo existe e é somente leitura (<xref:System.IO.IOException>).  
  
- O disco está cheio (<xref:System.IO.IOException>).  
  
- O nome do caminho é muito longo (<xref:System.IO.PathTooLongException>).  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  

 Este exemplo cria um novo arquivo, se o arquivo ainda não existe. Se um aplicativo precisar criar um arquivo, ele precisará de acesso `Create` para a pasta. Se o arquivo já existe, o aplicativo precisa apenas de acesso `Write`, um privilégio menor. Sempre que possível, é mais seguro criar o arquivo durante a implantação e somente conceder acesso `Read` a um único arquivo, em vez de acesso `Create` a uma pasta.  
  
## <a name="see-also"></a>Confira também

- <xref:System.IO.StreamWriter>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFileWriter%2A>
- [Como: ler de arquivos de texto](how-to-read-from-text-files.md)
- [Gravar em arquivos](writing-to-files.md)
