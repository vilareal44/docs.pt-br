---
title: Como computar valores de coluna em um arquivo de texto CSV (LINQ) (C#)
description: Este exemplo mostra como executar computações de agregação usando LINQ em C#, como Sum, Average, min e Max, nas colunas de um arquivo. csv.
ms.date: 07/20/2015
ms.assetid: 4747f37a-a198-4df2-8efe-5b0731e0ea27
ms.openlocfilehash: 9137779f9767c8a9531489f7894ba3e69eb1faee
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105316"
---
# <a name="how-to-compute-column-values-in-a-csv-text-file-linq-c"></a>Como computar valores de coluna em um arquivo de texto CSV (LINQ) (C#)
Este exemplo mostra como executar cálculos de agregação, como soma, média, mín. e máx. nas colunas de um arquivo .csv. Os princípios de exemplo mostrados aqui podem ser aplicados a outros tipos de texto estruturado.  
  
## <a name="to-create-the-source-file"></a>Para criar o arquivo de origem  
  
1. Copie as seguintes linhas para um arquivo chamado scores.csv e salve-o na sua pasta do projeto. Suponha que a primeira coluna representa uma ID do aluno e as colunas subsequentes representam as notas de quatro provas.  
  
    ```csv
    111, 97, 92, 81, 60  
    112, 75, 84, 91, 39  
    113, 88, 94, 65, 91  
    114, 97, 89, 85, 82  
    115, 35, 72, 91, 70  
    116, 99, 86, 90, 94  
    117, 93, 92, 80, 87  
    118, 92, 90, 83, 78  
    119, 68, 79, 88, 92  
    120, 99, 82, 81, 79  
    121, 96, 85, 91, 60  
    122, 94, 92, 91, 91  
    ```  
  
## <a name="example"></a>Exemplo  
  
```csharp  
class SumColumns  
{  
    static void Main(string[] args)  
    {  
        string[] lines = System.IO.File.ReadAllLines(@"../../../scores.csv");  
  
        // Specifies the column to compute.  
        int exam = 3;  
  
        // Spreadsheet format:  
        // Student ID    Exam#1  Exam#2  Exam#3  Exam#4  
        // 111,          97,     92,     81,     60  
  
        // Add one to exam to skip over the first column,  
        // which holds the student ID.  
        SingleColumn(lines, exam + 1);  
        Console.WriteLine();  
        MultiColumns(lines);  
  
        Console.WriteLine("Press any key to exit");  
        Console.ReadKey();  
    }  
  
    static void SingleColumn(IEnumerable<string> strs, int examNum)  
    {  
        Console.WriteLine("Single Column Query:");  
  
        // Parameter examNum specifies the column to
        // run the calculations on. This value could be  
        // passed in dynamically at runtime.
  
        // Variable columnQuery is an IEnumerable<int>.  
        // The following query performs two steps:  
        // 1) use Split to break each row (a string) into an array
        //    of strings,
        // 2) convert the element at position examNum to an int  
        //    and select it.  
        var columnQuery =  
            from line in strs  
            let elements = line.Split(',')  
            select Convert.ToInt32(elements[examNum]);  
  
        // Execute the query and cache the results to improve  
        // performance. This is helpful only with very large files.  
        var results = columnQuery.ToList();  
  
        // Perform aggregate calculations Average, Max, and  
        // Min on the column specified by examNum.  
        double average = results.Average();  
        int max = results.Max();  
        int min = results.Min();  
  
        Console.WriteLine("Exam #{0}: Average:{1:##.##} High Score:{2} Low Score:{3}",  
                 examNum, average, max, min);  
    }  
  
    static void MultiColumns(IEnumerable<string> strs)  
    {  
        Console.WriteLine("Multi Column Query:");  
  
        // Create a query, multiColQuery. Explicit typing is used  
        // to make clear that, when executed, multiColQuery produces
        // nested sequences. However, you get the same results by  
        // using 'var'.  
  
        // The multiColQuery query performs the following steps:  
        // 1) use Split to break each row (a string) into an array
        //    of strings,
        // 2) use Skip to skip the "Student ID" column, and store the
        //    rest of the row in scores.  
        // 3) convert each score in the current row from a string to  
        //    an int, and select that entire sequence as one row
        //    in the results.  
        IEnumerable<IEnumerable<int>> multiColQuery =  
            from line in strs  
            let elements = line.Split(',')  
            let scores = elements.Skip(1)  
            select (from str in scores  
                    select Convert.ToInt32(str));  
  
        // Execute the query and cache the results to improve  
        // performance.
        // ToArray could be used instead of ToList.  
        var results = multiColQuery.ToList();  
  
        // Find out how many columns you have in results.  
        int columnCount = results[0].Count();  
  
        // Perform aggregate calculations Average, Max, and  
        // Min on each column.
        // Perform one iteration of the loop for each column
        // of scores.  
        // You can use a for loop instead of a foreach loop
        // because you already executed the multiColQuery
        // query by calling ToList.  
        for (int column = 0; column < columnCount; column++)  
        {  
            var results2 = from row in results  
                           select row.ElementAt(column);  
            double average = results2.Average();  
            int max = results2.Max();  
            int min = results2.Min();  
  
            // Add one to column because the first exam is Exam #1,  
            // not Exam #0.  
            Console.WriteLine("Exam #{0} Average: {1:##.##} High Score: {2} Low Score: {3}",  
                          column + 1, average, max, min);  
        }  
    }  
}  
/* Output:  
    Single Column Query:  
    Exam #4: Average:76.92 High Score:94 Low Score:39  
  
    Multi Column Query:  
    Exam #1 Average: 86.08 High Score: 99 Low Score: 35  
    Exam #2 Average: 86.42 High Score: 94 Low Score: 72  
    Exam #3 Average: 84.75 High Score: 91 Low Score: 65  
    Exam #4 Average: 76.92 High Score: 94 Low Score: 39  
 */  
```  
  
 A consulta funciona usando o método <xref:System.String.Split%2A> para converter cada linha de texto em uma matriz. Cada elemento da matriz representa uma coluna. Por fim, o texto em cada coluna é convertido em sua representação numérica. Se o arquivo for um arquivo separado por tabulações, é só atualizar o argumento no método `Split` para `\t`.  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Criar um projeto de aplicativo de console em C# com diretivas `using` para os namespaces System.Linq e System.IO.  
  
## <a name="see-also"></a>Confira também

- [LINQ e cadeias de caracteres (C#)](./linq-and-strings.md)
- [LINQ e diretórios de arquivos (C#)](./linq-and-file-directories.md)
