---
title: 'Como: consultar o número total de bytes em um conjunto de pastas (LINQ)'
ms.date: 07/20/2015
ms.assetid: bfe85ed2-44dc-4ef1-aac7-241622b80a69
ms.openlocfilehash: c6490c8863b5762b0b487c6f3f3b2809e16d6519
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397928"
---
# <a name="how-to-query-for-the-total-number-of-bytes-in-a-set-of-folders-linq-visual-basic"></a>Como consultar o número total de bytes em um conjunto de pastas (LINQ) (Visual Basic)
Este exemplo mostra como recuperar o número total de bytes usado por todos os arquivos em uma pasta especificada e todas as suas subpastas.  
  
## <a name="example"></a>Exemplo  
 O método <xref:System.Linq.Enumerable.Sum%2A> adiciona os valores de todos os itens selecionados na cláusula `select`. Você pode modificar essa consulta para recuperar o maior ou o menor arquivo na árvore de diretório especificada chamando o método <xref:System.Linq.Enumerable.Min%2A> ou <xref:System.Linq.Enumerable.Max%2A> em vez de <xref:System.Linq.Enumerable.Sum%2A>.  
  
```vb  
Module QueryTotalBytes  
    Sub Main()  
  
        ' Change the drive\path if necessary.  
        Dim root As String = "C:\Program Files\Microsoft Visual Studio 9.0\VB"  
  
        'Take a snapshot of the folder contents.  
        ' This method assumes that the application has discovery permissions  
        ' for all folders under the specified path.  
        Dim fileList = My.Computer.FileSystem.GetFiles _  
                  (root, FileIO.SearchOption.SearchAllSubDirectories, "*.*")  
  
        Dim fileQuery = From file In fileList _  
                        Select GetFileLength(file)  
  
        ' Force execution and cache the results to avoid multiple trips to the file system.  
        Dim fileLengths = fileQuery.ToArray()  
  
        ' Find the largest file  
        Dim maxSize = Aggregate aFile In fileLengths Into Max()  
  
        ' Find the total number of bytes  
        Dim totalBytes = Aggregate aFile In fileLengths Into Sum()  
  
        Console.WriteLine("The largest file is " & maxSize & " bytes")  
        Console.WriteLine("There are " & totalBytes & " total bytes in " & _  
                          fileList.Count & " files under " & root)  
  
        ' Keep the console window open in debug mode  
        Console.WriteLine("Press any key to exit.")  
        Console.ReadKey()  
    End Sub  
  
    ' This method is used to catch the possible exception  
    ' that can be raised when accessing the FileInfo.Length property.  
    Function GetFileLength(ByVal filename As String) As Long  
        Dim retval As Long  
        Try  
            Dim fi As New System.IO.FileInfo(filename)  
            retval = fi.Length  
        Catch ex As System.IO.FileNotFoundException  
            ' If a file is no longer present,  
            ' just return zero bytes.
            retval = 0  
        End Try  
  
        Return retval  
    End Function  
End Module  
```  
  
 Se precisar apenas contar o número de bytes em uma árvore de diretório especificada, você pode fazer isso com mais eficiência sem criar uma consulta LINQ, que gera a sobrecarga de criação da coleção de lista como uma fonte de dados. A utilidade da abordagem da LINQ aumenta conforme a consulta se torna mais complexa ou quando você precisa executar várias consultas na mesma fonte de dados.  
  
 A consulta chama um método separado para obter o tamanho de arquivo. Ela faz isso para consumir a possível exceção que será gerada se o arquivo tiver sido excluído em outro thread após o objeto <xref:System.IO.FileInfo> ter sido criado na chamada para `GetFiles`. Embora o objeto <xref:System.IO.FileInfo> já tenha sido criado, a exceção poderá ocorrer porque um objeto <xref:System.IO.FileInfo> tentará atualizar sua propriedade <xref:System.IO.FileInfo.Length%2A> com o tamanho mais atual na primeira vez que a propriedade foi acessada. Ao colocar essa operação em um bloco try-catch fora da consulta, o código segue a regra de evitar operações em consultas que podem causar efeitos colaterais. Em geral, deve-se ter muito cuidado ao consumir exceções para garantir que um aplicativo não seja deixado em um estado desconhecido.  
  
## <a name="compile-the-code"></a>Compilar o código  
Crie um projeto de aplicativo de console Visual Basic, com uma `Imports` instrução para o namespace System. Linq.
  
## <a name="see-also"></a>Confira também

- [LINQ to Objects (Visual Basic)](linq-to-objects.md)
- [LINQ e diretórios de arquivos (Visual Basic)](linq-and-file-directories.md)
