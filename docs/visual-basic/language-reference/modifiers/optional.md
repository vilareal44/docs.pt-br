---
title: Opcional
ms.date: 07/20/2015
f1_keywords:
- vb.Optional
- vb.optional
helpviewer_keywords:
- Optional keyword [Visual Basic], contexts
- Optional keyword [Visual Basic]
ms.assetid: 4571ce88-a539-4115-b230-54eb277c6aa7
ms.openlocfilehash: c46d06dba61158d7362d736731161be306af3f10
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392139"
---
# <a name="optional-visual-basic"></a>Opcional (Visual Basic)

Especifica que um argumento de procedimento pode ser omitido quando o procedimento é chamado.

## <a name="remarks"></a>Comentários

Para cada parâmetro opcional, você deve especificar uma expressão constante como o valor padrão desse parâmetro. Se a expressão for avaliada como [Nothing](../nothing.md), o valor padrão do tipo de dados Value será usado como o valor padrão do parâmetro.

Se a lista de parâmetros contiver um parâmetro opcional, todos os parâmetros que o segue também deverão ser opcionais.

O `Optional` modificador pode ser usado nesses contextos:

- [Instrução Declare](../statements/declare-statement.md)

- [Instrução Function](../statements/function-statement.md)

- [Instrução Property](../statements/property-statement.md)

- [Instrução Sub](../statements/sub-statement.md)

> [!NOTE]
> Ao chamar um procedimento com ou sem parâmetros opcionais, você pode passar argumentos por posição ou por nome. Para obter mais informações, consulte [passando argumentos por posição e por nome](../../programming-guide/language-features/procedures/passing-arguments-by-position-and-by-name.md).

> [!NOTE]
> Você também pode definir um procedimento com parâmetros opcionais usando sobrecarga. Se você tiver um parâmetro opcional, poderá definir duas versões sobrecarregadas do procedimento, uma que aceite o parâmetro e outra que não. Para obter mais informações, consulte [sobrecarga de procedimento](../../programming-guide/language-features/procedures/procedure-overloading.md).

## <a name="example"></a>Exemplo

O exemplo a seguir define um procedimento que tem um parâmetro opcional.

```vb
Public Function FindMatches(ByRef values As List(Of String),
                            ByVal searchString As String,
                            Optional ByVal matchCase As Boolean = False) As List(Of String)

    Dim results As IEnumerable(Of String)

    If matchCase Then
        results = From v In values
                  Where v.Contains(searchString)
    Else
        results = From v In values
                  Where UCase(v).Contains(UCase(searchString))
    End If

    Return results.ToList()
End Function
```

## <a name="example"></a>Exemplo

O exemplo a seguir demonstra como chamar um procedimento com argumentos passados por position e com argumentos passados pelo nome. O procedimento tem dois parâmetros opcionais.

[!code-vb[VbVbalrKeywords#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/class8.vb#21)]

## <a name="see-also"></a>Confira também

- [Lista de parâmetros](../statements/parameter-list.md)
- [Parâmetros Opcionais](../../programming-guide/language-features/procedures/optional-parameters.md)
- [Palavras-chave](../keywords/index.md)
