---
title: Como criar um procedimento que retorne um valor
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedures [Visual Basic], returning a value
ms.assetid: 8ee19f95-a9ef-4033-963b-d224dca207c4
ms.openlocfilehash: 2655aba6ce37be831d8dd4202ffd484cfd3fd5ef
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388343"
---
# <a name="how-to-create-a-procedure-that-returns-a-value-visual-basic"></a>Como criar um procedimento que retorne um valor (Visual Basic)
Você usa um `Function` procedimento para retornar um valor para o código de chamada.  
  
### <a name="to-create-a-procedure-that-returns-a-value"></a>Para criar um procedimento que retorna um valor  
  
1. Fora de qualquer outro procedimento, use uma `Function` instrução, seguida por uma `End Function` instrução.  
  
2. Na `Function` instrução, siga a `Function` palavra-chave com o nome do procedimento e, em seguida, a lista de parâmetros entre parênteses.  
  
3. Siga os parênteses com uma `As` cláusula para especificar o tipo de dados do valor retornado.  
  
4. Coloque as instruções de código do procedimento entre `Function` as `End Function` instruções e.  
  
5. Use uma `Return` instrução para retornar o valor para o código de chamada.  
  
     O procedimento a seguir `Function` calcula o lado mais longo, ou hipotenusa, de um triângulo à direita, dado os valores para os outros dois lados.  
  
     [!code-vb[VbVbcnProcedures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#1)]  
  
     O exemplo a seguir mostra uma chamada típica para `hypotenuse` .  
  
     [!code-vb[VbVbcnProcedures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Subprocedimentos](./sub-procedures.md)
- [Procedimentos de propriedade](./property-procedures.md)
- [Procedimentos do operador](./operator-procedures.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Instrução Function](../../../language-reference/statements/function-statement.md)
- [Como retornar um valor de um procedimento](./how-to-return-a-value-from-a-procedure.md)
- [Como chamar um procedimento que retorna um valor](./how-to-call-a-procedure-that-returns-a-value.md)
