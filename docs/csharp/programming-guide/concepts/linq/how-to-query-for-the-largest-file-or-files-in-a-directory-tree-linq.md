---
title: Como consultar o maior arquivo ou arquivos em uma árvore de diretório (LINQ) (C#)
description: Este exemplo de C# mostra cinco consultas LINQ relacionadas ao tamanho do arquivo em bytes. Você pode modificá-los para consultar uma outra Propriedade do objeto FileInfo.
ms.date: 07/20/2015
ms.assetid: 20c8a917-0552-4514-b489-0b8b6a4c3b4c
ms.openlocfilehash: c06c6017d6fd1efd6412729c5df63a2b819908a6
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104381"
---
# <a name="how-to-query-for-the-largest-file-or-files-in-a-directory-tree-linq-c"></a>Como consultar o maior arquivo ou arquivos em uma árvore de diretório (LINQ) (C#)
Este exemplo mostra cinco consultas relacionadas ao tamanho do arquivo em bytes:  
  
- Como recuperar o tamanho em bytes do maior arquivo.  
  
- Como recuperar o tamanho em bytes do menor arquivo.  
  
- Como recuperar o maior ou menor arquivo do objeto <xref:System.IO.FileInfo> de uma ou mais pastas em uma pasta raiz especificada.  
  
- Como recuperar uma sequência, como os 10 maiores arquivos.  
  
- Como ordenar os arquivos em grupos com base no tamanho do arquivo em bytes, ignorando arquivos menores do que um tamanho especificado.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir contém cinco consultas separadas que mostram como consultar e agrupar arquivos, dependendo do tamanho do arquivo em bytes. Você pode modificar facilmente esses exemplos para basear a consulta em outra propriedade do objeto <xref:System.IO.FileInfo>.  
  
```csharp  
class QueryBySize  
{  
    static void Main(string[] args)  
    {  
        QueryFilesBySize();  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
  
    private static void QueryFilesBySize()  
    {  
        string startFolder = @"c:\program files\Microsoft Visual Studio 9.0\";  
  
        // Take a snapshot of the file system.  
        System.IO.DirectoryInfo dir = new System.IO.DirectoryInfo(startFolder);  
  
        // This method assumes that the application has discovery permissions  
        // for all folders under the specified path.  
        IEnumerable<System.IO.FileInfo> fileList = dir.GetFiles("*.*", System.IO.SearchOption.AllDirectories);  
  
        //Return the size of the largest file  
        long maxSize =  
            (from file in fileList  
             let len = GetFileLength(file)  
             select len)  
             .Max();  
  
        Console.WriteLine("The length of the largest file under {0} is {1}",  
            startFolder, maxSize);  
  
        // Return the FileInfo object for the largest file  
        // by sorting and selecting from beginning of list  
        System.IO.FileInfo longestFile =  
            (from file in fileList  
             let len = GetFileLength(file)  
             where len > 0  
             orderby len descending  
             select file)  
            .First();  
  
        Console.WriteLine("The largest file under {0} is {1} with a length of {2} bytes",  
                            startFolder, longestFile.FullName, longestFile.Length);  
  
        //Return the FileInfo of the smallest file  
        System.IO.FileInfo smallestFile =  
            (from file in fileList  
             let len = GetFileLength(file)  
             where len > 0  
             orderby len ascending  
             select file).First();  
  
        Console.WriteLine("The smallest file under {0} is {1} with a length of {2} bytes",  
                            startFolder, smallestFile.FullName, smallestFile.Length);  
  
        //Return the FileInfos for the 10 largest files  
        // queryTenLargest is an IEnumerable<System.IO.FileInfo>  
        var queryTenLargest =  
            (from file in fileList  
             let len = GetFileLength(file)  
             orderby len descending  
             select file).Take(10);  
  
        Console.WriteLine("The 10 largest files under {0} are:", startFolder);  
  
        foreach (var v in queryTenLargest)  
        {  
            Console.WriteLine("{0}: {1} bytes", v.FullName, v.Length);  
        }  
  
        // Group the files according to their size, leaving out  
        // files that are less than 200000 bytes.
        var querySizeGroups =  
            from file in fileList  
            let len = GetFileLength(file)  
            where len > 0  
            group file by (len / 100000) into fileGroup  
            where fileGroup.Key >= 2  
            orderby fileGroup.Key descending  
            select fileGroup;  
  
        foreach (var filegroup in querySizeGroups)  
        {  
            Console.WriteLine(filegroup.Key.ToString() + "00000");  
            foreach (var item in filegroup)  
            {  
                Console.WriteLine("\t{0}: {1}", item.Name, item.Length);  
            }  
        }  
    }  
  
    // This method is used to swallow the possible exception  
    // that can be raised when accessing the FileInfo.Length property.  
    // In this particular case, it is safe to swallow the exception.  
    static long GetFileLength(System.IO.FileInfo fi)  
    {  
        long retval;  
        try  
        {  
            retval = fi.Length;  
        }  
        catch (System.IO.FileNotFoundException)  
        {  
            // If a file is no longer present,  
            // just add zero bytes to the total.  
            retval = 0;  
        }  
        return retval;  
    }  
  
}  
```  
  
 Para retornar um ou mais objetos <xref:System.IO.FileInfo> completos, a consulta deve primeiro examinar cada um dos objetos na fonte de dados e, em seguida, classificá-los segundo o valor de sua propriedade Length. Em seguida, ela pode retornar um único elemento ou a sequência com os maiores tamanhos. Use <xref:System.Linq.Enumerable.First%2A> para retornar o primeiro elemento em uma lista. Use <xref:System.Linq.Enumerable.Take%2A> para retornar o primeiro número n de elementos. Especifique uma ordem de classificação decrescente para colocar os menores elementos no início da lista.  
  
 A consulta chama um método separado para obter o tamanho do arquivo em bytes para consumir a exceção possível que ocorrerá caso um arquivo tenha sido excluído em outro thread no período desde que o objeto <xref:System.IO.FileInfo> foi criado na chamada para `GetFiles`. Embora o objeto <xref:System.IO.FileInfo> já tenha sido criado, a exceção poderá ocorrer porque um objeto <xref:System.IO.FileInfo> tentará atualizar sua propriedade <xref:System.IO.FileInfo.Length%2A> usando o tamanho mais atual em bytes na primeira vez que a propriedade foi acessada. Ao colocar essa operação em um bloco try-catch fora da consulta, nós seguimos a regra de evitar operações em consultas que podem causar efeitos colaterais. Em geral, deve-se ter muito cuidado ao consumir exceções para garantir que um aplicativo não seja deixado em um estado desconhecido.  
  
## <a name="compiling-the-code"></a>Compilando o código  
Criar um projeto de aplicativo de console em C# com diretivas `using` para os namespaces System.Linq e System.IO.

## <a name="see-also"></a>Confira também

- [LINQ to Objects (C#)](./linq-to-objects.md)
- [LINQ e diretórios de arquivos (C#)](./linq-and-file-directories.md)
