---
title: Instrução Do...Loop
ms.date: 07/20/2015
f1_keywords:
- vb.Do
- vb.Loop
- vb.Until
helpviewer_keywords:
- conditional statements [Visual Basic], Do�Loop
- while statement [Visual Basic], Do...Loop
- execution [Visual Basic], conditional
- Do loops
- Until keyword [Visual Basic], Do...Loop statement
- Do...Loop statement
- instructions, repeating
- Do statement [Visual Basic]
- Exit statement [Visual Basic], in Do...Loop statements
- loop structures [Visual Basic], Do�Loop statements
- do-while statements [Visual Basic]
- loops, exiting
- Loop keyword [Visual Basic], Do...Loop statement
ms.assetid: 892f9096-b3e2-4aee-834d-83bc4e2c379d
ms.openlocfilehash: a9ec6caccbe161a39b592a642a938b81bae911a6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404778"
---
# <a name="doloop-statement-visual-basic"></a>Instrução Do...Loop (Visual Basic)
Repete um bloco de instruções enquanto uma `Boolean` condição é `True` ou até que a condição se torne `True` .  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Do { While | Until } condition  
    [ statements ]  
    [ Continue Do ]  
    [ statements ]  
    [ Exit Do ]  
    [ statements ]  
Loop  
' -or-  
Do  
    [ statements ]  
    [ Continue Do ]  
    [ statements ]  
    [ Exit Do ]  
    [ statements ]  
Loop { While | Until } condition  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`Do`|Obrigatórios. Inicia a definição do `Do` loop.|  
|`While`|Necessário a menos que `Until` seja usado. Repita o loop até que `condition` esteja `False` .|  
|`Until`|Necessário a menos que `While` seja usado. Repita o loop até que `condition` esteja `True` .|  
|`condition`|Opcional. Expressão `Boolean`. Se `condition` for `Nothing` , Visual Basic tratará como `False` .|  
|`statements`|Opcional. Uma ou mais instruções repetidas enquanto, ou até, `condition` são `True` .|  
|`Continue Do`|Opcional. Transfere o controle para a próxima iteração do `Do` loop.|  
|`Exit Do`|Opcional. Transfere o controle do `Do` loop.|  
|`Loop`|Obrigatórios. Encerra a definição do `Do` loop.|  
  
## <a name="remarks"></a>Comentários  
 Use uma `Do...Loop` estrutura quando você quiser repetir um conjunto de instruções um número indefinido de vezes, até que uma condição seja satisfeita. Se você quiser repetir as instruções um número definido de vezes, o [para... A próxima instrução](for-next-statement.md) é geralmente uma opção melhor.  
  
 Você pode usar o `While` ou o `Until` para especificar `condition` , mas não ambos.  
  
 Você pode testar `condition` apenas uma vez, no início ou no fim do loop. Se você testar `condition` no início do loop (na `Do` instrução), o loop pode não ser executado mesmo uma vez. Se você testar no final do loop (na `Loop` instrução), o loop sempre será executado pelo menos uma vez.  
  
 A condição geralmente resulta de uma comparação de dois valores, mas pode ser qualquer expressão avaliada como um valor de [tipo de dados booliano](../data-types/boolean-data-type.md) ( `True` ou `False` ). Isso inclui valores de outros tipos de dados, como tipos numéricos, que foram convertidos em `Boolean` .  
  
 Você pode aninhar `Do` loops colocando um loop dentro de outro. Você também pode aninhar diferentes tipos de estruturas de controle entre si. Para obter mais informações, consulte [estruturas de controle aninhado](../../programming-guide/language-features/control-flow/nested-control-structures.md).  
  
> [!NOTE]
> A `Do...Loop` estrutura oferece mais flexibilidade do que o [while... Instrução End While](while-end-while-statement.md) porque ela permite que você decida se deseja encerrar o loop quando `condition` parar de ser `True` ou quando ele se tornar o primeiro `True` . Ele também permite que você teste `condition` no início ou no fim do loop.  
  
## <a name="exit-do"></a>Sair do  
 A instrução [Exit do](exit-statement.md) pode fornecer uma maneira alternativa de sair de um `Do…Loop` . `Exit Do`transfere o controle imediatamente para a instrução que segue a `Loop` instrução.  
  
 `Exit Do`geralmente é usado depois que alguma condição é avaliada, por exemplo, em uma `If...Then...Else` estrutura. Talvez você queira sair de um loop se detectar uma condição que o torne desnecessário ou impossível para continuar a iteração, como um valor errado ou uma solicitação de encerramento. Um uso de `Exit Do` é testar uma condição que poderia causar um *loop infinito*, que é um loop que poderia executar um número grande ou mesmo infinito de vezes. Você pode usar `Exit Do` para escapar o loop.  
  
 Você pode incluir qualquer número de `Exit Do` instruções em qualquer lugar em um `Do…Loop` .  
  
 Quando usado em `Do` loops aninhados, `Exit Do` o transfere o controle do loop mais interno e para o próximo nível mais alto de aninhamento.  
  
## <a name="example"></a>Exemplo  
 No exemplo a seguir, as instruções no loop continuam a ser executadas até que a `index` variável seja maior que 10. A `Until` cláusula está no final do loop.  
  
 [!code-vb[VbVbalrStatements#131](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#131)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa uma `While` cláusula em vez de uma `Until` cláusula e `condition` é testada no início do loop em vez de no final.  
  
 [!code-vb[VbVbalrStatements#132](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#132)]  
  
## <a name="example"></a>Exemplo  
 No exemplo a seguir, `condition` interrompe o loop quando a `index` variável é maior que 100. No `If` entanto, a instrução no loop faz com que a `Exit Do` instrução pare o loop quando a variável de índice for maior que 10.  
  
 [!code-vb[VbVbalrStatements#133](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#133)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir lê todas as linhas em um arquivo de texto. O <xref:System.IO.File.OpenText%2A> método abre o arquivo e retorna um <xref:System.IO.StreamReader> que lê os caracteres. Na `Do...Loop` condição, o <xref:System.IO.StreamReader.Peek%2A> método de `StreamReader` determina se há caracteres adicionais.  
  
 [!code-vb[VbVbalrStatements#134](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#134)]  
  
## <a name="see-also"></a>Confira também

- [Estruturas de Loop](../../programming-guide/language-features/control-flow/loop-structures.md)
- [Instrução For...Next](for-next-statement.md)
- [Tipo de dados booliano](../data-types/boolean-data-type.md)
- [Estruturas de Controle Aninhadas](../../programming-guide/language-features/control-flow/nested-control-structures.md)
- [Instrução Exit](exit-statement.md)
- [Instrução While...End While](while-end-while-statement.md)
