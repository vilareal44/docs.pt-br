---
title: Como contar ocorrências de uma palavra em uma cadeia de caracteres (LINQ) (C#)
description: Este exemplo usa uma consulta LINQ em C# para contar as ocorrências de uma palavra especificada em uma cadeia de caracteres. Ele usa o método Split para criar uma matriz de palavras.
ms.date: 07/20/2015
ms.assetid: f8e6f546-7c14-4aa1-8a75-e8d09f3b8ccd
ms.openlocfilehash: 1621e776510e366aa779f1d45468be34b3dec373
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103372"
---
# <a name="how-to-count-occurrences-of-a-word-in-a-string-linq-c"></a>Como contar ocorrências de uma palavra em uma cadeia de caracteres (LINQ) (C#)
Este exemplo mostra como usar uma consulta LINQ para contar as ocorrências de uma palavra especificada em uma cadeia de caracteres. Observe que para executar a contagem, primeiro o método <xref:System.String.Split%2A> é chamado para criar uma matriz de palavras. Há um custo de desempenho para o método <xref:System.String.Split%2A>. Se for a única operação na cadeia de caracteres for contar as palavras, você deverá considerar o uso dos métodos <xref:System.Text.RegularExpressions.Regex.Matches%2A> ou <xref:System.String.IndexOf%2A> em vez dele. No entanto, se o desempenho não for um problema crítico ou se você já tiver dividido a sentença para executar outros tipos de consulta nela, faz sentido usar LINQ para contar as palavras ou frases também.  
  
## <a name="example"></a>Exemplo  
  
```csharp  
class CountWords  
{  
    static void Main()  
    {  
        string text = @"Historically, the world of data and the world of objects" +  
          @" have not been well integrated. Programmers work in C# or Visual Basic" +  
          @" and also in SQL or XQuery. On the one side are concepts such as classes," +  
          @" objects, fields, inheritance, and .NET Framework APIs. On the other side" +  
          @" are tables, columns, rows, nodes, and separate languages for dealing with" +  
          @" them. Data types often require translation between the two worlds; there are" +  
          @" different standard functions. Because the object world has no notion of query, a" +  
          @" query can only be represented as a string without compile-time type checking or" +  
          @" IntelliSense support in the IDE. Transferring data from SQL tables or XML trees to" +  
          @" objects in memory is often tedious and error-prone.";  
  
        string searchTerm = "data";  
  
        //Convert the string into an array of words  
        string[] source = text.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);  
  
        // Create the query.  Use ToLowerInvariant to match "data" and "Data"
        var matchQuery = from word in source  
                         where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()  
                         select word;  
  
        // Count the matches, which executes the query.  
        int wordCount = matchQuery.Count();  
        Console.WriteLine("{0} occurrences(s) of the search term \"{1}\" were found.", wordCount, searchTerm);  
  
        // Keep console window open in debug mode  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
}  
/* Output:  
   3 occurrences(s) of the search term "data" were found.  
*/  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Criar um projeto de aplicativo de console em C# com diretivas `using` para os namespaces System.Linq e System.IO.  
  
## <a name="see-also"></a>Confira também

- [LINQ e cadeias de caracteres (C#)](./linq-and-strings.md)
