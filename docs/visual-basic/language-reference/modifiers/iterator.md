---
title: Iterador
ms.date: 07/20/2015
f1_keywords:
- vb.Iterator
helpviewer_keywords:
- Iterator keyword [Visual Basic]
ms.assetid: 69cb0b04-ac87-49d0-bcfe-810c0d60daff
ms.openlocfilehash: bb19289c69f4c523363e88e91a58f37d232b07df
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396227"
---
# <a name="iterator-visual-basic"></a>Iterador (Visual Basic)
Especifica que uma função ou `Get` acessador é um iterador.  
  
## <a name="remarks"></a>Comentários  
 Um *iterador* executa uma iteração personalizada em uma coleção. Um iterador usa a instrução [yield](../statements/yield-statement.md) para retornar cada elemento da coleção um de cada vez. Quando uma `Yield` instrução é alcançada, o local atual no código é retido. A execução será reiniciada desse local na próxima vez que a função iteradora for chamada.  
  
 Um iterador pode ser implementado como uma função ou como um `Get` acessador de uma definição de propriedade. O `Iterator` modificador é exibido na declaração da função ou `Get` acessador de iterador.  
  
 Você chama um iterador do código do cliente usando um [para cada... Próxima instrução](../statements/for-each-next-statement.md).  
  
 O tipo de retorno de uma função ou `Get` acessador de iterador pode ser <xref:System.Collections.IEnumerable> , <xref:System.Collections.Generic.IEnumerable%601> , <xref:System.Collections.IEnumerator> ou <xref:System.Collections.Generic.IEnumerator%601> .  
  
 Um iterador não pode ter nenhum `ByRef` parâmetro.  
  
 Um iterador não pode ocorrer em um evento, um construtor de instância, um construtor estático ou um destruidor estático.  
  
 Um iterador pode ser uma função anônima. Para obter mais informações, consulte [Iteradores](../../programming-guide/concepts/iterators.md).  
  
## <a name="usage"></a>Uso  
 O `Iterator` modificador pode ser usado nesses contextos:  
  
- [Instrução Function](../statements/function-statement.md)  
  
- [Instrução Property](../statements/property-statement.md)  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra uma função de iterador. A função Iterator tem uma `Yield` instrução que está dentro de um [para... Próximo](../statements/for-next-statement.md) loop. Cada iteração do corpo [de cada](../statements/for-each-next-statement.md) instrução no `Main` cria uma chamada para a `Power` função de iterador. Cada chamada à função iteradora prossegue para a próxima execução da instrução `Yield` que ocorre durante a próxima iteração do loop `For…Next`.  
  
 [!code-vb[VbVbalrStatements#98](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#98)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra um acessador `Get` que é um iterador. O `Iterator` modificador está na declaração de propriedade.  
  
 [!code-vb[VbVbalrStatements#99](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#99)]  
  
 Para obter exemplos adicionais, consulte [iteradores](../../programming-guide/concepts/iterators.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Runtime.CompilerServices.IteratorStateMachineAttribute>
- [Iterators](../../programming-guide/concepts/iterators.md)
- [Instrução Yield](../statements/yield-statement.md)
