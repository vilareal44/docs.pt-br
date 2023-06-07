---
title: Como usar árvores de expressão para criar consultas dinâmicas (C#)
description: Saiba como usar árvores de expressão para criar consultas dinâmicas de LINQ. Essas consultas são úteis quando as especificidades de uma consulta são desconhecidas no momento da compilação.
ms.date: 07/20/2015
ms.assetid: 52cd44dd-a3ec-441e-b93a-4eca388119c7
ms.openlocfilehash: edcef4068c19ba8e789683cf6ba4d5ef2477e0d8
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105595"
---
# <a name="how-to-use-expression-trees-to-build-dynamic-queries-c"></a>Como usar árvores de expressão para criar consultas dinâmicas (C#)
No LINQ, as árvores de expressão são usadas para representar consultas estruturadas que se destinam a fontes de dados que implementam <xref:System.Linq.IQueryable%601>. Por exemplo, o provedor LINQ implementa a interface <xref:System.Linq.IQueryable%601> para consultar repositórios de dados relacionais. O compilador do C# compila as consultas que se destinam a essas fontes de dados, no código que cria uma árvore de expressão em runtime. O provedor de consultas pode percorrer a estrutura de dados da árvore de expressão e convertê-la em uma linguagem de consulta apropriada para a fonte de dados.  
  
 As árvores de expressão também são usadas no LINQ para representar expressões lambda que são atribuídas a variáveis do tipo <xref:System.Linq.Expressions.Expression%601>.  
  
 Este tópico descreve como usar árvores de expressão para criar consultas LINQ dinâmicas. As consultas dinâmicas são úteis quando as especificações de uma consulta não são conhecidas em tempo de compilação. Por exemplo, um aplicativo pode fornecer uma interface do usuário que permite ao usuário final especificar um ou mais predicados para filtrar os dados. Para usar a LINQ para a consulta, esse tipo de aplicativo deve usar árvores de expressão para criar a consulta LINQ em runtime.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar árvores de expressão para construir uma consulta em uma fonte de dados `IQueryable` e, em seguida, executá-la. O código compila uma árvore de expressão para representar a consulta a seguir:  
  
 ```csharp
 companies.Where(company => (company.ToLower() == "coho winery" || company.Length > 16))
          .OrderBy(company => company)
 ```
  
 Os métodos de fábrica no namespace <xref:System.Linq.Expressions> são usados para criar árvores de expressão que representam as expressões que compõem a consulta geral. As expressões que representam as chamadas aos métodos do operador de consulta padrão, referem-se às implementações <xref:System.Linq.Queryable> dos métodos a seguir. A árvore de expressão final é passada para a implementação de <xref:System.Linq.IQueryProvider.CreateQuery%60%601%28System.Linq.Expressions.Expression%29> do provedor da fonte de dados `IQueryable` para criar uma consulta executável do tipo `IQueryable`. Os resultados são obtidos ao enumerar essa variável de consulta.  
  
```csharp  
// Add a using directive for System.Linq.Expressions.  
  
string[] companies = { "Consolidated Messenger", "Alpine Ski House", "Southridge Video", "City Power & Light",  
                   "Coho Winery", "Wide World Importers", "Graphic Design Institute", "Adventure Works",  
                   "Humongous Insurance", "Woodgrove Bank", "Margie's Travel", "Northwind Traders",  
                   "Blue Yonder Airlines", "Trey Research", "The Phone Company",  
                   "Wingtip Toys", "Lucerne Publishing", "Fourth Coffee" };  
  
// The IQueryable data to query.  
IQueryable<String> queryableData = companies.AsQueryable<string>();  
  
// Compose the expression tree that represents the parameter to the predicate.  
ParameterExpression pe = Expression.Parameter(typeof(string), "company");  
  
// ***** Where(company => (company.ToLower() == "coho winery" || company.Length > 16)) *****  
// Create an expression tree that represents the expression 'company.ToLower() == "coho winery"'.  
Expression left = Expression.Call(pe, typeof(string).GetMethod("ToLower", System.Type.EmptyTypes));  
Expression right = Expression.Constant("coho winery");  
Expression e1 = Expression.Equal(left, right);  
  
// Create an expression tree that represents the expression 'company.Length > 16'.  
left = Expression.Property(pe, typeof(string).GetProperty("Length"));  
right = Expression.Constant(16, typeof(int));  
Expression e2 = Expression.GreaterThan(left, right);  
  
// Combine the expression trees to create an expression tree that represents the  
// expression '(company.ToLower() == "coho winery" || company.Length > 16)'.  
Expression predicateBody = Expression.OrElse(e1, e2);  
  
// Create an expression tree that represents the expression  
// 'queryableData.Where(company => (company.ToLower() == "coho winery" || company.Length > 16))'  
MethodCallExpression whereCallExpression = Expression.Call(  
    typeof(Queryable),  
    "Where",  
    new Type[] { queryableData.ElementType },  
    queryableData.Expression,  
    Expression.Lambda<Func<string, bool>>(predicateBody, new ParameterExpression[] { pe }));  
// ***** End Where *****  
  
// ***** OrderBy(company => company) *****  
// Create an expression tree that represents the expression  
// 'whereCallExpression.OrderBy(company => company)'  
MethodCallExpression orderByCallExpression = Expression.Call(  
    typeof(Queryable),  
    "OrderBy",  
    new Type[] { queryableData.ElementType, queryableData.ElementType },  
    whereCallExpression,  
    Expression.Lambda<Func<string, string>>(pe, new ParameterExpression[] { pe }));  
// ***** End OrderBy *****  
  
// Create an executable query from the expression tree.  
IQueryable<string> results = queryableData.Provider.CreateQuery<string>(orderByCallExpression);  
  
// Enumerate the results.  
foreach (string company in results)  
    Console.WriteLine(company);  
  
/*  This code produces the following output:  
  
    Blue Yonder Airlines  
    City Power & Light  
    Coho Winery  
    Consolidated Messenger  
    Graphic Design Institute  
    Humongous Insurance  
    Lucerne Publishing  
    Northwind Traders  
    The Phone Company  
    Wide World Importers  
*/  
```  
  
 Esse código usa um número fixo de expressões no predicado que é passado para o método `Queryable.Where`. No entanto, você pode escrever um aplicativo que combina um número variável de expressões de predicado que dependam da entrada do usuário. Você também pode variar os operadores de consulta padrão que são chamados na consulta, dependendo da entrada do usuário.  
  
## <a name="compiling-the-code"></a>Compilando o código  
  
- Inclua o namespace System.Linq.Expressions.  
  
## <a name="see-also"></a>Confira também

- [Árvores de expressão (C#)](./index.md)
- [Como executar árvores de expressão (C#)](./how-to-execute-expression-trees.md)
- [Especificar filtros predicados dinamicamente em runtime](../../../linq/dynamically-specify-predicate-filters-at-runtime.md)
