---
title: System.Delegate e a palavra-chave `delegate`
description: Saiba mais sobre as classes no .NET que dão suporte a delegados e como elas são mapeadas para a palavra-chave ' delegate '.
ms.date: 06/20/2016
ms.technology: csharp-fundamentals
ms.assetid: f3742fda-13c2-4283-8966-9e21c2674393
ms.openlocfilehash: 9df8ad68f6bfa62863ee047875b6419fc81ad779
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062457"
---
# <a name="systemdelegate-and-the-delegate-keyword"></a>System.Delegate e a palavra-chave `delegate`

[Anterior](delegates-overview.md)

Este artigo aborda as classes no .NET que dão suporte a delegados e como elas são mapeadas para a `delegate` palavra-chave.

## <a name="define-delegate-types"></a>Definir tipos delegados

Vamos começar com a palavra-chave ‘delegate’, pois ela é basicamente o que você usará ao trabalhar com delegados. O código que o compilador gera quando você usa a palavra-chave `delegate` será mapeado para chamadas de método que invocam membros das classes <xref:System.Delegate> e <xref:System.MulticastDelegate>.

Você define um tipo de delegado usando uma sintaxe semelhante à definição de uma assinatura de método. Basta adicionar a palavra-chave `delegate` à definição.

Vamos continuar a usar o método List.Sort() como nosso exemplo. A primeira etapa é criar um tipo para o delegado de comparação:

```csharp
// From the .NET Core library

// Define the delegate type:
public delegate int Comparison<in T>(T left, T right);
```

O compilador gera uma classe, derivada de `System.Delegate`, que corresponde à assinatura usada (nesse caso, um método que retorna um inteiro e tem dois argumentos). O tipo do delegado é `Comparison`. O tipo delegado `Comparison` é um tipo genérico. Para obter detalhes sobre os genéricos, consulte [aqui](programming-guide/generics/index.md).

Observe que a sintaxe pode aparecer como se estivesse declarando uma variável, mas na verdade está declarando um *tipo*. Você pode definir tipos de delegado dentro de classes, diretamente dentro de namespaces ou até mesmo no namespace global.

> [!NOTE]
> Declarar tipos de delegado (ou outros tipos) diretamente no namespace global não é recomendado.

O compilador também gera manipuladores de adição e remoção para esse novo tipo de forma que os clientes dessa classe podem adicionar e remover métodos de uma lista de invocação de instância. O compilador imporá que a assinatura do método que está sendo adicionado ou removido corresponda à assinatura usada ao declarar o método.

## <a name="declare-instances-of-delegates"></a>Declarar instâncias de delegados

Depois de definir o delegado, você pode criar uma instância desse tipo.
Como todas as variáveis em C#, você não pode declarar instâncias de delegado diretamente em um namespace ou no namespace global.

```csharp
// inside a class definition:

// Declare an instance of that type:
public Comparison<T> comparator;
```

O tipo da variável é `Comparison<T>`, o tipo de delegado definido anteriormente. O nome da variável é `comparator`.

 Esse snippet de código acima declarou uma variável de membro dentro de uma classe. Você também pode declarar variáveis de delegado que são variáveis locais ou argumentos para métodos.

## <a name="invoke-delegates"></a>Invocar delegados

Você invoca os métodos que estão na lista de invocação de um delegado chamando esse delegado. Dentro do método `Sort()`, o código chamará o método de comparação para determinar em qual ordem posicionar objetos:

```csharp
int result = comparator(left, right);
```

Na linha acima, o código *invoca* o método anexado ao delegado.
Você trata a variável como um nome de método e a invoca usando a sintaxe de chamada de método normal.

Essa linha de código faz uma suposição não segura: não há garantia de que um destino foi adicionado ao delegado. Se nenhum destino tiver sido anexado, a linha acima fará com que um `NullReferenceException` seja lançado. As expressões usadas para resolver esse problema são mais complicadas do que uma simples verificação de null e são abordadas posteriormente nesta [série](delegates-patterns.md).

## <a name="assign-add-and-remove-invocation-targets"></a>Atribuir, adicionar e remover destinos de invocação

Essa é a forma como o tipo do delegado é definido e como as instâncias de delegado são declaradas e invocadas.

Os desenvolvedores que desejam usar o método `List.Sort()` precisa definir um método cuja assinatura corresponde à definição de tipo de delegado e atribuí-lo ao delegado usado pelo método de classificação. Esta atribuição adiciona o método à lista de invocação do objeto de delegado.

Suponha que você queira classificar uma lista de cadeias de caracteres pelo seu comprimento. A função de comparação pode ser a seguinte:

```csharp
private static int CompareLength(string left, string right) =>
    left.Length.CompareTo(right.Length);
```

O método é declarado como um método particular. Tudo bem. Você pode não desejar que esse método seja parte da sua interface pública. Ele ainda pode ser usado como o método de comparação ao anexar a um delegado. O código de chamada terá esse método anexado à lista de destino do objeto de delegado e pode acessá-lo por meio do delegado.

Você cria essa relação passando esse método para o método `List.Sort()`:

```csharp
phrases.Sort(CompareLength);
```

Observe que o nome do método é usado, sem parênteses. Usar o método como um argumento informa ao compilador para converter a referência de método em uma referência que pode ser usada como um destino de invocação do delegado e anexar esse método como um destino de invocação.

Você também poderia ter sido explícito declarando uma variável do tipo `Comparison<string>` e fazendo uma atribuição:

```csharp
Comparison<string> comparer = CompareLength;
phrases.Sort(comparer);
```

Em utilizações em que o método que está sendo usado como um destinos de delegado é um método pequeno, é comum usar a sintaxe da [expressão lambda](language-reference/operators/lambda-expressions.md) para executar a atribuição:

```csharp
Comparison<string> comparer = (left, right) => left.Length.CompareTo(right.Length);
phrases.Sort(comparer);
```

O uso de expressões lambda para destinos de delegado será abordado em uma [seção posterior](delegates-patterns.md).

O exemplo de Sort() normalmente anexa um único método de destino ao delegado. No entanto, objetos delegados dão suporte a listas de invocação que têm vários métodos de destino anexados a um objeto de delegado.

## <a name="delegate-and-multicastdelegate-classes"></a>Classes Delegate e MulticastDelegate

O suporte de linguagem descrito acima fornece os recursos e o suporte que você normalmente precisará para trabalhar com delegados. Esses recursos são criados com base em duas classes no .NET Core Framework: <xref:System.Delegate> e <xref:System.MulticastDelegate>.

A `System.Delegate` classe e sua única subclasse direta, `System.MulticastDelegate` fornecem o suporte à estrutura para criar delegados, registrar métodos como destinos delegados e invocar todos os métodos que são registrados como um destino delegado.

Curiosamente, as classes `System.Delegate` e `System.MulticastDelegate` não são em si tipos de delegado. Elas fornecem a base para todos os tipos de delegado específicos. Esse mesmo processo de design de linguagem determinou que você não pode declarar uma classe que deriva de `Delegate` ou `MulticastDelegate`. As regras da linguagem C# proíbem isso.

Em vez disso, o compilador C# cria instâncias de uma classe derivada de `MulticastDelegate` quando você usa a palavra-chave da linguagem C# para declarar os tipos de delegado.

Esse design tem suas raízes na primeira versão do C# e do .NET. Uma meta da equipe de design era garantir que a linguagem aplicava a segurança de tipos ao usar delegados. Isso significava garantir que os delegados fossem invocados com o tipo e o número de argumentos certos. E, que algum tipo de retorno fosse indicado no tempo de compilação. Os delegados faziam parte da versão 1.0 do .NET, que era anterior aos genéricos.

A melhor maneira de reforçar essa segurança de tipos foi o compilador criar as classes de delegado concretas que representavam a assinatura do método sendo usado.

Embora não seja possível criar classes derivadas diretamente, você usará os métodos definidos nessas classes. Vamos percorrer os métodos mais comuns que você usará ao trabalhar com delegados.

O primeiro e mais importante fato a se lembrar é que todos os delegados com os quais você trabalha são derivados de `MulticastDelegate`. Um delegado multicast significa que mais de um destino de método pode ser invocado durante a invocação através de um delegado. O design original considerava fazer uma distinção entre delegados em que somente um método de destino poderia ser anexado e invocado e delegados em que vários métodos de destino poderiam ser anexados e invocados. Essa distinção provou ser menos útil na prática do que pensado originalmente. As duas classes diferentes já foram criadas e estão na estrutura desde seu lançamento público inicial.

Os métodos que você usará mais com delegados são `Invoke()` e `BeginInvoke()` / `EndInvoke()`. `Invoke()` invocará todos os métodos que foram anexados a uma instância de delegado específica. Como você viu anteriormente, normalmente invoca delegados usando a sintaxe de chamada de método na variável de delegado. Como você verá [posteriormente nesta série](delegates-patterns.md), existem padrões que trabalham diretamente com esses métodos.

Agora que você viu a sintaxe da linguagem e as classes que oferecem suporte a delegados, vamos examinar como os delegados com rigidez de tipos são usados, criados e invocados.

[Próximo](delegates-strongly-typed.md)
