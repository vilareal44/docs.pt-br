---
title: Como retornar um valor de um procedimento
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], returning from
- procedures [Visual Basic], returning a value
ms.assetid: 4bcc4724-2b4e-4df8-9b4b-16054607f87d
ms.openlocfilehash: 917e52b711645fbf94a132216a3fa90b0dfc15b3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414318"
---
# <a name="how-to-return-a-value-from-a-procedure-visual-basic"></a>Como retornar um valor de um procedimento (Visual Basic)
Um `Function` procedimento retorna um valor para o código de chamada executando uma `Return` instrução ou encontrando uma `Exit Function` `End Function` instrução or.  
  
### <a name="to-return-a-value-using-the-return-statement"></a>Para retornar um valor usando a instrução return  
  
1. Coloque uma `Return` instrução no ponto em que a tarefa do procedimento está concluída.  
  
2. Siga a `Return` palavra-chave com uma expressão que produz o valor que você deseja retornar ao código de chamada.  
  
3. Você pode ter mais de um demonstrativo `Return` no mesmo procedimento.  
  
     O procedimento a seguir `Function` calcula o lado mais longo, ou hipotenusa, de um triângulo à direita e o retorna ao código de chamada.  
  
     [!code-vb[VbVbcnProcedures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#1)]  
  
     O exemplo a seguir mostra uma chamada típica para `hypotenuse` , que armazena o valor retornado.  
  
     [!code-vb[VbVbcnProcedures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#6)]  
  
### <a name="to-return-a-value-using-exit-function-or-end-function"></a>Para retornar um valor usando a função Exit ou End Function  
  
1. Em pelo menos um lugar no `Function` procedimento, atribua um valor ao nome do procedimento.  
  
2. Quando você executa uma `Exit Function` `End Function` instrução ou, Visual Basic retorna o valor atribuído mais recentemente ao nome do procedimento.  
  
3. Você pode ter mais de um demonstrativo `Exit Function` no mesmo procedimento e mesclar os demonstrativos `Return` e `Exit Function` no mesmo procedimento.  
  
4. Você pode ter apenas uma `End Function` instrução em um `Function` procedimento.  
  
     Para obter mais informações e um exemplo, consulte "valor de retorno" na [instrução function](../../../language-reference/statements/function-statement.md).  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Subprocedimentos](./sub-procedures.md)
- [Procedimentos de propriedade](./property-procedures.md)
- [Procedimentos do operador](./operator-procedures.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Instrução Function](../../../language-reference/statements/function-statement.md)
- [Instrução de retorno](../../../language-reference/statements/return-statement.md)
- [Como criar um procedimento que retorne um valor](./how-to-create-a-procedure-that-returns-a-value.md)
- [Como chamar um procedimento que retorna um valor](./how-to-call-a-procedure-that-returns-a-value.md)
