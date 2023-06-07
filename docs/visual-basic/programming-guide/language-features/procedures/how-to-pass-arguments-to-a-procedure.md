---
title: Como passar argumentos para um procedimento
ms.date: 07/20/2015
helpviewer_keywords:
- arguments [Visual Basic], passing to procedures
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- procedure arguments
- Visual Basic code, procedures
- procedure parameters
- procedures [Visual Basic], calling
- argument passing [Visual Basic], procedures
ms.assetid: 08723588-3890-4ddc-8249-79e049e0f241
ms.openlocfilehash: 903e05facccd1f2afdf4bb51b200531feb64aa79
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84387771"
---
# <a name="how-to-pass-arguments-to-a-procedure-visual-basic"></a>Como passar argumentos para um procedimento (Visual Basic)
Ao chamar um procedimento, você segue o nome do procedimento com uma lista de argumentos entre parênteses. Você fornece um argumento correspondente a cada parâmetro necessário que o procedimento define e, opcionalmente, pode fornecer argumentos para os `Optional` parâmetros. Se você não fornecer um `Optional` parâmetro na chamada, deverá incluir uma vírgula para marcar seu lugar na lista de argumentos, se você estiver fornecendo argumentos subsequentes.  
  
 Se você pretende passar um argumento de um tipo de dados diferente daquele de seu parâmetro correspondente, como `Byte` para `String` , você pode definir a opção de verificação de tipo ([instrução Option Strict](../../../language-reference/statements/option-strict-statement.md)) para `Off` . Se `Option Strict` for `On` , você deve usar conversões de alargamento ou palavras-chave de conversão explícita. Para obter mais informações, consulte [conversões de ampliação e estreitamento](../data-types/widening-and-narrowing-conversions.md) e [funções de conversão de tipo](../../../language-reference/functions/type-conversion-functions.md).  
  
 Para obter mais informações, consulte [parâmetros e argumentos de procedimento](./procedure-parameters-and-arguments.md).  
  
### <a name="to-pass-one-or-more-arguments-to-a-procedure"></a>Para passar um ou mais argumentos para um procedimento  
  
1. Na instrução de chamada, siga o nome do procedimento com parênteses.  
  
2. Dentro dos parênteses, coloque uma lista de argumentos. Inclua um argumento para cada parâmetro necessário que o procedimento define e separe os argumentos com vírgulas.  
  
3. Certifique-se de que cada argumento é uma expressão válida que é avaliada como um tipo de dados conversível para o tipo que o procedimento define para o parâmetro correspondente.  
  
4. Se um parâmetro for definido como [opcional](../../../language-reference/modifiers/optional.md), você poderá incluí-lo na lista de argumentos ou omiti-lo. Se você omiti-lo, o procedimento usará o valor padrão definido para esse parâmetro.  
  
5. Se você omitir um argumento para um `Optional` parâmetro e houver outro parâmetro depois dele na lista de parâmetros, você poderá marcar o lugar do argumento omitido por uma vírgula extra na lista de argumentos.  
  
     O exemplo a seguir chama a <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A> função Visual Basic.  
  
     [!code-vb[VbVbcnProcedures#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#34)]  
  
     O exemplo anterior fornece o primeiro argumento necessário, que é a cadeia de caracteres da mensagem a ser exibida. Ele omite um argumento para o segundo parâmetro opcional, que especifica os botões a serem exibidos na caixa de mensagem. Como a chamada não fornece um valor, `MsgBox` o usa o valor padrão, `MsgBoxStyle.OKOnly` , que exibe apenas um botão **OK** .  
  
     A segunda vírgula na lista de argumentos marca o lugar do segundo argumento omitido e a última cadeia de caracteres é passada para o terceiro parâmetro opcional de `MsgBox` , que é o texto a ser exibido na barra de título.  
  
## <a name="see-also"></a>Confira também

- [Subprocedimentos](./sub-procedures.md)
- [Procedimentos de função](./function-procedures.md)
- [Procedimentos de propriedade](./property-procedures.md)
- [Procedimentos do operador](./operator-procedures.md)
- [Como definir um parâmetro para um procedimento](./how-to-define-a-parameter-for-a-procedure.md)
- [Passar argumentos por valor e por referência](./passing-arguments-by-value-and-by-reference.md)
- [Procedimentos recursivos](./recursive-procedures.md)
- [Sobrecarga de procedimento](./procedure-overloading.md)
- [Objetos e classes](../objects-and-classes/index.md)
- [Programação orientada a objeto (Visual Basic)](../../concepts/object-oriented-programming.md)
