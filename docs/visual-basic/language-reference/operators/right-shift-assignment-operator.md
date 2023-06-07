---
title: '>>Operador ='
ms.date: 07/20/2015
f1_keywords:
- vb.>>=
helpviewer_keywords:
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- operator >>= [Visual Basic]
- compound assignment statements [Visual Basic]
- '>>= operator [Visual Basic]'
ms.assetid: 2bcd9abb-7a8c-4229-b75d-8816ff1dc700
ms.openlocfilehash: c3afe2bcc4b9732b5afd34df1b83eaebe707b3f5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409290"
---
# <a name="-operator-visual-basic"></a>Operador >>= (Visual Basic)
Executa um deslocamento aritmético para a direita no valor de uma variável ou propriedade e atribui o resultado de volta à variável ou à propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
variableorproperty >>= amount  
```  
  
## <a name="parts"></a>Partes  
 `variableorproperty`  
 Obrigatórios. Variável ou propriedade de um tipo integral ( `SByte` ,,,,, `Byte` `Short` `UShort` `Integer` `UInteger` , `Long` ou `ULong` ).  
  
 `amount`  
 Obrigatórios. Expressão numérica de um tipo de dados que amplia para `Integer` .  
  
## <a name="remarks"></a>Comentários  
 O elemento no lado esquerdo do `>>=` operador pode ser uma variável escalar simples, uma propriedade ou um elemento de uma matriz. A variável ou a propriedade não pode ser [ReadOnly](../modifiers/readonly.md).  
  
 O `>>=` operador primeiro executa um deslocamento aritmético à direita no valor da variável ou da propriedade. Em seguida, o operador atribui o resultado dessa operação de volta à variável ou à propriedade.  
  
 Deslocamentos aritméticos não são circulares, o que significa que os bits deslocados uma extremidade do resultado não são reintroduzidos na outra extremidade. Em um deslocamento aritmético à direita, os bits deslocados além da posição de bits mais à direita são descartados e o bit mais à esquerda é propagado para as posições de bits vagas à esquerda. Isso significa que `variableorproperty` , se o tiver um valor negativo, as posições vagas serão definidas como um. Se `variableorproperty` for positivo ou se seu tipo de dados for um tipo não assinado, as posições vagadas serão definidas como zero.  
  
## <a name="overloading"></a>Sobrecarga  
 O [operador>> ](right-shift-operator.md) pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Sobrecarregar o `>>` operador afeta o comportamento do `>>=` operador. Se o seu código usa `>>=` em uma classe ou estrutura que sobrecarrega, certifique-se de `>>` entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `>>=` operador para deslocar o padrão de bits de uma `Integer` variável pelo valor especificado e atribuir o resultado à variável.  
  
 [!code-vb[VbVbalrOperators#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#15)]  
  
## <a name="see-also"></a>Confira também

- [Operador de>> ](right-shift-operator.md)
- [Operadores de atribuição](assignment-operators.md)
- [Operadores Bit Shift](bit-shift-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Instruções](../../programming-guide/language-features/statements.md)
