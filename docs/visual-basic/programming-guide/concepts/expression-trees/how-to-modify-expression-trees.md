---
title: Como modificar árvores de expressão
ms.date: 07/20/2015
ms.assetid: d1309fff-28bd-4d8e-a2cf-75725999e8f2
ms.openlocfilehash: 1f052120a2e7e12f5a985adce3ae193afec0e9af
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410986"
---
# <a name="how-to-modify-expression-trees-visual-basic"></a>Como modificar árvores de expressão (Visual Basic)

Este tópico mostra como modificar uma árvore de expressão. As árvores de expressão são imutáveis, o que significa que elas não podem ser diretamente modificadas. Para alterar uma árvore de expressão, você deve criar uma cópia de uma árvore de expressão existente e, ao criar a cópia, faça as alterações necessárias. Você pode usar a classe <xref:System.Linq.Expressions.ExpressionVisitor> para percorrer uma árvore de expressão existente e copiar cada nó que ela visitar.

## <a name="to-modify-an-expression-tree"></a>Para modificar uma árvore de expressão

1. Crie um novo projeto de **aplicativo de console** .

2. Adicione uma `Imports` instrução ao arquivo para o `System.Linq.Expressions` namespace.

3. Adicione a classe `AndAlsoModifier` ao seu projeto.

    ```vb
    Public Class AndAlsoModifier
        Inherits ExpressionVisitor

        Public Function Modify(ByVal expr As Expression) As Expression
            Return Visit(expr)
        End Function

        Protected Overrides Function VisitBinary(ByVal b As BinaryExpression) As Expression
            If b.NodeType = ExpressionType.AndAlso Then
                Dim left = Me.Visit(b.Left)
                Dim right = Me.Visit(b.Right)

                ' Make this binary expression an OrElse operation instead
                ' of an AndAlso operation.
                Return Expression.MakeBinary(ExpressionType.OrElse, left, right, _
                                             b.IsLiftedToNull, b.Method)
            End If

            Return MyBase.VisitBinary(b)
        End Function
    End Class
    ```

    Essa classe herda a classe <xref:System.Linq.Expressions.ExpressionVisitor> e é especializada para modificar expressões que representam operações `AND` condicionais. Ela muda essas operações de uma `AND` condicional para uma `OR` condicional. Para fazer isso, a classe substitui o método <xref:System.Linq.Expressions.ExpressionVisitor.VisitBinary%2A> do tipo base, pois as expressões `AND` condicionais são representadas como expressões binárias. No método `VisitBinary`, se a expressão que é passada a ele representa uma operação `AND` condicional, o código cria uma nova expressão que contém o operador `OR` condicional em vez do operador `AND` condicional. Se a expressão que é passada para o `VisitBinary` não representa uma operação `AND` condicional, o método adia para a implementação da classe base. Os métodos da classe base constroem nós que são semelhantes às árvores de expressão que são passadas, mas os nós têm suas subárvores substituídas pelas árvores de expressão que são produzidas recursivamente pelo visitante.

4. Adicione uma `Imports` instrução ao arquivo para o `System.Linq.Expressions` namespace.

5. Adicione código ao `Main` método no arquivo Module1. vb para criar uma árvore de expressão e passá-la para o método que irá modificá-la.

    ```vb
    Dim expr As Expression(Of Func(Of String, Boolean)) = _
        Function(name) name.Length > 10 AndAlso name.StartsWith("G")

    Console.WriteLine(expr)

    Dim modifier As New AndAlsoModifier()
    Dim modifiedExpr = modifier.Modify(CType(expr, Expression))

    Console.WriteLine(modifiedExpr)

    ' This code produces the following output:
    ' name => ((name.Length > 10) && name.StartsWith("G"))
    ' name => ((name.Length > 10) || name.StartsWith("G"))
    ```

    O código cria uma expressão que contém uma operação `AND` condicional. Em seguida, ele cria uma instância da classe `AndAlsoModifier` e passa a expressão ao método `Modify` dessa classe. A árvore de expressão original e a modificada são geradas para mostrar a alteração.

6. Compile e execute o aplicativo.

## <a name="see-also"></a>Confira também

- [Como executar árvores de expressão (Visual Basic)](how-to-execute-expression-trees.md)
- [Árvores de expressão (Visual Basic)](index.md)
