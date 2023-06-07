---
title: Instrução Yield
ms.date: 07/20/2015
f1_keywords:
- vb.Yield
helpviewer_keywords:
- iterators, Yield statement [Visual Basic]
- iterators [Visual Basic]
- Yield statement [Visual Basic]
ms.assetid: f33126c5-d7c4-43e2-8e36-4ae3f0703d97
ms.openlocfilehash: cc89e6f9bc2ccb4fff9a9fe12cd190a6b2d212dc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401362"
---
# <a name="yield-statement-visual-basic"></a>Instrução Yield (Visual Basic)
Envia o próximo elemento de uma coleção para uma `For Each...Next` instrução.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Yield expression  
```  
  
## <a name="parameters"></a>Parâmetros  
  
|Termo|Definição|  
|---|---|  
|`expression`|Obrigatórios. Uma expressão que é conversível implicitamente no tipo da função de iterador ou `Get` acessador que contém a `Yield` instrução.|  
  
## <a name="remarks"></a>Comentários  
 A `Yield` instrução retorna um elemento de uma coleção por vez. A `Yield` instrução é incluída em uma função ou `Get` acessador de iterador, que executa iterações personalizadas em uma coleção.  
  
 Você consome uma função de iterador usando um [para cada... Próxima instrução](for-each-next-statement.md) ou uma consulta LINQ. Cada iteração do `For Each` loop chama a função Iterator. Quando uma `Yield` instrução é alcançada na função de iterador, `expression` é retornada e o local atual no código é retido. A execução será reiniciada desse local na próxima vez que a função iteradora for chamada.  
  
 Uma conversão implícita deve existir do tipo de `expression` na `Yield` instrução para o tipo de retorno do iterador.  
  
 Você pode usar uma `Exit Function` `Return` instrução or para encerrar a iteração.  
  
 "Yield" não é uma palavra reservada e tem um significado especial apenas quando ela é usada em uma `Iterator` função ou `Get` acessador.  
  
 Para obter mais informações sobre funções e `Get` acessadores iteradores, consulte [iteradores](../../programming-guide/concepts/iterators.md).  
  
## <a name="iterator-functions-and-get-accessors"></a>Funções de iterador e get acessadores  
 A declaração de uma função de iterador ou `Get` acessador deve atender aos seguintes requisitos:  
  
- Ele deve incluir um modificador de [iterador](../modifiers/iterator.md) .  
  
- O tipo de retorno deve ser <xref:System.Collections.IEnumerable>, <xref:System.Collections.Generic.IEnumerable%601>, <xref:System.Collections.IEnumerator> ou <xref:System.Collections.Generic.IEnumerator%601>.  
  
- Ele não pode ter nenhum `ByRef` parâmetro.  
  
 Uma função de iterador não pode ocorrer em um evento, Construtor de instância, construtor estático ou destruidor estático.  
  
 Uma função de iterador pode ser uma função anônima. Para obter mais informações, consulte [Iteradores](../../programming-guide/concepts/iterators.md).  
  
## <a name="exception-handling"></a>Tratamento de exceção  
 Uma `Yield` instrução pode estar dentro de um `Try` bloco de um [try... Capturar... Instrução Finally](try-catch-finally-statement.md). Um `Try` bloco que tem uma `Yield` instrução pode ter `Catch` blocos e pode ter um `Finally` bloco.  
  
 Uma `Yield` instrução não pode estar dentro de um `Catch` bloco ou `Finally` bloco.  
  
 Se o `For Each` corpo (fora da função de iterador) gerar uma exceção, um `Catch` bloco na função de iterador não será executado, mas um `Finally` bloco na função de iterador será executado. Um `Catch` bloco dentro de uma função de iterador captura apenas as exceções que ocorrem dentro da função de iterador.  
  
## <a name="technical-implementation"></a>Implementação Técnica  
 O código a seguir retorna um `IEnumerable (Of String)` de uma função de iterador e, em seguida, itera pelos elementos do `IEnumerable (Of String)` .  
  
```vb  
Dim elements As IEnumerable(Of String) = MyIteratorFunction()  
    …  
For Each element As String In elements  
Next  
```  
  
 A chamada para `MyIteratorFunction` não executa o corpo da função. Em vez disso, a chamada retorna `IEnumerable(Of String)` na variável `elements`.  
  
 Em uma iteração do loop `For Each`, o método <xref:System.Collections.IEnumerator.MoveNext%2A> é chamado para `elements`. Essa chamada executará o corpo de `MyIteratorFunction` até que a próxima instrução `Yield` seja atingida. A `Yield` instrução retorna uma expressão que determina não apenas o valor da `element` variável para consumo pelo corpo do loop, mas também a <xref:System.Collections.Generic.IEnumerator%601.Current%2A> propriedade dos elementos, que é um `IEnumerable (Of String)` .  
  
 Em cada iteração subsequente do loop `For Each`, a execução do corpo do iterador continuará de onde parou, parando novamente quando atingir uma instrução `Yield`. O `For Each` loop é concluído quando o final da função de iterador ou `Return` uma `Exit Function` instrução or é atingido.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir tem uma `Yield` instrução que está dentro de um [para... Próximo](for-next-statement.md) loop. Cada iteração do corpo [de cada](for-each-next-statement.md) instrução no `Main` cria uma chamada para a `Power` função de iterador. Cada chamada à função iteradora prossegue para a próxima execução da instrução `Yield` que ocorre durante a próxima iteração do loop `For…Next`.  
  
 O tipo de retorno do método iterador é <xref:System.Collections.Generic.IEnumerable%601> , um tipo de interface do iterador. Quando o método iterador é chamado, ele retorna um objeto enumerável que contém as potências de um número.  
  
 [!code-vb[VbVbalrStatements#98](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#98)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra um acessador `Get` que é um iterador. A declaração de propriedade inclui um `Iterator` modificador.  
  
 [!code-vb[VbVbalrStatements#99](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class2.vb#99)]  
  
 Para obter exemplos adicionais, consulte [iteradores](../../programming-guide/concepts/iterators.md).  
  
## <a name="see-also"></a>Confira também

- [Instruções](index.md)
