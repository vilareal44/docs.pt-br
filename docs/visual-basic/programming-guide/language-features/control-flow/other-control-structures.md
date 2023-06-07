---
title: Outras estruturas de controle
ms.date: 07/20/2015
helpviewer_keywords:
- statements [Visual Basic], control flow
- control structures [Visual Basic]
ms.assetid: 24b811f7-98ba-40ec-8dd3-4d528cfa4574
ms.openlocfilehash: c6d80b237c7c3512c2904547842fdeb3c69bef68
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402012"
---
# <a name="other-control-structures-visual-basic"></a>Outras estruturas de controle (Visual Basic)
Visual Basic fornece estruturas de controle que ajudam você a descartar um recurso ou reduzir o número de vezes que você precisa repetir uma referência de objeto.  
  
## <a name="usingend-using-construction"></a>Usando... Terminar usando construção  
 A `Using...End Using` construção estabelece um bloco de instruções no qual você faz uso de um recurso, como uma conexão SQL. Opcionalmente, você pode adquirir o recurso com a `Using` instrução. Quando você sai do `Using` bloco, Visual Basic descarta automaticamente o recurso para que ele esteja disponível para uso por outro código. O recurso deve ser local e descartável. Para obter mais informações, consulte [usando a instrução](../../../language-reference/statements/using-statement.md).  
  
## <a name="withend-with-construction"></a>Com... Terminar com construção  
 A `With...End With` construção permite especificar uma referência de objeto uma vez e, em seguida, executar uma série de instruções que acessam seus membros. Isso pode simplificar seu código e melhorar o desempenho porque Visual Basic não precisa restabelecer a referência para cada instrução que a acessa. Para obter mais informações, consulte [com... Instrução End With](../../../language-reference/statements/with-end-with-statement.md).  
  
## <a name="see-also"></a>Confira também

- [Fluxo de controle](index.md)
- [Estruturas de Decisão](decision-structures.md)
- [Estruturas de Loop](loop-structures.md)
- [Estruturas de Controle Aninhadas](nested-control-structures.md)
- [Instrução using](../../../language-reference/statements/using-statement.md)
- [Instrução With...End With](../../../language-reference/statements/with-end-with-statement.md)
