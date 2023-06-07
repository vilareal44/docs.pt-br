---
title: Operador ^=
ms.date: 07/20/2015
f1_keywords:
- vb.^=
helpviewer_keywords:
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- ^= operator [Visual Basic]
- compound assignment statements [Visual Basic]
ms.assetid: 397da132-2d96-4a85-a7bc-f7c730a608c9
ms.openlocfilehash: e631cc9a484b56ee059449ca1fbd9fc69405333d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371395"
---
# <a name="-operator-visual-basic"></a>Operador ^= (Visual Basic)
Gera o valor de uma variável ou propriedade para a potência de uma expressão e atribui o resultado de volta à variável ou à propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
variableorproperty ^= expression  
```  
  
## <a name="parts"></a>Partes  
 `variableorproperty`  
 Obrigatórios. Qualquer variável numérica ou propriedade.  
  
 `expression`  
 Obrigatórios. Qualquer expressão numérica.  
  
## <a name="remarks"></a>Comentários  
 O elemento no lado esquerdo do `^=` operador pode ser uma variável escalar simples, uma propriedade ou um elemento de uma matriz. A variável ou a propriedade não pode ser [ReadOnly](../modifiers/readonly.md).  
  
 O `^=` operador primeiro gera o valor da variável ou da propriedade (no lado esquerdo do operador) para a potência do valor da expressão (no lado direito do operador).... Em seguida, o operador atribui o resultado dessa operação de volta à variável ou à propriedade.  
  
 Visual Basic sempre executa exponenciação no [tipo de dados Double](../data-types/double-data-type.md). Os operandos de qualquer tipo diferente são convertidos em `Double` e o resultado é sempre `Double` .  
  
 O valor de `expression` pode ser fracionário, negativo ou ambos.  
  
## <a name="overloading"></a>Sobrecarga  
 O [operador ^](exponentiation-operator.md) pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Sobrecarregar o `^` operador afeta o comportamento do `^=` operador. Se o seu código usa `^=` em uma classe ou estrutura que sobrecarrega, certifique-se de `^` entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `^=` operador para aumentar o valor de uma `Integer` variável para a potência de uma segunda variável e atribuir o resultado à primeira variável.  
  
 [!code-vb[VbVbalrOperators#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#21)]  
  
## <a name="see-also"></a>Confira também

- [Operador ^](exponentiation-operator.md)
- [Operadores de atribuição](assignment-operators.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Instruções](../../programming-guide/language-features/statements.md)
