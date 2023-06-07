---
title: 'Como: criar uma União C-C + + usando atributos'
ms.date: 07/20/2015
ms.assetid: 9352a7e4-c0da-4d07-aa14-55ed43736fcb
ms.openlocfilehash: ebab0ad947f776932f9379af3969e369eeec1941
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400675"
---
# <a name="how-to-create-a-cc-union-by-using-attributes-visual-basic"></a>Como criar uma união do C/C++ usando atributos (Visual Basic)

Usando atributos, você pode personalizar como structs são dispostos na memória. Por exemplo, você pode criar o que é conhecido como uma união no C/C++ usando os atributos `StructLayout(LayoutKind.Explicit)` e `FieldOffset`.

## <a name="example"></a>Exemplo

Neste segmento de código, todos os campos de `TestUnion` são iniciados no mesmo local na memória.

```vb
' Add an Imports statement for System.Runtime.InteropServices.

<System.Runtime.InteropServices.StructLayout(
      System.Runtime.InteropServices.LayoutKind.Explicit)>
Structure TestUnion
    <System.Runtime.InteropServices.FieldOffset(0)>
    Public i As Integer

    <System.Runtime.InteropServices.FieldOffset(0)>
    Public d As Double

    <System.Runtime.InteropServices.FieldOffset(0)>
    Public c As Char

    <System.Runtime.InteropServices.FieldOffset(0)>
    Public b As Byte
End Structure
```

## <a name="example"></a>Exemplo

A seguir, temos outro exemplo em que os campos são iniciados em locais diferentes definidos explicitamente.

```vb
' Add an Imports statement for System.Runtime.InteropServices.

 <System.Runtime.InteropServices.StructLayout(
      System.Runtime.InteropServices.LayoutKind.Explicit)>
Structure TestExplicit
     <System.Runtime.InteropServices.FieldOffset(0)>
     Public lg As Long

     <System.Runtime.InteropServices.FieldOffset(0)>
     Public i1 As Integer

     <System.Runtime.InteropServices.FieldOffset(4)>
     Public i2 As Integer

     <System.Runtime.InteropServices.FieldOffset(8)>
     Public d As Double

     <System.Runtime.InteropServices.FieldOffset(12)>
     Public c As Char

     <System.Runtime.InteropServices.FieldOffset(14)>
     Public b As Byte
 End Structure
```

Os dois campos inteiros, `i1` e `i2`, compartilham os mesmos locais de memória que `lg`. Esse tipo de controle sobre o layout do struct é útil ao usar a invocação de plataforma.

## <a name="see-also"></a>Confira também

- <xref:System.Reflection>
- <xref:System.Attribute>
- [Guia de programação do Visual Basic](../../index.md)
- [Atributos](../../../../standard/attributes/index.md)
- [Reflexão (Visual Basic)](../reflection.md)
- [Atributos (Visual Basic)](../../../language-reference/attributes.md)
- [Criando atributos personalizados (Visual Basic)](creating-custom-attributes.md)
- [Acessando atributos usando reflexão (Visual Basic)](accessing-attributes-by-using-reflection.md)
