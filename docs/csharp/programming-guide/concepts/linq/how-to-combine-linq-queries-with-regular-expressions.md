---
title: Como combinar consultas LINQ com expressões regulares (C#)
description: Este exemplo cria uma expressão regular para correspondência em cadeias de caracteres de texto usando a classe Regex em C#.
ms.date: 07/20/2015
ms.assetid: 6b003b65-20a4-4ca2-929e-2ee3f215aecc
ms.openlocfilehash: af63d096e3c2f19ed557180d82d606989a016120
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105344"
---
# <a name="how-to-combine-linq-queries-with-regular-expressions-c"></a>Como combinar consultas LINQ com expressões regulares (C#)
Este exemplo mostra como usar a classe <xref:System.Text.RegularExpressions.Regex> para criar uma expressão regular para correspondências mais complexas em cadeias de texto. A consulta LINQ torna fácil a aplicação de filtro exatamente nos arquivos que você deseja pesquisar com a expressão regular e formatar os resultados.  
  
## <a name="example"></a>Exemplo  
  
```csharp  
class QueryWithRegEx  
{  
    public static void Main()  
    {  
        // Modify this path as necessary so that it accesses your version of Visual Studio.  
        string startFolder = @"C:\Program Files (x86)\Microsoft Visual Studio 14.0\";  
        // One of the following paths may be more appropriate on your computer.  
        //string startFolder = @"C:\Program Files (x86)\Microsoft Visual Studio\2017\";  
  
        // Take a snapshot of the file system.  
        IEnumerable<System.IO.FileInfo> fileList = GetFiles(startFolder);  
  
        // Create the regular expression to find all things "Visual".  
        System.Text.RegularExpressions.Regex searchTerm =  
            new System.Text.RegularExpressions.Regex(@"Visual (Basic|C#|C\+\+|Studio)");  
  
        // Search the contents of each .htm file.  
        // Remove the where clause to find even more matchedValues!  
        // This query produces a list of files where a match  
        // was found, and a list of the matchedValues in that file.  
        // Note: Explicit typing of "Match" in select clause.  
        // This is required because MatchCollection is not a
        // generic IEnumerable collection.  
        var queryMatchingFiles =  
            from file in fileList  
            where file.Extension == ".htm"  
            let fileText = System.IO.File.ReadAllText(file.FullName)  
            let matches = searchTerm.Matches(fileText)  
            where matches.Count > 0  
            select new  
            {  
                name = file.FullName,  
                matchedValues = from System.Text.RegularExpressions.Match match in matches  
                                select match.Value  
            };  
  
        // Execute the query.  
        Console.WriteLine("The term \"{0}\" was found in:", searchTerm.ToString());  
  
        foreach (var v in queryMatchingFiles)  
        {  
            // Trim the path a bit, then write
            // the file name in which a match was found.  
            string s = v.name.Substring(startFolder.Length - 1);  
            Console.WriteLine(s);  
  
            // For this file, write out all the matching strings  
            foreach (var v2 in v.matchedValues)  
            {  
                Console.WriteLine("  " + v2);  
            }  
        }  
  
        // Keep the console window open in debug mode  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
  
    // This method assumes that the application has discovery
    // permissions for all folders under the specified path.  
    static IEnumerable<System.IO.FileInfo> GetFiles(string path)  
    {  
        if (!System.IO.Directory.Exists(path))  
            throw new System.IO.DirectoryNotFoundException();  
  
        string[] fileNames = null;  
        List<System.IO.FileInfo> files = new List<System.IO.FileInfo>();  
  
        fileNames = System.IO.Directory.GetFiles(path, "*.*", System.IO.SearchOption.AllDirectories);  
        foreach (string name in fileNames)  
        {  
            files.Add(new System.IO.FileInfo(name));  
        }  
        return files;  
    }  
}  
```  
  
 Observe que também é possível consultar o objeto <xref:System.Text.RegularExpressions.MatchCollection> retornado por uma pesquisa `RegEx`. Neste exemplo, apenas o valor de cada correspondência é produzido nos resultados. No entanto, também é possível usar a LINQ para executar todos os tipos de filtragem, classificação e agrupamento nessa coleção. Como <xref:System.Text.RegularExpressions.MatchCollection> é uma coleção <xref:System.Collections.IEnumerable> não genérica, é necessário declarar explicitamente o tipo da variável de intervalo na consulta.  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Criar um projeto de aplicativo de console em C# com diretivas `using` para os namespaces System.Linq e System.IO.  
  
## <a name="see-also"></a>Confira também

- [LINQ e cadeias de caracteres (C#)](./linq-and-strings.md)
- [LINQ e diretórios de arquivos (C#)](./linq-and-file-directories.md)
