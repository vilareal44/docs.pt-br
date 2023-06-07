---
title: Como executar árvores de expressão
ms.date: 07/20/2015
ms.assetid: 9dfb5ab3-f48f-417e-975f-f8f6f1cdc18d
ms.openlocfilehash: 7b7b08ea1a7a1310b1d98876be96f1fa28ecba91
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84375324"
---
# <a name="how-to-execute-expression-trees-visual-basic"></a>Como executar árvores de expressão (Visual Basic)
Este tópico mostra como executar uma árvore de expressão. Executar uma árvore de expressão pode retornar um valor ou apenas realizar uma ação, como chamar um método.  
  
 Somente árvores de expressão que representam expressões lambda podem ser executadas. Árvores de expressão que representam expressões lambda são do tipo <xref:System.Linq.Expressions.LambdaExpression> ou <xref:System.Linq.Expressions.Expression%601>. Para executar essas árvores de expressão, chame o método <xref:System.Linq.Expressions.LambdaExpression.Compile%2A> para criar um delegado executável e, em seguida, invoque o delegado.  
  
> [!NOTE]
> Se o tipo de delegado não for conhecido, ou seja, a expressão lambda for do tipo <xref:System.Linq.Expressions.LambdaExpression> e não <xref:System.Linq.Expressions.Expression%601>, você deverá chamar o método <xref:System.Delegate.DynamicInvoke%2A> sobre o delegado em vez de invocá-la diretamente.  
  
 Se uma árvore de expressão não representa uma expressão lambda, você pode criar uma nova expressão lambda que tenha a árvore de expressão original como corpo, chamando o método <xref:System.Linq.Expressions.Expression.Lambda%60%601%28System.Linq.Expressions.Expression%2CSystem.Collections.Generic.IEnumerable%7BSystem.Linq.Expressions.ParameterExpression%7D%29>. Em seguida, você pode executar a expressão lambda como descrito anteriormente nesta seção.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como executar uma árvore de expressão que representa a elevação de um número a uma potência, criando uma expressão lambda e executando-a. O resultado, representado pelo número elevado à potência, é exibido.  
  
```vb  
' The expression tree to execute.  
Dim be As BinaryExpression = Expression.Power(Expression.Constant(2.0R), Expression.Constant(3.0R))  
  
' Create a lambda expression.  
Dim le As Expression(Of Func(Of Double)) = Expression.Lambda(Of Func(Of Double))(be)  
  
' Compile the lambda expression.  
Dim compiledExpression As Func(Of Double) = le.Compile()  
  
' Execute the lambda expression.  
Dim result As Double = compiledExpression()  
  
' Display the result.  
MsgBox(result)  
  
' This code produces the following output:  
' 8  
```  
  
## <a name="compile-the-code"></a>Compilar o código  
  
- Inclua o namespace System.Linq.Expressions.  
  
## <a name="see-also"></a>Confira também

- [Árvores de expressão (Visual Basic)](index.md)
- [Como modificar árvores de expressão (Visual Basic)](how-to-modify-expression-trees.md)
