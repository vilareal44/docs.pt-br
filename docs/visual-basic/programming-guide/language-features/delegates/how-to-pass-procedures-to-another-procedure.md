---
title: Como passar procedimentos para outro procedimento
ms.date: 07/20/2015
helpviewer_keywords:
- AddressOf operator [Visual Basic]
- delegates [Visual Basic], passing procedures
ms.assetid: 5adbba15-5a1d-413f-ab3e-3ff6cc0a4669
ms.openlocfilehash: 36f623068372614ae034a8a7b31bffb7496f98b1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410689"
---
# <a name="how-to-pass-procedures-to-another-procedure-in-visual-basic"></a>Como passar procedimentos para outro procedimento no Visual Basic
Este exemplo mostra como usar delegados para passar um procedimento para outro procedimento.  
  
 Um delegado é um tipo que você pode usar como qualquer outro tipo em Visual Basic. O `AddressOf` operador retorna um objeto delegado quando aplicado a um nome de procedimento.  
  
 Este exemplo tem um procedimento com um parâmetro delegado que pode usar uma referência a outro procedimento, obtido com o `AddressOf` operador.  
  
### <a name="create-the-delegate-and-matching-procedures"></a>Criar o delegado e os procedimentos correspondentes  
  
1. Crie um delegado chamado `MathOperator` .  
  
     [!code-vb[VbVbalrDelegates#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#1)]  
  
2. Crie um procedimento chamado `AddNumbers` com parâmetros e valor de retorno que correspondam aos de `MathOperator` , para que as assinaturas correspondam.  
  
     [!code-vb[VbVbalrDelegates#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#2)]  
  
3. Crie um procedimento chamado `SubtractNumbers` com uma assinatura que corresponda a `MathOperator` .  
  
     [!code-vb[VbVbalrDelegates#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#3)]  
  
4. Crie um procedimento chamado `DelegateTest` que usa um delegado como parâmetro.  
  
     Esse procedimento pode aceitar uma referência a `AddNumbers` ou `SubtractNumbers` , porque suas assinaturas correspondem à `MathOperator` assinatura.  
  
     [!code-vb[VbVbalrDelegates#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#4)]  
  
5. Crie um procedimento chamado `Test` que chame `DelegateTest` uma vez com o delegado para `AddNumbers` como um parâmetro e novamente com o delegado para `SubtractNumbers` como um parâmetro.  
  
     [!code-vb[VbVbalrDelegates#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#5)]  
  
     Quando `Test` é chamado, ele primeiro exibe o resultado de `AddNumbers` atuar em `5` e `3` , que é 8. Em seguida, o resultado de `SubtractNumbers` atuar em `9` e `3` é exibido, que é 6.  
  
## <a name="see-also"></a>Confira também

- [Delegados](index.md)
- [Operador AddressOf](../../../language-reference/operators/addressof-operator.md)
- [Instrução Delegate](../../../language-reference/statements/delegate-statement.md)
- [Como invocar um método delegado](how-to-invoke-a-delegate-method.md)
