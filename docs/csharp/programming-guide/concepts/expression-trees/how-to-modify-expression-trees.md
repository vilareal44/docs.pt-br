---
title: Como modificar árvores de expressão (C#)
description: Saiba como modificar uma árvore de expressão criando uma cópia de uma árvore de expressão existente e fazendo as alterações necessárias.
ms.date: 07/20/2015
ms.assetid: 9b0cd8c2-457e-4833-9e36-31e79545f442
ms.openlocfilehash: 45aea18e253811d4e5c60f23f7f8496d4358f64c
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105603"
---
# <a name="how-to-modify-expression-trees-c"></a>Como modificar árvores de expressão (C#)
Este tópico mostra como modificar uma árvore de expressão. As árvores de expressão são imutáveis, o que significa que elas não podem ser diretamente modificadas. Para alterar uma árvore de expressão, você deve criar uma cópia de uma árvore de expressão existente e, ao criar a cópia, faça as alterações necessárias. Você pode usar a classe <xref:System.Linq.Expressions.ExpressionVisitor> para percorrer uma árvore de expressão existente e copiar cada nó que ela visitar.  
  
### <a name="to-modify-an-expression-tree"></a>Para modificar uma árvore de expressão  
  
1. Crie um novo projeto de **aplicativo de console** .  
  
2. Adicione uma diretiva `using` ao arquivo para o namespace `System.Linq.Expressions`.  
  
3. Adicione a classe `AndAlsoModifier` ao seu projeto.  
  
    ```csharp  
    public class AndAlsoModifier : ExpressionVisitor  
    {  
        public Expression Modify(Expression expression)  
        {  
            return Visit(expression);  
        }  
  
        protected override Expression VisitBinary(BinaryExpression b)  
        {  
            if (b.NodeType == ExpressionType.AndAlso)  
            {  
                Expression left = this.Visit(b.Left);  
                Expression right = this.Visit(b.Right);  
  
                // Make this binary expression an OrElse operation instead of an AndAlso operation.  
                return Expression.MakeBinary(ExpressionType.OrElse, left, right, b.IsLiftedToNull, b.Method);  
            }  
  
            return base.VisitBinary(b);  
        }  
    }  
    ```  
  
     Essa classe herda a classe <xref:System.Linq.Expressions.ExpressionVisitor> e é especializada para modificar expressões que representam operações `AND` condicionais. Ela muda essas operações de uma `AND` condicional para uma `OR` condicional. Para fazer isso, a classe substitui o método <xref:System.Linq.Expressions.ExpressionVisitor.VisitBinary%2A> do tipo base, pois as expressões `AND` condicionais são representadas como expressões binárias. No método `VisitBinary`, se a expressão que é passada a ele representa uma operação `AND` condicional, o código cria uma nova expressão que contém o operador `OR` condicional em vez do operador `AND` condicional. Se a expressão que é passada para o `VisitBinary` não representa uma operação `AND` condicional, o método adia para a implementação da classe base. Os métodos da classe base constroem nós que são semelhantes às árvores de expressão que são passadas, mas os nós têm suas subárvores substituídas pelas árvores de expressão que são produzidas recursivamente pelo visitante.  
  
4. Adicione uma diretiva `using` ao arquivo para o namespace `System.Linq.Expressions`.  
  
5. Adicione código para o método `Main` no arquivo Program.cs para criar uma árvore de expressão e passá-la ao método que a modificará.  
  
    ```csharp  
    Expression<Func<string, bool>> expr = name => name.Length > 10 && name.StartsWith("G");  
    Console.WriteLine(expr);  
  
    AndAlsoModifier treeModifier = new AndAlsoModifier();  
    Expression modifiedExpr = treeModifier.Modify((Expression) expr);  
  
    Console.WriteLine(modifiedExpr);  
  
    /*  This code produces the following output:  
  
        name => ((name.Length > 10) && name.StartsWith("G"))  
        name => ((name.Length > 10) || name.StartsWith("G"))  
    */  
    ```  
  
     O código cria uma expressão que contém uma operação `AND` condicional. Em seguida, ele cria uma instância da classe `AndAlsoModifier` e passa a expressão ao método `Modify` dessa classe. A árvore de expressão original e a modificada são geradas para mostrar a alteração.  
  
6. Compile e execute o aplicativo.  
  
## <a name="see-also"></a>Confira também

- [Como executar árvores de expressão (C#)](./how-to-execute-expression-trees.md)
- [Árvores de expressão (C#)](./index.md)
