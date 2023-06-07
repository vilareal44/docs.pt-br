---
title: Operador /=
ms.date: 07/20/2015
f1_keywords:
- vb./=
helpviewer_keywords:
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- /= operator [Visual Basic]
- operator /=
- compound assignment statements [Visual Basic]
ms.assetid: a1e22d0e-8380-4761-9da1-84fb51c34821
ms.openlocfilehash: 48ae78630aa66ad804d539f88524c456cf805889
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371239"
---
# <a name="-operator-visual-basic"></a>Operador /= (Visual Basic)
Divide o valor de uma variável ou propriedade pelo valor de uma expressão e atribui o resultado de ponto flutuante à variável ou à propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
variableorproperty /= expression  
```  
  
## <a name="parts"></a>Partes  
 `variableorproperty`  
 Obrigatórios. Qualquer variável numérica ou propriedade.  
  
 `expression`  
 Obrigatórios. Qualquer expressão numérica.  
  
## <a name="remarks"></a>Comentários  
 O elemento no lado esquerdo do `/=` operador pode ser uma variável escalar simples, uma propriedade ou um elemento de uma matriz. A variável ou a propriedade não pode ser [ReadOnly](../modifiers/readonly.md).  
  
 Em `/=` primeiro lugar, o operador divide o valor da variável ou da propriedade (no lado esquerdo do operador) pelo valor da expressão (no lado direito do operador) do. Em seguida, o operador atribui o resultado de ponto flutuante dessa operação à variável ou à propriedade.  
  
 Essa instrução atribui um `Double` valor à variável ou à propriedade à esquerda. Se `Option Strict` for `On` , `variableorproperty` deve ser um `Double` . Se `Option Strict` for `Off` , Visual Basic executará uma conversão implícita e atribuirá o valor resultante a `variableorproperty` , com um possível erro no tempo de execução. Para obter mais informações, consulte [conversões de alargamento e estreitamento](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md) e [instrução Option Strict](../statements/option-strict-statement.md).  
  
## <a name="overloading"></a>Sobrecarga  
 O [operador/(Visual Basic)](floating-point-division-operator.md) pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Sobrecarregar o `/` operador afeta o comportamento do `/=` operador. Se o seu código usa `/=` em uma classe ou estrutura que sobrecarrega, certifique-se de `/` entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `/=` operador para dividir uma `Integer` variável por um segundo e atribuir o quociente à primeira variável.  
  
 [!code-vb[VbVbalrOperators#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#17)]  
  
## <a name="see-also"></a>Confira também

- [Operador/(Visual Basic)](floating-point-division-operator.md)
- [\\= Operador](integer-division-assignment-operator.md)
- [Operadores de atribuição](assignment-operators.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Instruções](../../programming-guide/language-features/statements.md)
