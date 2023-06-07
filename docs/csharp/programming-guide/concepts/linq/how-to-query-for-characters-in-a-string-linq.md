---
title: Como consultar caracteres em uma cadeia (LINQ) (C#)
description: Você pode consultar uma cadeia de caracteres como uma sequência de personagens no LINQ. Este exemplo de C# consulta uma cadeia de caracteres para determinar o número de dígitos numéricos que ele contém.
ms.date: 07/20/2015
ms.assetid: 727a1be7-dbec-4ab8-b414-bc2d56feb6ff
ms.openlocfilehash: 3512be7c30843fcd8e881eab59761706a84a3ac8
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104557"
---
# <a name="how-to-query-for-characters-in-a-string-linq-c"></a>Como consultar caracteres em uma cadeia (LINQ) (C#)
Já que a classe <xref:System.String> implementa a interface <xref:System.Collections.Generic.IEnumerable%601> genérica, qualquer cadeia de caracteres pode ser consultada como uma sequência de caracteres. No entanto, esse não é um uso comum da LINQ. Para operações de correspondência de padrões complexas, use a classe <xref:System.Text.RegularExpressions.Regex>.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir consulta uma cadeia de caracteres para determinar quantos dígitos numéricos ela contém. Observe que a consulta é "reutilizada" depois que é executada pela primeira vez. Isso é possível porque a consulta em si não armazena nenhum resultado real.  
  
```csharp  
class QueryAString  
{  
    static void Main()  
    {  
        string aString = "ABCDE99F-J74-12-89A";  
  
        // Select only those characters that are numbers  
        IEnumerable<char> stringQuery =  
          from ch in aString  
          where Char.IsDigit(ch)  
          select ch;  
  
        // Execute the query  
        foreach (char c in stringQuery)  
            Console.Write(c + " ");  
  
        // Call the Count method on the existing query.  
        int count = stringQuery.Count();  
        Console.WriteLine("Count = {0}", count);  
  
        // Select all characters before the first '-'  
        IEnumerable<char> stringQuery2 = aString.TakeWhile(c => c != '-');  
  
        // Execute the second query  
        foreach (char c in stringQuery2)  
            Console.Write(c);  
  
        Console.WriteLine(System.Environment.NewLine + "Press any key to exit");  
        Console.ReadKey();  
    }  
}  
/* Output:  
  Output: 9 9 7 4 1 2 8 9  
  Count = 8  
  ABCDE99F  
*/  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Criar um projeto de aplicativo de console em C# com diretivas `using` para os namespaces System.Linq e System.IO.  
  
## <a name="see-also"></a>Confira também

- [LINQ e cadeias de caracteres (C#)](./linq-and-strings.md)
- [Como combinar consultas LINQ com expressões regulares (C#)](./how-to-combine-linq-queries-with-regular-expressions.md)
