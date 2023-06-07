---
title: Operador <<=
ms.date: 07/20/2015
f1_keywords:
- vb.<<=
helpviewer_keywords:
- operator <<=
- assignment statements [Visual Basic], compound
- <<= operator [Visual Basic]
- statements [Visual Basic], compound assignment
- operator<<=
- compound assignment statements [Visual Basic]
ms.assetid: 8ad26613-faff-4e2f-89ee-63feee33bfda
ms.openlocfilehash: ff7cbb02a9a10dbe11450491e93fd85fd77b44ba
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84370655"
---
# <a name="-operator-visual-basic"></a>\<\<= Operador (Visual Basic)
Executa um deslocamento aritmético para a esquerda no valor de uma variável ou propriedade e atribui o resultado de volta à variável ou à propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
variableorproperty <<= amount  
```  
  
## <a name="parts"></a>Partes  
 `variableorproperty`  
 Obrigatórios. Variável ou propriedade de um tipo integral ( `SByte` ,,,,, `Byte` `Short` `UShort` `Integer` `UInteger` , `Long` ou `ULong` ).  
  
 `amount`  
 Obrigatórios. Expressão numérica de um tipo de dados que amplia para `Integer` .  
  
## <a name="remarks"></a>Comentários  
 O elemento no lado esquerdo do `<<=` operador pode ser uma variável escalar simples, uma propriedade ou um elemento de uma matriz. A variável ou a propriedade não pode ser [ReadOnly](../modifiers/readonly.md).  
  
 O `<<=` operador primeiro executa um deslocamento aritmético para a esquerda no valor da variável ou da propriedade. Em seguida, o operador atribui o resultado dessa operação de volta para essa variável ou propriedade.  
  
 Deslocamentos aritméticos não são circulares, o que significa que os bits deslocados uma extremidade do resultado não são reintroduzidos na outra extremidade. Em um deslocamento aritmético à esquerda, os bits deslocados além do intervalo do tipo de dados de resultado são descartados e as posições de bits vagadas à direita são definidas como zero.  
  
## <a name="overloading"></a>Sobrecarga  
 O [operador<< ](left-shift-operator.md) pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Sobrecarregar o `<<` operador afeta o comportamento do `<<=` operador. Se o seu código usa `<<=` em uma classe ou estrutura que sobrecarrega, certifique-se de `<<` entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `<<=` operador para deslocar o padrão de bits de uma `Integer` variável à esquerda pelo valor especificado e atribuir o resultado à variável.  
  
 [!code-vb[VbVbalrOperators#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#13)]  
  
## <a name="see-also"></a>Confira também

- [Operador de<< ](left-shift-operator.md)
- [Operadores de atribuição](assignment-operators.md)
- [Operadores Bit Shift](bit-shift-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Instruções](../../programming-guide/language-features/statements.md)
