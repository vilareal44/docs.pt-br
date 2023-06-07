---
title: Operador *=
ms.date: 07/20/2015
f1_keywords:
- vb.*=
helpviewer_keywords:
- operator *=
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- '*= operator [Visual Basic]'
- compound assignment statements [Visual Basic]
ms.assetid: 96c86509-6eb8-4682-8226-3852e049376f
ms.openlocfilehash: b06ebcb4f4100a0621f52a769543c0fb24fbb4bf
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401492"
---
# <a name="-operator-visual-basic"></a>Operador *= (Visual Basic)
Multiplica o valor de uma variável ou propriedade pelo valor de uma expressão e atribui o resultado à variável ou à propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
variableorproperty *= expression  
```  
  
## <a name="parts"></a>Partes  
 `variableorproperty`  
 Obrigatórios. Qualquer variável numérica ou propriedade.  
  
 `expression`  
 Obrigatórios. Qualquer expressão numérica.  
  
## <a name="remarks"></a>Comentários  
 O elemento no lado esquerdo do `*=` operador pode ser uma variável escalar simples, uma propriedade ou um elemento de uma matriz. A variável ou a propriedade não pode ser [ReadOnly](../modifiers/readonly.md).  
  
 `*=`Primeiro, o operador multiplica o valor da expressão (no lado direito do operador) pelo valor da variável ou da propriedade (no lado esquerdo do operador), em seguida. Em seguida, o operador atribui o resultado dessa operação à variável ou à propriedade.  
  
## <a name="overloading"></a>Sobrecarga  
 O [operador *](multiplication-operator.md) pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Sobrecarregar o `*` operador afeta o comportamento do `*=` operador. Se o seu código usa `*=` em uma classe ou estrutura que sobrecarrega, certifique-se de `*` entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `*=` operador para multiplicar uma `Integer` variável por um segundo e atribuir o resultado à primeira variável.  
  
 [!code-vb[VbVbalrOperators#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#5)]  
  
## <a name="see-also"></a>Confira também

- [* Operador](multiplication-operator.md)
- [Operadores de atribuição](assignment-operators.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Instruções](../../programming-guide/language-features/statements.md)
