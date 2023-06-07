---
title: 'Como: popular coleções de objetos de várias fontes (LINQ)'
ms.date: 06/22/2018
ms.assetid: 63062a22-e6a9-42c0-b357-c7c965f58f33
ms.openlocfilehash: 9c6d8ff5165bf886d8aad87b64305819e65361ab
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396513"
---
# <a name="how-to-populate-object-collections-from-multiple-sources-linq-visual-basic"></a>Como: popular coleções de objetos de várias fontes (LINQ) (Visual Basic)

Este exemplo mostra como mesclar dados de diferentes fontes em uma sequência de novos tipos.

> [!NOTE]
> Não tente unir dados na memória ou dados no sistema de arquivos com os dados que ainda estão em um banco de dados. Essas junções entre domínios podem gerar resultados indefinidos, devido às diferentes formas em que as operações de junção podem ser definidas para consultas de banco de dados e outros tipos de fontes. Além disso, há um risco de que essa operação possa causar uma exceção de falta de memória, se a quantidade de dados no banco de dados for grande o suficiente. Para unir dados de um banco de dados com os dados na memória, primeiro chame `ToList` ou `ToArray` na consulta de banco de dados e, em seguida, realize a junção na coleção retornada.

## <a name="to-create-the-data-file"></a>Para criar o arquivo de dados

- Copie os arquivos names. csv e Scores. csv para a pasta do projeto, conforme descrito em [como: unir conteúdo de arquivos não semelhantes (LINQ) (Visual Basic)](how-to-join-content-from-dissimilar-files-linq.md).

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar um tipo nomeado `Student` para armazenar dados mesclados, de duas coleções na memória de cadeias de caracteres, que simulam dados de planilha no formato .csv. A primeira coleção de cadeias de caracteres representa os nomes e as IDs dos alunos e a segunda coleção, representa a ID do aluno (na primeira coluna) e quatro pontuações de exames. A ID é usada como a chave estrangeira.

```vb
Imports System.Collections.Generic
Imports System.Linq

Class Student
    Public FirstName As String
    Public LastName As String
    Public ID As Integer
    Public ExamScores As List(Of Integer)
End Class

Class PopulateCollection

    Shared Sub Main()

        ' Merge content from spreadsheets into a list of Student objects.

        ' These data files are defined in How to: Join Content from
        ' Dissimilar Files (LINQ).

        ' Each line of names.csv consists of a last name, a first name, and an
        ' ID number, separated by commas. For example, Omelchenko,Svetlana,111
        Dim names As String() = System.IO.File.ReadAllLines("../../../names.csv")

        ' Each line of scores.csv consists of an ID number and four test
        ' scores, separated by commas. For example, 111, 97, 92, 81, 60
        Dim scores As String() = System.IO.File.ReadAllLines("../../../scores.csv")

        ' The following query merges the content of two dissimilar spreadsheets
        ' based on common ID values.
        ' Multiple From clauses are used instead of a Join clause
        ' in order to store the results of scoreLine.Split.
        ' Note the dynamic creation of a list of integers for the
        ' ExamScores member. The first item is skipped in the split string
        ' because it is the student ID, not an exam score.
        Dim queryNamesScores = From nameLine In names
                          Let splitName = nameLine.Split(New Char() {","})
                          From scoreLine In scores
                          Let splitScoreLine = scoreLine.Split(New Char() {","})
                          Where Convert.ToInt32(splitName(2)) = Convert.ToInt32(splitScoreLine(0))
                          Select New Student() With {
                               .FirstName = splitName(1), .LastName = splitName(0), .ID = splitName(2),
                               .ExamScores = (From scoreAsText In splitScoreLine Skip 1
                                             Select Convert.ToInt32(scoreAsText)).ToList()}

        ' Optional. Store the query results for faster access in future
        ' queries. This could be useful with very large data files.
        Dim students As List(Of Student) = queryNamesScores.ToList()

        ' Display each student's name and exam score average.
        For Each s In students
            Console.WriteLine("The average score of " & s.FirstName & " " &
                              s.LastName & " is " & s.ExamScores.Average())
        Next

        ' Keep console window open in debug mode.
        Console.WriteLine("Press any key to exit.")
        Console.ReadKey()
    End Sub
End Class

' Output:
' The average score of Svetlana Omelchenko is 82.5
' The average score of Claire O'Donnell is 72.25
' The average score of Sven Mortensen is 84.5
' The average score of Cesar Garcia is 88.25
' The average score of Debra Garcia is 67
' The average score of Fadi Fakhouri is 92.25
' The average score of Hanying Feng is 88
' The average score of Hugo Garcia is 85.75
' The average score of Lance Tucker is 81.75
' The average score of Terry Adams is 85.25
' The average score of Eugene Zabokritski is 83
' The average score of Michael Tucker is 92
```

Na cláusula [Select cláusula](../../../language-reference/queries/select-clause.md) , um inicializador de objeto é usado para instanciar cada novo `Student` objeto usando os dados das duas fontes.

Se você não tiver que armazenar os resultados de uma consulta, os tipos anônimos poderão ser mais convenientes que os tipos nomeados. Os tipos nomeados são necessários se você passa os resultados da consulta para fora do método em que a consulta é executada. O exemplo a seguir realiza a mesma tarefa do exemplo anterior, mas usa tipos anônimos em vez de tipos nomeados:

```vb
' Merge the data by using an anonymous type.
' Note the dynamic creation of a list of integers for the
' ExamScores member. We skip 1 because the first string
' in the array is the student ID, not an exam score.
Dim queryNamesScores2 =
    From nameLine In names
    Let splitName = nameLine.Split(New Char() {","})
    From scoreLine In scores
    Let splitScoreLine = scoreLine.Split(New Char() {","})
    Where Convert.ToInt32(splitName(2)) = Convert.ToInt32(splitScoreLine(0))
    Select New With
           {.Last = splitName(0),
            .First = splitName(1),
            .ExamScores = (From scoreAsText In splitScoreLine Skip 1
                           Select Convert.ToInt32(scoreAsText)).ToList()}

' Display each student's name and exam score average.
For Each s In queryNamesScores2
    Console.WriteLine("The average score of " & s.First & " " &
                      s.Last & " is " & s.ExamScores.Average())
Next
```

## <a name="see-also"></a>Confira também

- [LINQ e cadeias de caracteres (Visual Basic)](linq-and-strings.md)
