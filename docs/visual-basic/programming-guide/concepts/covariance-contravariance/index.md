---
title: Covariância e contravariância
ms.date: 07/20/2015
ms.assetid: 59224c46-9931-466b-8c6e-3648c3e609c6
ms.openlocfilehash: 11dd71a8cfde12b7af1de79e3f5a095f79d8aa6e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400624"
---
# <a name="covariance-and-contravariance-visual-basic"></a>Covariância e contravariância (Visual Basic)

No Visual Basic, a covariância e a contravariância habilitam a conversão de referência implícita para tipos de matriz, tipos delegados e argumentos de tipo genérico. A covariância preserva a compatibilidade de atribuição, e a contravariância reverte.

O código a seguir demonstra a diferença entre a compatibilidade da atribuição, a covariância e a contravariância.

```vb
' Assignment compatibility.
Dim str As String = "test"
' An object of a more derived type is assigned to an object of a less derived type.
Dim obj As Object = str

' Covariance.
Dim strings As IEnumerable(Of String) = New List(Of String)()
' An object that is instantiated with a more derived type argument
' is assigned to an object instantiated with a less derived type argument.
' Assignment compatibility is preserved.
Dim objects As IEnumerable(Of Object) = strings

' Contravariance.
' Assume that there is the following method in the class:
' Shared Sub SetObject(ByVal o As Object)
' End Sub
Dim actObject As Action(Of Object) = AddressOf SetObject

' An object that is instantiated with a less derived type argument
' is assigned to an object instantiated with a more derived type argument.
' Assignment compatibility is reversed.
Dim actString As Action(Of String) = actObject
```

A covariância para matrizes permite a conversão implícita de uma matriz de um tipo mais derivado para uma matriz de um tipo menos derivado. Mas essa operação não é fortemente tipada, conforme mostrado no exemplo de código a seguir.

```vb
Dim array() As Object = New String(10) {}
' The following statement produces a run-time exception.
' array(0) = 10
```

O suporte de covariância e contravariância aos grupos de método permite a correspondência de assinaturas de método com tipos de delegados. Isso permite atribuir a delegados não apenas os métodos que têm correspondência de assinaturas, mas também métodos que retornam tipos mais derivados (covariância) ou que aceitam parâmetros que têm tipos menos derivados (contravariância) do que o especificado pelo tipo delegado. Para obter mais informações, consulte [Variação em delegados (Visual Basic)](variance-in-delegates.md) e [Usando variação em delegados (Visual Basic)](using-variance-in-delegates.md).

O exemplo de código a seguir mostra o suporte da covariância e da contravariância para grupos de método.

```vb
Shared Function GetObject() As Object
    Return Nothing
End Function

Shared Sub SetObject(ByVal obj As Object)
End Sub

Shared Function GetString() As String
    Return ""
End Function

Shared Sub SetString(ByVal str As String)

End Sub

Shared Sub Test()
    ' Covariance. A delegate specifies a return type as object,
    ' but you can assign a method that returns a string.
    Dim del As Func(Of Object) = AddressOf GetString

    ' Contravariance. A delegate specifies a parameter type as string,
    ' but you can assign a method that takes an object.
    Dim del2 As Action(Of String) = AddressOf SetObject
End Sub
```

No .NET Framework 4 ou posterior, Visual Basic dá suporte a covariância e contravariância em interfaces e delegados genéricos e permite a conversão implícita de parâmetros de tipo genérico. Para obter mais informações, consulte [Variação em interfaces genéricas (Visual Basic)](variance-in-generic-interfaces.md) e [Variação em delegados (Visual Basic)](variance-in-delegates.md).

O exemplo de código a seguir mostra a conversão de referência implícita para interfaces genéricas.

```vb
Dim strings As IEnumerable(Of String) = New List(Of String)
Dim objects As IEnumerable(Of Object) = strings
```

Uma interface ou delegado genérico será chamado *variante* se seus parâmetros genéricos forem declarados covariantes ou contravariantes. O Visual Basic permite que você crie suas próprias interfaces variantes e delegados. Para obter mais informações, consulte [Criando interfaces genéricas variantes (Visual Basic)](creating-variant-generic-interfaces.md) e [Variação em delegados (Visual Basic)](variance-in-delegates.md).

## <a name="related-topics"></a>Tópicos relacionados

|Title|Descrição|
|-----------|-----------------|
|[Variação em interfaces genéricas (Visual Basic)](variance-in-generic-interfaces.md)|Discute a covariância e a contravariância em interfaces genéricas e fornece uma lista de interfaces genéricas variáveis no .NET Framework.|
|[Criando interfaces genéricas variantes (Visual Basic)](creating-variant-generic-interfaces.md)|Mostra como criar interfaces variantes personalizadas.|
|[Usando variação em interfaces para coleções genéricas (Visual Basic)](using-variance-in-interfaces-for-generic-collections.md)|Mostra como o suporte de covariância e contravariância nas interfaces <xref:System.Collections.Generic.IEnumerable%601> e <xref:System.IComparable%601> pode ajudar na reutilização do código.|
|[Variação em delegados (Visual Basic)](variance-in-delegates.md)|Discute a covariância e a contravariância em interfaces genéricas e não genéricas e fornece uma lista de interfaces genéricas variáveis no .NET Framework.|
|[Usando variação em delegados (Visual Basic)](using-variance-in-delegates.md)|Mostra como usar o suporte de covariância e contravariância em delegados não genéricos para corresponder às assinaturas de método com tipos delegados.|
|[Usando variação para delegados genéricos Func e Action (Visual Basic)](using-variance-for-func-and-action-generic-delegates.md)|Mostra como o suporte de covariância e contravariância nos delegados `Func` e `Action` pode ajudar na reutilização do código.|
