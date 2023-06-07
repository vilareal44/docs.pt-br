---
title: Árvores de expressão
ms.date: 07/20/2015
ms.assetid: 8bbbb02d-7ffc-476b-8c25-118d82bf5d46
ms.openlocfilehash: 5d30b2e2e66aa322e6d43b5fbf4a4baf3435b2a6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410961"
---
# <a name="expression-trees-visual-basic"></a>Árvores de expressão (Visual Basic)
Árvores de expressão representam código em uma estrutura de dados de árvore, onde cada nó é, por exemplo, uma expressão, uma chamada de método ou uma operação binária como `x < y`.  
  
 Você pode compilar e executar código representado por árvores de expressão. Isso permite a modificação dinâmica de código executável, a execução de consultas LINQ em vários bancos de dados e a criação de consultas dinâmicas. Para obter mais informações sobre árvores de expressão no LINQ, consulte [Como usar árvores de expressão para compilar consultas dinâmicas (Visual Basic)](how-to-use-expression-trees-to-build-dynamic-queries.md).  
  
 Árvores de expressão também são usadas no runtime de linguagem dinâmica (DLR) para fornecer interoperabilidade entre linguagens dinâmicas e o .NET Framework e permitir que gravadores compiladores emitam árvores de expressão em vez de Microsoft intermediate language (MSIL). Para obter mais informações sobre o DLR, consulte [Visão geral do Dynamic Language Runtime](../../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md).  
  
 Você pode fazer o compilador C# ou do Visual Basic criar uma árvore de expressões para você com base em uma expressão lambda anônima ou criar árvores de expressão manualmente usando o namespace <xref:System.Linq.Expressions>.  
  
## <a name="creating-expression-trees-from-lambda-expressions"></a>Criando árvores de expressão de expressões Lambda  
 Quando uma expressão lambda é atribuída a uma variável do tipo <xref:System.Linq.Expressions.Expression%601>, o compilador emite código para criar uma árvore de expressão que representa a expressão lambda.  
  
 Os compiladores do Visual Basic podem gerar árvores de expressão de expressões lambdas (ou lambdas de linha única). Ele não é possível analisar instruções lambdas (ou lambdas de várias linhas). Para obter mais informações sobre expressões lambda no Visual Basic, consulte [Expressões lambda](../../language-features/procedures/lambda-expressions.md).  
  
 Os exemplos de código a seguir demonstram como fazer os compiladores do Visual Basic criarem uma árvore de expressão que representa a expressão lambda `Function(num) num < 5`.  
  
```vb  
Dim lambda As Expression(Of Func(Of Integer, Boolean)) =  
    Function(num) num < 5  
```  
  
## <a name="creating-expression-trees-by-using-the-api"></a>Criando árvores de expressão usando a API  
 Para criar árvores de expressão usando a API, use a classe <xref:System.Linq.Expressions.Expression>. Essa classe contém métodos de fábrica estáticos para criar nós de árvore de expressão de tipos específicos, por exemplo, <xref:System.Linq.Expressions.ParameterExpression>, que representa uma variável ou parâmetro, ou <xref:System.Linq.Expressions.MethodCallExpression>, que representa uma chamada de método. <xref:System.Linq.Expressions.ParameterExpression>, <xref:System.Linq.Expressions.MethodCallExpression> e os outros tipos específicos de expressão também são definidos no namespace <xref:System.Linq.Expressions>. Esses tipos derivam do tipo abstrato <xref:System.Linq.Expressions.Expression>.  
  
 O exemplo de código a seguir demonstra como criar uma árvore de expressão que representa a expressão lambda `Function(num) num < 5` usando a API.  
  
```vb  
' Import the following namespace to your project: System.Linq.Expressions  
  
' Manually build the expression tree for the lambda expression num => num < 5.  
Dim numParam As ParameterExpression = Expression.Parameter(GetType(Integer), "num")  
Dim five As ConstantExpression = Expression.Constant(5, GetType(Integer))  
Dim numLessThanFive As BinaryExpression = Expression.LessThan(numParam, five)  
Dim lambda1 As Expression(Of Func(Of Integer, Boolean)) =  
  Expression.Lambda(Of Func(Of Integer, Boolean))(  
        numLessThanFive,  
        New ParameterExpression() {numParam})  
```  
  
 No .NET Framework 4 ou posterior, a API de árvores de expressão também dá suporte a atribuições e expressões de fluxo de controle, como loops, blocos condicionais e blocos `try-catch`. Usando a API, você pode criar árvores de expressão mais complexas do que aquelas que podem ser criadas por meio de expressões lambda pelos compiladores do Visual Basic. O exemplo a seguir demonstra como criar uma árvore de expressão que calcula o fatorial de um número.  
  
```vb  
' Creating a parameter expression.  
Dim value As ParameterExpression =  
    Expression.Parameter(GetType(Integer), "value")  
  
' Creating an expression to hold a local variable.
Dim result As ParameterExpression =  
    Expression.Parameter(GetType(Integer), "result")  
  
' Creating a label to jump to from a loop.  
Dim label As LabelTarget = Expression.Label(GetType(Integer))  
  
' Creating a method body.  
Dim block As BlockExpression = Expression.Block(  
    New ParameterExpression() {result},  
    Expression.Assign(result, Expression.Constant(1)),  
    Expression.Loop(  
        Expression.IfThenElse(  
            Expression.GreaterThan(value, Expression.Constant(1)),  
            Expression.MultiplyAssign(result,  
                Expression.PostDecrementAssign(value)),  
            Expression.Break(label, result)  
        ),  
        label  
    )  
)  
  
' Compile an expression tree and return a delegate.  
Dim factorial As Integer =  
    Expression.Lambda(Of Func(Of Integer, Integer))(block, value).Compile()(5)  
  
Console.WriteLine(factorial)  
' Prints 120.  
```

Para saber mais, confira [Gerar métodos dinâmicos com árvores de expressão no Visual Studio 2010](https://devblogs.microsoft.com/csharpfaq/generating-dynamic-methods-with-expression-trees-in-visual-studio-2010/), que também se aplica a versões mais recentes do Visual Studio.
  
## <a name="parsing-expression-trees"></a>Analisando árvores de expressão  
 O exemplo de código a seguir demonstra como a árvore de expressão que representa a expressão lambda `Function(num) num < 5` pode ser decomposta em suas partes.  
  
```vb  
' Import the following namespace to your project: System.Linq.Expressions  
  
' Create an expression tree.  
Dim exprTree As Expression(Of Func(Of Integer, Boolean)) = Function(num) num < 5  
  
' Decompose the expression tree.  
Dim param As ParameterExpression = exprTree.Parameters(0)  
Dim operation As BinaryExpression = exprTree.Body  
Dim left As ParameterExpression = operation.Left  
Dim right As ConstantExpression = operation.Right  
  
Console.WriteLine(String.Format("Decomposed expression: {0} => {1} {2} {3}",  
                  param.Name, left.Name, operation.NodeType, right.Value))  
  
' This code produces the following output:  
'  
' Decomposed expression: num => num LessThan 5  
```  
  
## <a name="immutability-of-expression-trees"></a>Imutabilidade das árvores de expressão  
 Árvores de expressão devem ser imutáveis. Isso significa que se você deseja modificar uma árvore de expressão, deverá criar uma nova árvore de expressão, copiando a existente e substituindo seus nós. Você pode usar um visitantes de árvore expressão para percorrer a árvore de expressão existente. Para obter mais informações, consulte [Como modificar árvores de expressão (Visual Basic)](how-to-modify-expression-trees.md).  
  
## <a name="compiling-expression-trees"></a>Compilando árvores de expressão  
 O tipo <xref:System.Linq.Expressions.Expression%601> fornece o método <xref:System.Linq.Expressions.Expression%601.Compile%2A> que compila o código representado por uma árvore de expressão para um delegado executável.  
  
 O exemplo de código a seguir demonstra como compilar uma árvore de expressão e executar o código resultante.  
  
```vb  
' Creating an expression tree.  
Dim expr As Expression(Of Func(Of Integer, Boolean)) =  
    Function(num) num < 5  
  
' Compiling the expression tree into a delegate.  
Dim result As Func(Of Integer, Boolean) = expr.Compile()  
  
' Invoking the delegate and writing the result to the console.  
Console.WriteLine(result(4))  
  
' Prints True.  
  
' You can also use simplified syntax  
' to compile and run an expression tree.  
' The following line can replace two previous statements.  
Console.WriteLine(expr.Compile()(4))  
  
' Also prints True.  
```  
  
 Para obter mais informações, consulte [Como executar árvores de expressão (Visual Basic)](how-to-execute-expression-trees.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Linq.Expressions>
- [Como executar árvores de expressão (Visual Basic)](how-to-execute-expression-trees.md)
- [Como modificar árvores de expressão (Visual Basic)](how-to-modify-expression-trees.md)
- [Expressões lambda](../../language-features/procedures/lambda-expressions.md)
- [Visão geral do tempo de execução de linguagem dinâmica](../../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)
- [Conceitos de Programação (Visual Basic)](../index.md)
