---
title: Como usar árvores de expressão para compilar consultas dinâmicas
ms.date: 07/20/2015
ms.assetid: 16278787-7532-4b65-98b2-7a412406c4ee
ms.openlocfilehash: 1f686c37b5d04263ac5ae0dd6883aa8a519ed19e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410973"
---
# <a name="how-to-use-expression-trees-to-build-dynamic-queries-visual-basic"></a>Como: usar árvores de expressão para criar consultas dinâmicas (Visual Basic)

No LINQ, as árvores de expressão são usadas para representar consultas estruturadas que se destinam a fontes de dados que implementam <xref:System.Linq.IQueryable%601>. Por exemplo, o provedor LINQ implementa a interface <xref:System.Linq.IQueryable%601> para consultar repositórios de dados relacionais. O compilador Visual Basic compila consultas direcionadas a fontes de dados em código que cria uma árvore de expressão em tempo de execução. O provedor de consultas pode percorrer a estrutura de dados da árvore de expressão e convertê-la em uma linguagem de consulta apropriada para a fonte de dados.

As árvores de expressão também são usadas no LINQ para representar expressões lambda que são atribuídas a variáveis do tipo <xref:System.Linq.Expressions.Expression%601>.

Este tópico descreve como usar árvores de expressão para criar consultas LINQ dinâmicas. As consultas dinâmicas são úteis quando as especificações de uma consulta não são conhecidas em tempo de compilação. Por exemplo, um aplicativo pode fornecer uma interface do usuário que permite ao usuário final especificar um ou mais predicados para filtrar os dados. Para usar a LINQ para a consulta, esse tipo de aplicativo deve usar árvores de expressão para criar a consulta LINQ em runtime.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar árvores de expressão para construir uma consulta em uma fonte de dados `IQueryable` e, em seguida, executá-la. O código compila uma árvore de expressão para representar a consulta a seguir:

`companies.Where(Function(company) company.ToLower() = "coho winery" OrElse company.Length > 16).OrderBy(Function(company) company)`

Os métodos de fábrica no namespace <xref:System.Linq.Expressions> são usados para criar árvores de expressão que representam as expressões que compõem a consulta geral. As expressões que representam as chamadas aos métodos do operador de consulta padrão, referem-se às implementações <xref:System.Linq.Queryable> dos métodos a seguir. A árvore de expressão final é passada para a implementação de <xref:System.Linq.IQueryProvider.CreateQuery%60%601%28System.Linq.Expressions.Expression%29> do provedor da fonte de dados `IQueryable` para criar uma consulta executável do tipo `IQueryable`. Os resultados são obtidos ao enumerar essa variável de consulta.

```vb
' Add an Imports statement for System.Linq.Expressions.

Dim companies =
    {"Consolidated Messenger", "Alpine Ski House", "Southridge Video", "City Power & Light",
     "Coho Winery", "Wide World Importers", "Graphic Design Institute", "Adventure Works",
     "Humongous Insurance", "Woodgrove Bank", "Margie's Travel", "Northwind Traders",
     "Blue Yonder Airlines", "Trey Research", "The Phone Company",
     "Wingtip Toys", "Lucerne Publishing", "Fourth Coffee"}

' The IQueryable data to query.
Dim queryableData As IQueryable(Of String) = companies.AsQueryable()

' Compose the expression tree that represents the parameter to the predicate.
Dim pe As ParameterExpression = Expression.Parameter(GetType(String), "company")

' ***** Where(Function(company) company.ToLower() = "coho winery" OrElse company.Length > 16) *****
' Create an expression tree that represents the expression: company.ToLower() = "coho winery".
Dim left As Expression = Expression.Call(pe, GetType(String).GetMethod("ToLower", System.Type.EmptyTypes))
Dim right As Expression = Expression.Constant("coho winery")
Dim e1 As Expression = Expression.Equal(left, right)

' Create an expression tree that represents the expression: company.Length > 16.
left = Expression.Property(pe, GetType(String).GetProperty("Length"))
right = Expression.Constant(16, GetType(Integer))
Dim e2 As Expression = Expression.GreaterThan(left, right)

' Combine the expressions to create an expression tree that represents the
' expression: company.ToLower() = "coho winery" OrElse company.Length > 16).
Dim predicateBody As Expression = Expression.OrElse(e1, e2)

' Create an expression tree that represents the expression:
' queryableData.Where(Function(company) company.ToLower() = "coho winery" OrElse company.Length > 16)
Dim whereCallExpression As MethodCallExpression = Expression.Call(
        GetType(Queryable),
        "Where",
        New Type() {queryableData.ElementType},
        queryableData.Expression,
        Expression.Lambda(Of Func(Of String, Boolean))(predicateBody, New ParameterExpression() {pe}))
' ***** End Where *****

' ***** OrderBy(Function(company) company) *****
' Create an expression tree that represents the expression:
' whereCallExpression.OrderBy(Function(company) company)
Dim orderByCallExpression As MethodCallExpression = Expression.Call(
        GetType(Queryable),
        "OrderBy",
        New Type() {queryableData.ElementType, queryableData.ElementType},
        whereCallExpression,
        Expression.Lambda(Of Func(Of String, String))(pe, New ParameterExpression() {pe}))
' ***** End OrderBy *****

' Create an executable query from the expression tree.
Dim results As IQueryable(Of String) = queryableData.Provider.CreateQuery(Of String)(orderByCallExpression)

' Enumerate the results.
For Each company As String In results
    Console.WriteLine(company)
Next

' This code produces the following output:
'
' Blue Yonder Airlines
' City Power & Light
' Coho Winery
' Consolidated Messenger
' Graphic Design Institute
' Humongous Insurance
' Lucerne Publishing
' Northwind Traders
' The Phone Company
' Wide World Importers
```

Esse código usa um número fixo de expressões no predicado que é passado para o método `Queryable.Where`. No entanto, você pode escrever um aplicativo que combina um número variável de expressões de predicado que dependam da entrada do usuário. Você também pode variar os operadores de consulta padrão que são chamados na consulta, dependendo da entrada do usuário.

## <a name="compile-the-code"></a>Compilar o código

- Crie um novo projeto de **aplicativo de console** .

- Inclua o namespace System.Linq.Expressions.

- Copie o código do exemplo e cole-o no `Main` `Sub` procedimento.

## <a name="see-also"></a>Confira também

- [Árvores de expressão (Visual Basic)](index.md)
- [Como executar árvores de expressão (Visual Basic)](how-to-execute-expression-trees.md)
