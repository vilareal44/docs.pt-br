---
title: Coleções
ms.date: 07/20/2015
ms.assetid: 5f7749f3-aaf2-4319-b63c-bfa72e1e2b7a
ms.openlocfilehash: f264a0f9ee15707daf4bece5651b9f5f07ebbc39
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400649"
---
# <a name="collections-visual-basic"></a>Coleções (Visual Basic)

Para muitos aplicativos, você desejará criar e gerenciar grupos de objetos relacionados. Há duas maneiras de agrupar objetos: criando matrizes de objetos e criando coleções de objetos.

As matrizes são mais úteis para criar e trabalhar com um número fixo de objetos fortemente tipados. Para obter informações sobre matrizes, consulte [Matrizes](../language-features/arrays/index.md).

As coleções fornecem uma maneira mais flexível de trabalhar com grupos de objetos. Ao contrário das matrizes, o grupo de objetos com o qual você trabalha pode crescer e reduzir dinamicamente conforme as necessidades do aplicativo são alteradas. Para algumas coleções, você pode atribuir uma chave para qualquer objeto que coloque na coleção para que você possa recuperar rapidamente o objeto usando a chave.

Uma coleção é uma classe, portanto você deve declarar uma instância da classe antes de adicionar elementos a essa coleção.

Se a coleção contiver elementos de apenas um tipo de dados, você poderá usar uma das classes no namespace <xref:System.Collections.Generic?displayProperty=nameWithType>. Uma coleção genérica impõe segurança de tipos para que nenhum outro tipo de dados possa ser adicionado a ela. Ao recuperar um elemento de uma coleção genérica, você não precisa determinar seu tipo de dados ou convertê-lo.

> [!NOTE]
> Para os exemplos neste tópico, inclua as instruções [Imports](../../language-reference/statements/imports-statement-net-namespace-and-type.md) para os `System.Collections.Generic` `System.Linq` namespaces e.

<a name="BKMK_SimpleCollection"></a>

## <a name="using-a-simple-collection"></a>Usando uma coleção simples

Os exemplos nesta seção usam a classe genérica <xref:System.Collections.Generic.List%601>, que habilita você a trabalhar com uma lista de objetos fortemente tipados.

O exemplo a seguir cria uma lista de cadeias de caracteres e, em seguida, itera através das cadeias de caracteres usando um [para cada... Próxima](../../language-reference/statements/for-each-next-statement.md) instrução.

```vb
' Create a list of strings.
Dim salmons As New List(Of String)
salmons.Add("chinook")
salmons.Add("coho")
salmons.Add("pink")
salmons.Add("sockeye")

' Iterate through the list.
For Each salmon As String In salmons
    Console.Write(salmon & " ")
Next
'Output: chinook coho pink sockeye
```

Se o conteúdo de uma coleção for conhecido com antecedência, você poderá usar um *inicializador de coleção* para inicializar a coleção. Para obter mais informações, consulte [Inicializadores de coleção](../language-features/collection-initializers/index.md).

O exemplo a seguir é igual ao exemplo anterior, exceto que um inicializador de coleção é usado para adicionar elementos à coleção.

```vb
' Create a list of strings by using a
' collection initializer.
Dim salmons As New List(Of String) From
    {"chinook", "coho", "pink", "sockeye"}

For Each salmon As String In salmons
    Console.Write(salmon & " ")
Next
'Output: chinook coho pink sockeye
```

Você pode usar um [para... Próxima](../../language-reference/statements/for-next-statement.md) instrução em vez de uma `For Each` instrução para iterar por meio de uma coleção. Você realiza isso acessando os elementos da coleção pela posição do índice. O índice dos elementos começa em 0 e termina na contagem de elementos, menos de 1.

O exemplo a seguir itera nos elementos de uma coleção usando `For…Next` em vez de `For Each`.

```vb
Dim salmons As New List(Of String) From
    {"chinook", "coho", "pink", "sockeye"}

For index = 0 To salmons.Count - 1
    Console.Write(salmons(index) & " ")
Next
'Output: chinook coho pink sockeye
```

O exemplo a seguir remove um elemento da coleção, especificando o objeto a ser removido.

```vb
' Create a list of strings by using a
' collection initializer.
Dim salmons As New List(Of String) From
    {"chinook", "coho", "pink", "sockeye"}

' Remove an element in the list by specifying
' the object.
salmons.Remove("coho")

For Each salmon As String In salmons
    Console.Write(salmon & " ")
Next
'Output: chinook pink sockeye
```

O exemplo a seguir remove elementos de uma lista genérica. Em vez de uma `For Each` instrução, um [para... A próxima](../../language-reference/statements/for-next-statement.md) instrução que itera em ordem decrescente é usada. Isso é feito porque o método <xref:System.Collections.Generic.List%601.RemoveAt%2A> faz com que os elementos após um elemento removido tenham um valor de índice menor.

```vb
Dim numbers As New List(Of Integer) From
    {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}

' Remove odd numbers.
For index As Integer = numbers.Count - 1 To 0 Step -1
    If numbers(index) Mod 2 = 1 Then
        ' Remove the element by specifying
        ' the zero-based index in the list.
        numbers.RemoveAt(index)
    End If
Next

' Iterate through the list.
' A lambda expression is placed in the ForEach method
' of the List(T) object.
numbers.ForEach(
    Sub(number) Console.Write(number & " "))
' Output: 0 2 4 6 8
```

Para o tipo dos elementos na <xref:System.Collections.Generic.List%601>, você também pode definir sua própria classe. No exemplo a seguir, a classe `Galaxy` que é usada pela <xref:System.Collections.Generic.List%601> é definida no código.

```vb
Private Sub IterateThroughList()
    Dim theGalaxies As New List(Of Galaxy) From
        {
            New Galaxy With {.Name = "Tadpole", .MegaLightYears = 400},
            New Galaxy With {.Name = "Pinwheel", .MegaLightYears = 25},
            New Galaxy With {.Name = "Milky Way", .MegaLightYears = 0},
            New Galaxy With {.Name = "Andromeda", .MegaLightYears = 3}
        }

    For Each theGalaxy In theGalaxies
        With theGalaxy
            Console.WriteLine(.Name & "  " & .MegaLightYears)
        End With
    Next

    ' Output:
    '  Tadpole  400
    '  Pinwheel  25
    '  Milky Way  0
    '  Andromeda  3
End Sub

Public Class Galaxy
    Public Property Name As String
    Public Property MegaLightYears As Integer
End Class
```

<a name="BKMK_KindsOfCollections"></a>

## <a name="kinds-of-collections"></a>Tipos de coleções

Várias coleções comuns são fornecidas pelo .NET Framework. Cada tipo de coleção é projetado para uma finalidade específica.

Algumas das classes de coleção comuns são descritas nesta seção:

- Classes <xref:System.Collections.Generic>

- Classes <xref:System.Collections.Concurrent>

- Classes <xref:System.Collections>

- `Collection`Classe Visual Basic

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

Para obter informações adicionais, consulte [Tipos de coleção comumente usados](../../../standard/collections/commonly-used-collection-types.md), [Selecionando uma classe de coleção](../../../standard/collections/selecting-a-collection-class.md) e <xref:System.Collections.Generic?displayProperty=nameWithType>.

<a name="BKMK_Concurrent"></a>

### <a name="systemcollectionsconcurrent-classes"></a>Classes System.Collections.Concurrent

No .NET Framework 4 ou mais recente, as coleções no namespace <xref:System.Collections.Concurrent> fornecem operações thread-safe eficientes para acessar itens da coleção de vários threads.

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

<a name="BKMK_VisualBasic"></a>

### <a name="visual-basic-collection-class"></a>Visual Basic classe de coleção

Você pode usar a <xref:Microsoft.VisualBasic.Collection> classe Visual Basic para acessar um item de coleção usando um índice numérico ou uma `String` chave. Você pode adicionar itens a um objeto de coleção com ou sem especificar uma chave. Se você adicionar um item sem uma chave, deverá usar seu índice numérico para acessá-lo.

A `Collection` classe Visual Basic armazena todos os seus elementos como tipo `Object` , de modo que você pode adicionar um item de qualquer tipo de dados. Não há nenhuma proteção contra tipos de dados inadequados sendo adicionados.

Quando você usa a `Collection` classe Visual Basic, o primeiro item em uma coleção tem um índice de 1. Isso difere das classes de coleção .NET Framework, para as quais o índice inicial é 0.

Sempre que possível, você deve usar as coleções genéricas no <xref:System.Collections.Generic?displayProperty=nameWithType> namespace ou no <xref:System.Collections.Concurrent> namespace em vez da `Collection` classe Visual Basic.

Para obter mais informações, consulte <xref:Microsoft.VisualBasic.Collection>.

<a name="BKMK_KeyValuePairs"></a>

## <a name="implementing-a-collection-of-keyvalue-pairs"></a>Implementando uma coleção de pares chave-valor

A coleção genérica <xref:System.Collections.Generic.Dictionary%602> permite que você acesse elementos em uma coleção usando a chave de cada elemento. Cada adição ao dicionário consiste em um valor e a respectiva chave associada. A recuperação de um valor usando sua chave é rápida, porque a classe `Dictionary` é implementada como uma tabela de hash.

O exemplo a seguir cria uma coleção `Dictionary` e itera no dicionário usando uma instrução `For Each`.

```vb
Private Sub IterateThroughDictionary()
    Dim elements As Dictionary(Of String, Element) = BuildDictionary()

    For Each kvp As KeyValuePair(Of String, Element) In elements
        Dim theElement As Element = kvp.Value

        Console.WriteLine("key: " & kvp.Key)
        With theElement
            Console.WriteLine("values: " & .Symbol & " " &
                .Name & " " & .AtomicNumber)
        End With
    Next
End Sub

Private Function BuildDictionary() As Dictionary(Of String, Element)
    Dim elements As New Dictionary(Of String, Element)

    AddToDictionary(elements, "K", "Potassium", 19)
    AddToDictionary(elements, "Ca", "Calcium", 20)
    AddToDictionary(elements, "Sc", "Scandium", 21)
    AddToDictionary(elements, "Ti", "Titanium", 22)

    Return elements
End Function

Private Sub AddToDictionary(ByVal elements As Dictionary(Of String, Element),
ByVal symbol As String, ByVal name As String, ByVal atomicNumber As Integer)
    Dim theElement As New Element

    theElement.Symbol = symbol
    theElement.Name = name
    theElement.AtomicNumber = atomicNumber

    elements.Add(Key:=theElement.Symbol, value:=theElement)
End Sub

Public Class Element
    Public Property Symbol As String
    Public Property Name As String
    Public Property AtomicNumber As Integer
End Class
```

Para, em vez disso, usar um inicializador de coleção para criar a coleção `Dictionary`, você pode substituir os métodos `BuildDictionary` e `AddToDictionary` pelo seguinte método.

```vb
Private Function BuildDictionary2() As Dictionary(Of String, Element)
    Return New Dictionary(Of String, Element) From
        {
            {"K", New Element With
                {.Symbol = "K", .Name = "Potassium", .AtomicNumber = 19}},
            {"Ca", New Element With
                {.Symbol = "Ca", .Name = "Calcium", .AtomicNumber = 20}},
            {"Sc", New Element With
                {.Symbol = "Sc", .Name = "Scandium", .AtomicNumber = 21}},
            {"Ti", New Element With
                {.Symbol = "Ti", .Name = "Titanium", .AtomicNumber = 22}}
        }
End Function
```

O exemplo a seguir usa o método <xref:System.Collections.Generic.Dictionary%602.ContainsKey%2A> e a propriedade <xref:System.Collections.Generic.Dictionary%602.Item%2A> de `Dictionary` para localizar rapidamente um item por chave. A `Item` propriedade permite que você acesse um item na `elements` coleção usando o `elements(symbol)` código em Visual Basic.

```vb
Private Sub FindInDictionary(ByVal symbol As String)
    Dim elements As Dictionary(Of String, Element) = BuildDictionary()

    If elements.ContainsKey(symbol) = False Then
        Console.WriteLine(symbol & " not found")
    Else
        Dim theElement = elements(symbol)
        Console.WriteLine("found: " & theElement.Name)
    End If
End Sub
```

O exemplo a seguir usa o método <xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> para localizar rapidamente um item por chave.

```vb
Private Sub FindInDictionary2(ByVal symbol As String)
    Dim elements As Dictionary(Of String, Element) = BuildDictionary()

    Dim theElement As Element = Nothing
    If elements.TryGetValue(symbol, theElement) = False Then
        Console.WriteLine(symbol & " not found")
    Else
        Console.WriteLine("found: " & theElement.Name)
    End If
End Sub
```

<a name="BKMK_LINQ"></a>

## <a name="using-linq-to-access-a-collection"></a>Usando LINQ para acessar uma coleção

A LINQ (consulta integrada à linguagem) pode ser usada para acessar coleções. As consultas LINQ fornecem recursos de filtragem, classificação e agrupamento. Para obter mais informações, consulte [introdução com LINQ no Visual Basic](linq/getting-started-with-linq.md).

O exemplo a seguir executa uma consulta LINQ em uma `List` genérica. A consulta LINQ retorna uma coleção diferente que contém os resultados.

```vb
Private Sub ShowLINQ()
    Dim elements As List(Of Element) = BuildList()

    ' LINQ Query.
    Dim subset = From theElement In elements
                  Where theElement.AtomicNumber < 22
                  Order By theElement.Name

    For Each theElement In subset
        Console.WriteLine(theElement.Name & " " & theElement.AtomicNumber)
    Next

    ' Output:
    '  Calcium 20
    '  Potassium 19
    '  Scandium 21
End Sub

Private Function BuildList() As List(Of Element)
    Return New List(Of Element) From
        {
            {New Element With
                {.Symbol = "K", .Name = "Potassium", .AtomicNumber = 19}},
            {New Element With
                {.Symbol = "Ca", .Name = "Calcium", .AtomicNumber = 20}},
            {New Element With
                {.Symbol = "Sc", .Name = "Scandium", .AtomicNumber = 21}},
            {New Element With
                {.Symbol = "Ti", .Name = "Titanium", .AtomicNumber = 22}}
        }
End Function

Public Class Element
    Public Property Symbol As String
    Public Property Name As String
    Public Property AtomicNumber As Integer
End Class
```

<a name="BKMK_Sorting"></a>

## <a name="sorting-a-collection"></a>Classificando uma coleção

O exemplo a seguir ilustra um procedimento para a classificação de uma coleção. O exemplo classifica instâncias da classe `Car` que estão armazenados em uma <xref:System.Collections.Generic.List%601>. A classe `Car` implementa a interface <xref:System.IComparable%601>, que requer que o método <xref:System.IComparable%601.CompareTo%2A> seja implementado.

Cada chamada ao método <xref:System.IComparable%601.CompareTo%2A> faz uma comparação única que é usada para classificação. Os códigos escritos pelo usuário no método `CompareTo` retornam um valor para cada comparação do objeto atual com outro objeto. O valor retornado será menor que zero se o objeto atual for menor que o outro objeto, maior que zero se o objeto atual for maior que o outro objeto e zero, se eles forem iguais. Isso permite que você defina no código os critérios para maior que, menor que e igual.

No método `ListCars`, a instrução `cars.Sort()` classifica a lista. Essa chamada para o método <xref:System.Collections.Generic.List%601.Sort%2A> da <xref:System.Collections.Generic.List%601> faz com que o método `CompareTo` seja chamado automaticamente para os objetos `Car` na `List`.

```vb
Public Sub ListCars()

    ' Create some new cars.
    Dim cars As New List(Of Car) From
    {
        New Car With {.Name = "car1", .Color = "blue", .Speed = 20},
        New Car With {.Name = "car2", .Color = "red", .Speed = 50},
        New Car With {.Name = "car3", .Color = "green", .Speed = 10},
        New Car With {.Name = "car4", .Color = "blue", .Speed = 50},
        New Car With {.Name = "car5", .Color = "blue", .Speed = 30},
        New Car With {.Name = "car6", .Color = "red", .Speed = 60},
        New Car With {.Name = "car7", .Color = "green", .Speed = 50}
    }

    ' Sort the cars by color alphabetically, and then by speed
    ' in descending order.
    cars.Sort()

    ' View all of the cars.
    For Each thisCar As Car In cars
        Console.Write(thisCar.Color.PadRight(5) & " ")
        Console.Write(thisCar.Speed.ToString & " ")
        Console.Write(thisCar.Name)
        Console.WriteLine()
    Next

    ' Output:
    '  blue  50 car4
    '  blue  30 car5
    '  blue  20 car1
    '  green 50 car7
    '  green 10 car3
    '  red   60 car6
    '  red   50 car2
End Sub

Public Class Car
    Implements IComparable(Of Car)

    Public Property Name As String
    Public Property Speed As Integer
    Public Property Color As String

    Public Function CompareTo(ByVal other As Car) As Integer _
        Implements System.IComparable(Of Car).CompareTo
        ' A call to this method makes a single comparison that is
        ' used for sorting.

        ' Determine the relative order of the objects being compared.
        ' Sort by color alphabetically, and then by speed in
        ' descending order.

        ' Compare the colors.
        Dim compare As Integer
        compare = String.Compare(Me.Color, other.Color, True)

        ' If the colors are the same, compare the speeds.
        If compare = 0 Then
            compare = Me.Speed.CompareTo(other.Speed)

            ' Use descending order for speed.
            compare = -compare
        End If

        Return compare
    End Function
End Class
```

<a name="BKMK_CustomCollection"></a>

## <a name="defining-a-custom-collection"></a>Definindo uma coleção personalizada

Você pode definir uma coleção implementando a interface <xref:System.Collections.Generic.IEnumerable%601> ou <xref:System.Collections.IEnumerable>. Para obter informações adicionais, consulte [enumerando uma coleção](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/hwyysy67(v=vs.100)).

Embora seja possível definir uma coleção personalizada, é melhor usar as coleções que estão incluídas no .NET Framework, que estão descritas em [Tipos de coleções](#kinds-of-collections) anteriormente neste tópico.

O exemplo a seguir define uma classe de coleção personalizada chamada `AllColors`. Essa classe implementa a interface <xref:System.Collections.IEnumerable>, que requer que o método <xref:System.Collections.IEnumerable.GetEnumerator%2A> seja implementado.

O método `GetEnumerator` retorna uma instância da classe `ColorEnumerator`. `ColorEnumerator` implementa a interface <xref:System.Collections.IEnumerator>, que requer que a propriedade <xref:System.Collections.IEnumerator.Current%2A>, o método <xref:System.Collections.IEnumerator.MoveNext%2A> e o método <xref:System.Collections.IEnumerator.Reset%2A> sejam implementados.

```vb
Public Sub ListColors()
    Dim colors As New AllColors()

    For Each theColor As Color In colors
        Console.Write(theColor.Name & " ")
    Next
    Console.WriteLine()
    ' Output: red blue green
End Sub

' Collection class.
Public Class AllColors
    Implements System.Collections.IEnumerable

    Private _colors() As Color =
    {
        New Color With {.Name = "red"},
        New Color With {.Name = "blue"},
        New Color With {.Name = "green"}
    }

    Public Function GetEnumerator() As System.Collections.IEnumerator _
        Implements System.Collections.IEnumerable.GetEnumerator

        Return New ColorEnumerator(_colors)

        ' Instead of creating a custom enumerator, you could
        ' use the GetEnumerator of the array.
        'Return _colors.GetEnumerator
    End Function

    ' Custom enumerator.
    Private Class ColorEnumerator
        Implements System.Collections.IEnumerator

        Private _colors() As Color
        Private _position As Integer = -1

        Public Sub New(ByVal colors() As Color)
            _colors = colors
        End Sub

        Public ReadOnly Property Current() As Object _
            Implements System.Collections.IEnumerator.Current
            Get
                Return _colors(_position)
            End Get
        End Property

        Public Function MoveNext() As Boolean _
            Implements System.Collections.IEnumerator.MoveNext
            _position += 1
            Return (_position < _colors.Length)
        End Function

        Public Sub Reset() Implements System.Collections.IEnumerator.Reset
            _position = -1
        End Sub
    End Class
End Class

' Element class.
Public Class Color
    Public Property Name As String
End Class
```

<a name="BKMK_Iterators"></a>

## <a name="iterators"></a>Iterators

Um *iterador* é usado para realizar uma iteração personalizada em uma coleção. Um iterador pode ser um método ou um acessador `get`. Um iterador usa uma instrução [yield](../../language-reference/statements/yield-statement.md) para retornar cada elemento da coleção um de cada vez.

Você chama um iterador usando um [para cada... Próxima](../../language-reference/statements/for-each-next-statement.md) instrução. Cada iteração do loop `For Each` chama o iterador. Quando uma instrução `Yield` é alcançada no iterador, uma expressão é retornada e o local atual no código é retido. A execução será reiniciada desse local na próxima vez que o iterador for chamado.

Para obter mais informações, consulte [iteradores (Visual Basic)](iterators.md).

O exemplo a seguir usa um método iterador. O método Iterator tem uma `Yield` instrução que está dentro de um [para... Próximo](../../language-reference/statements/for-next-statement.md) loop. No método `ListEvenNumbers`, cada iteração do corpo da instrução `For Each` cria uma chamada ao método iterador, que avança para a próxima instrução `Yield`.

```vb
Public Sub ListEvenNumbers()
    For Each number As Integer In EvenSequence(5, 18)
        Console.Write(number & " ")
    Next
    Console.WriteLine()
    ' Output: 6 8 10 12 14 16 18
End Sub

Private Iterator Function EvenSequence(
ByVal firstNumber As Integer, ByVal lastNumber As Integer) _
As IEnumerable(Of Integer)

' Yield even numbers in the range.
    For number = firstNumber To lastNumber
        If number Mod 2 = 0 Then
            Yield number
        End If
    Next
End Function
```

## <a name="see-also"></a>Confira também

- [Inicializadores de coleção](../language-features/collection-initializers/index.md)
- [Conceitos de Programação (Visual Basic)](index.md)
- [Instrução Option Strict](../../language-reference/statements/option-strict-statement.md)
- [LINQ to Objects (Visual Basic)](linq/linq-to-objects.md)
- [LINQ paralelo (PLINQ)](../../../standard/parallel-programming/introduction-to-plinq.md)
- [Coleções e estruturas de dados](../../../standard/collections/index.md)
- [Selecionando uma classe de coleção](../../../standard/collections/selecting-a-collection-class.md)
- [Comparações e classificações dentro de coleções](../../../standard/collections/comparisons-and-sorts-within-collections.md)
- [Quando usar coleções genéricas](../../../standard/collections/when-to-use-generic-collections.md)
