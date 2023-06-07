---
title: Como chamar um procedimento de propriedade
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- Visual Basic code, properties
- procedures [Visual Basic], calling
- properties [Visual Basic], property procedures
- procedure calls [Visual Basic], property procedures
ms.assetid: 96bc4d74-d9c3-4b7a-954d-58ac8553cd94
ms.openlocfilehash: 006961a0f1d4be6b0d52be5bc273dad9733bfe56
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388693"
---
# <a name="how-to-call-a-property-procedure-visual-basic"></a>Como chamar um procedimento de propriedade (Visual Basic)
Você chama um procedimento de propriedade armazenando um valor na propriedade ou recuperando seu valor. Você acessa uma propriedade da mesma maneira que acessa uma variável.  
  
 O procedimento da propriedade `Set` armazena um valor e seu `Get` procedimento recupera o valor. No entanto, você não chama explicitamente esses procedimentos por nome. Você usa a propriedade em uma instrução de atribuição ou uma expressão, assim como você armazenaria ou recuperaria o valor de uma variável. Visual Basic faz as chamadas para os procedimentos da propriedade.  
  
### <a name="to-call-a-propertys-get-procedure"></a>Para chamar o procedimento Get de uma propriedade  
  
1. Use o nome da propriedade em uma expressão da mesma maneira que usaria um nome de variável. Você pode usar uma propriedade em qualquer lugar em que possa usar uma variável ou uma constante.  
  
     -ou-  
  
     Use o nome da propriedade após o sinal de igual ( `=` ) em uma instrução de atribuição.  
  
     O exemplo a seguir lê o valor da <xref:Microsoft.VisualBasic.DateAndTime.Now%2A> propriedade, chamando implicitamente seu `Get` procedimento.  
  
     [!code-vb[VbVbalrDateProperties#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDateProperties/VB/Module1.vb#4)]  
  
2. Se a propriedade usar argumentos, siga o nome da propriedade com parênteses para colocar a lista de argumentos. Se não houver argumentos, você pode opcionalmente omitir os parênteses.  
  
3. Coloque os argumentos na lista de argumentos dentro dos parênteses, separados por vírgulas. Certifique-se de fornecer os argumentos na mesma ordem em que a propriedade define os parâmetros correspondentes.  
  
 O valor da propriedade participa da expressão, assim como uma variável ou constante, ou é armazenado na variável ou propriedade no lado esquerdo da instrução de atribuição.  
  
### <a name="to-call-a-propertys-set-procedure"></a>Para chamar o procedimento de conjunto de uma propriedade  
  
1. Use o nome da propriedade no lado esquerdo de uma instrução de atribuição.  
  
     O exemplo a seguir define o valor da <xref:Microsoft.VisualBasic.DateAndTime.TimeOfDay%2A> propriedade, chamando implicitamente o `Set` procedimento.  
  
     [!code-vb[VbVbcnProcedures#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#11)]  
  
2. Se a propriedade usar argumentos, siga o nome da propriedade com parênteses para colocar a lista de argumentos. Se não houver argumentos, você pode opcionalmente omitir os parênteses.  
  
3. Coloque os argumentos na lista de argumentos dentro dos parênteses, separados por vírgulas. Certifique-se de fornecer os argumentos na mesma ordem em que a propriedade define os parâmetros correspondentes.  
  
 O valor gerado no lado direito da instrução de atribuição é armazenado na propriedade.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos de propriedade](./property-procedures.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Instrução Property](../../../language-reference/statements/property-statement.md)
- [Diferenças entre propriedades e variáveis no Visual Basic](./differences-between-properties-and-variables.md)
- [Como criar uma propriedade](./how-to-create-a-property.md)
- [Como declarar uma propriedade com níveis de acesso mistos](./how-to-declare-a-property-with-mixed-access-levels.md)
- [Como declarar e chamar uma propriedade padrão no Visual Basic](./how-to-declare-and-call-a-default-property.md)
- [Como inserir um valor em uma propriedade](./how-to-put-a-value-in-a-property.md)
- [Como obter um valor a partir de uma propriedade](./how-to-get-a-value-from-a-property.md)
- [Instrução Get](../../../language-reference/statements/get-statement.md)
- [Instrução SET](../../../language-reference/statements/set-statement.md)
