---
title: Como obter informações sobre arquivos, pastas e unidades – guia de programação em C#
description: Saiba como obter informações sobre arquivos, pastas e unidades. Consulte um exemplo de código e recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- files [C#], getting information about
ms.assetid: 22fc2da6-5494-405b-995e-c0b99142a93e
ms.openlocfilehash: f696cd90f197bede1a64949d211a563ce9a18376
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87299923"
---
# <a name="how-to-get-information-about-files-folders-and-drives--c-programming-guide"></a>Como obter informações sobre arquivos, pastas e unidades (guia de programação C#)
No .NET, você pode acessar as informações do sistema de arquivos usando as seguintes classes:  
  
- <xref:System.IO.FileInfo?displayProperty=nameWithType>  
  
- <xref:System.IO.DirectoryInfo?displayProperty=nameWithType>  
  
- <xref:System.IO.DriveInfo?displayProperty=nameWithType>  
  
- <xref:System.IO.Directory?displayProperty=nameWithType>  
  
- <xref:System.IO.File?displayProperty=nameWithType>  
  
 As classes <xref:System.IO.FileInfo> e <xref:System.IO.DirectoryInfo> representam um arquivo ou diretório e contêm propriedades que expõem muitos dos atributos de arquivo que têm suporte pelo sistema de arquivos NTFS. Elas também contêm métodos para abrir, fechar, mover e excluir arquivos e pastas. Você pode criar instâncias dessas classes passando uma cadeia de caracteres que representa o nome do arquivo, pasta ou unidade para o construtor:  
  
```csharp  
System.IO.DriveInfo di = new System.IO.DriveInfo(@"C:\");  
```  
  
 Você também pode obter os nomes das unidades, pastas ou arquivos por meio de chamadas para <xref:System.IO.DirectoryInfo.GetDirectories%2A?displayProperty=nameWithType>, <xref:System.IO.DirectoryInfo.GetFiles%2A?displayProperty=nameWithType> e <xref:System.IO.DriveInfo.RootDirectory%2A?displayProperty=nameWithType>.  
  
 As classes <xref:System.IO.Directory?displayProperty=nameWithType> e <xref:System.IO.File?displayProperty=nameWithType> fornecem métodos estáticos para recuperar informações sobre arquivos e diretórios.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra várias maneiras de acessar informações sobre arquivos e pastas.  
  
 [!code-csharp[csFilesandFolders#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#6)]  
  
## <a name="robust-programming"></a>Programação robusta  
 Quando você processa cadeias de caracteres do caminho especificado pelo usuário, você também deve tratar exceções para as seguintes condições:  
  
- O nome do arquivo está malformado. Por exemplo, ele contém caracteres inválidos ou somente espaço em branco.  
  
- O nome do arquivo é nulo.  
  
- O nome de arquivo é maior que o comprimento máximo definido pelo sistema.  
  
- O nome de arquivo contém dois-pontos (:).  
  
 Se o aplicativo não tem permissões suficientes para ler o arquivo especificado, o método `Exists` retorna `false` independentemente de se um caminho existe, o método não gera uma exceção.  
  
## <a name="see-also"></a>Confira também

- <xref:System.IO?displayProperty=nameWithType>
- [Guia de programação C#](../index.md)
- [Sistema de arquivos e o Registro (Guia de Programação em C#)](./index.md)
