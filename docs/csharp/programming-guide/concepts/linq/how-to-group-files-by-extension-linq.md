---
title: Como agrupar arquivos por extensão (LINQ) (C#)
description: Saiba como usar o LINQ para realizar operações avançadas de agrupamento e classificação em listas de arquivos ou pastas em C#. O exemplo mostra como paginar a saída no console.
ms.date: 07/20/2015
ms.assetid: 21a98320-a5a1-4981-82d8-6a637e7d9018
ms.openlocfilehash: 6113392170063cac1fd89017efaf0c7dad3ba34b
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105037"
---
# <a name="how-to-group-files-by-extension-linq-c"></a>Como agrupar arquivos por extensão (LINQ) (C#)
Este exemplo mostra como o LINQ pode ser usado para realizar operações avançadas de classificação e agrupamento em listas de arquivos ou pastas. Ele também mostra como paginar a saída na janela do console usando os métodos <xref:System.Linq.Enumerable.Skip%2A> e <xref:System.Linq.Enumerable.Take%2A>.  
  
## <a name="example"></a>Exemplo  
 A consulta a seguir mostra como agrupar o conteúdo de uma árvore de diretórios especificada, pela extensão de nome de arquivo.  
  
```csharp  
class GroupByExtension  
{  
    // This query will sort all the files under the specified folder  
    //  and subfolder into groups keyed by the file extension.  
    private static void Main()  
    {  
        // Take a snapshot of the file system.  
        string startFolder = @"c:\program files\Microsoft Visual Studio 9.0\Common7";  
  
        // Used in WriteLine to trim output lines.  
        int trimLength = startFolder.Length;  
  
        // Take a snapshot of the file system.  
        System.IO.DirectoryInfo dir = new System.IO.DirectoryInfo(startFolder);  
  
        // This method assumes that the application has discovery permissions  
        // for all folders under the specified path.  
        IEnumerable<System.IO.FileInfo> fileList = dir.GetFiles("*.*", System.IO.SearchOption.AllDirectories);  
  
        // Create the query.  
        var queryGroupByExt =  
            from file in fileList  
            group file by file.Extension.ToLower() into fileGroup  
            orderby fileGroup.Key  
            select fileGroup;  
  
        // Display one group at a time. If the number of
        // entries is greater than the number of lines  
        // in the console window, then page the output.  
        PageOutput(trimLength, queryGroupByExt);  
    }  
  
    // This method specifically handles group queries of FileInfo objects with string keys.  
    // It can be modified to work for any long listings of data. Note that explicit typing  
    // must be used in method signatures. The groupbyExtList parameter is a query that produces  
    // groups of FileInfo objects with string keys.  
    private static void PageOutput(int rootLength,  
                                    IEnumerable<System.Linq.IGrouping<string, System.IO.FileInfo>> groupByExtList)  
    {  
        // Flag to break out of paging loop.  
        bool goAgain = true;  
  
        // "3" = 1 line for extension + 1 for "Press any key" + 1 for input cursor.  
        int numLines = Console.WindowHeight - 3;  
  
        // Iterate through the outer collection of groups.  
        foreach (var filegroup in groupByExtList)  
        {  
            // Start a new extension at the top of a page.  
            int currentLine = 0;  
  
            // Output only as many lines of the current group as will fit in the window.  
            do  
            {  
                Console.Clear();  
                Console.WriteLine(filegroup.Key == String.Empty ? "[none]" : filegroup.Key);  
  
                // Get 'numLines' number of items starting at number 'currentLine'.  
                var resultPage = filegroup.Skip(currentLine).Take(numLines);  
  
                //Execute the resultPage query  
                foreach (var f in resultPage)  
                {  
                    Console.WriteLine("\t{0}", f.FullName.Substring(rootLength));  
                }  
  
                // Increment the line counter.  
                currentLine += numLines;  
  
                // Give the user a chance to escape.  
                Console.WriteLine("Press any key to continue or the 'End' key to break...");  
                ConsoleKey key = Console.ReadKey().Key;  
                if (key == ConsoleKey.End)  
                {  
                    goAgain = false;  
                    break;  
                }  
            } while (currentLine < filegroup.Count());  
  
            if (goAgain == false)  
                break;  
        }  
    }  
}  
```  
  
 A saída desse programa pode ser longa, dependendo dos detalhes do sistema de arquivos local e o que está definido em `startFolder`. Para habilitar a exibição de todos os resultados, este exemplo mostra como paginá-los. As mesmas técnicas podem ser aplicadas a aplicativos do Windows e aplicativos Web. Observe que, como o código dispõe os itens em um grupo, é necessário um loop `foreach` aninhado. Há também alguma lógica adicional para calcular a posição atual na lista e para permitir que o usuário interrompa a paginação e saia do programa. Nesse caso específico, a consulta de paginação é executada nos resultados da consulta original armazenados em cache. Em outros contextos, como LINQ to SQL, esse cache não é necessário.  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Criar um projeto de aplicativo de console em C# com diretivas `using` para os namespaces System.Linq e System.IO.  
  
## <a name="see-also"></a>Confira também

- [LINQ to Objects (C#)](./linq-to-objects.md)
- [LINQ e diretórios de arquivos (C#)](./linq-and-file-directories.md)
