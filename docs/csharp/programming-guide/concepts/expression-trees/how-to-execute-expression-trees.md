---
title: Como executar árvores de expressão (C#)
description: Saiba como executar uma árvore de expressão para retornar um valor ou executar uma ação como chamar um método.
ms.date: 07/20/2015
ms.assetid: b8c40db5-2464-4bb9-9001-8c2bc7f006c5
ms.openlocfilehash: 9e306da545ba6c6275f36b8f6dd4e98bb91ed54e
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105612"
---
# <a name="how-to-execute-expression-trees-c"></a>Como executar árvores de expressão (C#)
Este tópico mostra como executar uma árvore de expressão. Executar uma árvore de expressão pode retornar um valor ou apenas realizar uma ação, como chamar um método.  
  
 Somente árvores de expressão que representam expressões lambda podem ser executadas. Árvores de expressão que representam expressões lambda são do tipo <xref:System.Linq.Expressions.LambdaExpression> ou <xref:System.Linq.Expressions.Expression%601>. Para executar essas árvores de expressão, chame o método <xref:System.Linq.Expressions.LambdaExpression.Compile%2A> para criar um delegado executável e, em seguida, invoque o delegado.  
  
> [!NOTE]
> Se o tipo de delegado não for conhecido, ou seja, a expressão lambda for do tipo <xref:System.Linq.Expressions.LambdaExpression> e não <xref:System.Linq.Expressions.Expression%601>, você deverá chamar o método <xref:System.Delegate.DynamicInvoke%2A> sobre o delegado em vez de invocá-la diretamente.  
  
 Se uma árvore de expressão não representa uma expressão lambda, você pode criar uma nova expressão lambda que tenha a árvore de expressão original como corpo, chamando o método <xref:System.Linq.Expressions.Expression.Lambda%60%601%28System.Linq.Expressions.Expression%2CSystem.Collections.Generic.IEnumerable%7BSystem.Linq.Expressions.ParameterExpression%7D%29>. Em seguida, você pode executar a expressão lambda como descrito anteriormente nesta seção.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como executar uma árvore de expressão que representa a elevação de um número a uma potência, criando uma expressão lambda e executando-a. O resultado, representado pelo número elevado à potência, é exibido.  
  
```csharp  
// The expression tree to execute.  
BinaryExpression be = Expression.Power(Expression.Constant(2D), Expression.Constant(3D));  
  
// Create a lambda expression.  
Expression<Func<double>> le = Expression.Lambda<Func<double>>(be);  
  
// Compile the lambda expression.  
Func<double> compiledExpression = le.Compile();  
  
// Execute the lambda expression.  
double result = compiledExpression();  
  
// Display the result.  
Console.WriteLine(result);  
  
// This code produces the following output:  
// 8  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
  
- Inclua o namespace System.Linq.Expressions.  
  
## <a name="see-also"></a>Confira também

- [Árvores de expressão (C#)](./index.md)
- [Como modificar árvores de expressão (C#)](./how-to-modify-expression-trees.md)
