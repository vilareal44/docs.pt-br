---
title: Instrução While...End While
ms.date: 07/20/2015
f1_keywords:
- vb.While
- vb.While...EndWhile
helpviewer_keywords:
- While statement [Visual Basic], While...End While
- While statement [Visual Basic]
- While...End While statements [Visual Basic]
ms.assetid: b931d1ce-e8ed-44d8-a13d-92a4f5458a1e
ms.openlocfilehash: d9eb8cb95d46e860aa127954d7b44e37991d4a13
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84391580"
---
# <a name="whileend-while-statement-visual-basic"></a>Instrução While...End While (Visual Basic)
Executa uma série de instruções desde que uma determinada condição seja `True` .  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
While condition  
    [ statements ]  
    [ Continue While ]  
    [ statements ]  
    [ Exit While ]  
    [ statements ]  
End While  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`condition`|Obrigatórios. Expressão `Boolean`. Se `condition` for `Nothing` , Visual Basic tratará como `False` .|  
|`statements`|Opcional. Uma ou mais instruções `While` a seguir, que são executadas toda vez `condition` são `True` .|  
|`Continue While`|Opcional. Transfere o controle para a próxima iteração do `While` bloco.|  
|`Exit While`|Opcional. Transfere o controle do `While` bloco.|  
|`End While`|Obrigatórios. Finaliza a definição do bloco `While`.|  
  
## <a name="remarks"></a>Comentários  
 Use uma `While...End While` estrutura quando você quiser repetir um conjunto de instruções um número indefinido de vezes, desde que uma condição permaneça `True` . Se você quiser mais flexibilidade com o local em que você testa a condição ou o resultado para o qual você o testa, talvez prefira o [... Instrução loop](do-loop-statement.md). Se você quiser repetir as instruções um número definido de vezes, o [para... A próxima instrução](for-next-statement.md) é geralmente uma opção melhor.  
  
> [!NOTE]
> A `While` palavra-chave também é usada no... [ Instrução loop](do-loop-statement.md), a [cláusula Skip while](../queries/skip-while-clause.md) e a [cláusula Take While](../queries/take-while-clause.md).  
  
 Se `condition` for `True` , toda a `statements` execução até que a `End While` instrução seja encontrada. Em seguida, o controle retorna à `While` instrução e `condition` é verificado novamente. Se `condition` ainda for `True` , o processo será repetido. Se for `False` , o controle passará para a instrução que segue a `End While` instrução.  
  
 A `While` instrução sempre verifica a condição antes de iniciar o loop. O loop continua enquanto a condição permanece `True` . Se `condition` for `False` , quando você inserir o loop pela primeira vez, ele não será executado mesmo uma.  
  
 `condition`Normalmente, os resultados de uma comparação de dois valores, mas podem ser qualquer expressão avaliada como um valor de [tipo de dados booliano](../data-types/boolean-data-type.md) ( `True` ou `False` ). Essa expressão pode incluir um valor de outro tipo de dados, como um tipo numérico, que foi convertido em `Boolean` .  
  
 Você pode aninhar `While` loops colocando um loop dentro de outro. Você também pode aninhar diferentes tipos de estruturas de controle entre si. Para obter mais informações, consulte [estruturas de controle aninhado](../../programming-guide/language-features/control-flow/nested-control-structures.md).  
  
## <a name="exit-while"></a>Sair enquanto  
 A instrução [Exit while](exit-statement.md) pode fornecer outra maneira de sair de um `While` loop. `Exit While`transfere imediatamente o controle para a instrução que segue a `End While` instrução.  
  
 Normalmente, você usa `Exit While` depois que alguma condição é avaliada (por exemplo, em uma `If...Then...Else` estrutura). Talvez você queira sair de um loop se detectar uma condição que o torne desnecessário ou impossível para continuar a iteração, como um valor errado ou uma solicitação de encerramento. Você pode usar `Exit While` ao testar uma condição que poderia causar um *loop infinito*, que é um loop que poderia executar um número muito grande ou mesmo infinito de vezes. Em seguida, você pode usar `Exit While` para escapar o loop.  
  
 Você pode inserir qualquer número de `Exit While` instruções em qualquer lugar no `While` loop.  
  
 Quando usado em `While` loops aninhados, `Exit While` o transfere o controle do loop mais interno e para o próximo nível mais alto de aninhamento.  
  
 A `Continue While` instrução transfere imediatamente o controle para a próxima iteração do loop. Para obter mais informações, consulte [instrução Continue](continue-statement.md).  
  
## <a name="example"></a>Exemplo  
 No exemplo a seguir, as instruções no loop continuam a ser executadas até que a `index` variável seja maior que 10.  
  
 [!code-vb[VbVbalrStatements#171](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class14.vb#171)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir ilustra o uso das `Continue While` `Exit While` instruções e.  
  
 [!code-vb[VbVbalrStatements#172](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class14.vb#172)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir lê todas as linhas em um arquivo de texto. O <xref:System.IO.File.OpenText%2A> método abre o arquivo e retorna um <xref:System.IO.StreamReader> que lê os caracteres. Na `While` condição, o <xref:System.IO.StreamReader.Peek%2A> método de `StreamReader` determina se o arquivo contém caracteres adicionais.  
  
 [!code-vb[VbVbalrStatements#173](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class14.vb#173)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de Loop](../../programming-guide/language-features/control-flow/loop-structures.md)
- [Instrução Do...Loop](do-loop-statement.md)
- [Instrução For...Next](for-next-statement.md)
- [Tipo de dados booliano](../data-types/boolean-data-type.md)
- [Estruturas de Controle Aninhadas](../../programming-guide/language-features/control-flow/nested-control-structures.md)
- [Instrução Exit](exit-statement.md)
- [Instrução Continue](continue-statement.md)
