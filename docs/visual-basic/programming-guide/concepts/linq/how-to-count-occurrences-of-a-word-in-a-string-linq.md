---
title: 'Como: contar as ocorrências de uma palavra em uma cadeia de caracteres (LINQ)'
ms.date: 07/20/2015
ms.assetid: bc367e46-f7cc-45f9-936f-754e661b7bb9
ms.openlocfilehash: c6894359e5785419ccf8f283f976c0a897288a5d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84405320"
---
# <a name="how-to-count-occurrences-of-a-word-in-a-string-linq-visual-basic"></a>Como: contar ocorrências de uma palavra em uma cadeia de caracteres (LINQ) (Visual Basic)

Este exemplo mostra como usar uma consulta LINQ para contar as ocorrências de uma palavra especificada em uma cadeia de caracteres. Observe que para executar a contagem, primeiro o método <xref:System.String.Split%2A> é chamado para criar uma matriz de palavras. Há um custo de desempenho para o método <xref:System.String.Split%2A>. Se for a única operação na cadeia de caracteres for contar as palavras, você deverá considerar o uso dos métodos <xref:System.Text.RegularExpressions.Regex.Matches%2A> ou <xref:System.String.IndexOf%2A> em vez dele. No entanto, se o desempenho não for um problema crítico ou se você já tiver dividido a sentença para executar outros tipos de consulta nela, faz sentido usar LINQ para contar as palavras ou frases também.

## <a name="example"></a>Exemplo

```vb
Class CountWords

    Shared Sub Main()

        Dim text As String = "Historically, the world of data and the world of objects" &
                  " have not been well integrated. Programmers work in C# or Visual Basic" &
                  " and also in SQL or XQuery. On the one side are concepts such as classes," &
                  " objects, fields, inheritance, and .NET Framework APIs. On the other side" &
                  " are tables, columns, rows, nodes, and separate languages for dealing with" &
                  " them. Data types often require translation between the two worlds; there are" &
                  " different standard functions. Because the object world has no notion of query, a" &
                  " query can only be represented as a string without compile-time type checking or" &
                  " IntelliSense support in the IDE. Transferring data from SQL tables or XML trees to" &
                  " objects in memory is often tedious and error-prone."

        Dim searchTerm As String = "data"

        ' Convert the string into an array of words.
        Dim dataSource As String() = text.Split(New Char() {" ", ",", ".", ";", ":"},
                                                 StringSplitOptions.RemoveEmptyEntries)

        ' Create and execute the query. It executes immediately
        ' because a singleton value is produced.
        ' Use ToLower to match "data" and "Data"
        Dim matchQuery = From word In dataSource
                      Where word.ToLowerInvariant() = searchTerm.ToLowerInvariant()
                      Select word

        ' Count the matches.
        Dim count As Integer = matchQuery.Count()
        Console.WriteLine(count & " occurrence(s) of the search term """ &
                          searchTerm & """ were found.")

        ' Keep console window open in debug mode.
        Console.WriteLine("Press any key to exit.")
        Console.ReadKey()
    End Sub
End Class
' Output:
' 3 occurrence(s) of the search term "data" were found.
```

## <a name="compile-the-code"></a>Compilar o código

Crie um projeto de aplicativo de console Visual Basic, com uma `Imports` instrução para o namespace System. Linq.

## <a name="see-also"></a>Confira também

- [LINQ e cadeias de caracteres (Visual Basic)](linq-and-strings.md)
