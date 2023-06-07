---
title: Operador Not
ms.date: 07/20/2015
f1_keywords:
- vb.Not
helpviewer_keywords:
- Boolean expressions, negating
- operators [Visual Basic], bitwise
- negation operator [Visual Basic]
- inverse bit values in variables [Visual Basic]
- bitwise operators [Visual Basic], NOT operator
- bitwise comparison [Visual Basic]
- Not operator [Visual Basic]
- logical negation
- operators [Visual Basic], negation
ms.assetid: 8f2ea83c-d2ed-480a-a474-3042a1cad9b5
ms.openlocfilehash: 56cdeb80a217dbce15921eddd6a43d8d1b049376
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401453"
---
# <a name="not-operator-visual-basic"></a>Operador Not (Visual Basic)
Executa uma negação lógica em uma `Boolean` expressão ou uma negação de bits em uma expressão numérica.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
result = Not expression  
```  
  
## <a name="parts"></a>Partes  
 `result`  
 Obrigatórios. Qualquer `Boolean` ou expressão numérica.  
  
 `expression`  
 Obrigatórios. Qualquer `Boolean` ou expressão numérica.  
  
## <a name="remarks"></a>Comentários  
 Para `Boolean` expressões, a tabela a seguir ilustra como o `result` é determinado.  
  
|Se `expression` for |O valor de `result` é|  
|------------------------|------------------------------|  
|`True`|`False`|  
|`False`|`True`|  
  
 Para expressões numéricas, o `Not` operador inverte os valores de bit de qualquer expressão numérica e define o bit correspondente de `result` acordo com a tabela a seguir.  
  
|Se o bit in `expression` for|O bit em `result` é|  
|-------------------------------|----------------------------|  
|1|0|  
|0|1|  
  
> [!NOTE]
> Uma vez que os operadores lógicos e bit-a-or têm uma precedência mais baixa do que outros operadores aritméticos e relacionais, todas as operações de bit-nte devem ser colocadas entre parênteses para garantir a execução  
  
## <a name="data-types"></a>Tipos de dados  
 Para uma negação booliana, o tipo de dados do resultado é `Boolean` . Para uma negação de bit que não é possível, o tipo de dados de resultado é o mesmo do `expression` . No entanto, se expression for `Decimal` , o resultado será `Long` .  
  
## <a name="overloading"></a>Sobrecarga  
 O `Not` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando seu operando tem o tipo dessa classe ou estrutura. Se o seu código usar esse operador em uma classe ou estrutura desse tipo, certifique-se de entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `Not` operador para executar a negação lógica em uma `Boolean` expressão. O resultado é um `Boolean` valor que representa o inverso do valor da expressão.  
  
 [!code-vb[VbVbalrOperators#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#33)]  
  
 O exemplo anterior produz resultados de `False` e `True` , respectivamente.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `Not` operador para executar a negação lógica dos bits individuais de uma expressão numérica. O bit no padrão de resultado é definido como o inverso do bit correspondente no padrão de operando, incluindo o bit de sinal.  
  
 [!code-vb[VbVbalrOperators#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#34)]  
  
 O exemplo anterior produz resultados de – 11, – 9 e – 7, respectivamente.  
  
## <a name="see-also"></a>Confira também

- [Operadores lógicos/bit a bit (Visual Basic)](logical-bitwise-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Operadores lógicos e bit a bit no Visual Basic](../../programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
