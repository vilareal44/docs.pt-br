---
title: Como definir um operador de conversão
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- operators [Visual Basic], defining
- procedures [Visual Basic], operator
- operators [Visual Basic], overloading
- return values [Visual Basic], Operator procedures
- operator overloading
ms.assetid: 54203dfa-c24b-463f-9942-d5153e89e762
ms.openlocfilehash: 53b0211c6304625edd7ac24fa52ff0c051d8f0a0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388083"
---
# <a name="how-to-define-a-conversion-operator-visual-basic"></a>Como definir um operador de conversão (Visual Basic)
Se você tiver definido uma classe ou estrutura, poderá definir um operador de conversão de tipo entre o tipo de sua classe ou estrutura e outro tipo de dados (como `Integer` , `Double` ou `String` ).  
  
 Defina a conversão de tipo como um procedimento de [função CType](../../../language-reference/functions/ctype-function.md) dentro da classe ou estrutura. Todos os procedimentos de conversão devem ser `Public Shared` , e cada um deve especificar o [alargamento](../../../language-reference/modifiers/widening.md) ou [restringir](../../../language-reference/modifiers/narrowing.md).  
  
 A definição de um operador em uma classe ou estrutura também é chamada de *sobrecarga* do operador.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir define os operadores de conversão entre uma estrutura chamada `digit` e um `Byte` .  
  
 [!code-vb[VbVbcnProcedures#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#27)]  
  
 Você pode testar a estrutura `digit` com o código a seguir.  
  
 [!code-vb[VbVbcnProcedures#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#28)]  
  
## <a name="see-also"></a>Confira também

- [Procedimentos do operador](./operator-procedures.md)
- [Como definir um operador](./how-to-define-an-operator.md)
- [Como chamar um procedimento de operador](./how-to-call-an-operator-procedure.md)
- [Como usar uma classe que define operadores](./how-to-use-a-class-that-defines-operators.md)
- [Instrução Operator](../../../language-reference/statements/operator-statement.md)
- [Instrução Structure](../../../language-reference/statements/structure-statement.md)
- [Como: Declarar uma estrutura](../data-types/how-to-declare-a-structure.md)
- [Conversões implícitas e explícitas](../data-types/implicit-and-explicit-conversions.md)
- [Conversões de Widening e Narrowing](../data-types/widening-and-narrowing-conversions.md)
