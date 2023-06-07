---
title: Como preencher coleções de objetos de várias fontes (LINQ) (C#)
description: Saiba como mesclar dados de diferentes fontes em uma sequência de novos tipos usando o LINQ em C#. Esses exemplos usam tipos anônimos e nomeados.
ms.date: 06/12/2018
ms.assetid: 8ad7d480-b46c-4ccc-8c57-76f2d04ccc6d
ms.openlocfilehash: 9dc9f98ae09e0fe3437b5d2ccab32b3dbcd93714
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104724"
---
# <a name="how-to-populate-object-collections-from-multiple-sources-linq-c"></a>Como preencher coleções de objetos de várias fontes (LINQ) (C#)

Este exemplo mostra como mesclar dados de diferentes fontes em uma sequência de novos tipos.

> [!NOTE]
> Não tente unir dados na memória ou dados no sistema de arquivos com os dados que ainda estão em um banco de dados. Essas junções entre domínios podem gerar resultados indefinidos, devido às diferentes formas em que as operações de junção podem ser definidas para consultas de banco de dados e outros tipos de fontes. Além disso, há um risco de que essa operação possa causar uma exceção de falta de memória, se a quantidade de dados no banco de dados for grande o suficiente. Para unir dados de um banco de dados com os dados na memória, primeiro chame `ToList` ou `ToArray` na consulta de banco de dados e, em seguida, realize a junção na coleção retornada.

## <a name="to-create-the-data-file"></a>Para criar o arquivo de dados

Copie os arquivos de names.csv e scores.csv na pasta do projeto, conforme descrito em [como unir conteúdo de arquivos diferentes (LINQ) (C#)](./how-to-join-content-from-dissimilar-files-linq.md).

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar um tipo nomeado `Student` para armazenar dados mesclados, de duas coleções na memória de cadeias de caracteres, que simulam dados de planilha no formato .csv. A primeira coleção de cadeias de caracteres representa os nomes e as IDs dos alunos e a segunda coleção, representa a ID do aluno (na primeira coluna) e quatro pontuações de exames. A ID é usada como a chave estrangeira.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Student
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public int ID { get; set; }
    public List<int> ExamScores { get; set; }
}

class PopulateCollection
{
    static void Main()
    {
        // These data files are defined in How to join content from
        // dissimilar files (LINQ).

        // Each line of names.csv consists of a last name, a first name, and an
        // ID number, separated by commas. For example, Omelchenko,Svetlana,111
        string[] names = System.IO.File.ReadAllLines(@"../../../names.csv");

        // Each line of scores.csv consists of an ID number and four test
        // scores, separated by commas. For example, 111, 97, 92, 81, 60
        string[] scores = System.IO.File.ReadAllLines(@"../../../scores.csv");

        // Merge the data sources using a named type.
        // var could be used instead of an explicit type. Note the dynamic
        // creation of a list of ints for the ExamScores member. The first item
        // is skipped in the split string because it is the student ID,
        // not an exam score.
        IEnumerable<Student> queryNamesScores =
            from nameLine in names
            let splitName = nameLine.Split(',')
            from scoreLine in scores
            let splitScoreLine = scoreLine.Split(',')
            where Convert.ToInt32(splitName[2]) == Convert.ToInt32(splitScoreLine[0])
            select new Student()
            {
                FirstName = splitName[0],
                LastName = splitName[1],
                ID = Convert.ToInt32(splitName[2]),
                ExamScores = (from scoreAsText in splitScoreLine.Skip(1)
                              select Convert.ToInt32(scoreAsText)).
                              ToList()
            };

        // Optional. Store the newly created student objects in memory
        // for faster access in future queries. This could be useful with
        // very large data files.
        List<Student> students = queryNamesScores.ToList();

        // Display each student's name and exam score average.
        foreach (var student in students)
        {
            Console.WriteLine("The average score of {0} {1} is {2}.",
                student.FirstName, student.LastName,
                student.ExamScores.Average());
        }

        //Keep console window open in debug mode
        Console.WriteLine("Press any key to exit.");
        Console.ReadKey();
    }
}
/* Output:
    The average score of Omelchenko Svetlana is 82.5.
    The average score of O'Donnell Claire is 72.25.
    The average score of Mortensen Sven is 84.5.
    The average score of Garcia Cesar is 88.25.
    The average score of Garcia Debra is 67.
    The average score of Fakhouri Fadi is 92.25.
    The average score of Feng Hanying is 88.
    The average score of Garcia Hugo is 85.75.
    The average score of Tucker Lance is 81.75.
    The average score of Adams Terry is 85.25.
    The average score of Zabokritski Eugene is 83.
    The average score of Tucker Michael is 92.
 */
```

Na cláusula [select](../../../language-reference/keywords/select-clause.md), um inicializador de objeto é usado para instanciar cada novo objeto `Student`, usando os dados das duas fontes.

Se você não tiver que armazenar os resultados de uma consulta, os tipos anônimos poderão ser mais convenientes que os tipos nomeados. Os tipos nomeados são necessários se você passa os resultados da consulta para fora do método em que a consulta é executada. O exemplo a seguir realiza a mesma tarefa do exemplo anterior, mas usa tipos anônimos em vez de tipos nomeados:

```csharp
// Merge the data sources by using an anonymous type.
// Note the dynamic creation of a list of ints for the
// ExamScores member. We skip 1 because the first string
// in the array is the student ID, not an exam score.
var queryNamesScores2 =
    from nameLine in names
    let splitName = nameLine.Split(',')
    from scoreLine in scores
    let splitScoreLine = scoreLine.Split(',')
    where Convert.ToInt32(splitName[2]) == Convert.ToInt32(splitScoreLine[0])
    select new
    {
        First = splitName[0],
        Last = splitName[1],
        ExamScores = (from scoreAsText in splitScoreLine.Skip(1)
                      select Convert.ToInt32(scoreAsText))
                      .ToList()
    };

// Display each student's name and exam score average.
foreach (var student in queryNamesScores2)
{
    Console.WriteLine("The average score of {0} {1} is {2}.",
        student.First, student.Last, student.ExamScores.Average());
}
```

## <a name="see-also"></a>Confira também

- [LINQ e cadeias de caracteres (C#)](./linq-and-strings.md)
- [Inicializadores de objeto e coleção](../../classes-and-structs/object-and-collection-initializers.md)
- [Tipos anônimos](../../classes-and-structs/anonymous-types.md)
