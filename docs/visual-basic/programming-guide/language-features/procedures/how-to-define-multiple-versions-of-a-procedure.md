---
title: Como definir várias versões de um procedimento
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- procedures [Visual Basic], overloading
- procedures [Visual Basic], multiple versions
- procedure overloading [Visual Basic], multiple versions
ms.assetid: 71ccdd66-1b00-4b66-bee4-6926c0d696f4
ms.openlocfilehash: 870a18dbf3a7e28b7d7b612e853beeec6908cf6f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84387927"
---
# <a name="how-to-define-multiple-versions-of-a-procedure-visual-basic"></a>Como definir várias versões de um procedimento (Visual Basic)
Você pode definir um procedimento em várias versões *sobrecarregando* -a, usando o mesmo nome, mas uma lista de parâmetros diferente para cada versão. A finalidade do sobrecarregamento é definir várias versões de um procedimento bem relacionadas sem precisar diferenciá-las por nome.  
  
 Para obter mais informações, consulte [sobrecarga de procedimento](./procedure-overloading.md).  
  
### <a name="to-define-multiple-versions-of-a-procedure"></a>Para definir várias versões de um procedimento  
  
1. Grave uma `Sub` `Function` instrução de declaração ou para cada versão do procedimento que você deseja definir. Use o mesmo nome de procedimento em cada declaração.  
  
2. Preceda a `Sub` `Function` palavra-chave ou em cada declaração com a palavra-chave [Overloads](../../../language-reference/modifiers/overloads.md) . Opcionalmente, você pode omitir `Overloads` as declarações, mas se você incluí-las em qualquer uma das declarações, deverá incluí-las em cada declaração.  
  
3. Seguindo cada instrução de declaração, escreva o código do procedimento para lidar com o caso específico em que o código de chamada fornece argumentos correspondentes à lista de parâmetros da versão. Você não precisa testar quais parâmetros o código de chamada forneceu. Visual Basic passa o controle para a versão correspondente do procedimento.  
  
4. Encerre cada versão do procedimento com a `End Sub` instrução ou `End Function` conforme apropriado.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir define um `Sub` procedimento para lançar uma transação em relação ao saldo do cliente. Ele usa a `Overloads` palavra-chave para definir duas versões do procedimento, uma que aceita o cliente pelo nome e a outra por número da conta.  
  
 [!code-vb[VbVbcnProcedures#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#72)]  
  
 O código de chamada pode obter a identificação do cliente como um `String` ou um `Integer` e, em seguida, usar a mesma instrução de chamada em ambos os casos.  
  
 Para obter informações sobre como chamar essas versões do `post` procedimento, consulte [como: chamar um procedimento sobrecarregado](./how-to-call-an-overloaded-procedure.md).  
  
## <a name="compile-the-code"></a>Compilar o código  
 Verifique se cada uma de suas versões sobrecarregadas tem o mesmo nome de procedimento, mas uma lista de parâmetros diferente.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Solucionando problemas de procedimentos](./troubleshooting-procedures.md)
- [Como sobrecarregar um procedimento que usa parâmetros opcionais](./how-to-overload-a-procedure-that-takes-optional-parameters.md)
- [Como sobrecarregar um procedimento que usa um número indefinido de parâmetros](./how-to-overload-a-procedure-that-takes-an-indefinite-number-of-parameters.md)
- [Considerações sobre procedimentos de sobrecarga](./considerations-in-overloading-procedures.md)
- [Resolução de sobrecarga](./overload-resolution.md)
