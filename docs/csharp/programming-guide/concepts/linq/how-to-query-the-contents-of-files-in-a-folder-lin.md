---
title: Como consultar o conteúdo de arquivos de texto em uma pasta (LINQ) (C#)
description: Saiba como usar o LINQ em C# para consultar todos os arquivos em uma árvore de diretório, abrir cada arquivo e inspecionar seu conteúdo.
ms.date: 07/20/2015
ms.assetid: f5b4dce7-1a34-4eb4-9bf1-60d5bdda264c
ms.openlocfilehash: 216edc2ee6fc43fd06a3c89b1b6b73f693f752f8
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104268"
---
# <a name="how-to-query-the-contents-of-text-files-in-a-folder-linq-c"></a>Como consultar o conteúdo de arquivos de texto em uma pasta (LINQ) (C#)
Este exemplo mostra como consultar todos os arquivos em uma árvore de diretório especificada, abrir cada arquivo e inspecionar seu conteúdo. Este tipo de técnica pode ser usado para criar índices ou inverter os índices do conteúdo de uma árvore de diretório. Uma pesquisa de cadeia de caracteres simples é executada neste exemplo. No entanto, os tipos de correspondência de padrões mais complexos podem ser executados com uma expressão regular. Para obter mais informações, consulte [como combinar consultas LINQ com expressões regulares (C#)](./how-to-combine-linq-queries-with-regular-expressions.md).  
  
## <a name="example"></a>Exemplo  
  
```csharp  
class QueryContents  
{  
    public static void Main()  
    {  
        // Modify this path as necessary.  
        string startFolder = @"c:\program files\Microsoft Visual Studio 9.0\";  
  
        // Take a snapshot of the file system.  
        System.IO.DirectoryInfo dir = new System.IO.DirectoryInfo(startFolder);  
  
        // This method assumes that the application has discovery permissions  
        // for all folders under the specified path.  
        IEnumerable<System.IO.FileInfo> fileList = dir.GetFiles("*.*", System.IO.SearchOption.AllDirectories);  
  
        string searchTerm = @"Visual Studio";  
  
        // Search the contents of each file.  
        // A regular expression created with the RegEx class  
        // could be used instead of the Contains method.  
        // queryMatchingFiles is an IEnumerable<string>.  
        var queryMatchingFiles =  
            from file in fileList  
            where file.Extension == ".htm"  
            let fileText = GetFileText(file.FullName)  
            where fileText.Contains(searchTerm)  
            select file.FullName;  
  
        // Execute the query.  
        Console.WriteLine("The term \"{0}\" was found in:", searchTerm);  
        foreach (string filename in queryMatchingFiles)  
        {  
            Console.WriteLine(filename);  
        }  
  
        // Keep the console window open in debug mode.  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
  
    // Read the contents of the file.  
    static string GetFileText(string name)  
    {  
        string fileContents = String.Empty;  
  
        // If the file has been deleted since we took
        // the snapshot, ignore it and return the empty string.  
        if (System.IO.File.Exists(name))  
        {  
            fileContents = System.IO.File.ReadAllText(name);  
        }  
        return fileContents;  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
Criar um projeto de aplicativo de console em C# com diretivas `using` para os namespaces System.Linq e System.IO.
  
## <a name="see-also"></a>Confira também

- [LINQ e diretórios de arquivos (C#)](./linq-and-file-directories.md)
- [LINQ to Objects (C#)](./linq-to-objects.md)
