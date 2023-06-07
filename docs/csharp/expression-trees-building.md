---
title: Criando árvores de expressão
description: Saiba mais sobre técnicas de criação de árvores de expressão.
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: 542754a9-7f40-4293-b299-b9f80241902c
ms.openlocfilehash: c153ca2c75738571c81057364390f489d2decb05
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656144"
---
# <a name="building-expression-trees"></a>Criando árvores de expressão

[Anterior – Interpretando expressões](expression-trees-interpreting.md)

Todas as árvores de expressão que você viu até agora foram criadas pelo compilador C#. Tudo que eu tive que fazer foi criar uma expressão lambda que foi atribuída a uma variável tipada como um `Expression<Func<T>>` ou algum tipo semelhante. Essa não é a única maneira de criar uma árvore de expressão. Para muitos cenários, você pode descobrir que precisa criar uma expressão na memória em runtime.

Criar árvores de expressão é complicado pelo fato de que essas árvores de expressão são imutáveis. Ser imutável significa que você precisa criar a árvore de folhas até a raiz. As APIs que você usará para criar as árvores de expressão refletem esse fato: os métodos que você usará para criar um nó usam todos os seus filhos como argumentos. Vejamos alguns exemplos para mostrar as técnicas a você.

## <a name="creating-nodes"></a>Criando nós

Vamos começar de forma relativamente simples mais uma vez. Vamos usar a expressão de adição com que tenho trabalhado durante essas seções:

```csharp
Expression<Func<int>> sum = () => 1 + 2;
```

Para construir essa árvore de expressão, você precisará criar os nós de folha.
Os nós de folha são constantes, de modo que você pode usar o método `Expression.Constant` para criar os nós:

```csharp
var one = Expression.Constant(1, typeof(int));
var two = Expression.Constant(2, typeof(int));
```

Em seguida, você criará a expressão de adição:

```csharp
var addition = Expression.Add(one, two);
```

Depois que tiver a expressão de adição, você pode criar a expressão lambda:

```csharp
var lambda = Expression.Lambda(addition);
```

Essa é uma expressão lambda muito simples, pois não contém argumentos.
Posteriormente nesta seção, você verá como mapear argumentos para parâmetros e criar expressões mais complicadas.

Para expressões que são simples como essa, você pode combinar todas as chamadas em uma única instrução:

```csharp
var lambda = Expression.Lambda(
    Expression.Add(
        Expression.Constant(1, typeof(int)),
        Expression.Constant(2, typeof(int))
    )
);
```

## <a name="building-a-tree"></a>Criando uma árvore

Estes são os conceitos básicos da criação de uma árvore de expressão na memória. Árvores mais complexas geralmente significam mais tipos de nó e mais nós na árvore. Vamos percorrer mais um exemplo e mostrar dois outros tipos de nó que você normalmente criará quando criar árvores de expressão: os nós de argumento e os nós de chamada de método.

Vamos criar uma árvore de expressão para criar esta expressão:

```csharp
Expression<Func<double, double, double>> distanceCalc =
    (x, y) => Math.Sqrt(x * x + y * y);
```

Você começará criando expressões de parâmetro para `x` e `y`:

```csharp
var xParameter = Expression.Parameter(typeof(double), "x");
var yParameter = Expression.Parameter(typeof(double), "y");
```

A criação de expressões de multiplicação e adição segue o padrão que você já viu:

```csharp
var xSquared = Expression.Multiply(xParameter, xParameter);
var ySquared = Expression.Multiply(yParameter, yParameter);
var sum = Expression.Add(xSquared, ySquared);
```

Em seguida, você precisa criar uma expressão de chamada de método para a chamada para `Math.Sqrt`.

```csharp
var sqrtMethod = typeof(Math).GetMethod("Sqrt", new[] { typeof(double) });
var distance = Expression.Call(sqrtMethod, sum);
```

Depois, por fim, você coloca a chamada de método em uma expressão lambda e define os argumentos para a expressão lambda:

```csharp
var distanceLambda = Expression.Lambda(
    distance,
    xParameter,
    yParameter);
```

Neste exemplo mais complicado, você verá mais algumas técnicas de que frequentemente precisará para criar árvores de expressão.

Primeiro, você precisa criar os objetos que representam parâmetros ou variáveis locais antes de usá-los. Após ter criado esses objetos, você pode usá-los em sua árvore de expressão quando for necessário.

Depois, você precisa usar um subconjunto das APIs de reflexão para criar um objeto `MethodInfo` para que possa criar uma árvore de expressão para acessar esse método. Você deve se limitar ao subconjunto das APIs de reflexão que estão disponíveis na plataforma do .NET Core. Mais uma vez, essas técnicas se estenderão a outras árvores de expressão.

## <a name="building-code-in-depth"></a>Criando código profundamente

Você não fica limitado ao que pode criar usando essas APIs. No entanto, quanto mais complicada for a árvore de expressão que você quer criar, mais difícil ser;a gerenciar e ler o código.

Vamos criar uma árvore de expressão que é o equivalente a este código:

```csharp
Func<int, int> factorialFunc = (n) =>
{
    var res = 1;
    while (n > 1)
    {
        res = res * n;
        n--;
    }
    return res;
};
```

Acima, observe que eu não criei a árvore de expressão, apenas o delegado. Usando a classe `Expression`, não é possível criar lambdas de instrução. Este é o código que é necessário para criar a mesma funcionalidade. Ele é complicado pelo fato de que não há uma API para criar um loop `while`. Em vez disso, você precisa criar um loop que contém um teste condicional e um destino para o rótulo para interromper o loop.

```csharp
var nArgument = Expression.Parameter(typeof(int), "n");
var result = Expression.Variable(typeof(int), "result");

// Creating a label that represents the return value
LabelTarget label = Expression.Label(typeof(int));

var initializeResult = Expression.Assign(result, Expression.Constant(1));

// This is the inner block that performs the multiplication,
// and decrements the value of 'n'
var block = Expression.Block(
    Expression.Assign(result,
        Expression.Multiply(result, nArgument)),
    Expression.PostDecrementAssign(nArgument)
);

// Creating a method body.
BlockExpression body = Expression.Block(
    new[] { result },
    initializeResult,
    Expression.Loop(
        Expression.IfThenElse(
            Expression.GreaterThan(nArgument, Expression.Constant(1)),
            block,
            Expression.Break(label, result)
        ),
        label
    )
);
```

O código para criar a árvore de expressão para a função fatorial é bem mais longo, mais complicado e está cheio de rótulos e instruções de interrupção, bem como outros elementos que gostamos de evitar em nossas tarefas de codificação cotidianas.

Para esta seção, também atualizei o código de visitante para visitar cada nó nessa árvore de expressão e gravar informações sobre os nós que são criados neste exemplo. Você pode [exibir ou baixar o código de exemplo](https://github.com/dotnet/samples/tree/master/csharp/expression-trees) no repositório dotnet/docs do GitHub. Experimente por conta própria criando e executando os exemplos. Para obter instruções de download, consulte [Exemplos e tutoriais](../samples-and-tutorials/index.md#view-and-download-samples).

## <a name="examining-the-apis"></a>Examinando as APIs

As APIs de árvore de expressão são algumas das mais difíceis de navegar no .NET Core, mas não tem problema. Sua finalidade é uma tarefa bastante complexa: escrever código que gera código em runtime. Eles são necessariamente complicadas para fornecer um equilíbrio entre dar suporte a todas as estruturas de controle disponíveis na linguagem C# e manter a área de superfície das APIs tão pequena quanto for razoável. Esse equilíbrio significa que muitas estruturas de controle são representadas não por seus constructos em C#, mas por constructos que representam a lógica subjacente que o compilador gera desses constructos de nível superior.

Além disso, no momento, há expressões de C# que não podem ser criadas diretamente usando os métodos de classe `Expression`. Em geral, esses serão os operadores e expressões mais novos adicionadas no C# 5 e no C# 6. (Por exemplo, expressões `async` não podem ser criadas e o novo operador `?.` não pode ser criado diretamente.)

[Próximo – Traduzindo expressões](expression-trees-translating.md)
