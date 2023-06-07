---
title: 'Como: abrir e anexar a um arquivo de log'
description: Abra e acrescente a um arquivo de log usando as classes StreamWriter e StreamReader no .NET, que gravam caracteres e lêem caracteres de fluxos.
ms.date: 01/21/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- log files, opening
- streams, opening and appending to log file
- log files, appending to
- I/O [.NET Framework], log files
ms.assetid: 74423362-1721-49cb-aa0a-e04005f72a06
ms.openlocfilehash: a66dadd24cc327824e91df733f11a23112cd384a
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769165"
---
# <a name="how-to-open-and-append-to-a-log-file"></a>Como: abrir e anexar a um arquivo de log
<xref:System.IO.StreamWriter> e <xref:System.IO.StreamReader> gravam caracteres e leem caracteres de fluxos. O exemplo de código a seguir abre o arquivo *log.txt* para a entrada ou cria o arquivo caso ele ainda não exista e acrescenta informações de log ao final do arquivo. Em seguida, o exemplo grava o conteúdo do arquivo na saída padrão para exibição.

Como alternativa para esse exemplo, você pode armazenar as informações como uma única cadeia de caracteres ou uma matriz de cadeia de caracteres e usar o método <xref:System.IO.File.WriteAllText%2A?displayProperty=nameWithType> ou <xref:System.IO.File.WriteAllLines%2A?displayProperty=nameWithType> para obter a mesma funcionalidade.  
  
> [!NOTE]
> Os usuários do Visual Basic podem optar por usar os métodos e propriedades fornecidas pela classe <xref:Microsoft.VisualBasic.Logging.Log> ou pela classe <xref:Microsoft.VisualBasic.FileIO.FileSystem> para criar ou gravar em arquivos de log.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[Conceptual.BasicIO.TextFiles#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/source2.cs#2)]
 [!code-vb[Conceptual.BasicIO.TextFiles#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/source2.vb#2)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.IO.StreamWriter>  
- <xref:System.IO.StreamReader>  
- <xref:System.IO.File.AppendText%2A?displayProperty=nameWithType>  
- <xref:System.IO.File.OpenText%2A?displayProperty=nameWithType>  
- <xref:System.IO.StreamReader.ReadLine%2A?displayProperty=nameWithType>  
- [Como: enumerar diretórios e arquivos](how-to-enumerate-directories-and-files.md)  
- [Como: ler e gravar em um arquivo de dados recém-criado](how-to-read-and-write-to-a-newly-created-data-file.md)  
- [Como: ler texto de um arquivo](how-to-read-text-from-a-file.md)  
- [Como: gravar texto em um arquivo](how-to-write-text-to-a-file.md)  
- [Como: ler caracteres de uma cadeia de caracteres](how-to-read-characters-from-a-string.md)  
- [Como: gravar caracteres em uma cadeia de caracteres](how-to-write-characters-to-a-string.md)  
- [E/S de arquivo e de fluxo](index.md)
