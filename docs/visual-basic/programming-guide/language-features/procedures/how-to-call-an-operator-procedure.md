---
title: Como chamar um procedimento de operador
ms.date: 07/20/2015
helpviewer_keywords:
- operator procedures [Visual Basic], calling
- procedures [Visual Basic], operator
- procedure calls [Visual Basic], operator overloading
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- return values [Visual Basic], Operator procedures
- overloaded operators [Visual Basic], calling
- operator overloading
ms.assetid: 0dce42cc-f0b0-4c14-9f62-018b21f33497
ms.openlocfilehash: fa2bc5417b8b917ff48502a5bd0a4daa21fab67e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388563"
---
# <a name="how-to-call-an-operator-procedure-visual-basic"></a>Como chamar um procedimento de operador (Visual Basic)
Você chama um procedimento de operador usando o símbolo de operador em uma expressão. No caso de um operador de conversão, você chama a [função CType](../../../language-reference/functions/ctype-function.md) para converter um valor de um tipo de dados para outro.  
  
 Você não chama os procedimentos de operador explicitamente. Basta usar o operador, ou a `CType` função, em uma instrução de atribuição ou em uma expressão, da mesma maneira que você usa um operador normalmente. Visual Basic faz a chamada para o procedimento de operador.  
  
 A definição de um operador em uma classe ou estrutura também é chamada de *sobrecarga* do operador.  
  
### <a name="to-call-an-operator-procedure"></a>Para chamar um procedimento de operador  
  
1. Use o símbolo do operador em uma expressão da maneira comum.  
  
2. Verifique se os tipos de dados dos operandos são apropriados para o operador e na ordem correta.  
  
3. O operador contribui para o valor da expressão conforme esperado.  
  
### <a name="to-call-a-conversion-operator-procedure"></a>Para chamar um procedimento de operador de conversão  
  
1. Use `CType` dentro de uma expressão.  
  
2. Verifique se os tipos de dados dos operandos são apropriados para a conversão e na ordem correta.  
  
3. `CType`chama o procedimento de operador de conversão e retorna o valor convertido.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria duas <xref:System.TimeSpan> estruturas, adiciona-as juntas e armazena o resultado em uma terceira <xref:System.TimeSpan> estrutura. A <xref:System.TimeSpan> estrutura define os procedimentos de operador para sobrecarregar vários operadores padrão.  
  
 [!code-vb[VbVbcnProcedures#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#29)]  
  
 Como o <xref:System.TimeSpan> sobrecarrega o operador padrão `+` , o exemplo anterior chama um procedimento de operador quando calcula o valor de `combinedSpan` .  
  
 Para obter um exemplo de como chamar um procedimento de operador de conversa, consulte [como: usar uma classe que define operadores](./how-to-use-a-class-that-defines-operators.md).  
  
## <a name="compile-the-code"></a>Compilar o código  
 Certifique-se de que a classe ou estrutura que você está usando define o operador que você deseja usar.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos do operador](./operator-procedures.md)
- [Como definir um operador](./how-to-define-an-operator.md)
- [Como definir um operador de conversão](./how-to-define-a-conversion-operator.md)
- [Instrução Operator](../../../language-reference/statements/operator-statement.md)
- [Widening](../../../language-reference/modifiers/widening.md)
- [Narrowing](../../../language-reference/modifiers/narrowing.md)
- [Instrução Structure](../../../language-reference/statements/structure-statement.md)
- [Como: Declarar uma estrutura](../data-types/how-to-declare-a-structure.md)
- [Conversões implícitas e explícitas](../data-types/implicit-and-explicit-conversions.md)
- [Conversões de Widening e Narrowing](../data-types/widening-and-narrowing-conversions.md)
