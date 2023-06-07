---
title: Interpretando Expressões
description: Aprenda como escrever código para examinar a estrutura de um árvore de expressão.
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: adf73dde-1e52-4df3-9929-2e0670e28e16
ms.openlocfilehash: ea205d42b02ea7b38c04cb70d322329cf7c1d495
ms.sourcegitcommit: 03fec33630b46e78d5e81e91b40518f32c4bd7b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84004641"
---
# <a name="interpreting-expressions"></a>Interpretando Expressões

[Anterior – Executando expressões](expression-trees-execution.md)

Agora, vamos escrever código para examinar a estrutura de um *árvore de expressão*. Cada nó em uma árvore de expressão será um objeto de uma classe derivada de `Expression`.

Esse design torna visitar todos os nós em uma árvore de expressão uma operação recursiva relativamente simples. A estratégia geral é iniciar no nó raiz e determine que tipo de nó ele é.

Se o tipo de nó tiver filhos, visite os filhos recursivamente. Em cada nó filho, repita o processo usado no nó raiz: determine o tipo e, se o tipo tiver filhos, visite cada um dos filhos.

## <a name="examining-an-expression-with-no-children"></a>Examinando uma expressão sem filhos
Vamos começar visitando cada nó em uma árvore de expressão simples.
Este é o código que cria uma expressão constante e, em seguida, examina suas propriedades:

```csharp
var constant = Expression.Constant(24, typeof(int));

Console.WriteLine($"This is a/an {constant.NodeType} expression type");
Console.WriteLine($"The type of the constant value is {constant.Type}");
Console.WriteLine($"The value of the constant value is {constant.Value}");
```

Isso imprimirá o seguinte:

```output
This is an Constant expression type
The type of the constant value is System.Int32
The value of the constant value is 24
```

Agora, vamos escrever o código que examinaria essa expressão e escrever algumas propriedades importantes sobre ele. Este é o código:

## <a name="examining-a-simple-addition-expression"></a>Examinando uma expressão de adição simples

Vamos começar com o exemplo de adição da introdução desta seção.

```csharp
Expression<Func<int>> sum = () => 1 + 2;
```

> Não estou usando `var` para declarar essa árvore de expressão, pois isso não é possível porque o lado direito da atribuição é de um tipo implícito.

O nó raiz é um `LambdaExpression`. Para obter o código interessante no lado direito do `=>` operador, você precisa encontrar um dos filhos do `LambdaExpression` .... Faremos isso com todas as expressões nesta seção. O nó pai nos ajudar a localizar o tipo de retorno do `LambdaExpression`.

Para examinar cada nó nesta expressão, precisaremos visitar recursivamente alguns nós. Esta é uma primeira implementação simples:

```csharp
Expression<Func<int, int, int>> addition = (a, b) => a + b;

Console.WriteLine($"This expression is a {addition.NodeType} expression type");
Console.WriteLine($"The name of the lambda is {((addition.Name == null) ? "<null>" : addition.Name)}");
Console.WriteLine($"The return type is {addition.ReturnType.ToString()}");
Console.WriteLine($"The expression has {addition.Parameters.Count} arguments. They are:");
foreach(var argumentExpression in addition.Parameters)
{
    Console.WriteLine($"\tParameter Type: {argumentExpression.Type.ToString()}, Name: {argumentExpression.Name}");
}

var additionBody = (BinaryExpression)addition.Body;
Console.WriteLine($"The body is a {additionBody.NodeType} expression");
Console.WriteLine($"The left side is a {additionBody.Left.NodeType} expression");
var left = (ParameterExpression)additionBody.Left;
Console.WriteLine($"\tParameter Type: {left.Type.ToString()}, Name: {left.Name}");
Console.WriteLine($"The right side is a {additionBody.Right.NodeType} expression");
var right= (ParameterExpression)additionBody.Right;
Console.WriteLine($"\tParameter Type: {right.Type.ToString()}, Name: {right.Name}");
```

Este exemplo imprime a seguinte saída:

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 2 arguments. They are:
        Parameter Type: System.Int32, Name: a
        Parameter Type: System.Int32, Name: b
The body is a/an Add expression
The left side is a Parameter expression
        Parameter Type: System.Int32, Name: a
The right side is a Parameter expression
        Parameter Type: System.Int32, Name: b
```

Você vai notar que há muita repetição no exemplo de código acima.
Vamos limpar tudo isso e criar um visitante de nós de expressão com uma finalidade mais geral. Para isso, precisaremos escrever um algoritmo recursivo. Qualquer nó poderia ser de um tipo que pode ter filhos.
Qualquer nó que tem filhos exige que nós visitemos esses filhos e determinemos o que é esse nó. Esta é a versão limpa que utiliza a recursão para visitar as operações de adição:

```csharp
// Base Visitor class:
public abstract class Visitor
{
    private readonly Expression node;

    protected Visitor(Expression node)
    {
        this.node = node;
    }

    public abstract void Visit(string prefix);

    public ExpressionType NodeType => this.node.NodeType;
    public static Visitor CreateFromExpression(Expression node)
    {
        switch(node.NodeType)
        {
            case ExpressionType.Constant:
                return new ConstantVisitor((ConstantExpression)node);
            case ExpressionType.Lambda:
                return new LambdaVisitor((LambdaExpression)node);
            case ExpressionType.Parameter:
                return new ParameterVisitor((ParameterExpression)node);
            case ExpressionType.Add:
                return new BinaryVisitor((BinaryExpression)node);
            default:
                Console.Error.WriteLine($"Node not processed yet: {node.NodeType}");
                return default(Visitor);
        }
    }
}

// Lambda Visitor
public class LambdaVisitor : Visitor
{
    private readonly LambdaExpression node;
    public LambdaVisitor(LambdaExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This expression is a {NodeType} expression type");
        Console.WriteLine($"{prefix}The name of the lambda is {((node.Name == null) ? "<null>" : node.Name)}");
        Console.WriteLine($"{prefix}The return type is {node.ReturnType.ToString()}");
        Console.WriteLine($"{prefix}The expression has {node.Parameters.Count} argument(s). They are:");
        // Visit each parameter:
        foreach (var argumentExpression in node.Parameters)
        {
            var argumentVisitor = Visitor.CreateFromExpression(argumentExpression);
            argumentVisitor.Visit(prefix + "\t");
        }
        Console.WriteLine($"{prefix}The expression body is:");
        // Visit the body:
        var bodyVisitor = Visitor.CreateFromExpression(node.Body);
        bodyVisitor.Visit(prefix + "\t");
    }
}

// Binary Expression Visitor:
public class BinaryVisitor : Visitor
{
    private readonly BinaryExpression node;
    public BinaryVisitor(BinaryExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This binary expression is a {NodeType} expression");
        var left = Visitor.CreateFromExpression(node.Left);
        Console.WriteLine($"{prefix}The Left argument is:");
        left.Visit(prefix + "\t");
        var right = Visitor.CreateFromExpression(node.Right);
        Console.WriteLine($"{prefix}The Right argument is:");
        right.Visit(prefix + "\t");
    }
}

// Parameter visitor:
public class ParameterVisitor : Visitor
{
    private readonly ParameterExpression node;
    public ParameterVisitor(ParameterExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This is an {NodeType} expression type");
        Console.WriteLine($"{prefix}Type: {node.Type.ToString()}, Name: {node.Name}, ByRef: {node.IsByRef}");
    }
}

// Constant visitor:
public class ConstantVisitor : Visitor
{
    private readonly ConstantExpression node;
    public ConstantVisitor(ConstantExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This is an {NodeType} expression type");
        Console.WriteLine($"{prefix}The type of the constant value is {node.Type}");
        Console.WriteLine($"{prefix}The value of the constant value is {node.Value}");
    }
}
```

Esse algoritmo é a base de um algoritmo que pode visitar qualquer `LambdaExpression` arbitrário. Há muitos buracos, ou seja, o código que criei apenas procura uma amostra muito pequena dos possíveis conjuntos de nós da árvore de expressão que ele pode encontrar. No entanto, ainda é possível aprender bastante com o que ele produz. (O caso padrão no método `Visitor.CreateFromExpression` imprime uma mensagem no console de erro quando um novo tipo de nó é encontrado. Dessa forma, você sabe que precisa adicionar um novo tipo de expressão.)

Quando executa esse visitante na expressão de adição mostrada acima, você obtém a saída a seguir:

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 2 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: a, ByRef: False
        This is an Parameter expression type
        Type: System.Int32, Name: b, ByRef: False
The expression body is:
        This binary expression is a Add expression
        The Left argument is:
                This is an Parameter expression type
                Type: System.Int32, Name: a, ByRef: False
        The Right argument is:
                This is an Parameter expression type
                Type: System.Int32, Name: b, ByRef: False
```

Agora que criou uma implementação de visitante mais geral, você pode visitar e processar muitos tipos diferentes de expressões.

## <a name="examining-an-addition-expression-with-many-levels"></a>Examinando uma expressão de adição com vários níveis

Vamos testar um exemplo mais complicado, mas ainda limitar os tipos de nó somente à adição:

```csharp
Expression<Func<int>> sum = () => 1 + 2 + 3 + 4;
```

Antes de executar isso no algoritmo de visitante, tente pensar no que poderia ser a saída. Lembre-se de que o operador `+` é um *operador binário*: ele deve ter dois filhos, que representam os operandos esquerdo e direito. Há várias maneiras possíveis de construir uma árvore que podem ser corretas:

```csharp
Expression<Func<int>> sum1 = () => 1 + (2 + (3 + 4));
Expression<Func<int>> sum2 = () => ((1 + 2) + 3) + 4;

Expression<Func<int>> sum3 = () => (1 + 2) + (3 + 4);
Expression<Func<int>> sum4 = () => 1 + ((2 + 3) + 4);
Expression<Func<int>> sum5 = () => (1 + (2 + 3)) + 4;
```

Você pode ver a separação em duas possíveis respostas para realçar a mais promissora. A primeira representa expressões *associativas à direita*. A segunda representa expressões *associativas à esquerda*.
A vantagem desses dois formatos é que o formato pode ser dimensionado para qualquer número arbitrário de expressões de adição.

Se você executar essa expressão por meio do visitante, verá essa saída, verificando se a expressão de adição simples é a *associação à esquerda*.

Para executar esse exemplo e ver a árvore de expressão completa, eu precise fazer uma alteração na árvore de expressão de origem. Quando a árvore de expressão contém todas as constantes, a árvore resultante contém apenas o valor constante de `10`. O compilador executa toda a adição e reduz a expressão a sua forma mais simples. Simplesmente adicionar uma variável à expressão é suficiente para ver a árvore original:

```csharp
Expression<Func<int, int>> sum = (a) => 1 + a + 3 + 4;
```

Crie um visitante para essa soma e execute o visitante; você verá esta saída:

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 1 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: a, ByRef: False
The expression body is:
        This binary expression is a Add expression
        The Left argument is:
                This binary expression is a Add expression
                The Left argument is:
                        This binary expression is a Add expression
                        The Left argument is:
                                This is an Constant expression type
                                The type of the constant value is System.Int32
                                The value of the constant value is 1
                        The Right argument is:
                                This is an Parameter expression type
                                Type: System.Int32, Name: a, ByRef: False
                The Right argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 3
        The Right argument is:
                This is an Constant expression type
                The type of the constant value is System.Int32
                The value of the constant value is 4
```

Você também pode executar qualquer um dos outros exemplos pelo código visitante e ver que árvore ele representa. Veja um exemplo da expressão `sum3` acima (com um parâmetro adicional para impedir que o compilador calcule a constante):

```csharp
Expression<Func<int, int, int>> sum3 = (a, b) => (1 + a) + (3 + b);
```

Esta é a saída do visitante:

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 2 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: a, ByRef: False
        This is an Parameter expression type
        Type: System.Int32, Name: b, ByRef: False
The expression body is:
        This binary expression is a Add expression
        The Left argument is:
                This binary expression is a Add expression
                The Left argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 1
                The Right argument is:
                        This is an Parameter expression type
                        Type: System.Int32, Name: a, ByRef: False
        The Right argument is:
                This binary expression is a Add expression
                The Left argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 3
                The Right argument is:
                        This is an Parameter expression type
                        Type: System.Int32, Name: b, ByRef: False
```

Observe que os parênteses não fazem parte da saída. Não há nenhum nó na árvore de expressão que representa os parênteses na expressão de entrada. A estrutura de árvore de expressão contém todas as informações necessárias para comunicar a precedência.

## <a name="extending-from-this-sample"></a>Estendendo deste exemplo

O exemplo lida apenas com as árvores de expressão mais rudimentares. O código que você viu nesta seção só lida com inteiros constantes e com o operador `+` binário. Como um exemplo final, vamos atualizar o visitante para lidar com uma expressão mais complicada. Vamos fazer com que ele funcione para isto:

```csharp
Expression<Func<int, int>> factorial = (n) =>
    n == 0 ?
    1 :
    Enumerable.Range(1, n).Aggregate((product, factor) => product * factor);
```

Este código representa uma possível implementação da função *fatorial* matemática. A maneira como escrevi este código destaca duas limitações da criação de árvores de expressão atribuindo expressões lambda a Expressões. Primeiro, lambdas de instrução não são permitidos. Isso significa que eu não posso usar loops, blocos, instruções if/else e outras estruturas de controle comuns em C#. Estou limitado ao uso de expressões. Em segundo lugar, não posso chamar recursivamente a mesma expressão.
Eu poderia se ela já fosse um delegado, mas não posso chamá-la em sua forma de árvore de expressão. Na seção sobre a [criação de árvores de expressão](expression-trees-building.md), você aprenderá técnicas para superar essas limitações.

Nesta expressão, você encontrará todos esses tipos de nós:

1. Igual (expressão binária)
2. Multiplicar (expressão binária)
3. Condicional (a expressão ? :)
4. Expressão de chamada de método (chamar `Range()` e `Aggregate()`)

Uma maneira de modificar o algoritmo do visitante é continuar executando-o e escrever o tipo de nó toda vez que você atingir sua cláusula `default`. Após algumas iterações, você terá cisto todos os nós potenciais. Então, você tem tudo de que você precisa. O resultado seria algo semelhante a:

```csharp
public static Visitor CreateFromExpression(Expression node)
{
    switch(node.NodeType)
    {
        case ExpressionType.Constant:
            return new ConstantVisitor((ConstantExpression)node);
        case ExpressionType.Lambda:
            return new LambdaVisitor((LambdaExpression)node);
        case ExpressionType.Parameter:
            return new ParameterVisitor((ParameterExpression)node);
        case ExpressionType.Add:
        case ExpressionType.Equal:
        case ExpressionType.Multiply:
            return new BinaryVisitor((BinaryExpression)node);
        case ExpressionType.Conditional:
            return new ConditionalVisitor((ConditionalExpression)node);
        case ExpressionType.Call:
            return new MethodCallVisitor((MethodCallExpression)node);
        default:
            Console.Error.WriteLine($"Node not processed yet: {node.NodeType}");
            return default(Visitor);
    }
}
```

O ConditionalVisitor e o MethodCallVisitor processam esses dois nós:

```csharp
public class ConditionalVisitor : Visitor
{
    private readonly ConditionalExpression node;
    public ConditionalVisitor(ConditionalExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This expression is a {NodeType} expression");
        var testVisitor = Visitor.CreateFromExpression(node.Test);
        Console.WriteLine($"{prefix}The Test for this expression is:");
        testVisitor.Visit(prefix + "\t");
        var trueVisitor = Visitor.CreateFromExpression(node.IfTrue);
        Console.WriteLine($"{prefix}The True clause for this expression is:");
        trueVisitor.Visit(prefix + "\t");
        var falseVisitor = Visitor.CreateFromExpression(node.IfFalse);
        Console.WriteLine($"{prefix}The False clause for this expression is:");
        falseVisitor.Visit(prefix + "\t");
    }
}

public class MethodCallVisitor : Visitor
{
    private readonly MethodCallExpression node;
    public MethodCallVisitor(MethodCallExpression node) : base(node)
    {
        this.node = node;
    }

    public override void Visit(string prefix)
    {
        Console.WriteLine($"{prefix}This expression is a {NodeType} expression");
        if (node.Object == null)
            Console.WriteLine($"{prefix}This is a static method call");
        else
        {
            Console.WriteLine($"{prefix}The receiver (this) is:");
            var receiverVisitor = Visitor.CreateFromExpression(node.Object);
            receiverVisitor.Visit(prefix + "\t");
        }

        var methodInfo = node.Method;
        Console.WriteLine($"{prefix}The method name is {methodInfo.DeclaringType}.{methodInfo.Name}");
        // There is more here, like generic arguments, and so on.
        Console.WriteLine($"{prefix}The Arguments are:");
        foreach(var arg in node.Arguments)
        {
            var argVisitor = Visitor.CreateFromExpression(arg);
            argVisitor.Visit(prefix + "\t");
        }
    }
}
```

E a saída da árvore de expressão seria:

```output
This expression is a/an Lambda expression type
The name of the lambda is <null>
The return type is System.Int32
The expression has 1 argument(s). They are:
        This is an Parameter expression type
        Type: System.Int32, Name: n, ByRef: False
The expression body is:
        This expression is a Conditional expression
        The Test for this expression is:
                This binary expression is a Equal expression
                The Left argument is:
                        This is an Parameter expression type
                        Type: System.Int32, Name: n, ByRef: False
                The Right argument is:
                        This is an Constant expression type
                        The type of the constant value is System.Int32
                        The value of the constant value is 0
        The True clause for this expression is:
                This is an Constant expression type
                The type of the constant value is System.Int32
                The value of the constant value is 1
        The False clause for this expression is:
                This expression is a Call expression
                This is a static method call
                The method name is System.Linq.Enumerable.Aggregate
                The Arguments are:
                        This expression is a Call expression
                        This is a static method call
                        The method name is System.Linq.Enumerable.Range
                        The Arguments are:
                                This is an Constant expression type
                                The type of the constant value is System.Int32
                                The value of the constant value is 1
                                This is an Parameter expression type
                                Type: System.Int32, Name: n, ByRef: False
                        This expression is a Lambda expression type
                        The name of the lambda is <null>
                        The return type is System.Int32
                        The expression has 2 arguments. They are:
                                This is an Parameter expression type
                                Type: System.Int32, Name: product, ByRef: False
                                This is an Parameter expression type
                                Type: System.Int32, Name: factor, ByRef: False
                        The expression body is:
                                This binary expression is a Multiply expression
                                The Left argument is:
                                        This is an Parameter expression type
                                        Type: System.Int32, Name: product, ByRef: False
                                The Right argument is:
                                        This is an Parameter expression type
                                        Type: System.Int32, Name: factor, ByRef: False
```

## <a name="extending-the-sample-library"></a>Estendendo a biblioteca de exemplo

Os exemplos nesta seção mostram as principais técnicas para visitar e examinar nós em uma árvore de expressão. Eu encobri várias ações de que talvez você precise para nos concentrarmos nas tarefas principais de visitar e acessar nós em uma árvore de expressão.

Primeiro, os visitantes lidam somente com constantes que são números inteiros. Os valores das constantes podem ser de qualquer outro tipo numérico e a linguagem C# dá suporte a conversões e promoções entre esses tipos. Uma versão mais robusta desse código espelharia todos esses recursos.

Até o último exemplo reconhece um subconjunto dos tipos de nó possíveis.
Você ainda poderá alimentá-lo com muitas expressões que o fariam falhar.
Uma implementação completa está incluída no .NET Standard com o nome <xref:System.Linq.Expressions.ExpressionVisitor> e pode lidar com todos os tipos de nó possíveis.

Por fim, a biblioteca usada neste artigo foi desenvolvida para demonstração e aprendizado. Ela não está otimizada. Eu o escrevi para tornar as estruturas usadas claras e destacar as técnicas usadas para visitar os nós e analisar o que está lá. Uma implementação de produção dedicaria mais atenção ao desempenho do que eu dediquei.

Mesmo com essas limitações, você deve estar bem no processo de escrever algoritmos que leem e entendem árvores de expressão.

[Próximo – Compilando expressões](expression-trees-building.md)
