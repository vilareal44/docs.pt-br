---
title: Instrução GoTo
ms.date: 07/20/2015
f1_keywords:
- vb.GoTo
helpviewer_keywords:
- GoTo statement [Visual Basic]
- control flow [Visual Basic], branching
- unconditional branching [Visual Basic]
- branching [Visual Basic]
- branching [Visual Basic], unconditional
- branching [Visual Basic], conditional
- conditional statements [Visual Basic], GoTo statement
- GoTo statement [Visual Basic], syntax
ms.assetid: 313274c2-8ab3-4b9c-9ba3-0fd6798e4f6d
ms.openlocfilehash: eb6f48d04b7d14591003e340464451da7df45cd6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404609"
---
# <a name="goto-statement"></a>Instrução GoTo
Ramifica incondicionalmente para uma linha especificada em um procedimento.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
GoTo line  
```  
  
## <a name="part"></a>Parte  
 `line`  
 Obrigatórios. Qualquer rótulo de linha.  
  
## <a name="remarks"></a>Comentários  
 A `GoTo` instrução pode ramificar somente para linhas no procedimento em que ele aparece. A linha deve ter um rótulo de linha que `GoTo` possa se referir. Para obter mais informações, consulte [instruções de rótulo](../../programming-guide/program-structure/how-to-label-statements.md).  
  
> [!NOTE]
> `GoTo`as instruções podem dificultar a leitura e a manutenção do código. Sempre que possível, use uma estrutura de controle em vez disso. Para obter mais informações, consulte [Control Flow](../../programming-guide/language-features/control-flow/index.md).  
  
 Você não pode usar uma `GoTo` instrução para ramificação de fora de um..., `For` `Next` `For Each` . `Next` `SyncLock` `End SyncLock` `Try` `Catch` ..,...,... ... `Finally` , `With` ... `End With` , ou `Using` ... `End Using` Construction para um rótulo dentro de.  
  
## <a name="branching-and-try-constructions"></a>Ramificação e experimente construções  
 Em um `Try` ... `Catch` ...`Finally` construção, as regras a seguir se aplicam à ramificação com a `GoTo` instrução.  
  
|Bloco ou região|Ramificando de fora|Ramificação de dentro|  
|---------------------|-------------------------------|-------------------------------|  
|`Try`impeça|Somente de um `Catch` bloco da mesma construção <sup>1</sup>|Somente para fora da construção inteira|  
|`Catch`impeça|Nunca permitido|Somente para fora da construção inteira ou para o `Try` bloco da mesma construção <sup>1</sup>|  
|`Finally`impeça|Nunca permitido|Nunca permitido|  
  
 <sup>1</sup> se houver `Try` . `Catch` .. ...`Finally` a construção é aninhada dentro de outra, um `Catch` bloco pode ramificar no `Try` bloco em seu próprio nível de aninhamento, mas não em nenhum outro `Try` bloco. Um aninhado `Try` ... `Catch` ...`Finally` a construção deve estar contida completamente em `Try` um `Catch` bloco ou da construção na qual está aninhada.  
  
 A ilustração a seguir mostra uma `Try` construção aninhada dentro de outra. Várias ramificações entre os blocos das duas construções são indicadas como válidas ou inválidas.  
  
 ![Diagrama gráfico de ramificação em construções try](./media/goto-statement/try-construction-branching.gif)  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a `GoTo` instrução para ramificar para rótulos de linha em um procedimento.  
  
 [!code-vb[VbVbalrStatements#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#31)]  
  
## <a name="see-also"></a>Confira também

- [Instrução Do...Loop](do-loop-statement.md)
- [Instrução For...Next](for-next-statement.md)
- [Instrução For Each...Next](for-each-next-statement.md)
- [Instrução If...Then...Else](if-then-else-statement.md)
- [Instrução Select...Case](select-case-statement.md)
- [Instrução Try...Catch...Finally](try-catch-finally-statement.md)
- [Instrução While...End While](while-end-while-statement.md)
- [Instrução With...End With](with-end-with-statement.md)
