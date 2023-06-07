---
title: Operador =
ms.date: 07/20/2015
f1_keywords:
- vb.Assign
- vb.=
helpviewer_keywords:
- = operator [Visual Basic]
- = assignment statements [Visual Basic]
ms.assetid: 2dac2e49-86c8-42f8-80c1-458452fb5e29
ms.openlocfilehash: 516cb21e02d9fb2cd4b8d72282bb74163e1fb14b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371759"
---
# <a name="-operator-visual-basic"></a>Operador = (Visual Basic)
Atribui um valor a uma variável ou propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
variableorproperty = value  
```  
  
## <a name="parts"></a>Partes  
 `variableorproperty`  
 Qualquer variável gravável ou qualquer propriedade.  
  
 `value`  
 Qualquer literal, constante ou expressão.  
  
## <a name="remarks"></a>Comentários  
 O elemento no lado esquerdo do sinal de igual ( `=` ) pode ser uma variável escalar simples, uma propriedade ou um elemento de uma matriz. A variável ou a propriedade não pode ser [ReadOnly](../modifiers/readonly.md). O `=` operador atribui o valor à sua direita à variável ou à propriedade à esquerda.  
  
> [!NOTE]
> O `=` operador também é usado como um operador de comparação. Para obter detalhes, consulte [operadores de comparação](comparison-operators.md).  
  
## <a name="overloading"></a>Sobrecarga  
 O `=` operador só pode ser sobrecarregado como um operador de comparação relacional, não como um operador de atribuição. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra o operador de atribuição. O valor à direita é atribuído à variável à esquerda.  
  
 [!code-vb[VbVbalrOperators#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>Confira também

- [Operador&=](and-assignment-operator.md)
- [Operador * =](multiplication-assignment-operator.md)
- [Operador + =](addition-assignment-operator.md)
- [Operador -= (Visual Basic)](subtraction-assignment-operator.md)
- [Operador/= (Visual Basic)](floating-point-division-assignment-operator.md)
- [\\= Operador](integer-division-assignment-operator.md)
- [Operador ^ =](exponentiation-assignment-operator.md)
- [Instruções](../../programming-guide/language-features/statements.md)
- [Operadores de comparação](comparison-operators.md)
- [Leitura](../modifiers/readonly.md)
- [Inferência de Tipo de Variável Local](../../programming-guide/language-features/variables/local-type-inference.md)
