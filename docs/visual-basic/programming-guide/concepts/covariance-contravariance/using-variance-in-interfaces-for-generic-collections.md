---
title: Usar variação em interfaces para coleções genéricas
ms.date: 07/20/2015
ms.assetid: c867fcea-7462-4995-b9c5-542feec74036
ms.openlocfilehash: b762ce42215f9b24371313446637e95962677bfb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84375635"
---
# <a name="using-variance-in-interfaces-for-generic-collections-visual-basic"></a>Usando variação em interfaces para coleções genéricas (Visual Basic)

Uma interface de covariante permite que seus métodos retornem mais tipos derivados daquelas especificadas na interface. Uma interface de contravariante permite que seus métodos aceitem parâmetros de tipos menos derivados do que os especificados na interface.

No .NET Framework 4, várias interfaces existentes se tornaram covariantes e contravariantes. Eles incluem <xref:System.Collections.Generic.IEnumerable%601> e <xref:System.IComparable%601>. Isso permite que você reutilize métodos que operam com coleções genéricas de tipos base para coleções de tipos derivados.

Para obter uma lista de interfaces variantes na .NET Framework, consulte [variação em interfaces genéricas (Visual Basic)](variance-in-generic-interfaces.md).

## <a name="converting-generic-collections"></a>Convertendo coleções genéricas

O exemplo a seguir ilustra os benefícios do suporte à covariância na interface <xref:System.Collections.Generic.IEnumerable%601>. O método `PrintFullName` aceita uma coleção do tipo `IEnumerable(Of Person)` como um parâmetro. No entanto, você pode reutilizá-lo para uma coleção do tipo `IEnumerable(Of Person)` porque `Employee` herda `Person`.

```vb
' Simple hierarchy of classes.
Public Class Person
    Public Property FirstName As String
    Public Property LastName As String
End Class

Public Class Employee
    Inherits Person
End Class

' The method has a parameter of the IEnumerable(Of Person) type.
Public Sub PrintFullName(ByVal persons As IEnumerable(Of Person))
    For Each person As Person In persons
        Console.WriteLine(
            "Name: " & person.FirstName & " " & person.LastName)
    Next
End Sub

Sub Main()
    Dim employees As IEnumerable(Of Employee) = New List(Of Employee)

    ' You can pass IEnumerable(Of Employee),
    ' although the method expects IEnumerable(Of Person).

    PrintFullName(employees)

End Sub
```

## <a name="comparing-generic-collections"></a>Comparando coleções genéricas

O exemplo a seguir ilustra os benefícios do suporte à contravariância na interface <xref:System.Collections.Generic.IComparer%601>. A classe `PersonComparer` implementa a interface `IComparer(Of Person)`. No entanto, você pode reutilizar essa classe para comparar uma sequência de objetos do tipo `Employee` porque `Employee` herda `Person`.

```vb
' Simple hierarchy of classes.
Public Class Person
    Public Property FirstName As String
    Public Property LastName As String
End Class

Public Class Employee
    Inherits Person
End Class
' The custom comparer for the Person type
' with standard implementations of Equals()
' and GetHashCode() methods.
Class PersonComparer
    Implements IEqualityComparer(Of Person)

    Public Function Equals1(
        ByVal x As Person,
        ByVal y As Person) As Boolean _
        Implements IEqualityComparer(Of Person).Equals

        If x Is y Then Return True
        If x Is Nothing OrElse y Is Nothing Then Return False
        Return (x.FirstName = y.FirstName) AndAlso
            (x.LastName = y.LastName)
    End Function
    Public Function GetHashCode1(
        ByVal person As Person) As Integer _
        Implements IEqualityComparer(Of Person).GetHashCode

        If person Is Nothing Then Return 0
        Dim hashFirstName =
            If(person.FirstName Is Nothing,
            0, person.FirstName.GetHashCode())
        Dim hashLastName = person.LastName.GetHashCode()
        Return hashFirstName Xor hashLastName
    End Function
End Class

Sub Main()
    Dim employees = New List(Of Employee) From {
        New Employee With {.FirstName = "Michael", .LastName = "Alexander"},
        New Employee With {.FirstName = "Jeff", .LastName = "Price"}
    }

    ' You can pass PersonComparer,
    ' which implements IEqualityComparer(Of Person),
    ' although the method expects IEqualityComparer(Of Employee)

    Dim noduplicates As IEnumerable(Of Employee) = employees.Distinct(New PersonComparer())

    For Each employee In noduplicates
        Console.WriteLine(employee.FirstName & " " & employee.LastName)
    Next
End Sub
```

## <a name="see-also"></a>Confira também

- [Variação em interfaces genéricas (Visual Basic)](variance-in-generic-interfaces.md)
