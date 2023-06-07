---
title: Como usar uma classe que define operadores
ms.date: 07/20/2015
helpviewer_keywords:
- operator procedures [Visual Basic], calling
- procedures [Visual Basic], operator
- procedures [Visual Basic], calling
- examples [Visual Basic], CType
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- return values [Visual Basic], Operator procedures
- operator overloading
ms.assetid: 7ccce94a-6ca0-47d1-9f3f-13385d34f5d5
ms.openlocfilehash: fe15e976e6a5469f2a9d1b3521a70a3e1860fdd3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414344"
---
# <a name="how-to-use-a-class-that-defines-operators-visual-basic"></a>Como usar uma classe que define operadores (Visual Basic)
Se você estiver usando uma classe ou estrutura que define seus próprios operadores, poderá acessar esses operadores de Visual Basic.  
  
 A definição de um operador em uma classe ou estrutura também é chamada de *sobrecarga* do operador.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir acessa a estrutura SQL <xref:System.Data.SqlTypes.SqlString> , que define os operadores de conversão ([função CType](../../../language-reference/functions/ctype-function.md)) em ambas as direções entre uma cadeia de caracteres SQL e uma Visual Basic Cadeia de caracteres. Use a `CType(` *expressão de cadeia de caracteres SQL*, `String)` para converter uma cadeia de caracteres SQL em uma Visual Basic Cadeia de caracteres e `CType(` *Visual Basic expressão de cadeia de caracteres*, <xref:System.Data.SqlTypes.SqlString> `)` para converter na outra direção.  
  
 [!code-vb[VbVbcnProcedures#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#30)]  
  
 [!code-vb[VbVbcnProcedures#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#31)]  
  
 A <xref:System.Data.SqlTypes.SqlString> estrutura define um operador de conversão ([função CType](../../../language-reference/functions/ctype-function.md)) de `String` para <xref:System.Data.SqlTypes.SqlString> e outra de <xref:System.Data.SqlTypes.SqlString> para `String` . A instrução que o atribui `title` para usa `jobTitle` o primeiro operador e a <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A> chamada de função usa a segunda.  
  
## <a name="compile-the-code"></a>Compilar o código  
 Certifique-se de que a classe ou estrutura que você está usando define o operador que você deseja usar. Não presuma que a classe ou estrutura tenha definido todos os operadores disponíveis para sobrecarga. Para obter uma lista de operadores disponíveis, consulte a [instrução Operator](../../../language-reference/statements/operator-statement.md).  
  
 Inclua a `Imports` instrução apropriada para a cadeia de caracteres SQL no início do seu arquivo de origem (nesse caso <xref:System.Data.SqlTypes> ).  
  
 Seu projeto deve ter referências a System. Data e System. XML.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos do operador](./operator-procedures.md)
- [Como definir um operador](./how-to-define-an-operator.md)
- [Como definir um operador de conversão](./how-to-define-a-conversion-operator.md)
- [Como chamar um procedimento de operador](./how-to-call-an-operator-procedure.md)
- [Widening](../../../language-reference/modifiers/widening.md)
- [Narrowing](../../../language-reference/modifiers/narrowing.md)
- [Instrução Structure](../../../language-reference/statements/structure-statement.md)
- [Como: Declarar uma estrutura](../data-types/how-to-declare-a-structure.md)
- [Conversões implícitas e explícitas](../data-types/implicit-and-explicit-conversions.md)
- [Conversões de Widening e Narrowing](../data-types/widening-and-narrowing-conversions.md)
