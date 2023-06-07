---
title: Como consultar uma ArrayList com LINQ (C#)
description: Este exemplo usa LINQ para consultar em uma ArrayList em C#. Você deve declarar o tipo da variável de intervalo para refletir o tipo dos objetos na coleção.
ms.date: 07/20/2015
ms.assetid: 2bfb471c-6e9a-4e60-bd83-4a1778abde11
ms.openlocfilehash: 5c251e17de062a4578f06fc1a40ea3ede9f3ab67
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104604"
---
# <a name="how-to-query-an-arraylist-with-linq-c"></a>Como consultar uma ArrayList com LINQ (C#)
Ao usar a LINQ para consultar coleções <xref:System.Collections.IEnumerable> não genéricas como <xref:System.Collections.ArrayList>, você deve declarar explicitamente o tipo da variável de intervalo para refletir o tipo específico dos objetos na coleção. Por exemplo, se você tiver um <xref:System.Collections.ArrayList> de objetos `Student`, sua [cláusula from](../../../language-reference/keywords/from-clause.md) deverá ter uma aparência semelhante a esta:  
  
```csharp
var query = from Student s in arrList  
//...
```  
  
 Especificando o tipo da variável de intervalo, você está convertendo cada item na <xref:System.Collections.ArrayList> em um `Student`.  
  
 O uso de uma variável de intervalo de tipo explícito em uma expressão de consulta é equivalente a chamar o método <xref:System.Linq.Enumerable.Cast%2A>. <xref:System.Linq.Enumerable.Cast%2A> lança uma exceção se a conversão especificada não puder ser realizada. <xref:System.Linq.Enumerable.Cast%2A> e <xref:System.Linq.Enumerable.OfType%2A> são os dois métodos de operador de consulta padrão que operam em tipos <xref:System.Collections.IEnumerable> não genéricos. Para obter mais informações, consulte [Relacionamentos de tipo em operações de consulta LINQ](./type-relationships-in-linq-query-operations.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra uma consulta simples sobre um <xref:System.Collections.ArrayList>. Observe que este exemplo usa os inicializadores de objeto quando o código chama o método <xref:System.Collections.ArrayList.Add%2A>, mas isso não é um requisito.  
  
```csharp  
using System;  
using System.Collections;  
using System.Linq;  
  
namespace NonGenericLINQ  
{  
    public class Student  
    {  
        public string FirstName { get; set; }  
        public string LastName { get; set; }  
        public int[] Scores { get; set; }  
    }  
  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            ArrayList arrList = new ArrayList();  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Svetlana", LastName = "Omelchenko", Scores = new int[] { 98, 92, 81, 60 }  
                    });  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Claire", LastName = "O’Donnell", Scores = new int[] { 75, 84, 91, 39 }  
                    });  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Sven", LastName = "Mortensen", Scores = new int[] { 88, 94, 65, 91 }  
                    });  
            arrList.Add(  
                new Student  
                    {  
                        FirstName = "Cesar", LastName = "Garcia", Scores = new int[] { 97, 89, 85, 82 }  
                    });  
  
            var query = from Student student in arrList  
                        where student.Scores[0] > 95  
                        select student;  
  
            foreach (Student s in query)  
                Console.WriteLine(s.LastName + ": " + s.Scores[0]);  
  
            // Keep the console window open in debug mode.  
            Console.WriteLine("Press any key to exit.");  
            Console.ReadKey();  
        }  
    }  
}  
/* Output:
    Omelchenko: 98  
    Garcia: 97  
*/  
```  
  
## <a name="see-also"></a>Confira também

- [LINQ to Objects (C#)](./linq-to-objects.md)
