---
title: Conversão de delegado reduzida
ms.date: 07/20/2015
helpviewer_keywords:
- relaxed delegate conversion [Visual Basic]
- delegates [Visual Basic], relaxed conversion
- conversions [Visual Basic], relaxed delegate
ms.assetid: 64f371d0-5416-4f65-b23b-adcbf556e81c
ms.openlocfilehash: a581ffae77c496908d2e4e38df53491a54ae2ab8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410664"
---
# <a name="relaxed-delegate-conversion-visual-basic"></a>Conversão de delegado reduzida (Visual Basic)
A conversão de delegado reduzida permite que você atribua sub-rotinas e funções a delegados ou manipuladores, mesmo quando suas assinaturas não forem idênticas. Portanto, a associação a delegados se torna consistente com a associação já permitida para invocações de método.  
  
## <a name="parameters-and-return-type"></a>Parâmetros e tipo de retorno  
 Em vez de uma correspondência exata de assinatura, a conversão reduzida exige que as seguintes condições sejam atendidas quando `Option Strict` é definido como `On` :  
  
- Uma conversão de ampliação deve existir do tipo de dados de cada parâmetro delegado para o tipo de dados do parâmetro correspondente da função atribuída ou `Sub` . No exemplo a seguir, o delegado `Del1` tem um parâmetro, um `Integer` . `m`O parâmetro nas expressões lambda atribuídas deve ter um tipo de dados para o qual há uma conversão de ampliação de `Integer` , como `Long` ou `Double` .  
  
     [!code-vb[VbVbalrRelaxedDelegates#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#1)]  
  
     [!code-vb[VbVbalrRelaxedDelegates#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#2)]  
  
     Conversões redutoras são permitidas somente quando `Option Strict` é definido como `Off` .  
  
     [!code-vb[VbVbalrRelaxedDelegates#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module2.vb#8)]  
  
- Uma conversão de ampliação deve existir na direção oposta do tipo de retorno da função atribuída ou `Sub` ao tipo de retorno do delegado. Nos exemplos a seguir, o corpo de cada expressão lambda atribuída deve ser avaliado como um tipo de dados que se expande como `Integer` o tipo de retorno de `del1` é `Integer` .  
  
     [!code-vb[VbVbalrRelaxedDelegates#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#3)]  
  
 Se `Option Strict` é definido como `Off` , a restrição de ampliação é removida em ambas as direções.  
  
 [!code-vb[VbVbalrRelaxedDelegates#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module2.vb#4)]  
  
## <a name="omitting-parameter-specifications"></a>Omitindo especificações de parâmetro  
 Delegados relaxados também permitem que você omita completamente as especificações de parâmetro no método atribuído:  
  
 [!code-vb[VbVbalrRelaxedDelegates#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#5)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#6)]  
  
 Observe que você não pode especificar alguns parâmetros e omitir outros.  
  
 [!code-vb[VbVbalrRelaxedDelegates#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#15)]  
  
 A capacidade de omitir parâmetros é útil em uma situação como definir um manipulador de eventos, em que vários parâmetros complexos estão envolvidos. Os argumentos para alguns manipuladores de eventos não são usados. Em vez disso, o manipulador acessa diretamente o estado do controle no qual o evento está registrado e ignora os argumentos. Delegados reduzidos permitem omitir os argumentos em tais declarações quando nenhuma ambiguidade resulta. No exemplo a seguir, o método totalmente especificado `OnClick` pode ser reescrito como `RelaxedOnClick` .  
  
```vb  
Sub OnClick(ByVal sender As Object, ByVal e As EventArgs) Handles b.Click  
    MessageBox.Show("Hello World from" + b.Text)  
End Sub  
  
Sub RelaxedOnClick() Handles b.Click  
    MessageBox.Show("Hello World from" + b.Text)  
End Sub  
```  
  
## <a name="addressof-examples"></a>Exemplos de AddressOf  
 As expressões lambda são usadas nos exemplos anteriores para facilitar a visualização das relações de tipo. No entanto, as mesmas reduções são permitidas para atribuições de delegado que usam `AddressOf` , `Handles` ou `AddHandler` .  
  
 No exemplo a seguir, `f1` as funções,, `f2` `f3` e `f4` podem ser atribuídas a `Del1` .  
  
 [!code-vb[VbVbalrRelaxedDelegates#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#1)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#7)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#9)]  
  
 O exemplo a seguir é válido somente quando `Option Strict` é definido como `Off` .  
  
 [!code-vb[VbVbalrRelaxedDelegates#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module2.vb#14)]  
  
## <a name="dropping-function-returns"></a>Descartando retornos de função  
 A conversão de delegado reduzida permite que você atribua uma função a um `Sub` delegado, ignorando efetivamente o valor de retorno da função. No entanto, você não pode atribuir um `Sub` a um delegado de função. No exemplo a seguir, o endereço da função `doubler` é atribuído ao `Sub` delegado `Del3` .  
  
 [!code-vb[VbVbalrRelaxedDelegates#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#10)]  
  
 [!code-vb[VbVbalrRelaxedDelegates#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrRelaxedDelegates/VB/Module1.vb#11)]  
  
## <a name="see-also"></a>Confira também

- [Expressões lambda](../procedures/lambda-expressions.md)
- [Conversões de Widening e Narrowing](../data-types/widening-and-narrowing-conversions.md)
- [Delegados](index.md)
- [Como passar procedimentos para outro procedimento no Visual Basic](how-to-pass-procedures-to-another-procedure.md)
- [Inferência de Tipo de Variável Local](../variables/local-type-inference.md)
- [Instrução Option Strict](../../../language-reference/statements/option-strict-statement.md)
