---
title: O operando 'IsNot' do tipo 'typename' só pode ser comparado a 'Nothing', porque 'typename' é um tipo que permite valor nulo
ms.date: 07/20/2015
f1_keywords:
- bc32128
- vbc32128
helpviewer_keywords:
- BC32128
ms.assetid: 1155b23a-ad75-4bab-b9da-73f35c767a36
ms.openlocfilehash: fb61b04021bd844fade94413b4f3b28b82f6411b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402792"
---
# <a name="isnot-operand-of-type-typename-can-only-be-compared-to-nothing-because-typename-is-a-nullable-type"></a>O operando 'IsNot' do tipo 'typename' só pode ser comparado a 'Nothing', porque 'typename' é um tipo que permite valor nulo

Uma variável declarada como um tipo de valor anulável foi comparada a uma expressão diferente de `Nothing` usar o `IsNot` operador.

**ID do erro:** BC32128

## <a name="to-correct-this-error"></a>Para corrigir este erro

Para comparar um tipo anulável a uma expressão que não seja `Nothing` usando o `IsNot` operador, chame o `GetType` método no tipo anulável e compare o resultado com a expressão, conforme mostrado no exemplo a seguir.

```vb
Dim number? As Integer = 5

If number IsNot Nothing Then
  If number.GetType() IsNot Type.GetType("System.Int32") Then

  End If
End If
```

## <a name="see-also"></a>Confira também

- [Tipos de valor anulável](../../programming-guide/language-features/data-types/nullable-value-types.md)
- [Operador IsNot](../operators/isnot-operator.md)
