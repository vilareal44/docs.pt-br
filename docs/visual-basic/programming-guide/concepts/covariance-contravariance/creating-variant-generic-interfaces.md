---
title: Criar interfaces genéricas variáveis
ms.date: 07/20/2015
ms.assetid: d4037dd2-dfe9-4811-9150-93d4e8b20113
ms.openlocfilehash: 884349159d2738d8481b217f9dab383483616f2b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400636"
---
# <a name="creating-variant-generic-interfaces-visual-basic"></a>Criando interfaces genéricas variantes (Visual Basic)

Você pode declarar parâmetros de tipo genérico em interfaces como covariantes ou contravariantes. A *Covariância* permite que os métodos de interface tenham tipos de retorno mais derivados que aqueles definidos pelos parâmetros de tipo genérico. A *Contravariância* permite que os métodos de interface tenham tipos de argumentos que são menos derivados que aqueles especificados pelos parâmetros genéricos. Uma interface genérica que tenha parâmetros de tipo genérico covariantes ou contravariantes é chamada de *variante*.

> [!NOTE]
> O .NET Framework 4 introduziu o suporte à variação para diversas interfaces genéricas existentes. Para obter a lista das interfaces variantes na .NET Framework, consulte [variação em interfaces genéricas (Visual Basic)](variance-in-generic-interfaces.md).

## <a name="declaring-variant-generic-interfaces"></a>Declarando interfaces genéricas variantes

Você pode declarar interfaces genéricas variantes usando as palavras-chave `in` e `out` para parâmetros de tipo genérico.

> [!IMPORTANT]
> `ByRef`os parâmetros na Visual Basic não podem ser Variant. Os tipos de valor também não dão suporte à variância.

Você pode declarar um parâmetro de tipo genérico como covariante usando a palavra-chave `out`. O tipo de covariante deve satisfazer as condições a seguir:

- O tipo é usado apenas como um tipo de retorno dos métodos de interface e não é usado como um tipo de argumentos de método. Isso é ilustrado no exemplo a seguir, no qual o tipo `R` é declarado covariante.

    ```vb
    Interface ICovariant(Of Out R)
        Function GetSomething() As R
        ' The following statement generates a compiler error.
        ' Sub SetSomething(ByVal sampleArg As R)
    End Interface
    ```

    Há uma exceção a essa regra. Se você tiver um delegado genérico contravariante como um parâmetro de método, você poderá usar o tipo como um parâmetro de tipo genérico para o delegado. Isso é ilustrado pelo tipo `R` no exemplo a seguir. Para obter mais informações, consulte [variação em delegados (Visual Basic)](variance-in-delegates.md) e [usando a variação para Func e delegados genéricos de ação (Visual Basic)](using-variance-for-func-and-action-generic-delegates.md).

    ```vb
    Interface ICovariant(Of Out R)
        Sub DoSomething(ByVal callback As Action(Of R))
    End Interface
    ```

- O tipo não é usado como uma restrição genérica para os métodos de interface. O código a seguir ilustra isso.

    ```vb
    Interface ICovariant(Of Out R)
        ' The following statement generates a compiler error
        ' because you can use only contravariant or invariant types
        ' in generic constraints.
        ' Sub DoSomething(Of T As R)()
    End Interface
    ```

Você pode declarar um parâmetro de tipo genérico como contravariante usando a palavra-chave `in`. O tipo contravariante pode ser usado apenas como um tipo de argumentos de método e não como um tipo de retorno dos métodos de interface. O tipo contravariante também pode ser usado para restrições genéricas. O código a seguir mostra como declarar uma interface contravariante e usar uma restrição genérica para um de seus métodos.

```vb
Interface IContravariant(Of In A)
    Sub SetSomething(ByVal sampleArg As A)
    Sub DoSomething(Of T As A)()
    ' The following statement generates a compiler error.
    ' Function GetSomething() As A
End Interface
```

Também é possível oferecer suporte à covariância e contravariância na mesma interface, mas para parâmetros de tipo diferentes, conforme mostrado no exemplo de código a seguir.

```vb
Interface IVariant(Of Out R, In A)
    Function GetSomething() As R
    Sub SetSomething(ByVal sampleArg As A)
    Function GetSetSomething(ByVal sampleArg As A) As R
End Interface
```

No Visual Basic, você não pode declarar eventos em interfaces variantes sem especificar o tipo delegado. Além disso, uma interface variante não pode ter classes aninhadas, enums ou estruturas, mas pode ter interfaces aninhadas. O código a seguir ilustra isso.

```vb
Interface ICovariant(Of Out R)
    ' The following statement generates a compiler error.
    ' Event SampleEvent()
    ' The following statement specifies the delegate type and
    ' does not generate an error.
    Event AnotherEvent As EventHandler

    ' The following statements generate compiler errors,
    ' because a variant interface cannot have
    ' nested enums, classes, or structures.

    'Enum SampleEnum : test : End Enum
    'Class SampleClass : End Class
    'Structure SampleStructure : Dim value As Integer : End Structure

    ' Variant interfaces can have nested interfaces.
    Interface INested : End Interface
End Interface
```

## <a name="implementing-variant-generic-interfaces"></a>Implementando interfaces genéricas variantes

Você pode implementar interfaces genéricas variantes em classes, através da mesma sintaxe que é usada para interfaces invariantes. O exemplo de código a seguir mostra como implementar uma interface covariante em uma classe genérica.

```vb
Interface ICovariant(Of Out R)
    Function GetSomething() As R
End Interface

Class SampleImplementation(Of R)
    Implements ICovariant(Of R)
    Public Function GetSomething() As R _
    Implements ICovariant(Of R).GetSomething
        ' Some code.
    End Function
End Class
```

As classes que implementam interfaces variantes são invariantes. Por exemplo, considere o código a seguir.

```vb
 The interface is covariant.
Dim ibutton As ICovariant(Of Button) =
    New SampleImplementation(Of Button)
Dim iobj As ICovariant(Of Object) = ibutton

' The class is invariant.
Dim button As SampleImplementation(Of Button) =
    New SampleImplementation(Of Button)
' The following statement generates a compiler error
' because classes are invariant.
' Dim obj As SampleImplementation(Of Object) = button
```

## <a name="extending-variant-generic-interfaces"></a>Estendendo interfaces genéricas variantes

Quando você estende uma interface genérica variante, é necessário usar as palavras-chave `in` e `out` para especificar explicitamente se a interface derivada dará suporte à variância. O compilador não deduz a variância da interface que está sendo estendida. Por exemplo, considere as seguintes interfaces.

```vb
Interface ICovariant(Of Out T)
End Interface

Interface IInvariant(Of T)
    Inherits ICovariant(Of T)
End Interface

Interface IExtCovariant(Of Out T)
    Inherits ICovariant(Of T)
End Interface
```

Na `Invariant(Of T)` interface, o parâmetro de tipo genérico `T` é invariável, enquanto no `IExtCovariant (Of Out T)` parâmetro de tipo é covariant, embora ambas as interfaces estendam a mesma interface. A mesma regra é aplicada aos parâmetros de tipo genérico contravariantes.

Você pode criar uma interface que estende tanto a interface em que o parâmetro de tipo genérico `T` é covariante, quanto a interface em que ele é contravariante, caso na interface de extensão o parâmetro de tipo genérico `T` seja invariante. Isso é ilustrado no exemplo de código a seguir.

```vb
Interface ICovariant(Of Out T)
End Interface

Interface IContravariant(Of In T)
End Interface

Interface IInvariant(Of T)
    Inherits ICovariant(Of T), IContravariant(Of T)
End Interface
```

No entanto, se um parâmetro de tipo genérico `T` for declarado covariante em uma interface, você não poderá declará-lo contravariante na interface de extensão ou vice-versa. Isso é ilustrado no exemplo de código a seguir.

```vb
Interface ICovariant(Of Out T)
End Interface

' The following statements generate a compiler error.
' Interface ICoContraVariant(Of In T)
'     Inherits ICovariant(Of T)
' End Interface
```

### <a name="avoiding-ambiguity"></a>Evitando ambiguidade

Ao implementar interfaces genéricas variantes, a variância, às vezes, pode levar à ambiguidade. Isso deve ser evitado.

Por exemplo, se você implementar explicitamente a mesma interface genérica variante com parâmetros de tipo genérico diferentes em uma classe, isso poderá criar ambiguidade. O compilador não produzirá um erro nesse caso, mas não está especificado qual implementação de interface será escolhida em runtime. Isso poderá causar bugs sutis no seu código. Considere o exemplo de código a seguir.

> [!NOTE]
> Com `Option Strict Off` o, Visual Basic gera um aviso do compilador quando há uma implementação de interface ambígua. Com `Option Strict On` , Visual Basic gera um erro do compilador.

```vb
' Simple class hierarchy.
Class Animal
End Class

Class Cat
    Inherits Animal
End Class

Class Dog
    Inherits Animal
End Class

' This class introduces ambiguity
' because IEnumerable(Of Out T) is covariant.
Class Pets
    Implements IEnumerable(Of Cat), IEnumerable(Of Dog)

    Public Function GetEnumerator() As IEnumerator(Of Cat) _
        Implements IEnumerable(Of Cat).GetEnumerator
        Console.WriteLine("Cat")
        ' Some code.
    End Function

    Public Function GetEnumerator1() As IEnumerator(Of Dog) _
        Implements IEnumerable(Of Dog).GetEnumerator
        Console.WriteLine("Dog")
        ' Some code.
    End Function

    Public Function GetEnumerator2() As IEnumerator _
        Implements IEnumerable.GetEnumerator
        ' Some code.
    End Function
End Class

Sub Main()
    Dim pets As IEnumerable(Of Animal) = New Pets()
    pets.GetEnumerator()
End Sub
```

Neste exemplo, não está especificado como o método `pets.GetEnumerator` escolherá entre `Cat` e `Dog`. Isso poderá causar problemas em seu código.

## <a name="see-also"></a>Confira também

- [Variação em interfaces genéricas (Visual Basic)](variance-in-generic-interfaces.md)
- [Usando variação para delegados genéricos Func e Action (Visual Basic)](using-variance-for-func-and-action-generic-delegates.md)
