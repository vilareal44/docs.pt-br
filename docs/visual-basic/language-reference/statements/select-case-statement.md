---
title: Instrução Select...Case
ms.date: 07/20/2015
f1_keywords:
- vb.Select
- vb.Case
helpviewer_keywords:
- Select statement [Visual Basic]
- Case statement [Visual Basic]
- Select...Case statements
- conditional statements [Visual Basic], Select Case
- control flow [Visual Basic], branching
- Else keyword [Visual Basic], in Select...Case statements
- execution [Visual Basic], conditional
- To keyword [Visual Basic], in Select...Case statements
- Select Case statement [Visual Basic], Select...Case
- Select statement [Visual Basic], Select...Case
- Is operator [Visual Basic], in Select...Case statements
- branching [Visual Basic], conditional
- Case Else statement [Visual Basic], Select...Case
- End keyword [Visual Basic], Select Case statements
- Case statement [Visual Basic], Select...Case
ms.assetid: 68877b65-5419-4bf0-a465-20cd0e4c7d44
ms.openlocfilehash: 3dedd43f920b493a0aca9ce48460b00815e1af5c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404233"
---
# <a name="selectcase-statement-visual-basic"></a>Instrução Select...Case (Visual Basic)
Executa um dos vários grupos de instruções, dependendo do valor de uma expressão.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Select [ Case ] testexpression  
    [ Case expressionlist  
        [ statements ] ]  
    [ Case Else  
        [ elsestatements ] ]  
End Select  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`testexpression`|Obrigatórios. Expressão. Deve ser avaliada como um dos tipos de dados elementares (,,,,,, `Boolean` `Byte` `Char` `Date` `Double` `Decimal` `Integer` , `Long` , `Object` , `SByte` ,, `Short` `Single` , `String` , `UInteger` , `ULong` e `UShort` ).|  
|`expressionlist`|Necessário em uma `Case` instrução. Lista de cláusulas de expressão que representam valores de correspondência para `testexpression` . Várias cláusulas de expressão são separadas por vírgulas. Cada cláusula pode ter um dos seguintes formatos:<br /><br /> -   *expression1* `To` *expression2*<br />-[ `Is` ] *comparisonoperator* *expressão* ComparisonOperator<br />-   *expression*<br /><br /> Use a `To` palavra-chave para especificar os limites de um intervalo de valores de correspondência para `testexpression` . O valor de `expression1` deve ser menor ou igual ao valor de `expression2` .<br /><br /> Use a `Is` palavra-chave com um operador de comparação (,,,, `=` `<>` `<` `<=` `>` ou `>=` ) para especificar uma restrição nos valores de correspondência para `testexpression` . Se a `Is` palavra-chave não for fornecida, ela será inserida automaticamente antes de *ComparisonOperator*.<br /><br /> O formulário que especifica apenas `expression` é tratado como um caso especial do `Is` formulário em que *ComparisonOperator* é o sinal de igual ( `=` ). Este formulário é avaliado como `testexpression`  =  `expression` .<br /><br /> As expressões em `expressionlist` podem ser de qualquer tipo de dados, desde que sejam conversíveis implicitamente para o tipo `testexpression` e o apropriado `comparisonoperator` seja válido para os dois tipos com os quais ele está sendo usado.|  
|`statements`|Opcional. Uma ou mais instruções que se seguem são `Case` executadas se o `testexpression` corresponde a qualquer cláusula no `expressionlist` .|  
|`elsestatements`|Opcional. Uma ou mais instruções `Case Else` que se seguem são executadas se não `testexpression` correspondem a nenhuma cláusula no `expressionlist` de qualquer uma das `Case` instruções.|  
|`End Select`|Encerra a definição da `Select` construção... `Case` .|  
  
## <a name="remarks"></a>Comentários  
 Se `testexpression` corresponder a qualquer `Case` `expressionlist` cláusula, as instruções após essa `Case` instrução são executadas até a `Case` próxima `Case Else` instrução, ou `End Select` . Em seguida, o controle passa para a instrução a seguir `End Select` . Se `testexpression` corresponder a uma `expressionlist` cláusula em mais de uma `Case` cláusula, somente as instruções após a primeira correspondência são executadas.  
  
 A `Case Else` instrução é usada para introduzir o `elsestatements` a ser executado se nenhuma correspondência for encontrada entre o `testexpression` e uma `expressionlist` cláusula em qualquer uma das outras `Case` instruções. Embora não seja necessário, é uma boa ideia ter uma `Case Else` instrução em sua `Select Case` construção para lidar com valores imprevistos `testexpression` . Se nenhuma `Case` `expressionlist` cláusula corresponder `testexpression` e não houver `Case Else` instrução, o controle passará para a instrução a seguir `End Select` .  
  
 Você pode usar várias expressões ou intervalos em cada `Case` cláusula. Por exemplo, a linha a seguir é válida.  
  
 `Case 1 To 4, 7 To 9, 11, 13, Is > maxNumber`  
  
> [!NOTE]
> A `Is` palavra-chave usada `Case` nas `Case Else` instruções e não é a mesma que o [operador is](../operators/is-operator.md), que é usado para comparação de referência de objeto.  
  
 Você pode especificar intervalos e várias expressões para cadeias de caracteres. No exemplo a seguir, `Case` corresponde a qualquer cadeia de caracteres que seja exatamente igual a "maçãs", tem um valor entre "porcas" e "sopa" em ordem alfabética ou contém exatamente o mesmo valor que o valor atual de `testItem` .  
  
 `Case "apples", "nuts" To "soup", testItem`  
  
 A configuração de `Option Compare` pode afetar comparações de cadeia de caracteres. Em `Option Compare Text` , as cadeias de caracteres "maçãs" e "maçãs" são comparadas como iguais, mas em `Option Compare Binary` , elas não.  
  
> [!NOTE]
> Uma `Case` instrução com várias cláusulas pode exibir o comportamento conhecido como *curto-circuito*. Visual Basic avalia as cláusulas da esquerda para a direita e, se uma delas produz uma correspondência com `testexpression` , as cláusulas restantes não são avaliadas. O curto circuito pode melhorar o desempenho, mas pode produzir resultados inesperados se você espera que todas as expressões sejam `expressionlist` avaliadas. Para obter mais informações sobre o curto-circuito, consulte [expressões booleanas](../../programming-guide/language-features/operators-and-expressions/boolean-expressions.md).  
  
 Se o código dentro de `Case` um `Case Else` bloco de instrução ou não precisar executar nenhuma das instruções no bloco, ele poderá sair do bloco usando a `Exit Select` instrução. Isso transfere o controle imediatamente para a instrução a seguir `End Select` .  
  
 `Select Case`as construções podem ser aninhadas. Cada construção aninhada `Select Case` deve ter uma `End Select` instrução correspondente e deve estar completamente contida em um `Case` bloco único ou `Case Else` de instrução da `Select Case` construção externa dentro da qual está aninhada.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa uma `Select Case` construção para gravar uma linha correspondente ao valor da variável `number` . A segunda `Case` instrução contém o valor que corresponde ao valor atual de `number` , portanto, a instrução que grava "entre 6 e 8, inclusivo" é executada.  
  
 [!code-vb[VbVbalrStatements#54](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#54)]  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Interaction.Choose%2A>
- [Instrução End](end-statement.md)
- [Instrução If...Then...Else](if-then-else-statement.md)
- [Instrução Option Compare](option-compare-statement.md)
- [Instrução Exit](exit-statement.md)
