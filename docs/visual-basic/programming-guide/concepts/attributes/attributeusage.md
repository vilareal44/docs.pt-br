---
title: AttributeUsage
ms.date: 07/20/2015
ms.assetid: 48757216-c21d-4051-86d5-8a3e03c39d2c
ms.openlocfilehash: 677d49aba38801f2adf42cc745983af30b3eddc5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400726"
---
# <a name="attributeusage-visual-basic"></a>AttributeUsage (Visual Basic)

Determina como uma classe de atributo personalizado pode ser usada. `AttributeUsage` é um atributo que pode ser aplicado às definições de atributo personalizado para controlar como o novo atributo pode ser aplicado. As configurações padrão têm essa aparência quando aplicadas explicitamente:

```vb
<System.AttributeUsage(System.AttributeTargets.All,
                   AllowMultiple:=False,
                   Inherited:=True)>
Class NewAttribute
    Inherits System.Attribute
End Class
```

Neste exemplo, a classe `NewAttribute` pode ser aplicada a qualquer entidade de código capaz de receber atributo, mas pode ser aplicadas apenas uma vez para cada entidade. Ela é herdada por classes derivadas quando aplicada a uma classe base.

Os argumentos `AllowMultiple` e `Inherited` são opcionais, portanto, esse código tem o mesmo efeito:

```vb
<System.AttributeUsage(System.AttributeTargets.All)>
Class NewAttribute
    Inherits System.Attribute
End Class
```

O primeiro argumento `AttributeUsage` deve ser um ou mais elementos da enumeração <xref:System.AttributeTargets>. Vários tipos de destino podem ser vinculados junto com o operador OR, dessa maneira:

```vb
<AttributeUsage(AttributeTargets.Property Or AttributeTargets.Field)>
Class NewPropertyOrFieldAttribute
    Inherits Attribute
End Class
```

Se o argumento `AllowMultiple` for definido como `true`, o atributo resultante poderá ser aplicado mais de uma vez a uma única entidade, dessa maneira:

```vb
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=True)>
Class MultiUseAttr
    Inherits Attribute
End Class

<MultiUseAttr(), MultiUseAttr()>
Class Class1
End Class
```

Nesse caso `MultiUseAttr` pode ser aplicado repetidas vezes porque `AllowMultiple` está definido como `true`. Os dois formatos mostrados para a aplicação de vários atributos são válidos.

Se `Inherited` for definido como `false`, o atributo não será herdado por classes derivadas de uma classe que é atribuída. Por exemplo:

```vb
<AttributeUsage(AttributeTargets.Class, Inherited:=False)>
Class Attr1
    Inherits Attribute
End Class

<Attr1()>
Class BClass

End Class

Class DClass
    Inherits BClass
End Class
```

Nesse caso `Attr1` não é aplicado a `DClass` por meio de herança.

## <a name="remarks"></a>Comentários

O atributo `AttributeUsage` é um atributo de uso único. Ele não pode ser aplicado mais de uma vez para a mesma classe. `AttributeUsage` é um alias para <xref:System.AttributeUsageAttribute>.

Para obter mais informações, consulte [Acessando atributos usando reflexão (Visual Basic)](accessing-attributes-by-using-reflection.md).

## <a name="example"></a>Exemplo

O exemplo a seguir demonstra o efeito dos argumentos `Inherited` e `AllowMultiple` no atributo `AttributeUsage` e como os atributos personalizados aplicados a uma classe podem ser enumerados.

```vb
' Create some custom attributes:
<AttributeUsage(System.AttributeTargets.Class, Inherited:=False)>
Class A1
    Inherits System.Attribute
End Class

<AttributeUsage(System.AttributeTargets.Class)>
Class A2
    Inherits System.Attribute
End Class

<AttributeUsage(System.AttributeTargets.Class, AllowMultiple:=True)>
Class A3
    Inherits System.Attribute
End Class

' Apply custom attributes to classes:
<A1(), A2()>
Class BaseClass

End Class

<A3(), A3()>
Class DerivedClass
    Inherits BaseClass
End Class

Public Class TestAttributeUsage
    Sub Main()
        Dim b As New BaseClass
        Dim d As New DerivedClass
        ' Display custom attributes for each class.
        Console.WriteLine("Attributes on Base Class:")
        Dim attrs() As Object = b.GetType().GetCustomAttributes(True)

        For Each attr In attrs
            Console.WriteLine(attr)
        Next

        Console.WriteLine("Attributes on Derived Class:")
        attrs = d.GetType().GetCustomAttributes(True)
        For Each attr In attrs
            Console.WriteLine(attr)
        Next
    End Sub
End Class
```

## <a name="sample-output"></a>Saída de exemplo

```console
Attributes on Base Class:
A1
A2
Attributes on Derived Class:
A3
A3
A2
```

## <a name="see-also"></a>Confira também

- <xref:System.Attribute>
- <xref:System.Reflection>
- [Guia de programação do Visual Basic](../../index.md)
- [Atributos](../../../../standard/attributes/index.md)
- [Reflexão (Visual Basic)](../reflection.md)
- [Atributos (Visual Basic)](../../../language-reference/attributes.md)
- [Criando atributos personalizados (Visual Basic)](creating-custom-attributes.md)
- [Acessando atributos usando reflexão (Visual Basic)](accessing-attributes-by-using-reflection.md)
