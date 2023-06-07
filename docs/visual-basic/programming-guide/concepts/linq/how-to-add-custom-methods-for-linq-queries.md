---
title: 'Como: adicionar métodos personalizados para consultas LINQ'
ms.date: 07/20/2015
ms.assetid: 099b2e2a-83cd-45c6-aa4d-01b398b5faaf
ms.openlocfilehash: 55004441d2d1d74556da6841f28d113b876d1048
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400598"
---
# <a name="how-to-add-custom-methods-for-linq-queries-visual-basic"></a>Como: adicionar métodos personalizados para consultas LINQ (Visual Basic)

Você pode estender o conjunto de métodos que você pode usar para consultas LINQ, adicionando os métodos de extensão à interface <xref:System.Collections.Generic.IEnumerable%601>. Por exemplo, além do padrão médio ou máximo de operações, você pode criar um método de agregação personalizado para calcular um único valor de uma sequência de valores. Você também pode criar um método que funciona como um filtro personalizado ou como uma transformação de dados específicos para uma sequência de valores e que retorna uma nova sequência. Exemplos desses métodos são <xref:System.Linq.Enumerable.Distinct%2A>, <xref:System.Linq.Enumerable.Skip%2A> e <xref:System.Linq.Enumerable.Reverse%2A>.

Ao estender a interface <xref:System.Collections.Generic.IEnumerable%601>, você pode aplicar seus métodos personalizados para qualquer coleção enumerável. Para obter mais informações, consulte [Métodos de extensão](../../language-features/procedures/extension-methods.md).

## <a name="adding-an-aggregate-method"></a>Adicionando um método de agregação

Um método de agregação calcula um valor único de um conjunto de valores. O LINQ fornece vários métodos de agregação, incluindo <xref:System.Linq.Enumerable.Average%2A>, <xref:System.Linq.Enumerable.Min%2A> e <xref:System.Linq.Enumerable.Max%2A>. Você pode criar seu próprio método de agregação, adicionando um método de extensão à interface <xref:System.Collections.Generic.IEnumerable%601>.

O exemplo de código a seguir mostra como criar um método de extensão chamado `Median` para calcular uma mediana de uma sequência de números do tipo `double`.

```vb
Imports System.Runtime.CompilerServices

Module LINQExtension

    ' Extension method for the IEnumerable(of T) interface.
    ' The method accepts only values of the Double type.
    <Extension()>
    Function Median(ByVal source As IEnumerable(Of Double)) As Double
        If source.Count = 0 Then
            Throw New InvalidOperationException("Cannot compute median for an empty set.")
        End If

        Dim sortedSource = From number In source
                           Order By number

        Dim itemIndex = sortedSource.Count \ 2

        If sortedSource.Count Mod 2 = 0 Then
            ' Even number of items in list.
            Return (sortedSource(itemIndex) + sortedSource(itemIndex - 1)) / 2
        Else
            ' Odd number of items in list.
            Return sortedSource(itemIndex)
        End If
    End Function
End Module
```

Você chama esse método de extensão para qualquer coleção enumerável da mesma maneira que chama outros métodos de agregação da interface <xref:System.Collections.Generic.IEnumerable%601>.

> [!NOTE]
> No Visual Basic, você pode usar uma chamada de método ou sintaxe de consulta padrão para `Aggregate` a `Group By` cláusula or. Para obter mais informações, consulte [cláusula Aggregate](../../../language-reference/queries/aggregate-clause.md) e [cláusula Group by](../../../language-reference/queries/group-by-clause.md).

O exemplo de código a seguir mostra como usar o método `Median` para uma matriz do tipo `double`.

```vb
Dim numbers1() As Double = {1.9, 2, 8, 4, 5.7, 6, 7.2, 0}

Dim query1 = Aggregate num In numbers1 Into Median()

Console.WriteLine("Double: Median = " & query1)
```

```vb
' This code produces the following output:
'
' Double: Median = 4.85
```

### <a name="overloading-an-aggregate-method-to-accept-various-types"></a>Sobrecarregando um método de agregação para aceitar vários tipos

Você pode sobrecarregar o método de agregação para que ele aceite sequências de vários tipos. A abordagem padrão é criar uma sobrecarga para cada tipo. Outra abordagem é criar uma sobrecarga que vai pegar um tipo genérico e convertê-lo em um tipo específico, usando um delegado. Você também pode combinar as duas abordagens.

#### <a name="to-create-an-overload-for-each-type"></a>Para criar uma sobrecarga para cada tipo

Você pode criar uma sobrecarga específica para cada tipo que deseja oferecer suporte. O exemplo de código a seguir mostra uma sobrecarga do método `Median` para o tipo `integer`.

```vb
' Integer overload

<Extension()>
Function Median(ByVal source As IEnumerable(Of Integer)) As Double
    Return Aggregate num In source Select CDbl(num) Into med = Median()
End Function
```

Agora você pode chamar as sobrecargas `Median` para os tipos `integer` e `double`, conforme mostrado no código a seguir:

```vb
Dim numbers1() As Double = {1.9, 2, 8, 4, 5.7, 6, 7.2, 0}

Dim query1 = Aggregate num In numbers1 Into Median()

Console.WriteLine("Double: Median = " & query1)
```

```vb
Dim numbers2() As Integer = {1, 2, 3, 4, 5}

Dim query2 = Aggregate num In numbers2 Into Median()

Console.WriteLine("Integer: Median = " & query2)
```

```vb
' This code produces the following output:
'
' Double: Median = 4.85
' Integer: Median = 3
```

#### <a name="to-create-a-generic-overload"></a>Para criar uma sobrecarga genérica

Você também pode criar uma sobrecarga que aceita uma sequência de objetos genéricos. Essa sobrecarga recebe um delegado como parâmetro e usa-o para converter uma sequência de objetos de um tipo genérico em um tipo específico.

O código a seguir mostra uma sobrecarga do método `Median` que recebe o delegado <xref:System.Func%602> como um parâmetro. Esse delegado recebe um objeto de tipo genérico T e retorna um objeto do tipo `double`.

```vb
' Generic overload.

<Extension()>
Function Median(Of T)(ByVal source As IEnumerable(Of T),
                      ByVal selector As Func(Of T, Double)) As Double
    Return Aggregate num In source Select selector(num) Into med = Median()
End Function
```

Agora você pode chamar o método `Median` para uma sequência de objetos de qualquer tipo. Se o tipo não tem sua própria sobrecarga de método, você precisa passar um parâmetro delegado. No Visual Basic, você pode usar uma expressão lambda para essa finalidade. Além disso, se você usar `Aggregate` a `Group By` cláusula or em vez da chamada de método, poderá passar qualquer valor ou expressão que esteja no escopo dessa cláusula.

O exemplo de código a seguir mostra como chamar o método `Median` para uma matriz de inteiros e para uma matriz de cadeias de caracteres. Será calculada a mediana dos comprimentos das cadeias de caracteres na matriz. O exemplo também mostra como passar o parâmetro delegado <xref:System.Func%602> ao método `Median` para cada caso.

```vb
Dim numbers3() As Integer = {1, 2, 3, 4, 5}

' You can use num as a parameter for the Median method
' so that the compiler will implicitly convert its value to double.
' If there is no implicit conversion, the compiler will
' display an error message.

Dim query3 = Aggregate num In numbers3 Into Median(num)

Console.WriteLine("Integer: Median = " & query3)

Dim numbers4() As String = {"one", "two", "three", "four", "five"}

' With the generic overload, you can also use numeric properties of objects.

Dim query4 = Aggregate str In numbers4 Into Median(str.Length)

Console.WriteLine("String: Median = " & query4)

' This code produces the following output:
'
' Integer: Median = 3
' String: Median = 4
```

## <a name="adding-a-method-that-returns-a-collection"></a>Adicionando um método que retorna uma coleção

Você pode estender a interface <xref:System.Collections.Generic.IEnumerable%601> com um método de consulta personalizada que retorna uma sequência de valores. Nesse caso, o método deve retornar uma coleção do tipo <xref:System.Collections.Generic.IEnumerable%601>. Esses métodos podem ser usados para aplicar transformações de dados ou filtros a uma sequência de valores.

O exemplo a seguir mostra como criar um método de extensão chamado `AlternateElements` que retorna todos os outros elementos em uma coleção, começando pelo primeiro elemento.

```vb
' Extension method for the IEnumerable(of T) interface.
' The method returns every other element of a sequence.

<Extension()>
Function AlternateElements(Of T)(
    ByVal source As IEnumerable(Of T)
    ) As IEnumerable(Of T)

    Dim list As New List(Of T)
    Dim i = 0
    For Each element In source
        If (i Mod 2 = 0) Then
            list.Add(element)
        End If
        i = i + 1
    Next
    Return list
End Function
```

Você pode chamar esse método de extensão para qualquer coleção enumerável exatamente como chamaria outros métodos da interface <xref:System.Collections.Generic.IEnumerable%601>, conforme mostrado no código a seguir:

```vb
Dim strings() As String = {"a", "b", "c", "d", "e"}

Dim query = strings.AlternateElements()

For Each element In query
    Console.WriteLine(element)
Next

' This code produces the following output:
'
' a
' c
' e
```

## <a name="see-also"></a>Confira também

- <xref:System.Collections.Generic.IEnumerable%601>
- [Métodos de Extensão](../../language-features/procedures/extension-methods.md)
