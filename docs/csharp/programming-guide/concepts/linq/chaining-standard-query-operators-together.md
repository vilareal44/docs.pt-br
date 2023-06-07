---
title: Encadeando operadores de consulta padrão juntos (C#)
description: Este exemplo mostra como os operadores de consulta padrão também podem ser encadeados juntos em C#. A consulta não materializa os resultados intermediários.
ms.date: 07/20/2015
ms.assetid: 66f2b0a9-2c23-4735-988e-bbc9dfb55c7b
ms.openlocfilehash: 41a7e4c7910c783d07181fe16254b0cac6902794
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104069"
---
# <a name="chaining-standard-query-operators-together-c"></a>Encadeando operadores de consulta padrão juntos (C#)
Este é o tópico final no tutorial do [Tutorial: encadear consultas juntas (C#)](./deferred-execution-and-lazy-evaluation-in-linq-to-xml.md).  
  
 Os operadores de consulta padrão podem também ser encadeados juntos. Por exemplo, você pode interject o operador de <xref:System.Linq.Enumerable.Where%2A?displayProperty=nameWithType> , e também funciona em uma forma lazy. Resultados intermediária é materializado por ele.  
  
## <a name="example"></a>Exemplo  
 Nesse exemplo, o método de <xref:System.Linq.Enumerable.Where%2A> é chamado antes de chamar `ConvertCollectionToUpperCase`. O método de <xref:System.Linq.Enumerable.Where%2A> opera quase exatamente a mesma maneira que os métodos lentos usados em exemplos anteriores neste tutorial, `ConvertCollectionToUpperCase` e `AppendString`.  
  
 Uma diferença é que nesse caso, o método de <xref:System.Linq.Enumerable.Where%2A> itera através da coleção fonte, determina que o primeiro item não passa o predicado e em seguida, o próximo item, que passa. Produz no segundo item.  
  
 Entretanto, a exibição básica é a mesma: As coleções intermediários não são materializadas a menos que têm que ser.  
  
 Quando as expressões de consulta são usadas, são convertidas para chamadas aos operadores de consulta padrão, e os mesmos princípios se aplicam.  
  
 Todos os exemplos nesta seção que estão vendo documentos do Office Open XML usam o mesmo princípio. A execução adiada e a avaliação lazy são alguns conceitos fundamentais que você deve compreender para usar efetivamente LINQ (e LINQ to XML).  
  
```csharp  
public static class LocalExtensions  
{  
    public static IEnumerable<string>  
      ConvertCollectionToUpperCase(this IEnumerable<string> source)  
    {  
        foreach (string str in source)  
        {  
            Console.WriteLine("ToUpper: source >{0}<", str);  
            yield return str.ToUpper();  
        }  
    }  
  
    public static IEnumerable<string>  
      AppendString(this IEnumerable<string> source, string stringToAppend)  
    {  
        foreach (string str in source)  
        {  
            Console.WriteLine("AppendString: source >{0}<", str);  
            yield return str + stringToAppend;  
        }  
    }  
}  
  
class Program  
{  
    static void Main(string[] args)  
    {  
        string[] stringArray = { "abc", "def", "ghi" };  
  
        IEnumerable<string> q1 =  
            from s in stringArray.ConvertCollectionToUpperCase()  
            where s.CompareTo("D") >= 0  
            select s;  
  
        IEnumerable<string> q2 =  
            from s in q1.AppendString("!!!")  
            select s;  
  
        foreach (string str in q2)  
        {  
            Console.WriteLine("Main: str >{0}<", str);  
            Console.WriteLine();  
        }  
    }  
}  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```output  
ToUpper: source >abc<  
ToUpper: source >def<  
AppendString: source >DEF<  
Main: str >DEF!!!<  
  
ToUpper: source >ghi<  
AppendString: source >GHI<  
Main: str >GHI!!!<  
```  
