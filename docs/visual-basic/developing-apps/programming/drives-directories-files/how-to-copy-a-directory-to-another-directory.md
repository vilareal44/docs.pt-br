---
title: 'Como: copiar um diretório para outro diretório'
ms.date: 07/20/2015
helpviewer_keywords:
- I/O [Visual Basic], copying directories
- I/O [Visual Basic], copying folders
- folders [Visual Basic], copying
- directories [Visual Basic], copying
ms.assetid: 2a370bd7-10ba-4219-afc4-4519d031eb6c
ms.openlocfilehash: e28f50f6a812188ac7af801cea691818488bd6cd
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401713"
---
# <a name="how-to-copy-a-directory-to-another-directory-in-visual-basic"></a>Como copiar um diretório para outro diretório no Visual Basic

Use o método <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyDirectory%2A> para copiar um diretório para outro diretório. Esse método copia o conteúdo do diretório, bem como o próprio diretório. Se o diretório de destino não existir, ele será criado. Se existir um diretório com o mesmo nome no local de destino e `overwrite` estiver definido como `False`, o conteúdo dos dois diretórios será mesclado. Você pode especificar um novo nome para o diretório durante a operação.

Ao copiar arquivos em um diretório, podem ser geradas exceções que são causadas pelo arquivo específico, como um arquivo existente durante a mesclagem enquanto `overwrite` é definido como `False`. Quando essas exceções são lançadas, elas são consolidadas em uma única exceção, cuja propriedade `Data` contém entradas em que o caminho do arquivo ou do diretório é a chave e a mensagem de exceção específica está contida no valor correspondente.

## <a name="to-copy-a-directory-to-another-directory"></a>Para copiar um diretório para outro diretório

- Use o método `CopyDirectory`, especificando nomes de diretório de origem e destino. O exemplo a seguir copia o diretório denominado `TestDirectory1` para `TestDirectory2`, substituindo arquivos existentes.

    [!code-vb[VbVbcnMyFileSystem#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#16)]

    Este exemplo de código também está disponível como um snippet de código do IntelliSense. No selecionador de snippet de código, ele está localizado em **Sistema de Arquivos – Processando Unidades, Pastas e Arquivos**. Para obter mais informações, consulte [Snippets de Código](/visualstudio/ide/code-snippets).

## <a name="robust-programming"></a>Programação robusta

As seguintes condições podem causar uma exceção:

- O novo nome especificado para o diretório contém dois pontos (:) ou barra (\ ou /) (<xref:System.ArgumentException>).

- O caminho não é válido por um dos seguintes motivos: é uma cadeia de comprimento zero, contém apenas espaços em branco, contém caracteres inválidos ou é um caminho de dispositivo (começa com \\\\.\\) (<xref:System.ArgumentException>).

- O caminho não é válido porque é `Nothing` (<xref:System.ArgumentNullException>).

- `destinationDirectoryName` é `Nothing` ou é uma cadeia de caracteres vazia (<xref:System.ArgumentNullException>)

- O diretório de origem não existe (<xref:System.IO.DirectoryNotFoundException>).

- O diretório de origem é um diretório raiz (<xref:System.IO.IOException>).

- O caminho combinado aponta para um arquivo existente (<xref:System.IO.IOException>).

- O caminho de origem e o caminho de destino são os mesmos (<xref:System.IO.IOException>).

- `ShowUI` está definido como `UIOption.AllDialogs` e o usuário cancelou a operação ou um ou mais arquivos no diretório não podem ser copiados (<xref:System.OperationCanceledException>).

- A operação é cíclica (<xref:System.InvalidOperationException>).

- O caminho contém dois pontos (:) (<xref:System.NotSupportedException>).

- O caminho excede o comprimento máximo definido pelo sistema (<xref:System.IO.PathTooLongException>).

- Um nome de pasta no caminho contém dois pontos (:) ou está em um formato inválido (<xref:System.NotSupportedException>).

- O usuário não possui permissões necessárias para exibir o caminho (<xref:System.Security.SecurityException>).

- Um arquivo de destino existe, mas não pode ser acessado (<xref:System.UnauthorizedAccessException>).

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyDirectory%2A>
- [Como: localizar subdiretórios com um padrão específico](how-to-find-subdirectories-with-a-specific-pattern.md)
- [Como: obter a coleção de arquivos em um diretório](how-to-get-the-collection-of-files-in-a-directory.md)
