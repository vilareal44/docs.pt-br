---
title: O modificador 'Custom' não é válido em eventos declarados sem tipos delegados explícitos
ms.date: 07/20/2015
f1_keywords:
- vbc31122
- bc31122
helpviewer_keywords:
- BC31122
ms.assetid: 6911f0d1-641a-473b-906d-8ee5681194be
ms.openlocfilehash: 0c5a4188fedf9685afdd1cde4c1de93a0b43b919
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409771"
---
# <a name="custom-modifier-is-not-valid-on-events-declared-without-explicit-delegate-types"></a>O modificador 'Custom' não é válido em eventos declarados sem tipos delegados explícitos
Ao contrário de um evento não personalizado, uma `Custom Event` declaração requer uma `As` cláusula após o nome do evento que especifica explicitamente o tipo delegado para o evento.  
  
 Eventos não personalizados podem ser definidos com uma `As` cláusula e um tipo de delegado explícito, ou com uma lista de parâmetros imediatamente após o nome do evento.  
  
 **ID do erro:** BC31122  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Defina um delegado com a mesma lista de parâmetros que o evento personalizado.  
  
     Por exemplo, se o `Custom Event` foi definido por `Custom Event Test(ByVal sender As Object, ByVal i As Integer)` , o delegado correspondente seria o seguinte.  
  
     [!code-vb[VbVbalrEventError#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEventError/VB/VbVbalrEventError.vb#18)]  
  
2. Substitua a lista de parâmetros do evento personalizado por uma `As` cláusula especificando o tipo delegado.  
  
     Continuando com o exemplo, a `Custom Event` declaração seria reescrita da seguinte maneira.  
  
     [!code-vb[VbVbalrEventError#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEventError/VB/VbVbalrEventError.vb#19)]  
  
## <a name="example"></a>Exemplo  
 Este exemplo declara um `Custom Event` e especifica a cláusula necessária `As` com um tipo delegate.  
  
 [!code-vb[VbVbalrEventError#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEventError/VB/VbVbalrEventError.vb#2)]  
  
## <a name="see-also"></a>Confira também

- [Instrução Event](../statements/event-statement.md)
- [Instrução Delegate](../statements/delegate-statement.md)
- [Eventos](../../programming-guide/language-features/events/index.md)
