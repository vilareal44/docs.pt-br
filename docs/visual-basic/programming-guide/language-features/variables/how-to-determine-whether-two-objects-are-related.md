---
title: Como determinar se dois objetos estão relacionados
ms.date: 07/20/2015
helpviewer_keywords:
- inheritance [Visual Basic], Visual Basic objects
- objects [Visual Basic], inheritance
- object variables [Visual Basic], determining relation
ms.assetid: da002e3f-6616-4bad-a229-f842d06652bb
ms.openlocfilehash: 30e88a21e737aa57513745899577381ed34151a2
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410458"
---
# <a name="how-to-determine-whether-two-objects-are-related-visual-basic"></a>Como determinar se dois objetos estão relacionados (Visual Basic)

Você pode comparar dois objetos para determinar a relação, se houver, entre as classes das quais elas são criadas. O <xref:System.Type.IsInstanceOfType%2A> método da <xref:System.Type?displayProperty=nameWithType> classe retorna `True` se a classe especificada herda da classe atual ou se o tipo atual é uma interface com suporte da classe especificada.

### <a name="to-determine-if-one-object-inherits-from-another-objects-class-or-interface"></a>Para determinar se um objeto herda da classe ou interface de outro objeto

1. No objeto que você acredita que pode ser do tipo base, invoque o <xref:System.Object.GetType%2A> método.

2. No <xref:System.Type?displayProperty=nameWithType> objeto retornado por <xref:System.Object.GetType%2A> , invoque o <xref:System.Type.IsInstanceOfType%2A> método.

3. Na lista de argumentos para <xref:System.Type.IsInstanceOfType%2A> , especifique o objeto que você acredita que pode ser do tipo derivado.

    <xref:System.Type.IsInstanceOfType%2A>retorna `True` se o tipo de argumento herda do <xref:System.Type?displayProperty=nameWithType> tipo de objeto.

## <a name="example"></a>Exemplo
 O exemplo a seguir determina se um objeto representa uma classe derivada da classe de outro objeto.

```vb
Public Class baseClass
End Class
Public Class derivedClass : Inherits baseClass
End Class
Public Class testTheseClasses
    Public Sub seeIfRelated()
        Dim baseObj As Object = New baseClass()
        Dim derivedObj As Object = New derivedClass()
        Dim related As Boolean
        related = baseObj.GetType().IsInstanceOfType(derivedObj)
        MsgBox(CStr(related))
    End Sub
End Class
```

Observe o posicionamento inesperado das duas variáveis de objeto na chamada para <xref:System.Type.IsInstanceOfType%2A> . O tipo base esperado é usado para gerar a <xref:System.Type?displayProperty=nameWithType> classe e o tipo derivado esperado é passado como um argumento para o <xref:System.Type.IsInstanceOfType%2A> método.

## <a name="see-also"></a>Confira também

- <xref:System.Object.GetType%2A>
- <xref:System.Type?displayProperty=nameWithType>
- <xref:System.Type.IsInstanceOfType%2A>
- [Tipo de dados Object](../../../language-reference/data-types/object-data-type.md)
- [Variáveis de Objeto](object-variables.md)
- [Valores de Variável de Objeto](object-variable-values.md)
- [Como determinar se dois objetos são idênticos](how-to-determine-whether-two-objects-are-identical.md)
