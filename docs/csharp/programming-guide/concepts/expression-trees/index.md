---
title: Árvores de expressão (C#)
description: Saiba mais sobre árvores de expressão. Veja como compilar e executar o código representado por essas estruturas de dados, onde cada nó é uma expressão.
ms.date: 07/20/2015
ms.assetid: 7d0ac21a-6d90-4e2e-8903-528cb78615b7
ms.openlocfilehash: a5c84673f0b45b92be18b955a6d1e7268bb73c26
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063309"
---
# <a name="expression-trees-c"></a>Árvores de expressão (C#)
Árvores de expressão representam código em uma estrutura de dados de árvore, onde cada nó é, por exemplo, uma expressão, uma chamada de método ou uma operação binária como `x < y`.  
  
 Você pode compilar e executar código representado por árvores de expressão. Isso permite a modificação dinâmica de código executável, a execução de consultas LINQ em vários bancos de dados e a criação de consultas dinâmicas. Para obter mais informações sobre árvores de expressão no LINQ, consulte [como usar árvores de expressão para criar consultas dinâmicas (C#)](./how-to-use-expression-trees-to-build-dynamic-queries.md).
  
 As árvores de expressão também são usadas no DLR (tempo de execução de linguagem dinâmica) para fornecer interoperabilidade entre linguagens dinâmicas e .NET e para permitir que os escritores de compilador emitam árvores de expressão em vez da MSIL (Microsoft Intermediate Language). Para obter mais informações sobre o DLR, consulte [Visão geral do Dynamic Language Runtime](../../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md).  
  
 Você pode fazer o compilador C# ou do Visual Basic criar uma árvore de expressões para você com base em uma expressão lambda anônima ou criar árvores de expressão manualmente usando o namespace <xref:System.Linq.Expressions>.  
  
## <a name="creating-expression-trees-from-lambda-expressions"></a>Criando árvores de expressão de expressões Lambda  
 Quando uma expressão lambda é atribuída a uma variável do tipo <xref:System.Linq.Expressions.Expression%601>, o compilador emite código para criar uma árvore de expressão que representa a expressão lambda.  
  
 Os compiladores do C# podem gerar árvores de expressão apenas por meio de expressões lambda (ou lambdas de linha única). Ele não é possível analisar instruções lambdas (ou lambdas de várias linhas). Para obter mais informações sobre expressões lambda no C#, consulte [Expressões lambda](../../../language-reference/operators/lambda-expressions.md).  
  
 Os exemplos de código a seguir demonstram como fazer os compiladores do C# criarem uma árvore de expressão que representa a expressão lambda `num => num < 5`.  
  
```csharp  
Expression<Func<int, bool>> lambda = num => num < 5;  
```  
  
## <a name="creating-expression-trees-by-using-the-api"></a>Criando árvores de expressão usando a API  
 Para criar árvores de expressão usando a API, use a classe <xref:System.Linq.Expressions.Expression>. Essa classe contém métodos de fábrica estáticos para criar nós de árvore de expressão de tipos específicos, por exemplo, <xref:System.Linq.Expressions.ParameterExpression>, que representa uma variável ou parâmetro, ou <xref:System.Linq.Expressions.MethodCallExpression>, que representa uma chamada de método. <xref:System.Linq.Expressions.ParameterExpression>, <xref:System.Linq.Expressions.MethodCallExpression> e os outros tipos específicos de expressão também são definidos no namespace <xref:System.Linq.Expressions>. Esses tipos derivam do tipo abstrato <xref:System.Linq.Expressions.Expression>.  
  
 O exemplo de código a seguir demonstra como criar uma árvore de expressão que representa a expressão lambda `num => num < 5` usando a API.  
  
```csharp  
// Add the following using directive to your code file:  
// using System.Linq.Expressions;  
  
// Manually build the expression tree for
// the lambda expression num => num < 5.  
ParameterExpression numParam = Expression.Parameter(typeof(int), "num");  
ConstantExpression five = Expression.Constant(5, typeof(int));  
BinaryExpression numLessThanFive = Expression.LessThan(numParam, five);  
Expression<Func<int, bool>> lambda1 =  
    Expression.Lambda<Func<int, bool>>(  
        numLessThanFive,  
        new ParameterExpression[] { numParam });  
```  
  
 No .NET Framework 4 ou posterior, a API de árvores de expressão também dá suporte a atribuições e expressões de fluxo de controle, como loops, blocos condicionais e blocos `try-catch`. Usando a API, você pode criar árvores de expressão mais complexas do que aquelas que podem ser criadas por meio de expressões lambda pelos compiladores do C#. O exemplo a seguir demonstra como criar uma árvore de expressão que calcula o fatorial de um número.  
  
```csharp  
// Creating a parameter expression.  
ParameterExpression value = Expression.Parameter(typeof(int), "value");  
  
// Creating an expression to hold a local variable.
ParameterExpression result = Expression.Parameter(typeof(int), "result");  
  
// Creating a label to jump to from a loop.  
LabelTarget label = Expression.Label(typeof(int));  
  
// Creating a method body.  
BlockExpression block = Expression.Block(  
    // Adding a local variable.  
    new[] { result },  
    // Assigning a constant to a local variable: result = 1  
    Expression.Assign(result, Expression.Constant(1)),  
    // Adding a loop.  
        Expression.Loop(  
    // Adding a conditional block into the loop.  
           Expression.IfThenElse(  
    // Condition: value > 1  
               Expression.GreaterThan(value, Expression.Constant(1)),  
    // If true: result *= value --  
               Expression.MultiplyAssign(result,  
                   Expression.PostDecrementAssign(value)),  
    // If false, exit the loop and go to the label.  
               Expression.Break(label, result)  
           ),  
    // Label to jump to.  
       label  
    )  
);  
  
// Compile and execute an expression tree.  
int factorial = Expression.Lambda<Func<int, int>>(block, value).Compile()(5);  
  
Console.WriteLine(factorial);  
// Prints 120.  
```

Para saber mais, confira [Gerar métodos dinâmicos com árvores de expressão no Visual Studio 2010](https://devblogs.microsoft.com/csharpfaq/generating-dynamic-methods-with-expression-trees-in-visual-studio-2010/), que também se aplica a versões mais recentes do Visual Studio.
  
## <a name="parsing-expression-trees"></a>Analisando árvores de expressão  
 O exemplo de código a seguir demonstra como a árvore de expressão que representa a expressão lambda `num => num < 5` pode ser decomposta em suas partes.  
  
```csharp  
// Add the following using directive to your code file:  
// using System.Linq.Expressions;  
  
// Create an expression tree.  
Expression<Func<int, bool>> exprTree = num => num < 5;  
  
// Decompose the expression tree.  
ParameterExpression param = (ParameterExpression)exprTree.Parameters[0];  
BinaryExpression operation = (BinaryExpression)exprTree.Body;  
ParameterExpression left = (ParameterExpression)operation.Left;  
ConstantExpression right = (ConstantExpression)operation.Right;  
  
Console.WriteLine("Decomposed expression: {0} => {1} {2} {3}",  
                  param.Name, left.Name, operation.NodeType, right.Value);  
  
// This code produces the following output:  
  
// Decomposed expression: num => num LessThan 5  
```  
  
## <a name="immutability-of-expression-trees"></a>Imutabilidade das árvores de expressão  
 Árvores de expressão devem ser imutáveis. Isso significa que se você deseja modificar uma árvore de expressão, deverá criar uma nova árvore de expressão, copiando a existente e substituindo seus nós. Você pode usar um visitantes de árvore expressão para percorrer a árvore de expressão existente. Para obter mais informações, consulte [como modificar árvores de expressão (C#)](./how-to-modify-expression-trees.md).
  
## <a name="compiling-expression-trees"></a>Compilando árvores de expressão  
 O tipo <xref:System.Linq.Expressions.Expression%601> fornece o método <xref:System.Linq.Expressions.Expression%601.Compile%2A> que compila o código representado por uma árvore de expressão para um delegado executável.  
  
 O exemplo de código a seguir demonstra como compilar uma árvore de expressão e executar o código resultante.  
  
```csharp  
// Creating an expression tree.  
Expression<Func<int, bool>> expr = num => num < 5;  
  
// Compiling the expression tree into a delegate.  
Func<int, bool> result = expr.Compile();  
  
// Invoking the delegate and writing the result to the console.  
Console.WriteLine(result(4));  
  
// Prints True.  
  
// You can also use simplified syntax  
// to compile and run an expression tree.  
// The following line can replace two previous statements.  
Console.WriteLine(expr.Compile()(4));  
  
// Also prints True.  
```  
  
 Para obter mais informações, consulte [como executar árvores de expressão (C#)](./how-to-execute-expression-trees.md).
  
## <a name="see-also"></a>Consulte também

- <xref:System.Linq.Expressions>
- [Como executar árvores de expressão (C#)](./how-to-execute-expression-trees.md)
- [Como modificar árvores de expressão (C#)](./how-to-modify-expression-trees.md)
- [Expressões lambda](../../../language-reference/operators/lambda-expressions.md)
- [Visão geral do tempo de execução de linguagem dinâmica](../../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)
- [Conceitos de programação (C#)](../index.md)
