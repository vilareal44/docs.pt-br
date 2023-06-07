---
title: 'Como: consultar um ArrayList com LINQ'
ms.date: 07/20/2015
ms.assetid: 176358a9-d765-4b57-9557-7feb4428138d
ms.openlocfilehash: b7b75e017fb314b5e5998b743dbf922f34fd9b7c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396461"
---
# <a name="how-to-query-an-arraylist-with-linq-visual-basic"></a>Como consultar um ArrayList com LINQ (Visual Basic)

Ao usar a LINQ para consultar coleções <xref:System.Collections.IEnumerable> não genéricas como <xref:System.Collections.ArrayList>, você deve declarar explicitamente o tipo da variável de intervalo para refletir o tipo específico dos objetos na coleção. Por exemplo, se você tiver um <xref:System.Collections.ArrayList> de `Student` objetos, sua [cláusula from](../../../language-reference/queries/from-clause.md) deverá ter esta aparência:

```vb
Dim query = From student As Student In arrList
'...
```

Especificando o tipo da variável de intervalo, você está convertendo cada item na <xref:System.Collections.ArrayList> em um `Student`.

O uso de uma variável de intervalo de tipo explícito em uma expressão de consulta é equivalente a chamar o método <xref:System.Linq.Enumerable.Cast%2A>. <xref:System.Linq.Enumerable.Cast%2A> lança uma exceção se a conversão especificada não puder ser realizada. <xref:System.Linq.Enumerable.Cast%2A> e <xref:System.Linq.Enumerable.OfType%2A> são os dois métodos de operador de consulta padrão que operam em tipos <xref:System.Collections.IEnumerable> não genéricos. Em Visual Basic, você deve chamar explicitamente o <xref:System.Linq.Enumerable.Cast%2A> método na fonte de dados para garantir um tipo de variável de intervalo específico. Para obter mais informações, consulte [relações de tipo em operações de consulta (Visual Basic)](type-relationships-in-query-operations.md).

## <a name="example"></a>Exemplo

O exemplo a seguir mostra uma consulta simples sobre um <xref:System.Collections.ArrayList>. Observe que este exemplo usa os inicializadores de objeto quando o código chama o método <xref:System.Collections.ArrayList.Add%2A>, mas isso não é um requisito.

```vb
Imports System.Collections
Imports System.Linq

Module Module1

    Public Class Student
        Public Property FirstName As String
        Public Property LastName As String
        Public Property Scores As Integer()
    End Class

    Sub Main()

        Dim student1 As New Student With {.FirstName = "Svetlana",
                                     .LastName = "Omelchenko",
                                     .Scores = New Integer() {98, 92, 81, 60}}
        Dim student2 As New Student With {.FirstName = "Claire",
                                    .LastName = "O'Donnell",
                                    .Scores = New Integer() {75, 84, 91, 39}}
        Dim student3 As New Student With {.FirstName = "Cesar",
                                    .LastName = "Garcia",
                                    .Scores = New Integer() {97, 89, 85, 82}}
        Dim student4 As New Student With {.FirstName = "Sven",
                                    .LastName = "Mortensen",
                                    .Scores = New Integer() {88, 94, 65, 91}}

        Dim arrList As New ArrayList()
        arrList.Add(student1)
        arrList.Add(student2)
        arrList.Add(student3)
        arrList.Add(student4)

        ' Use an explicit type for non-generic collections
        Dim query = From student As Student In arrList
                    Where student.Scores(0) > 95
                    Select student

        For Each student As Student In query
            Console.WriteLine(student.LastName & ": " & student.Scores(0))
        Next
        ' Keep the console window open in debug mode.
        Console.WriteLine("Press any key to exit.")
        Console.ReadKey()
    End Sub

End Module
' Output:
'   Omelchenko: 98
'   Garcia: 97
```

## <a name="see-also"></a>Confira também

- [LINQ to Objects (Visual Basic)](linq-to-objects.md)
