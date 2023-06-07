---
title: Iterators
description: Saiba como usar iteradores C# internos e como criar seus próprios métodos iteradores personalizados.
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: 5cf36f45-f91a-4fca-a0b7-87f233e108e9
ms.openlocfilehash: ee72331cb85ba1a03d48e2f58526ad432c7fe6d4
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656092"
---
# <a name="iterators"></a>Iterators

Quase todos os programas que você escrever terão alguma necessidade de iterar em uma coleção. Você escreverá um código que examina cada item em uma coleção.

Você também criará métodos iteradores que são métodos que produzem um iterador (que é um objeto que atravessa um contêiner, especialmente listas) para os elementos dessa classe. Eles podem ser usados para:

+ Executar uma ação em cada item em uma coleção.
+ Enumerar uma coleção personalizada.
+ Estender [LINQ](linq/index.md) ou outras bibliotecas.
+ Criar um pipeline de dados em que os dados fluem com eficiência pelos métodos de iterador.

A linguagem C# fornece recursos para esses dois cenários. Este artigo fornece uma visão geral desses recursos.

Este tutorial tem várias etapas. Após cada etapa, você poderá executar o aplicativo e ver o progresso. Você também pode [exibir ou baixar o exemplo concluído](https://github.com/dotnet/samples/blob/master/csharp/iterators) deste tópico. Para obter instruções de download, consulte [Exemplos e tutoriais](../samples-and-tutorials/index.md#view-and-download-samples).

## <a name="iterating-with-foreach"></a>iterando com foreach

Enumerar uma coleção é simples: a palavra-chave `foreach` enumera uma coleção, executando a instrução inserida uma vez para cada elemento na coleção:

```csharp
foreach (var item in collection)
{
   Console.WriteLine(item.ToString());
}
```

Isso é tudo para ela. Para iterar em todo o conteúdo de uma coleção, a instrução `foreach` é tudo o que você precisa. No entanto, a instrução `foreach` não é mágica. Ele se baseia em duas interfaces genéricas definidas na biblioteca do .NET Core para gerar o código necessário para iterar uma coleção: `IEnumerable<T>` e `IEnumerator<T>`. Esse mecanismo é explicado mais detalhadamente abaixo.

Essas duas interfaces também têm contrapartes não genéricas: `IEnumerable` e `IEnumerator`. As versões [genéricas](programming-guide/generics/index.md) são preferenciais para o código moderno.

## <a name="enumeration-sources-with-iterator-methods"></a>Fontes de enumeração com métodos de iterador

Outro ótimo recurso da linguagem C# permite que você crie métodos que criam uma fonte para uma enumeração. Eles são chamados de *métodos de iterador*. Um método de iterador define como gerar os objetos em uma sequência quando solicitado. Você usa as palavras-chave contextuais `yield return` para definir um método iterador.

Você poderia escrever esse método para produzir a sequência de inteiros de 0 a 9:

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    yield return 0;
    yield return 1;
    yield return 2;
    yield return 3;
    yield return 4;
    yield return 5;
    yield return 6;
    yield return 7;
    yield return 8;
    yield return 9;
}
```

O código acima mostra instruções `yield return` distintas para destacar o fato de que você pode usar várias instruções `yield return` discretas em um método iterador.
Você pode (e frequentemente o faz) usar outros constructos de linguagem para simplificar o código de um método iterador. A definição do método abaixo produz a mesma sequência exata de números:

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;
}
```

Você não precisa determinar uma ou a outra. Você pode ter quantas instruções `yield return` forem necessárias para atender as necessidades do seu método:

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;

    yield return 50;

    index = 100;
    while (index < 110)
        yield return index++;
}
```

Essa é a sintaxe básica. Vamos considerar um exemplo do mundo real em que você escreveria um método iterador. Imagine que você está em um projeto de IoT e os sensores de dispositivo geram um enorme fluxo de dados. Para ter uma noção dos dados, você pode escrever um método realiza a amostragem a cada N elementos de dados. Esse pequeno método iterador resolve:

```csharp
public static IEnumerable<T> Sample(this IEnumerable<T> sourceSequence, int interval)
{
    int index = 0;
    foreach (T item in sourceSequence)
    {
        if (index++ % interval == 0)
            yield return item;
    }
}
```

Há uma restrição importante em métodos de iterador: você não pode ter uma instrução `return` e uma instrução `yield return` no mesmo método. O seguinte não compilará:

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;

    yield return 50;

    // generates a compile time error:
    var items = new int[] {100, 101, 102, 103, 104, 105, 106, 107, 108, 109 };
    return items;
}
```

Essa restrição normalmente não é um problema. Você tem a opção de usar `yield return` em todo o método ou separar o método original em vários métodos, alguns usando `return` e alguns usando `yield return`.

Você pode modificar um pouco o último método para usar `yield return` em todos os lugares:

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;

    yield return 50;

    var items = new int[] {100, 101, 102, 103, 104, 105, 106, 107, 108, 109 };
    foreach (var item in items)
        yield return item;
}
```

Às vezes, a resposta certa é dividir um método iterador em dois métodos diferentes. Um que usa `return` e outro que usa `yield return`. Considere uma situação em que você talvez queira retornar uma coleção vazia ou os primeiros cinco números ímpares, com base em um argumento booliano. Você poderia escrever isso como esses dois métodos:

```csharp
public IEnumerable<int> GetSingleDigitOddNumbers(bool getCollection)
{
    if (getCollection == false)
        return new int[0];
    else
        return IteratorMethod();
}

private IEnumerable<int> IteratorMethod()
{
    int index = 0;
    while (index < 10)
    {
        if (index % 2 == 1)
            yield return index;
        index++;
    }
}
```

Observe os métodos acima. O primeiro usa a instrução `return` padrão para retornar uma coleção vazia ou o iterador criado pelo segundo método. O segundo método usa a instrução `yield return` para criar a sequência solicitada.

## <a name="deeper-dive-into-foreach"></a>Aprofundamento em `foreach`

A instrução `foreach` se expande em uma expressão padrão que usa as interfaces `IEnumerable<T>` e `IEnumerator<T>` para iterar em todos os elementos de uma coleção. Ela também minimiza os erros cometidos pelos desenvolvedores por não gerenciarem os recursos adequadamente.

O compilador converte o loop `foreach` mostrado no primeiro exemplo em algo semelhante a esse constructo:

```csharp
IEnumerator<int> enumerator = collection.GetEnumerator();
while (enumerator.MoveNext())
{
    var item = enumerator.Current;
    Console.WriteLine(item.ToString());
}
```

O constructo acima representa o código gerado pelo compilador C# da versão 5 e posterior. Antes da versão 5, a variável `item` tinha um escopo diferente:

```csharp
// C# versions 1 through 4:
IEnumerator<int> enumerator = collection.GetEnumerator();
int item = default(int);
while (enumerator.MoveNext())
{
    item = enumerator.Current;
    Console.WriteLine(item.ToString());
}
```

Isso foi alterado porque o comportamento anterior poderia levar a bugs sutis e difíceis de diagnosticar envolvendo expressões lambda. Para saber mais sobre expressões lambda, confira o artigo sobre [expressões lambda](language-reference/operators/lambda-expressions.md).

O código exato gerado pelo compilador é um pouco mais complicada e lida com situações em que o objeto retornado por `GetEnumerator()` implementa a interface `IDisposable`. A expansão completa gera um código mais semelhante a esse:

```csharp
{
    var enumerator = collection.GetEnumerator();
    try
    {
        while (enumerator.MoveNext())
        {
            var item = enumerator.Current;
            Console.WriteLine(item.ToString());
        }
    } finally
    {
        // dispose of enumerator.
    }
}
```

A maneira na qual o enumerador é descartado depende das características do tipo de `enumerator`. Em geral, a cláusula `finally` expande para:

```csharp
finally
{
   (enumerator as IDisposable)?.Dispose();
}
```

No entanto, se o tipo de `enumerator` é um tipo lacrado e não há nenhuma conversão implícita do tipo de `enumerator` para `IDisposable`, a cláusula `finally` se expande para um bloco vazio:

```csharp
finally
{
}
```

Se houver uma conversão implícita do tipo de `enumerator` para `IDisposable` e `enumerator` for um tipo de valor que não aceita valores nulos, a cláusula `finally` se expandirá para:

```csharp
finally
{
   ((IDisposable)enumerator).Dispose();
}
```

Felizmente, você não precisa se lembrar de todos esses detalhes. A instrução `foreach` trata todas essas nuances para você. O compilador gerará o código correto para qualquer um desses constructos.
