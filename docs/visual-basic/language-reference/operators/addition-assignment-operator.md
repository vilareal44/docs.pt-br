---
title: Operador +=
ms.date: 07/20/2015
f1_keywords:
- vb.+=
helpviewer_keywords:
- += operator [Visual Basic]
- assignment statements [Visual Basic], compound
- statements [Visual Basic], compound assignment
- += operator [Visual Basic], appending strings
- compound assignment statements [Visual Basic]
ms.assetid: d3e959f4-85d4-4e47-87c4-77b62335a5b3
ms.openlocfilehash: c2ce384901a9f0207e8279a5a07a88600c875e7f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84372201"
---
# <a name="-operator-visual-basic"></a>Operador += (Visual Basic)
Adiciona o valor de uma expressão numérica ao valor de uma variável numérica ou propriedade e atribui o resultado à variável ou à propriedade. Também pode ser usado para concatenar uma `String` expressão a uma `String` variável ou propriedade e atribuir o resultado à variável ou à propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
variableorproperty += expression  
```  
  
## <a name="parts"></a>Partes  
 `variableorproperty`  
 Obrigatórios. Qualquer propriedade ou numérico ou `String` variável.  
  
 `expression`  
 Obrigatórios. Qualquer expressão ou numérica `String` .  
  
## <a name="remarks"></a>Comentários  
 O elemento no lado esquerdo do `+=` operador pode ser uma variável escalar simples, uma propriedade ou um elemento de uma matriz. A variável ou a propriedade não pode ser [ReadOnly](../modifiers/readonly.md).  
  
 O `+=` operador adiciona o valor à direita à variável ou à propriedade à esquerda e atribui o resultado à variável ou à propriedade à esquerda. O `+=` operador também pode ser usado para concatenar a `String` expressão à direita para a `String` variável ou propriedade à esquerda e atribuir o resultado à variável ou à propriedade à esquerda.  
  
> [!NOTE]
> Ao usar o `+=` operador, talvez você não consiga determinar se a adição ou a concatenação de cadeia de caracteres ocorrerá. Use o `&=` operador para concatenação para eliminar ambigüidade e fornecer código de autodocumentação.  
  
 Esse operador de atribuição executa implicitamente as conversões de alargamento, mas não de estreitamento se o ambiente de compilação impõe semântica estrita. Para obter mais informações sobre essas conversões, consulte [conversões de alargamento e estreitamento](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md). Para obter mais informações sobre semântica estrita e permissiva, consulte [Option Strict Statement](../statements/option-strict-statement.md).  
  
 Se a semântica permissiva for permitida, o `+=` operador executará implicitamente uma variedade de cadeias de caracteres e conversões numéricas idênticas àquelas executadas pelo `+` operador. Para obter detalhes sobre essas conversões, consulte [operador +](addition-operator.md).  
  
## <a name="overloading"></a>Sobrecarga  
 O `+` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Sobrecarregar o `+` operador afeta o comportamento do `+=` operador. Se o seu código usa `+=` em uma classe ou estrutura que sobrecarrega, certifique-se de `+` entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `+=` operador para combinar o valor de uma variável com outra. A primeira parte usa `+=` com variáveis numéricas para adicionar um valor a outro. A segunda parte usa `+=` com `String` variáveis para concatenar um valor com outro. Em ambos os casos, o resultado é atribuído à primeira variável.  
  
 [!code-vb[VbVbalrOperators#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#7)]  
  
 [!code-vb[VbVbalrOperators#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#8)]  
  
 O valor de `num1` é agora 13, e o valor de `str1` é agora "103".  
  
## <a name="see-also"></a>Confira também

- [Operador +](addition-operator.md)
- [Operadores de atribuição](assignment-operators.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Operadores de concatenação](concatenation-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Instruções](../../programming-guide/language-features/statements.md)
