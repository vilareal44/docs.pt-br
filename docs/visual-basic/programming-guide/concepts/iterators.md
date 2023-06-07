---
title: Iterators
ms.date: 07/20/2015
ms.assetid: f26b5c1e-fe9d-4004-b287-da7919d717ae
ms.openlocfilehash: e638d35aeb86837d91fb14681d300772e3c2375a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410923"
---
# <a name="iterators-visual-basic"></a>Iteradores (Visual Basic)

Um *iterador* pode ser usado para percorrer coleções, como listas e matrizes.

Um método iterador ou um acessador `get` realiza uma iteração personalizada em uma coleção. Um método iterador usa a instrução [yield](../../language-reference/statements/yield-statement.md) para retornar cada elemento um de cada vez. Quando uma instrução `Yield` for atingida, o local atual no código será lembrado. A execução será reiniciada desse local na próxima vez que a função iteradora for chamada.

Você consome um iterador do código do cliente usando um [para cada... Próxima](../../language-reference/statements/for-each-next-statement.md) instrução ou usando uma consulta LINQ.

No exemplo a seguir, a primeira iteração do loop `For Each` faz a execução continue no método iterador `SomeNumbers` até que a primeira instrução `Yield` seja alcançada. Essa iteração retorna um valor de 3 e o local atual no método iterador é mantido. Na próxima iteração do loop, a execução no método iterador continuará de onde parou, parando novamente quando alcançar uma instrução `Yield`. Essa iteração retorna um valor de 5 e o local atual no método iterador é mantido novamente. O loop terminará quando o final do método iterador for alcançado.

```vb
Sub Main()
    For Each number As Integer In SomeNumbers()
        Console.Write(number & " ")
    Next
    ' Output: 3 5 8
    Console.ReadKey()
End Sub

Private Iterator Function SomeNumbers() As System.Collections.IEnumerable
    Yield 3
    Yield 5
    Yield 8
End Function
```

O tipo de retorno de um método iterador ou acessador `get` pode ser <xref:System.Collections.IEnumerable>, <xref:System.Collections.Generic.IEnumerable%601>, <xref:System.Collections.IEnumerator> ou <xref:System.Collections.Generic.IEnumerator%601>.

Você pode usar uma `Exit Function` `Return` instrução or para encerrar a iteração.

Uma função de iterador ou `get` declaração de acessador Visual Basic inclui um modificador de [iterador](../../language-reference/modifiers/iterator.md) .

Os iteradores foram introduzidos em Visual Basic no Visual Studio 2012.

**Neste tópico**

- [Iterador simples](#BKMK_SimpleIterator)

- [Criando uma classe de coleção](#BKMK_CollectionClass)

- [Blocos try](#BKMK_TryBlocks)

- [Métodos anônimos](#BKMK_AnonymousMethods)

- [Usando iteradores com uma lista genérica](#BKMK_GenericList)

- [Informações de sintaxe](#BKMK_SyntaxInformation)

- [Implementação técnica](#BKMK_Technical)

- [Uso de iteradores](#BKMK_UseOfIterators)

> [!NOTE]
> Para todos os exemplos no tópico, exceto o exemplo de iterador simples, inclua instruções [Imports](../../language-reference/statements/imports-statement-net-namespace-and-type.md) para os `System.Collections` `System.Collections.Generic` namespaces e.

## <a name="simple-iterator"></a><a name="BKMK_SimpleIterator"></a>Iterador simples

O exemplo a seguir tem uma única `Yield` instrução que está dentro de um [para... Próximo](../../language-reference/statements/for-next-statement.md) loop. Em `Main`, cada iteração do corpo da instrução `For Each` cria uma chamada à função iteradora, que avança para a próxima instrução `Yield`.

```vb
Sub Main()
    For Each number As Integer In EvenSequence(5, 18)
        Console.Write(number & " ")
    Next
    ' Output: 6 8 10 12 14 16 18
    Console.ReadKey()
End Sub

Private Iterator Function EvenSequence(
ByVal firstNumber As Integer, ByVal lastNumber As Integer) _
As System.Collections.Generic.IEnumerable(Of Integer)

    ' Yield even numbers in the range.
    For number As Integer = firstNumber To lastNumber
        If number Mod 2 = 0 Then
            Yield number
        End If
    Next
End Function
```

## <a name="creating-a-collection-class"></a><a name="BKMK_CollectionClass"></a> Criando uma classe de coleção

No exemplo a seguir, a classe `DaysOfTheWeek` implementa a interface <xref:System.Collections.IEnumerable>, que requer um método <xref:System.Collections.IEnumerable.GetEnumerator%2A>. O compilador chama implicitamente o método `GetEnumerator`, que retorna um <xref:System.Collections.IEnumerator>.

O `GetEnumerator` método retorna cada cadeia de caracteres uma por vez usando a `Yield` instrução, e um `Iterator` modificador está na declaração da função.

```vb
Sub Main()
    Dim days As New DaysOfTheWeek()
    For Each day As String In days
        Console.Write(day & " ")
    Next
    ' Output: Sun Mon Tue Wed Thu Fri Sat
    Console.ReadKey()
End Sub

Private Class DaysOfTheWeek
    Implements IEnumerable

    Public days =
        New String() {"Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"}

    Public Iterator Function GetEnumerator() As IEnumerator _
        Implements IEnumerable.GetEnumerator

        ' Yield each day of the week.
        For i As Integer = 0 To days.Length - 1
            Yield days(i)
        Next
    End Function
End Class
```

O exemplo a seguir cria uma classe `Zoo` que contém uma coleção de animais.

A instrução `For Each`, que faz referência à instância de classe (`theZoo`), chama implicitamente o método `GetEnumerator`. As instruções `For Each`, que fazem referência às propriedades `Birds` e `Mammals`, usam o método iterador nomeado `AnimalsForType`.

```vb
Sub Main()
    Dim theZoo As New Zoo()

    theZoo.AddMammal("Whale")
    theZoo.AddMammal("Rhinoceros")
    theZoo.AddBird("Penguin")
    theZoo.AddBird("Warbler")

    For Each name As String In theZoo
        Console.Write(name & " ")
    Next
    Console.WriteLine()
    ' Output: Whale Rhinoceros Penguin Warbler

    For Each name As String In theZoo.Birds
        Console.Write(name & " ")
    Next
    Console.WriteLine()
    ' Output: Penguin Warbler

    For Each name As String In theZoo.Mammals
        Console.Write(name & " ")
    Next
    Console.WriteLine()
    ' Output: Whale Rhinoceros

    Console.ReadKey()
End Sub

Public Class Zoo
    Implements IEnumerable

    ' Private members.
    Private animals As New List(Of Animal)

    ' Public methods.
    Public Sub AddMammal(ByVal name As String)
        animals.Add(New Animal With {.Name = name, .Type = Animal.TypeEnum.Mammal})
    End Sub

    Public Sub AddBird(ByVal name As String)
        animals.Add(New Animal With {.Name = name, .Type = Animal.TypeEnum.Bird})
    End Sub

    Public Iterator Function GetEnumerator() As IEnumerator _
        Implements IEnumerable.GetEnumerator

        For Each theAnimal As Animal In animals
            Yield theAnimal.Name
        Next
    End Function

    ' Public members.
    Public ReadOnly Property Mammals As IEnumerable
        Get
            Return AnimalsForType(Animal.TypeEnum.Mammal)
        End Get
    End Property

    Public ReadOnly Property Birds As IEnumerable
        Get
            Return AnimalsForType(Animal.TypeEnum.Bird)
        End Get
    End Property

    ' Private methods.
    Private Iterator Function AnimalsForType( _
    ByVal type As Animal.TypeEnum) As IEnumerable
        For Each theAnimal As Animal In animals
            If (theAnimal.Type = type) Then
                Yield theAnimal.Name
            End If
        Next
    End Function

    ' Private class.
    Private Class Animal
        Public Enum TypeEnum
            Bird
            Mammal
        End Enum

        Public Property Name As String
        Public Property Type As TypeEnum
    End Class
End Class
```

## <a name="try-blocks"></a><a name="BKMK_TryBlocks"></a>Blocos try

Visual Basic permite que uma `Yield` instrução no `Try` bloco de um [try... Capturar... Instrução Finally](../../language-reference/statements/try-catch-finally-statement.md). Um `Try` bloco que tem uma `Yield` instrução pode ter `Catch` blocos e pode ter um `Finally` bloco.

O exemplo a seguir inclui os `Try` `Catch` blocos, e `Finally` em uma função de iterador. O `Finally` bloco na função de iterador é executado antes de a `For Each` iteração ser concluída.

```vb
Sub Main()
    For Each number As Integer In Test()
        Console.WriteLine(number)
    Next
    Console.WriteLine("For Each is done.")

    ' Output:
    '  3
    '  4
    '  Something happened. Yields are done.
    '  Finally is called.
    '  For Each is done.
    Console.ReadKey()
End Sub

Private Iterator Function Test() As IEnumerable(Of Integer)
    Try
        Yield 3
        Yield 4
        Throw New Exception("Something happened. Yields are done.")
        Yield 5
        Yield 6
    Catch ex As Exception
        Console.WriteLine(ex.Message)
    Finally
        Console.WriteLine("Finally is called.")
    End Try
End Function
```

Uma `Yield` instrução não pode estar dentro de um `Catch` bloco ou `Finally` bloco.

Se o `For Each` corpo (em vez do método iterador) gerar uma exceção, um `Catch` bloco na função de iterador não será executado, mas um `Finally` bloco na função de iterador será executado. Um `Catch` bloco dentro de uma função de iterador captura apenas as exceções que ocorrem dentro da função de iterador.

## <a name="anonymous-methods"></a><a name="BKMK_AnonymousMethods"></a>Métodos anônimos

No Visual Basic, uma função anônima pode ser uma função de iterador. O exemplo a seguir ilustra isto.

```vb
Dim iterateSequence = Iterator Function() _
                      As IEnumerable(Of Integer)
                          Yield 1
                          Yield 2
                      End Function

For Each number As Integer In iterateSequence()
    Console.Write(number & " ")
Next
' Output: 1 2
Console.ReadKey()
```

O exemplo a seguir tem um método que não é iterador que valida os argumentos. O método retorna o resultado de um iterador anônimo que descreve os elementos da coleção.

```vb
Sub Main()
    For Each number As Integer In GetSequence(5, 10)
        Console.Write(number & " ")
    Next
    ' Output: 5 6 7 8 9 10
    Console.ReadKey()
End Sub

Public Function GetSequence(ByVal low As Integer, ByVal high As Integer) _
As IEnumerable
    ' Validate the arguments.
    If low < 1 Then
        Throw New ArgumentException("low is too low")
    End If
    If high > 140 Then
        Throw New ArgumentException("high is too high")
    End If

    ' Return an anonymous iterator function.
    Dim iterateSequence = Iterator Function() As IEnumerable
                              For index = low To high
                                  Yield index
                              Next
                          End Function
    Return iterateSequence()
End Function
```

Se a validação estiver dentro da função de iterador, a validação não poderá ser executada até o início da primeira iteração do `For Each` corpo.

## <a name="using-iterators-with-a-generic-list"></a><a name="BKMK_GenericList"></a>Usando iteradores com uma lista genérica

No exemplo a seguir, a classe `Stack(Of T)` genérica implementa a interface genérica <xref:System.Collections.Generic.IEnumerable%601>. O método `Push` atribui valores a uma matriz do tipo `T`. O método <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> retorna os valores da matriz usando a instrução `Yield`.

Além do método <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> genérico, o método <xref:System.Collections.IEnumerable.GetEnumerator%2A> não genérico também deve ser implementado. Isso ocorre porque <xref:System.Collections.Generic.IEnumerable%601> herda de <xref:System.Collections.IEnumerable>. A implementação não genérica adia a implementação genérica.

O exemplo usa iteradores nomeados para dar suporte a várias maneiras de iterar na mesma coleção de dados. Esses iteradores nomeados são as propriedades `TopToBottom` e `BottomToTop` e o método `TopN`.

A `BottomToTop` declaração de propriedade inclui a `Iterator` palavra-chave.

```vb
Sub Main()
    Dim theStack As New Stack(Of Integer)

    ' Add items to the stack.
    For number As Integer = 0 To 9
        theStack.Push(number)
    Next

    ' Retrieve items from the stack.
    ' For Each is allowed because theStack implements
    ' IEnumerable(Of Integer).
    For Each number As Integer In theStack
        Console.Write("{0} ", number)
    Next
    Console.WriteLine()
    ' Output: 9 8 7 6 5 4 3 2 1 0

    ' For Each is allowed, because theStack.TopToBottom
    ' returns IEnumerable(Of Integer).
    For Each number As Integer In theStack.TopToBottom
        Console.Write("{0} ", number)
    Next
    Console.WriteLine()
    ' Output: 9 8 7 6 5 4 3 2 1 0

    For Each number As Integer In theStack.BottomToTop
        Console.Write("{0} ", number)
    Next
    Console.WriteLine()
    ' Output: 0 1 2 3 4 5 6 7 8 9

    For Each number As Integer In theStack.TopN(7)
        Console.Write("{0} ", number)
    Next
    Console.WriteLine()
    ' Output: 9 8 7 6 5 4 3

    Console.ReadKey()
End Sub

Public Class Stack(Of T)
    Implements IEnumerable(Of T)

    Private values As T() = New T(99) {}
    Private top As Integer = 0

    Public Sub Push(ByVal t As T)
        values(top) = t
        top = top + 1
    End Sub

    Public Function Pop() As T
        top = top - 1
        Return values(top)
    End Function

    ' This function implements the GetEnumerator method. It allows
    ' an instance of the class to be used in a For Each statement.
    Public Iterator Function GetEnumerator() As IEnumerator(Of T) _
        Implements IEnumerable(Of T).GetEnumerator

        For index As Integer = top - 1 To 0 Step -1
            Yield values(index)
        Next
    End Function

    Public Iterator Function GetEnumerator1() As IEnumerator _
        Implements IEnumerable.GetEnumerator

        Yield GetEnumerator()
    End Function

    Public ReadOnly Property TopToBottom() As IEnumerable(Of T)
        Get
            Return Me
        End Get
    End Property

    Public ReadOnly Iterator Property BottomToTop As IEnumerable(Of T)
        Get
            For index As Integer = 0 To top - 1
                Yield values(index)
            Next
        End Get
    End Property

    Public Iterator Function TopN(ByVal itemsFromTop As Integer) _
        As IEnumerable(Of T)

        ' Return less than itemsFromTop if necessary.
        Dim startIndex As Integer =
            If(itemsFromTop >= top, 0, top - itemsFromTop)

        For index As Integer = top - 1 To startIndex Step -1
            Yield values(index)
        Next
    End Function
End Class
```

## <a name="syntax-information"></a><a name="BKMK_SyntaxInformation"></a>Informações de sintaxe

Um iterador pode ocorrer como um método ou como um acessador `get`. Um iterador não pode ocorrer em um evento, um construtor de instância, um construtor estático ou um destruidor estático.

Deve existir uma conversão implícita do tipo de expressão na instrução `Yield`, para o tipo de retorno do iterador.

No Visual Basic, um método iterador não pode ter `ByRef` parâmetros.

No Visual Basic, "yield" não é uma palavra reservada e tem um significado especial apenas quando é usado em um `Iterator` método ou `get` acessador.

## <a name="technical-implementation"></a><a name="BKMK_Technical"></a>Implementação técnica

Embora você escreva um iterador como um método, o compilador o traduz em uma classe aninhada que é, na verdade, uma máquina de estado. Essa classe mantém o controle da posição do iterador enquanto o loop `For Each...Next` no código cliente continuar.

Para ver o que o compilador faz, você pode usar a ferramenta Ildasm.exe para exibir o código Microsoft Intermediate Language que é gerado para um método iterador.

Quando você cria um iterador para uma [classe](../../language-reference/statements/class-statement.md) ou [estrutura](../../language-reference/statements/structure-statement.md), não é necessário implementar toda a <xref:System.Collections.IEnumerator> interface. Quando o compilador detecta o iterador, ele gera automaticamente os métodos `Current`, `MoveNext` e `Dispose` da interface <xref:System.Collections.IEnumerator> ou <xref:System.Collections.Generic.IEnumerator%601>.

A cada iteração sucessiva do loop `For Each…Next` (ou a chamada direta ao `IEnumerator.MoveNext`), o próximo corpo de código do iterador continua, depois da instrução `Yield` anterior. Em seguida, ele continua para a próxima `Yield` instrução até que o final do corpo do iterador seja atingido ou até que uma `Exit Function` `Return` instrução ou seja encontrada.

Os iteradores não dão suporte ao <xref:System.Collections.IEnumerator.Reset%2A?displayProperty=nameWithType> método. Para iterar novamente desde o início, você deve obter um novo iterador.

Para obter informações adicionais, consulte a [especificação da linguagem Visual Basic](../../reference/language-specification/index.md).

## <a name="use-of-iterators"></a><a name="BKMK_UseOfIterators"></a>Uso de iteradores

Os iteradores permitem que você mantenha a simplicidade de um loop `For Each` quando for necessário usar um código complexo para preencher uma sequência de lista. Isso pode ser útil quando você quiser fazer o seguinte:

- Modificar a sequência de lista após a primeira iteração de loop `For Each`.

- Evitar o carregamento completo de uma grande lista antes da primeira iteração de um loop `For Each`. Um exemplo é uma busca paginada para carregar um lote de linhas da tabela. Outro exemplo é o método <xref:System.IO.DirectoryInfo.EnumerateFiles%2A>, que implementa os iteradores dentro do .NET Framework.

- Encapsular a criação da lista no iterador. No método iterador, você pode criar a lista e, em seguida, gerar cada resultado em um loop.

## <a name="see-also"></a>Confira também

- <xref:System.Collections.Generic>
- <xref:System.Collections.Generic.IEnumerable%601>
- [Instrução For Each...Next](../../language-reference/statements/for-each-next-statement.md)
- [Instrução Yield](../../language-reference/statements/yield-statement.md)
- [Iterador](../../language-reference/modifiers/iterator.md)
