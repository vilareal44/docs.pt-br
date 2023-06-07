---
title: 'Como: gravar texto em arquivos'
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], writing to
- text, writing to files
- writing to files [Visual Basic]
- examples [Visual Basic], text files
ms.assetid: 304956eb-530d-4df7-b48f-9b4d1f2581a0
ms.openlocfilehash: f95a0c4df4a2eab0069a6dab0d4c3fa338d1d8ef
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411532"
---
# <a name="how-to-write-text-to-files-in-visual-basic"></a>Como gravar texto em arquivos no Visual Basic

O método <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A> pode ser usado para gravar texto em arquivos. Se o arquivo especificado não existir, ele será criado.  
  
## <a name="procedure"></a>Procedimento  
  
#### <a name="to-write-text-to-a-file"></a>Para gravar texto em um arquivo  
  
- Use o método `WriteAllText` para gravar texto em um arquivo, especificando o arquivo e o texto a ser gravado. Este exemplo grava a linha `"This is new text."` no arquivo chamado `test.txt`, anexando o texto a qualquer texto existente no arquivo.  
  
     [!code-vb[VbFileIOWrite#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#3)]  
  
#### <a name="to-write-a-series-of-strings-to-a-file"></a>Para gravar uma série de cadeias de caracteres em um arquivo  
  
- Faça um loop na coleção de cadeia de caracteres. Use o método `WriteAllText` para gravar texto em um arquivo, especificando o arquivo de destino e a cadeia de caracteres a ser adicionada e configurando `append` como `True`.  
  
     Este exemplo grava os nomes dos arquivos no diretório `Documents and Settings` em `FileList.txt`, inserindo um retorno de carro entre cada um para obter melhor legibilidade.  
  
     [!code-vb[VbFileIOWrite#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#4)]  
  
## <a name="robust-programming"></a>Programação robusta  

 As seguintes condições podem causar uma exceção:  
  
- O caminho não é válido por um dos seguintes motivos: é uma cadeia de comprimento zero, contém apenas espaços em branco, contém caracteres inválidos ou é um caminho de dispositivo (começa com \\\\.\\) (<xref:System.ArgumentException>).  
  
- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).  
  
- `File` aponta para um caminho que não existe (<xref:System.IO.FileNotFoundException> ou <xref:System.IO.DirectoryNotFoundException>).  
  
- O arquivo está sendo usado por outro processo, ou ocorre um erro de E/S (<xref:System.IO.IOException>).  
  
- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).  
  
- Um nome de arquivo ou de diretório no caminho contém dois-pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).  
  
- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>).  
  
- O disco está cheio e a chamada a `WriteAllText` falha (<xref:System.IO.IOException>).  
  
 Se você estiver executando em um contexto de confiança parcial, o código pode gerar uma exceção devido a privilégios insuficientes. Para obter mais informações, consulte [Noções Básicas da Segurança de Acesso do Código](../../../../framework/misc/code-access-security-basics.md).  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A>
- [Como: ler de arquivos de texto](how-to-read-from-text-files.md)
