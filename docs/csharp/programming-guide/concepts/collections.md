---
title: Coleções (C#)
description: Saiba mais sobre coleções em C#, que são usadas para trabalhar com grupos de objetos. As coleções podem aumentar e reduzir dinamicamente conforme as necessidades da alteração do aplicativo.
ms.date: 07/20/2015
ms.assetid: 317d7dc3-8587-4873-8b3e-556f86497939
ms.openlocfilehash: 2375980e2915d4daa5a221a94eee2aea41959852
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86924923"
---
# <a name="collections-c"></a>Coleções (C#)

Para muitos aplicativos, você desejará criar e gerenciar grupos de objetos relacionados. Há duas maneiras de agrupar objetos: criando matrizes de objetos e criando coleções de objetos.

As matrizes são mais úteis para criar e trabalhar com um número fixo de objetos fortemente tipados. Para obter informações sobre matrizes, consulte [Matrizes](../arrays/index.md).

As coleções fornecem uma maneira mais flexível de trabalhar com grupos de objetos. Ao contrário das matrizes, o grupo de objetos com o qual você trabalha pode crescer e reduzir dinamicamente conforme as necessidades do aplicativo são alteradas. Para algumas coleções, você pode atribuir uma chave para qualquer objeto que coloque na coleção para que você possa recuperar rapidamente o objeto usando a chave.

Uma coleção é uma classe, portanto você deve declarar uma instância da classe antes de adicionar elementos a essa coleção.

Se a coleção contiver elementos de apenas um tipo de dados, você poderá usar uma das classes no namespace <xref:System.Collections.Generic?displayProperty=nameWithType>. Uma coleção genérica impõe segurança de tipos para que nenhum outro tipo de dados possa ser adicionado a ela. Ao recuperar um elemento de uma coleção genérica, você não precisa determinar seu tipo de dados ou convertê-lo.

> [!NOTE]
> Para os exemplos neste tópico, inclua diretivas [using](../../language-reference/keywords/using-directive.md) para os namespaces `System.Collections.Generic` e `System.Linq`.

 **Neste tópico**

- [Usando uma coleção simples](#BKMK_SimpleCollection)

- [Tipos de coleções](#BKMK_KindsOfCollections)

  - [Classes System.Collections.Generic](#BKMK_Generic)

  - [Classes System.Collections.Concurrent](#BKMK_Concurrent)

  - [Classes System.Collections](#BKMK_Collections)

- [Implementando uma coleção de pares chave-valor](#BKMK_KeyValuePairs)

- [Usando LINQ para acessar uma coleção](#BKMK_LINQ)

- [Classificando uma coleção](#BKMK_Sorting)

- [Definindo uma coleção personalizada](#BKMK_CustomCollection)

- [Iterators](#BKMK_Iterators)

<a name="BKMK_SimpleCollection"></a>

## <a name="using-a-simple-collection"></a>Usando uma coleção simples

Os exemplos nesta seção usam a classe genérica <xref:System.Collections.Generic.List%601>, que habilita você a trabalhar com uma lista de objetos fortemente tipados.

O exemplo a seguir cria uma lista de cadeias de caracteres e, em seguida, itera nas cadeias de caracteres usando uma instrução [foreach](../../language-reference/keywords/foreach-in.md).

```csharp
// Create a list of strings.
var salmons = new List<string>();
salmons.Add("chinook");
salmons.Add("coho");
salmons.Add("pink");
salmons.Add("sockeye");

// Iterate through the list.
foreach (var salmon in salmons)
{
    Console.Write(salmon + " ");
}
// Output: chinook coho pink sockeye
```

Se o conteúdo de uma coleção for conhecido com antecedência, você poderá usar um *inicializador de coleção* para inicializar a coleção. Para obter mais informações, consulte [Inicializadores de coleção e objeto](../classes-and-structs/object-and-collection-initializers.md).

O exemplo a seguir é igual ao exemplo anterior, exceto que um inicializador de coleção é usado para adicionar elementos à coleção.

```csharp
// Create a list of strings by using a
// collection initializer.
var salmons = new List<string> { "chinook", "coho", "pink", "sockeye" };

// Iterate through the list.
foreach (var salmon in salmons)
{
    Console.Write(salmon + " ");
}
// Output: chinook coho pink sockeye
```

Você pode usar uma instrução [for](../../language-reference/keywords/for.md) em vez de uma instrução `foreach` para iterar em uma coleção. Você realiza isso acessando os elementos da coleção pela posição do índice. O índice dos elementos começa em 0 e termina na contagem de elementos, menos de 1.

O exemplo a seguir itera nos elementos de uma coleção usando `for` em vez de `foreach`.

```csharp
// Create a list of strings by using a
// collection initializer.
var salmons = new List<string> { "chinook", "coho", "pink", "sockeye" };

for (var index = 0; index < salmons.Count; index++)
{
    Console.Write(salmons[index] + " ");
}
// Output: chinook coho pink sockeye
```

O exemplo a seguir remove um elemento da coleção, especificando o objeto a ser removido.

```csharp
// Create a list of strings by using a
// collection initializer.
var salmons = new List<string> { "chinook", "coho", "pink", "sockeye" };

// Remove an element from the list by specifying
// the object.
salmons.Remove("coho");

// Iterate through the list.
foreach (var salmon in salmons)
{
    Console.Write(salmon + " ");
}
// Output: chinook pink sockeye
```

O exemplo a seguir remove elementos de uma lista genérica. Em vez de uma instrução `foreach`, é usada uma instrução [for](../../language-reference/keywords/for.md) que itera em ordem decrescente. Isso é feito porque o método <xref:System.Collections.Generic.List%601.RemoveAt%2A> faz com que os elementos após um elemento removido tenham um valor de índice menor.

```csharp
var numbers = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };

// Remove odd numbers.
for (var index = numbers.Count - 1; index >= 0; index--)
{
    if (numbers[index] % 2 == 1)
    {
        // Remove the element by specifying
        // the zero-based index in the list.
        numbers.RemoveAt(index);
    }
}

// Iterate through the list.
// A lambda expression is placed in the ForEach method
// of the List(T) object.
numbers.ForEach(
    number => Console.Write(number + " "));
// Output: 0 2 4 6 8
```

Para o tipo dos elementos na <xref:System.Collections.Generic.List%601>, você também pode definir sua própria classe. No exemplo a seguir, a classe `Galaxy` que é usada pela <xref:System.Collections.Generic.List%601> é definida no código.

```csharp
private static void IterateThroughList()
{
    var theGalaxies = new List<Galaxy>
        {
            new Galaxy() { Name="Tadpole", MegaLightYears=400},
            new Galaxy() { Name="Pinwheel", MegaLightYears=25},
            new Galaxy() { Name="Milky Way", MegaLightYears=0},
            new Galaxy() { Name="Andromeda", MegaLightYears=3}
        };

    foreach (Galaxy theGalaxy in theGalaxies)
    {
        Console.WriteLine(theGalaxy.Name + "  " + theGalaxy.MegaLightYears);
    }

    // Output:
    //  Tadpole  400
    //  Pinwheel  25
    //  Milky Way  0
    //  Andromeda  3
}

public class Galaxy
{
    public string Name { get; set; }
    public int MegaLightYears { get; set; }
}
```

<a name="BKMK_KindsOfCollections"></a>

## <a name="kinds-of-collections"></a>Tipos de coleções

Muitas coleções comuns são fornecidas pelo .NET. Cada tipo de coleção é projetado para uma finalidade específica.

Algumas das classes de coleção comuns são descritas nesta seção:

- Classes <xref:System.Collections.Generic>

- Classes <xref:System.Collections.Concurrent>

- Classes <xref:System.Collections>

<a name="BKMK_Generic"></a>

### <a name="systemcollectionsgeneric-classes"></a>Classes System.Collections.Generic

Você pode criar uma coleção genérica usando uma das classes no namespace <xref:System.Collections.Generic>. Uma coleção genérica é útil quando cada item na coleção tem o mesmo tipo de dados. Uma coleção genérica impõe tipagem forte, permitindo que apenas o tipo de dados desejado seja adicionado.

A tabela a seguir lista algumas das classes frequentemente usadas do namespace <xref:System.Collections.Generic?displayProperty=nameWithType>:

|Classe|Descrição|
|---|---|
|<xref:System.Collections.Generic.Dictionary%602>|Representa uma coleção de pares chave-valor organizados com base na chave.|
|<xref:System.Collections.Generic.List%601>|Representa uma lista de objetos que podem ser acessados por índice. Fornece métodos para pesquisar, classificar e modificar listas.|
|<xref:System.Collections.Generic.Queue%601>|Representa uma coleção de objetos PEPS (primeiro a entrar, primeiro a sair).|
|<xref:System.Collections.Generic.SortedList%602>|Representa uma coleção de pares chave/valor que são classificados por chave com base na implementação de <xref:System.Collections.Generic.IComparer%601> associada.|
|<xref:System.Collections.Generic.Stack%601>|Representa uma coleção de objetos UEPS (último a entrar, primeiro a sair).|

Para obter informações adicionais, consulte [Tipos de coleção comumente usados](../../../standard/collections/commonly-used-collection-types.md), [Selecionando uma classe de coleção](../../../standard/collections/selecting-a-collection-class.md) e <xref:System.Collections.Generic>.

<a name="BKMK_Concurrent"></a>

### <a name="systemcollectionsconcurrent-classes"></a>Classes System.Collections.Concurrent

No .NET Framework 4 e versões posteriores, as coleções no <xref:System.Collections.Concurrent> namespace fornecem operações eficientes de thread-safe para acessar itens de coleção de vários threads.

As classes no namespace <xref:System.Collections.Concurrent> deverão ser usadas em vez dos tipos correspondentes nos namespaces <xref:System.Collections.Generic?displayProperty=nameWithType> e <xref:System.Collections?displayProperty=nameWithType> sempre que vários threads estiverem acessando a coleção simultaneamente. Para obter mais informações, veja [Coleções thread-safe](../../../standard/collections/thread-safe/index.md) e <xref:System.Collections.Concurrent>.

Algumas classes incluídas no namespace <xref:System.Collections.Concurrent> são <xref:System.Collections.Concurrent.BlockingCollection%601>, <xref:System.Collections.Concurrent.ConcurrentDictionary%602>, <xref:System.Collections.Concurrent.ConcurrentQueue%601> e <xref:System.Collections.Concurrent.ConcurrentStack%601>.

<a name="BKMK_Collections"></a>

### <a name="systemcollections-classes"></a>Classes System.Collections

As classes no namespace <xref:System.Collections?displayProperty=nameWithType> não armazenam elementos como objetos especificamente tipados, mas como objetos do tipo `Object`.

Sempre que possível, você deve usar as coleções genéricas no namespace <xref:System.Collections.Generic?displayProperty=nameWithType> ou no <xref:System.Collections.Concurrent> em vez dos tipos herdados no namespace `System.Collections`.

A tabela a seguir lista algumas das classes frequentemente usadas no namespace `System.Collections`:

|Classe|Descrição|
|---|---|
|<xref:System.Collections.ArrayList>|Representa uma matriz de objetos cujo tamanho é aumentado dinamicamente conforme necessário.|
|<xref:System.Collections.Hashtable>|Representa uma coleção de pares chave-valor organizados com base no código hash da chave.|
|<xref:System.Collections.Queue>|Representa uma coleção de objetos PEPS (primeiro a entrar, primeiro a sair).|
|<xref:System.Collections.Stack>|Representa uma coleção de objetos UEPS (último a entrar, primeiro a sair).|

O namespace <xref:System.Collections.Specialized> fornece classes de coleções especializadas e fortemente tipadas, como coleções somente de cadeias de caracteres, bem como de dicionários híbridos e de listas vinculadas.

<a name="BKMK_KeyValuePairs"></a>

## <a name="implementing-a-collection-of-keyvalue-pairs"></a>Implementando uma coleção de pares chave-valor

A coleção genérica <xref:System.Collections.Generic.Dictionary%602> permite que você acesse elementos em uma coleção usando a chave de cada elemento. Cada adição ao dicionário consiste em um valor e a respectiva chave associada. A recuperação de um valor usando sua chave é rápida, porque a classe `Dictionary` é implementada como uma tabela de hash.

O exemplo a seguir cria uma coleção `Dictionary` e itera no dicionário usando uma instrução `foreach`.

```csharp
private static void IterateThruDictionary()
{
    Dictionary<string, Element> elements = BuildDictionary();

    foreach (KeyValuePair<string, Element> kvp in elements)
    {
        Element theElement = kvp.Value;

        Console.WriteLine("key: " + kvp.Key);
        Console.WriteLine("values: " + theElement.Symbol + " " +
            theElement.Name + " " + theElement.AtomicNumber);
    }
}

private static Dictionary<string, Element> BuildDictionary()
{
    var elements = new Dictionary<string, Element>();

    AddToDictionary(elements, "K", "Potassium", 19);
    AddToDictionary(elements, "Ca", "Calcium", 20);
    AddToDictionary(elements, "Sc", "Scandium", 21);
    AddToDictionary(elements, "Ti", "Titanium", 22);

    return elements;
}

private static void AddToDictionary(Dictionary<string, Element> elements,
    string symbol, string name, int atomicNumber)
{
    Element theElement = new Element();

    theElement.Symbol = symbol;
    theElement.Name = name;
    theElement.AtomicNumber = atomicNumber;

    elements.Add(key: theElement.Symbol, value: theElement);
}

public class Element
{
    public string Symbol { get; set; }
    public string Name { get; set; }
    public int AtomicNumber { get; set; }
}
```

Para, em vez disso, usar um inicializador de coleção para criar a coleção `Dictionary`, você pode substituir os métodos `BuildDictionary` e `AddToDictionary` pelo seguinte método.

```csharp
private static Dictionary<string, Element> BuildDictionary2()
{
    return new Dictionary<string, Element>
    {
        {"K",
            new Element() { Symbol="K", Name="Potassium", AtomicNumber=19}},
        {"Ca",
            new Element() { Symbol="Ca", Name="Calcium", AtomicNumber=20}},
        {"Sc",
            new Element() { Symbol="Sc", Name="Scandium", AtomicNumber=21}},
        {"Ti",
            new Element() { Symbol="Ti", Name="Titanium", AtomicNumber=22}}
    };
}
```

O exemplo a seguir usa o método <xref:System.Collections.Generic.Dictionary%602.ContainsKey%2A> e a propriedade <xref:System.Collections.Generic.Dictionary%602.Item%2A> de `Dictionary` para localizar rapidamente um item por chave. A propriedade `Item` permite que você acesse um item na coleção `elements` usando o `elements[symbol]` no C#.

```csharp
private static void FindInDictionary(string symbol)
{
    Dictionary<string, Element> elements = BuildDictionary();

    if (elements.ContainsKey(symbol) == false)
    {
        Console.WriteLine(symbol + " not found");
    }
    else
    {
        Element theElement = elements[symbol];
        Console.WriteLine("found: " + theElement.Name);
    }
}
```

O exemplo a seguir usa o método <xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> para localizar rapidamente um item por chave.

```csharp
private static void FindInDictionary2(string symbol)
{
    Dictionary<string, Element> elements = BuildDictionary();

    Element theElement = null;
    if (elements.TryGetValue(symbol, out theElement) == false)
        Console.WriteLine(symbol + " not found");
    else
        Console.WriteLine("found: " + theElement.Name);
}
```

<a name="BKMK_LINQ"></a>

## <a name="using-linq-to-access-a-collection"></a>Usando LINQ para acessar uma coleção

A LINQ (consulta integrada à linguagem) pode ser usada para acessar coleções. As consultas LINQ fornecem recursos de filtragem, classificação e agrupamento. Para obter mais informações, consulte [introdução com LINQ em C#](linq/index.md).

O exemplo a seguir executa uma consulta LINQ em uma `List` genérica. A consulta LINQ retorna uma coleção diferente que contém os resultados.

```csharp
private static void ShowLINQ()
{
    List<Element> elements = BuildList();

    // LINQ Query.
    var subset = from theElement in elements
                 where theElement.AtomicNumber < 22
                 orderby theElement.Name
                 select theElement;

    foreach (Element theElement in subset)
    {
        Console.WriteLine(theElement.Name + " " + theElement.AtomicNumber);
    }

    // Output:
    //  Calcium 20
    //  Potassium 19
    //  Scandium 21
}

private static List<Element> BuildList()
{
    return new List<Element>
    {
        { new Element() { Symbol="K", Name="Potassium", AtomicNumber=19}},
        { new Element() { Symbol="Ca", Name="Calcium", AtomicNumber=20}},
        { new Element() { Symbol="Sc", Name="Scandium", AtomicNumber=21}},
        { new Element() { Symbol="Ti", Name="Titanium", AtomicNumber=22}}
    };
}

public class Element
{
    public string Symbol { get; set; }
    public string Name { get; set; }
    public int AtomicNumber { get; set; }
}
```

<a name="BKMK_Sorting"></a>

## <a name="sorting-a-collection"></a>Classificando uma coleção

O exemplo a seguir ilustra um procedimento para a classificação de uma coleção. O exemplo classifica instâncias da classe `Car` que estão armazenados em uma <xref:System.Collections.Generic.List%601>. A classe `Car` implementa a interface <xref:System.IComparable%601>, que requer que o método <xref:System.IComparable%601.CompareTo%2A> seja implementado.

Cada chamada ao método <xref:System.IComparable%601.CompareTo%2A> faz uma comparação única que é usada para classificação. Os códigos escritos pelo usuário no método `CompareTo` retornam um valor para cada comparação do objeto atual com outro objeto. O valor retornado será menor que zero se o objeto atual for menor que o outro objeto, maior que zero se o objeto atual for maior que o outro objeto e zero, se eles forem iguais. Isso permite que você defina no código os critérios para maior que, menor que e igual.

No método `ListCars`, a instrução `cars.Sort()` classifica a lista. Essa chamada para o método <xref:System.Collections.Generic.List%601.Sort%2A> da <xref:System.Collections.Generic.List%601> faz com que o método `CompareTo` seja chamado automaticamente para os objetos `Car` na `List`.

```csharp
private static void ListCars()
{
    var cars = new List<Car>
    {
        { new Car() { Name = "car1", Color = "blue", Speed = 20}},
        { new Car() { Name = "car2", Color = "red", Speed = 50}},
        { new Car() { Name = "car3", Color = "green", Speed = 10}},
        { new Car() { Name = "car4", Color = "blue", Speed = 50}},
        { new Car() { Name = "car5", Color = "blue", Speed = 30}},
        { new Car() { Name = "car6", Color = "red", Speed = 60}},
        { new Car() { Name = "car7", Color = "green", Speed = 50}}
    };

    // Sort the cars by color alphabetically, and then by speed
    // in descending order.
    cars.Sort();

    // View all of the cars.
    foreach (Car thisCar in cars)
    {
        Console.Write(thisCar.Color.PadRight(5) + " ");
        Console.Write(thisCar.Speed.ToString() + " ");
        Console.Write(thisCar.Name);
        Console.WriteLine();
    }

    // Output:
    //  blue  50 car4
    //  blue  30 car5
    //  blue  20 car1
    //  green 50 car7
    //  green 10 car3
    //  red   60 car6
    //  red   50 car2
}

public class Car : IComparable<Car>
{
    public string Name { get; set; }
    public int Speed { get; set; }
    public string Color { get; set; }

    public int CompareTo(Car other)
    {
        // A call to this method makes a single comparison that is
        // used for sorting.

        // Determine the relative order of the objects being compared.
        // Sort by color alphabetically, and then by speed in
        // descending order.

        // Compare the colors.
        int compare;
        compare = String.Compare(this.Color, other.Color, true);

        // If the colors are the same, compare the speeds.
        if (compare == 0)
        {
            compare = this.Speed.CompareTo(other.Speed);

            // Use descending order for speed.
            compare = -compare;
        }

        return compare;
    }
}
```

<a name="BKMK_CustomCollection"></a>

## <a name="defining-a-custom-collection"></a>Definindo uma coleção personalizada

Você pode definir uma coleção implementando a interface <xref:System.Collections.Generic.IEnumerable%601> ou <xref:System.Collections.IEnumerable>.

Embora você possa definir uma coleção personalizada, geralmente é melhor usar as coleções que estão incluídas no .NET, que são descritas em [tipos de coleções](#BKMK_KindsOfCollections) anteriormente neste artigo.

O exemplo a seguir define uma classe de coleção personalizada chamada `AllColors`. Essa classe implementa a interface <xref:System.Collections.IEnumerable>, que requer que o método <xref:System.Collections.IEnumerable.GetEnumerator%2A> seja implementado.

O método `GetEnumerator` retorna uma instância da classe `ColorEnumerator`. `ColorEnumerator` implementa a interface <xref:System.Collections.IEnumerator>, que requer que a propriedade <xref:System.Collections.IEnumerator.Current%2A>, o método <xref:System.Collections.IEnumerator.MoveNext%2A> e o método <xref:System.Collections.IEnumerator.Reset%2A> sejam implementados.

```csharp
private static void ListColors()
{
    var colors = new AllColors();

    foreach (Color theColor in colors)
    {
        Console.Write(theColor.Name + " ");
    }
    Console.WriteLine();
    // Output: red blue green
}

// Collection class.
public class AllColors : System.Collections.IEnumerable
{
    Color[] _colors =
    {
        new Color() { Name = "red" },
        new Color() { Name = "blue" },
        new Color() { Name = "green" }
    };

    public System.Collections.IEnumerator GetEnumerator()
    {
        return new ColorEnumerator(_colors);

        // Instead of creating a custom enumerator, you could
        // use the GetEnumerator of the array.
        //return _colors.GetEnumerator();
    }

    // Custom enumerator.
    private class ColorEnumerator : System.Collections.IEnumerator
    {
        private Color[] _colors;
        private int _position = -1;

        public ColorEnumerator(Color[] colors)
        {
            _colors = colors;
        }

        object System.Collections.IEnumerator.Current
        {
            get
            {
                return _colors[_position];
            }
        }

        bool System.Collections.IEnumerator.MoveNext()
        {
            _position++;
            return (_position < _colors.Length);
        }

        void System.Collections.IEnumerator.Reset()
        {
            _position = -1;
        }
    }
}

// Element class.
public class Color
{
    public string Name { get; set; }
}
```

<a name="BKMK_Iterators"></a>

## <a name="iterators"></a>Iterators

Um *iterador* é usado para realizar uma iteração personalizada em uma coleção. Um iterador pode ser um método ou um acessador `get`. Um iterador usa uma instrução [yield return](../../language-reference/keywords/yield.md) para retornar um elemento da coleção por vez.

Você chama um iterador usando uma instrução [foreach](../../language-reference/keywords/foreach-in.md). Cada iteração do loop `foreach` chama o iterador. Quando uma instrução `yield return` é alcançada no iterador, uma expressão é retornada e o local atual no código é retido. A execução será reiniciada desse local na próxima vez que o iterador for chamado.

Para obter mais informações, consulte [Iteradores (C#)](./iterators.md).

O exemplo a seguir usa um método iterador. O método iterador tem uma instrução `yield return` que está dentro de um loop [for](../../language-reference/keywords/for.md). No método `ListEvenNumbers`, cada iteração do corpo da instrução `foreach` cria uma chamada ao método iterador, que avança para a próxima instrução `yield return`.

```csharp
private static void ListEvenNumbers()
{
    foreach (int number in EvenSequence(5, 18))
    {
        Console.Write(number.ToString() + " ");
    }
    Console.WriteLine();
    // Output: 6 8 10 12 14 16 18
}

private static IEnumerable<int> EvenSequence(
    int firstNumber, int lastNumber)
{
    // Yield even numbers in the range.
    for (var number = firstNumber; number <= lastNumber; number++)
    {
        if (number % 2 == 0)
        {
            yield return number;
        }
    }
}
```

## <a name="see-also"></a>Confira também

- [Inicializadores de objeto e coleção](../classes-and-structs/object-and-collection-initializers.md)
- [Conceitos de programação (C#)](./index.md)
- [Instrução Option Strict](../../../visual-basic/language-reference/statements/option-strict-statement.md)
- [LINQ to Objects (C#)](./linq/linq-to-objects.md)
- [LINQ paralelo (PLINQ)](../../../standard/parallel-programming/introduction-to-plinq.md)
- [Coleções e estruturas de dados](../../../standard/collections/index.md)
- [Selecionando uma classe de coleção](../../../standard/collections/selecting-a-collection-class.md)
- [Comparações e classificações dentro de coleções](../../../standard/collections/comparisons-and-sorts-within-collections.md)
- [Quando usar coleções genéricas](../../../standard/collections/when-to-use-generic-collections.md)
