---
title: Instrução Continue
ms.date: 07/20/2015
f1_keywords:
- vb.continue
helpviewer_keywords:
- Continue statement [Visual Basic]
- loops, transferring to next iteration
ms.assetid: 3ad00103-358b-4af3-a3a8-1b9ea0e995d3
ms.openlocfilehash: fd604b281a590073a5e76398788d7648cadd145c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84382088"
---
# <a name="continue-statement-visual-basic"></a>Instrução Continue (Visual Basic)
Transfere o controle imediatamente para a próxima iteração de um loop.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Continue { Do | For | While }  
```  
  
## <a name="remarks"></a>Comentários  
 Você pode transferir de dentro de `Do` um `For` loop, ou `While` para a próxima iteração desse loop. O controle passa imediatamente para o teste de condição de loop, que é equivalente a transferir para a `For` `While` instrução or, ou para a `Do` `Loop` instrução or que contém a `Until` `While` cláusula or.  
  
 Você pode usar `Continue` em qualquer local no loop que permita transferências. As regras que permitem a transferência de controle são as mesmas que a [instrução goto](goto-statement.md).  
  
 Por exemplo, se um loop estiver totalmente contido em um `Try` bloco, um `Catch` bloco ou um `Finally` bloco, você poderá usar `Continue` para transferir para fora do loop. Se, por outro lado, a `Try` estrutura... `End Try` estiver contida no loop, você não poderá usar o `Continue` para transferir o controle para fora do `Finally` bloco e poderá usá-lo para transferir para fora de `Try` um `Catch` bloco ou somente se você transferir completamente da `Try` estrutura... `End Try` .  
  
 Se você tiver loops aninhados do mesmo tipo, por exemplo um `Do` loop dentro de outro `Do` loop, uma `Continue Do` instrução passará para a próxima iteração do `Do` loop mais interno que o contém. Você não pode usar `Continue` para pular para a próxima iteração de um loop de contenção do mesmo tipo.  
  
 Se você tiver loops aninhados de tipos diferentes, por exemplo um `Do` loop dentro de um `For` loop, poderá pular para a próxima iteração de qualquer um dos loops usando o `Continue Do` ou o `Continue For` .  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir usa a `Continue While` instrução para pular para a próxima coluna de uma matriz se um divisor for zero. O `Continue While` está dentro de um `For` loop. Ele transfere para a `While col < lastcol` instrução, que é a próxima iteração do `While` loop mais interno que contém o `For` loop.  
  
 [!code-vb[VbVbalrStatements#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#14)]  
  
## <a name="see-also"></a>Confira também

- [Instrução Do...Loop](do-loop-statement.md)
- [Instrução For...Next](for-next-statement.md)
- [Instrução While...End While](while-end-while-statement.md)
- [Instrução Try...Catch...Finally](try-catch-finally-statement.md)
