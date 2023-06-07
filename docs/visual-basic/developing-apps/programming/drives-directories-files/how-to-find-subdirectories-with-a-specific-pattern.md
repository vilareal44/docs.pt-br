---
title: 'Como: localizar subdiretórios com um padrão específico'
ms.date: 07/20/2015
helpviewer_keywords:
- pattern matching
- folders, finding
ms.assetid: c9265fd1-7483-4150-8b7f-ff642caa939d
ms.openlocfilehash: 5b57914a518b568732955e5c73bb2031824c84dd
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401622"
---
# <a name="how-to-find-subdirectories-with-a-specific-pattern-in-visual-basic"></a>Como localizar subdiretórios com um padrão específico no Visual Basic

O método <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetDirectories%2A> retorna uma coleção somente leitura de cadeias de caracteres que representam os nomes de caminho para os subdiretórios em um diretório. É possível usar o parâmetro `wildCards` para especificar um padrão específico. Caso deseje incluir os conteúdos dos subdiretórios na pesquisa, defina o parâmetro `searchType` para `SearchOption.SearchAllSubDirectories`.

Uma coleção vazia será retornada se nenhum diretório correspondente ao padrão especificado for encontrado.

## <a name="to-find-subdirectories-with-a-specific-pattern"></a>Localizar subdiretórios com um padrão específico

Use o método `GetDirectories`, fornecendo o nome e o caminho do diretório a ser pesquisado. O exemplo a seguir retorna todos os diretórios na estrutura de diretório que contém a palavra “Logs” em seu nome e as adiciona a `ListBox1`.

[!code-vb[VbVbcnFileAccess#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnFileAccess/VB/Class1.vb#1)]

## <a name="robust-programming"></a>Programação robusta

As seguintes condições podem causar uma exceção:

- O caminho não é válido por um dos seguintes motivos: é uma cadeia de comprimento zero, contém apenas espaços em branco, contém caracteres inválidos ou é um caminho de dispositivo (começa com \\\\.\\) (<xref:System.ArgumentException>).

- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).

- Um ou mais dos caracteres curinga especificados é `Nothing`, uma cadeia de caracteres vazia ou contém somente espaços (<xref:System.ArgumentNullException>).

- `directory` não existe (<xref:System.IO.DirectoryNotFoundException>).

- `directory` aponta para um arquivo existente (<xref:System.IO.IOException>).

- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).

- Um nome de pasta no caminho contém dois pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).

- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>).

- O usuário não possui as permissões necessárias (<xref:System.UnauthorizedAccessException>).

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetDirectories%2A>
- [Como: localizar arquivos com um padrão específico](how-to-find-files-with-a-specific-pattern.md)
