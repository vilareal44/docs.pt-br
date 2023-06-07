---
title: Operador Or
ms.date: 07/20/2015
f1_keywords:
- vb.Or
helpviewer_keywords:
- Or operator [Visual Basic]
- operators [Visual Basic], bitwise
- inclusive Or operator [Visual Basic]
- bitwise operators [Visual Basic], OR operator
- Or keyword [Visual Basic]
- operators [Visual Basic], inclusive or
- operators [Visual Basic], disjunction
- bitwise comparison [Visual Basic]
- logical disjunction
- disjunction operator [Visual Basic]
ms.assetid: 41ed6905-bf3d-468a-9e3b-03c10d461891
ms.openlocfilehash: 657b2a473b23789a8ba25fbd11c6b83538fa7803
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401414"
---
# <a name="or-operator-visual-basic"></a>Operador Or (Visual Basic)
Executa uma disjunção lógica em duas `Boolean` expressões ou uma disjunção bit a bit em duas expressões numéricas.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
result = expression1 Or expression2  
```  
  
## <a name="parts"></a>Partes  
 `result`  
 Obrigatórios. Qualquer `Boolean` ou expressão numérica. Para `Boolean` comparação, `result` é a disjunção lógica inclusiva de dois `Boolean` valores. No caso de operações bit-ais, `result` é um valor numérico que representa a disjunção de bits inclusiva de dois padrões de bit numérico.  
  
 `expression1`  
 Obrigatórios. Qualquer `Boolean` ou expressão numérica.  
  
 `expression2`  
 Obrigatórios. Qualquer `Boolean` ou expressão numérica.  
  
## <a name="remarks"></a>Comentários  
 Para `Boolean` comparação, `result` é `False` se e somente se for `expression1` e `expression2` for avaliado como `False` . A tabela a seguir ilustra como o `result` é determinado.  
  
|Se `expression1` for |E `expression2` é|O valor de `result` é|  
|-------------------------|--------------------------|------------------------------|  
|`True`|`True`|`True`|  
|`True`|`False`|`True`|  
|`False`|`True`|`True`|  
|`False`|`False`|`False`|  
  
> [!NOTE]
> Em uma `Boolean` comparação, o `Or` operador sempre avalia as duas expressões, o que pode incluir fazer chamadas de procedimento. O [Operador OrElse](orelse-operator.md) executa o *curto-circuito*, o que significa que `expression1` , se for `True` , `expression2` não será avaliado.  
  
 No caso de operações bit-a-bit, o `Or` operador executa uma comparação de bits com posição idêntica em duas expressões numéricas e define o bit correspondente de `result` acordo com a tabela a seguir.  
  
|Se o bit in `expression1` for|E o bit in `expression2` é|O bit em `result` é|  
|--------------------------------|---------------------------------|----------------------------|  
|1|1|1|  
|1|0|1|  
|0|1|1|  
|0|0|0|  
  
> [!NOTE]
> Uma vez que os operadores lógicos e bit-a-or têm uma precedência mais baixa do que outros operadores aritméticos e relacionais, todas as operações de bit-nte devem ser colocadas entre parênteses para garantir a execução  
  
## <a name="data-types"></a>Tipos de dados  
 Se os operandos contiverem uma `Boolean` expressão e uma expressão numérica, Visual Basic converterá a `Boolean` expressão em um valor numérico (– 1 para `True` e 0 para `False` ) e executará uma operação bit a bit.  
  
 Para uma `Boolean` comparação, o tipo de dados do resultado é `Boolean` . Para uma comparação de bits, o tipo de dados de resultado é um tipo numérico apropriado para os tipos de dados de `expression1` e `expression2` . Consulte a tabela "comparações relacionais e de bits-bit" em [tipos de dados de resultados do operador](data-types-of-operator-results.md).  
  
## <a name="overloading"></a>Sobrecarga  
 O `Or` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Se o seu código usar esse operador em uma classe ou estrutura desse tipo, certifique-se de entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `Or` operador para executar uma disjunção lógica inclusiva em duas expressões. O resultado é um `Boolean` valor que representa se uma das duas expressões é `True` .  
  
 [!code-vb[VbVbalrOperators#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#35)]  
  
 O exemplo anterior produz resultados de `True` , `True` , e `False` , respectivamente.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `Or` operador para executar uma disjunção lógica inclusiva em bits individuais de duas expressões numéricas. O bit no padrão de resultado será definido se qualquer um dos bits correspondentes nos operandos for definido como 1.  
  
 [!code-vb[VbVbalrOperators#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#36)]  
  
 O exemplo anterior produz resultados de 10, 14 e 14, respectivamente.  
  
## <a name="see-also"></a>Confira também

- [Operadores lógicos/bit a bit (Visual Basic)](logical-bitwise-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Operador OrElse](orelse-operator.md)
- [Operadores lógicos e bit a bit no Visual Basic](../../programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
