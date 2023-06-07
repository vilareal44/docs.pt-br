---
title: Como ler de um arquivo de texto – guia de programação C#
description: Saiba como ler de um arquivo de texto usando métodos estáticos da classe File. Consulte um exemplo de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
f1_keywords:
- StreamReader.ReadToEnd
helpviewer_keywords:
- text files, writing to
- reading text files
- reading data, text files
- text files, reading
ms.assetid: 92246c5b-e819-4eea-9370-1a9460e12de3
ms.openlocfilehash: 80ac6f8412f456b23d05ee87882dca8e16a132c3
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301652"
---
# <a name="how-to-read-from-a-text-file-c-programming-guide"></a>Como ler de um arquivo de texto (guia de programação C#)
Este exemplo lê o conteúdo de um arquivo de texto usando os métodos estáticos <xref:System.IO.File.ReadAllText%2A> e <xref:System.IO.File.ReadAllLines%2A> da classe <xref:System.IO.File?displayProperty=nameWithType>.  
  
Para obter um exemplo que usa <xref:System.IO.StreamReader> , consulte [como ler um arquivo de texto, uma linha por vez](./how-to-read-a-text-file-one-line-at-a-time.md).
  
> [!NOTE]
> Os arquivos usados neste exemplo são criados no tópico [como gravar em um arquivo de texto](./how-to-write-to-a-text-file.md).
  
## <a name="example"></a>Exemplo  
 [!code-csharp[csFilesandFolders#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#4)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Copie o código e cole-o em um aplicativo de console em C#.  
  
Se você não estiver usando os arquivos de texto de [como gravar em um arquivo de texto](./how-to-write-to-a-text-file.md), substitua o argumento para `ReadAllText` e `ReadAllLines` pelo caminho e nome de arquivo apropriados no computador.
  
## <a name="robust-programming"></a>Programação robusta  
 As seguintes condições podem causar uma exceção:  
  
- O arquivo não existe ou não existe no local especificado. Verifique o caminho e a ortografia do nome do arquivo.  
  
## <a name="net-security"></a>Segurança do .NET  
 Não confie no nome de um arquivo para determinar o conteúdo do arquivo. Por exemplo, o arquivo `myFile.cs` pode não ser um arquivo de origem do C#.  
  
## <a name="see-also"></a>Veja também

- <xref:System.IO?displayProperty=nameWithType>
- [Guia de programação C#](../index.md)
- [Sistema de arquivos e o Registro (Guia de Programação em C#)](./index.md)
