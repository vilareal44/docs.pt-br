---
title: Como consultar arquivos com um atributo ou nome especificado (C#)
description: Saiba como usar o LINQ em C# para localizar arquivos que têm uma extensão de nome de arquivo especificada em uma árvore de diretório e como retornar o arquivo mais recente ou mais antigo.
ms.date: 07/20/2015
ms.assetid: 560e3879-b0b3-4549-ad02-0a53aff2f83c
ms.openlocfilehash: 9820b96e19d805b792e18ff242e64dfb6cf4a606
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104498"
---
# <a name="how-to-query-for-files-with-a-specified-attribute-or-name-c"></a>Como consultar arquivos com um atributo ou nome especificado (C#)
Este exemplo mostra como localizar todos os arquivos que têm uma extensão de nome de arquivo especificada (por exemplo ".txt") em uma árvore de diretório especificada. Ele também mostra como retornar tanto os arquivos mais recentes como os mais antigo na árvore com base na hora de criação.  
  
## <a name="example"></a>Exemplo  
  
```csharp  
class FindFileByExtension  
{  
    // This query will produce the full path for all .txt files  
    // under the specified folder including subfolders.  
    // It orders the list according to the file name.  
    static void Main()  
    {  
        string startFolder = @"c:\program files\Microsoft Visual Studio 9.0\";  
  
        // Take a snapshot of the file system.  
        System.IO.DirectoryInfo dir = new System.IO.DirectoryInfo(startFolder);  
  
        // This method assumes that the application has discovery permissions  
        // for all folders under the specified path.  
        IEnumerable<System.IO.FileInfo> fileList = dir.GetFiles("*.*", System.IO.SearchOption.AllDirectories);  
  
        //Create the query  
        IEnumerable<System.IO.FileInfo> fileQuery =  
            from file in fileList  
            where file.Extension == ".txt"  
            orderby file.Name  
            select file;  
  
        //Execute the query. This might write out a lot of files!  
        foreach (System.IO.FileInfo fi in fileQuery)  
        {  
            Console.WriteLine(fi.FullName);  
        }  
  
        // Create and execute a new query by using the previous
        // query as a starting point. fileQuery is not
        // executed again until the call to Last()  
        var newestFile =  
            (from file in fileQuery  
             orderby file.CreationTime  
             select new { file.FullName, file.CreationTime })  
            .Last();  
  
        Console.WriteLine("\r\nThe newest .txt file is {0}. Creation time: {1}",  
            newestFile.FullName, newestFile.CreationTime);  
  
        // Keep the console window open in debug mode.  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
  Criar um projeto de aplicativo de console em C# com diretivas `using` para os namespaces System.Linq e System.IO.
  
## <a name="see-also"></a>Confira também

- [LINQ to Objects (C#)](./linq-to-objects.md)
- [LINQ e diretórios de arquivos (C#)](./linq-and-file-directories.md)
