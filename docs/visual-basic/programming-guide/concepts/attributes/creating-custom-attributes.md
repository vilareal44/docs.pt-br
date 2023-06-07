---
title: Criando atributos personalizados
ms.date: 07/20/2015
ms.assetid: 5c9ef584-6c7c-496b-92a9-6e42f8d9ca28
ms.openlocfilehash: 84b400c2fa1b2d4019eec32092f954d680e64978
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400688"
---
# <a name="creating-custom-attributes-visual-basic"></a>Criando atributos personalizados (Visual Basic)

Você pode criar seus próprios atributos personalizados definindo uma classe de atributos, uma classe que deriva direta ou indiretamente de <xref:System.Attribute>, o que faz com que a identificação das definições de atributo nos metadados seja rápida e fácil. Suponha que você queira marcar tipos com o nome do programador que escreveu o tipo. Você pode definir uma classe de atributos `Author` personalizada:

```vb
<System.AttributeUsage(System.AttributeTargets.Class Or
                       System.AttributeTargets.Struct)>
Public Class Author
    Inherits System.Attribute
    Private name As String
    Public version As Double
    Sub New(ByVal authorName As String)
        name = authorName
        version = 1.0
    End Sub
End Class
```

O nome de classe será o nome do atributo, `Author`. Ela é derivada de `System.Attribute`, portanto, é uma classe de atributo personalizado. Os parâmetros do construtor são parâmetros posicionais do atributo personalizado. Neste exemplo, `name` é um parâmetro posicional. Quaisquer propriedades ou campos públicos de leitura/gravação são chamados de parâmetros. Nesse caso, `version` é o único parâmetro nomeado. Observe o uso do atributo `AttributeUsage` para tornar o atributo `Author` válido apenas na classe e nas declarações `Structure`.

Você pode usar esse novo atributo da seguinte maneira:

```vb
<Author("P. Ackerman", Version:=1.1)>
Class SampleClass
    ' P. Ackerman's code goes here...
End Class
```

`AttributeUsage` tem um parâmetro nomeado, `AllowMultiple`, com o qual você pode fazer um atributo personalizado de uso único ou mulituso. No exemplo de código a seguir, um atributo multiuso é criado.

```vb
' multiuse attribute
<System.AttributeUsage(System.AttributeTargets.Class Or
                       System.AttributeTargets.Struct,
                       AllowMultiple:=True)>
Public Class Author
    Inherits System.Attribute
```

No exemplo de código a seguir, vários atributos do mesmo tipo são aplicados a uma classe.

```vb
<Author("P. Ackerman", Version:=1.1),
Author("R. Koch", Version:=1.2)>
Class SampleClass
    ' P. Ackerman's code goes here...
    ' R. Koch's code goes here...
End Class
```

> [!NOTE]
> Se sua classe de atributos contém uma propriedade, essa propriedade deve ser de leitura/gravação.

## <a name="see-also"></a>Confira também

- <xref:System.Reflection>
- [Guia de programação do Visual Basic](../../index.md)
- [Escrevendo atributos personalizados](../../../../standard/attributes/writing-custom-attributes.md)
- [Reflexão (Visual Basic)](../reflection.md)
- [Atributos (Visual Basic)](../../../language-reference/attributes.md)
- [Acessando atributos usando reflexão (Visual Basic)](accessing-attributes-by-using-reflection.md)
- [AttributeUsage (Visual Basic)](attributeusage.md)
