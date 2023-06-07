---
title: Operador And
ms.date: 07/20/2015
f1_keywords:
- vb.And
helpviewer_keywords:
- operators [Visual Basic], bitwise
- logical conjunction
- bitwise AND operator [Visual Basic]
- conjunction operator [Visual Basic]
- And operator [Visual Basic]
- bitwise operators [Visual Basic], AND operator
- operators [Visual Basic], conjunction
- bitwise comparison [Visual Basic]
ms.assetid: 2ea711f3-439a-4c7c-9e3a-1ffe3b0d6046
ms.openlocfilehash: c2b135d27e14816c011a4f70793543aa835d960a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371941"
---
# <a name="and-operator-visual-basic"></a>Operador And (Visual Basic)
Executa uma conjunção lógica em duas `Boolean` expressões, ou uma conjunção bit a bit em duas expressões numéricas.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
result = expression1 And expression2  
```  
  
## <a name="parts"></a>Partes  
 `result`  
 Obrigatórios. Qualquer `Boolean` ou expressão numérica. Para comparação booliana, `result` é a conjunção lógica de dois `Boolean` valores. No caso de operações bit-a-bit, `result` é um valor numérico que representa a conjunção de bit de dois padrões de bit numérico.  
  
 `expression1`  
 Obrigatórios. Qualquer `Boolean` ou expressão numérica.  
  
 `expression2`  
 Obrigatórios. Qualquer `Boolean` ou expressão numérica.  
  
## <a name="remarks"></a>Comentários  
 Para comparação booliana, `result` é `True` se e somente se for `expression1` e `expression2` for avaliado como `True` . A tabela a seguir ilustra como o `result` é determinado.  
  
|Se `expression1` for |E `expression2` é|O valor de `result` é|  
|-------------------------|--------------------------|------------------------------|  
|`True`|`True`|`True`|  
|`True`|`False`|`False`|  
|`False`|`True`|`False`|  
|`False`|`False`|`False`|  
  
> [!NOTE]
> Em uma comparação Booleana, o `And` operador sempre avalia as duas expressões, o que pode incluir fazer chamadas de procedimento. O [Operador AndAlso](andalso-operator.md) executa um *curto-circuito*, o que significa que `expression1` , se for `False` , `expression2` não será avaliado.  
  
 Quando aplicado a valores numéricos, o `And` operador executa uma comparação bit a bit de bits posicionados de forma idêntica em duas expressões numéricas e define o bit correspondente de `result` acordo com a tabela a seguir.  
  
|Se o bit in `expression1` for|E o bit in `expression2` é|O bit em `result` é|  
|--------------------------------|---------------------------------|----------------------------|  
|1|1|1|  
|1|0|0|  
|0|1|0|  
|0|0|0|  
  
> [!NOTE]
> Uma vez que os operadores lógicos e bit-a-or têm uma precedência mais baixa do que outros operadores aritméticos e relacionais, todas as operações de bit-nte devem estar entre parênteses para garantir resultados precisos  
  
## <a name="data-types"></a>Tipos de dados  
 Se os operandos contiverem uma `Boolean` expressão e uma expressão numérica, Visual Basic converterá a `Boolean` expressão em um valor numérico (– 1 para `True` e 0 para `False` ) e executará uma operação bit a bit.  
  
 Para uma comparação booliana, o tipo de dados do resultado é `Boolean` . Para uma comparação de bits, o tipo de dados de resultado é um tipo numérico apropriado para os tipos de dados de `expression1` e `expression2` . Consulte a tabela "comparações relacionais e de bits-bit" em [tipos de dados de resultados do operador](data-types-of-operator-results.md).  
  
> [!NOTE]
> O `And` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Se o seu código usar esse operador em uma classe ou estrutura desse tipo, certifique-se de entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `And` operador para executar uma conjunção lógica em duas expressões. O resultado é um `Boolean` valor que representa se ambas as expressões são `True` .  
  
 [!code-vb[VbVbalrOperators#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#22)]  
  
 O exemplo anterior produz resultados de `True` e `False` , respectivamente.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `And` operador para executar uma conjunção lógica em bits individuais de duas expressões numéricas. O bit no padrão de resultado será definido se os bits correspondentes nos operandos estiverem definidos como 1.  
  
 [!code-vb[VbVbalrOperators#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#23)]  
  
 O exemplo anterior produz resultados de 8, 2 e 0, respectivamente.  
  
## <a name="see-also"></a>Confira também

- [Operadores lógicos/bit a bit (Visual Basic)](logical-bitwise-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Operador AndAlso](andalso-operator.md)
- [Operadores lógicos e bit a bit no Visual Basic](../../programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
