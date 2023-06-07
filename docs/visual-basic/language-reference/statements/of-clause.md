---
title: Cláusula Of
ms.date: 07/20/2015
f1_keywords:
- Of
- vb.Of
- vb.of
helpviewer_keywords:
- Of keyword [Visual Basic]
- arguments [Visual Basic], data types
- constraints, Visual Basic generic types
- generic parameters
- generics [Visual Basic], constraints
- parameters [Visual Basic], type
- types [Visual Basic], generic
- parameters [Visual Basic], generic
- type parameters
- data type arguments
ms.assetid: 0db8f65c-65af-4089-ab7f-6fcfecb60444
ms.openlocfilehash: 8497f46453d586fb94e1f7c82c81c6b923dd6f60
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404415"
---
# <a name="of-clause-visual-basic"></a>Cláusula Of (Visual Basic)
Apresenta uma `Of` cláusula, que identifica um *parâmetro de tipo* em uma classe *genérica* , estrutura, interface, delegado ou procedimento. Para obter informações sobre tipos genéricos, consulte [tipos genéricos em Visual Basic](../../programming-guide/language-features/data-types/generic-types.md).  
  
## <a name="using-the-of-keyword"></a>Usando a palavra-chave of  
 O exemplo de código a seguir usa a `Of` palavra-chave para definir o contorno de uma classe que usa dois parâmetros de tipo. Ele *restringe* o `keyType` parâmetro pela <xref:System.IComparable> interface, o que significa que o código de consumo deve fornecer um argumento de tipo que implementa <xref:System.IComparable> . Isso é necessário para que o `add` procedimento possa chamar o <xref:System.IComparable.CompareTo%2A?displayProperty=nameWithType> método. Para obter mais informações sobre restrições, consulte [lista de tipos](type-list.md).  
  
```vb  
Public Class Dictionary(Of entryType, keyType As IComparable)  
    Public Sub add(ByVal e As entryType, ByVal k As keyType)  
        Dim dk As keyType  
        If k.CompareTo(dk) = 0 Then  
        End If  
    End Sub  
    Public Function find(ByVal k As keyType) As entryType  
    End Function  
End Class  
```  
  
 Se você concluir a definição de classe anterior, poderá construir uma variedade de `dictionary` classes a partir dela. Os tipos que você fornece `entryType` e `keyType` determinam que tipo de entrada a classe contém e que tipo de chave ela associa a cada entrada. Devido à restrição, você deve fornecer a `keyType` um tipo que implementa <xref:System.IComparable> .  
  
 O exemplo de código a seguir cria um objeto que contém `String` entradas e associa uma `Integer` chave a cada uma delas. `Integer`implementa <xref:System.IComparable> e, portanto, satisfaz a restrição em `keyType` .  
  
```vb  
Dim d As New dictionary(Of String, Integer)  
```  
  
 A `Of` palavra-chave pode ser usada nesses contextos:  
  
 [Instrução Class](class-statement.md)  
  
 [Instrução Delegate](delegate-statement.md)  
  
 [Instrução Function](function-statement.md)  
  
 [Instrução Interface](interface-statement.md)  
  
 [Instrução Structure](structure-statement.md)  
  
 [Instrução Sub](sub-statement.md)  
  
## <a name="see-also"></a>Confira também

- <xref:System.IComparable>
- [Lista de Tipos](type-list.md)
- [Tipos genéricos no Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
- [Em](../modifiers/in-generic-modifier.md)
- [Fora](../modifiers/out-generic-modifier.md)
