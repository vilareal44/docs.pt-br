---
title: '&amp;= Operador'
ms.date: 07/20/2015
f1_keywords:
- vb.&=
helpviewer_keywords:
- operator &=
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- '&= operator [Visual Basic]'
- compound assignment statements [Visual Basic]
ms.assetid: 0cf262fc-1a05-419a-a503-60013f111c8a
ms.openlocfilehash: db42f7be7225b866eacf5b73066754e91cd1a0f7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371980"
---
# <a name="amp-operator-visual-basic"></a>&amp;= Operador (Visual Basic)
Concatena uma `String` expressão a uma `String` variável ou propriedade e atribui o resultado à variável ou à propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
variableorproperty &= expression  
```  
  
## <a name="parts"></a>Partes  
 `variableorproperty`  
 Obrigatórios. Qualquer `String` variável ou propriedade.  
  
 `expression`  
 Obrigatórios. Qualquer expressão de `String` .  
  
## <a name="remarks"></a>Comentários  
 O elemento no lado esquerdo do `&=` operador pode ser uma variável escalar simples, uma propriedade ou um elemento de uma matriz. A variável ou a propriedade não pode ser [ReadOnly](../modifiers/readonly.md). O `&=` operador concatena a `String` expressão à direita para a `String` variável ou propriedade à esquerda e atribui o resultado à variável ou à propriedade à esquerda.  
  
## <a name="overloading"></a>Sobrecarga  
 O [operador&](concatenation-operator.md) pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Sobrecarregar o `&` operador afeta o comportamento do `&=` operador. Se o seu código usa `&=` em uma classe ou estrutura que sobrecarrega, certifique-se de `&` entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `&=` operador para concatenar duas `String` variáveis e atribuir o resultado à primeira variável.  
  
 [!code-vb[VbVbalrOperators#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#3)]  
  
## <a name="see-also"></a>Confira também

- [Operador de&](concatenation-operator.md)
- [Operador + =](addition-assignment-operator.md)
- [Operadores de atribuição](assignment-operators.md)
- [Operadores de concatenação](concatenation-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Instruções](../../programming-guide/language-features/statements.md)
