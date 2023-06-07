---
title: Acessando atributos usando reflexão
ms.date: 07/20/2015
ms.assetid: c56e41da-5433-464f-a7bf-2a722e78bc9f
ms.openlocfilehash: c0da5c4ae00eb2bc80b10f63f4bfd39763445e55
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400752"
---
# <a name="accessing-attributes-by-using-reflection-visual-basic"></a>Acessando atributos usando reflexão (Visual Basic)

O fato de que você pode definir atributos personalizados e colocá-los em seu código-fonte seria de pouco valor sem alguma maneira de recuperar essas informações e tomar ação sobre elas. Por meio de reflexão, você pode recuperar as informações que foram definidas com atributos personalizados. O método principal é o `GetCustomAttributes`, que retorna uma matriz de objetos que são equivalentes, em tempo de execução, aos atributos do código-fonte. Esse método tem várias versões sobrecarregadas. Para obter mais informações, consulte <xref:System.Attribute>.

Uma especificação de atributo, como:

```vb
<Author("P. Ackerman", Version:=1.1)>
Class SampleClass
    ' P. Ackerman's code goes here...
End Class
```

 é conceitualmente equivalente a esta:

```vb
Dim anonymousAuthorObject As Author = New Author("P. Ackerman")
anonymousAuthorObject.version = 1.1
```

No entanto, o código não será executado até que `SampleClass` tenha os atributos consultados. Chamar `GetCustomAttributes` na `SampleClass` faz com que um objeto `Author` seja criado e inicializado como acima. Se a classe tiver outros atributos, outros objetos de atributo serão construídos de forma semelhante. Então o `GetCustomAttributes` retornará o objeto `Author` e quaisquer outros objetos de atributo em uma matriz. Você poderá iterar sobre essa matriz, determinar quais atributos foram aplicados com base no tipo de cada elemento da matriz e extrair informações dos objetos de atributo.

## <a name="example"></a>Exemplo

Aqui está um exemplo completo. Um atributo personalizado é definido, aplicado a várias entidades e recuperado por meio da reflexão.

```vb
' Multiuse attribute
<System.AttributeUsage(System.AttributeTargets.Class Or
                       System.AttributeTargets.Struct,
                       AllowMultiple:=True)>
Public Class Author
    Inherits System.Attribute
    Private name As String
    Public version As Double
    Sub New(ByVal authorName As String)
        name = authorName

        ' Default value
        version = 1.0
    End Sub

    Function GetName() As String
        Return name
    End Function
End Class

' Class with the Author attribute
<Author("P. Ackerman")>
Public Class FirstClass
End Class

' Class without the Author attribute
Public Class SecondClass
End Class

' Class with multiple Author attributes.
<Author("P. Ackerman"), Author("R. Koch", Version:=2.0)>
Public Class ThirdClass
End Class

Class TestAuthorAttribute
    Sub Main()
        PrintAuthorInfo(GetType(FirstClass))
        PrintAuthorInfo(GetType(SecondClass))
        PrintAuthorInfo(GetType(ThirdClass))
    End Sub

    Private Shared Sub PrintAuthorInfo(ByVal t As System.Type)
        System.Console.WriteLine("Author information for {0}", t)

        ' Using reflection
        Dim attrs() As System.Attribute = System.Attribute.GetCustomAttributes(t)

        ' Displaying output
        For Each attr In attrs
            Dim a As Author = CType(attr, Author)
            System.Console.WriteLine("   {0}, version {1:f}", a.GetName(), a.version)
        Next
    End Sub

    ' Output:
    '   Author information for FirstClass
    '     P. Ackerman, version 1.00
    ' Author information for SecondClass
    ' Author information for ThirdClass
    '  R. Koch, version 2.00
    '  P. Ackerman, version 1.00

End Class
```

## <a name="see-also"></a>Confira também

- <xref:System.Reflection>
- <xref:System.Attribute>
- [Guia de programação do Visual Basic](../../index.md)
- [Recuperando informações armazenadas em atributos](../../../../standard/attributes/retrieving-information-stored-in-attributes.md)
- [Reflexão (Visual Basic)](../reflection.md)
- [Atributos (Visual Basic)](../../../language-reference/attributes.md)
- [Criando atributos personalizados (Visual Basic)](creating-custom-attributes.md)
