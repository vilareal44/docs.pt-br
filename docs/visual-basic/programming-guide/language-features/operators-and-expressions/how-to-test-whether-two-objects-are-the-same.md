---
title: Como testar se dois objetos são iguais
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], reference
- Is operator [Visual Basic], comparing objects
- reference variables [Visual Basic]
- variables [Visual Basic], referring to same object
- objects [Visual Basic], variables referring to same
- Visual Basic code, operators
ms.assetid: f760e828-8704-4256-bc2d-c22a4c93b524
ms.openlocfilehash: b215225dbc2d8c0d2bdfe2206e5d4a4f1faa6d0c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403415"
---
# <a name="how-to-test-whether-two-objects-are-the-same-visual-basic"></a>Como testar se dois objetos são iguais (Visual Basic)
Se você tiver duas variáveis que se referem a objetos, poderá usar o `Is` operador OR ou `IsNot` ambos, para determinar se eles se referem à mesma instância.  
  
### <a name="to-test-whether-two-objects-are-the-same"></a>Para testar se dois objetos são iguais  
  
- Use o [operador is](../../../language-reference/operators/is-operator.md) ou o [operador IsNot](../../../language-reference/operators/isnot-operator.md) com as duas variáveis como operandos.  
  
     [!code-vb[VbVbalrOperators#69](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#69)]  
  
 Talvez você queira executar uma determinada ação dependendo se dois objetos se referem à mesma instância. O exemplo anterior compara `c` o controle com o controle ativo no formulário `f` . Se não houver nenhum controle ativo, ou se houver um, mas não for a mesma instância de controle que `c` o, a `If` instrução falhará e o procedimento retornará sem processamento adicional.  
  
 Quer você use `Is` ou `IsNot` seja uma questão de conveniência pessoal para você. Uma delas pode ser mais fácil de ler do que a outra em uma determinada expressão.  
  
## <a name="see-also"></a>Confira também

- [Operadores de comparação no Visual Basic](comparison-operators.md)
