---
title: Operador AndAlso
ms.date: 07/20/2015
f1_keywords:
- vb.AndAlso
- AndAlso
helpviewer_keywords:
- short-circuiting
- AndAlso operator [Visual Basic]
- operators [Visual Basic], short-circuiting
- operators [Visual Basic], conjunction
- short-circuit evaluation
ms.assetid: bbc15191-b374-495b-9b8f-7b8c2f4388eb
ms.openlocfilehash: 8b67897956a21d06d465cf206856354d2e3f9d68
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371928"
---
# <a name="andalso-operator-visual-basic"></a>Operador AndAlso (Visual Basic)
Executa uma conjunção lógica de curto-circuito em duas expressões.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb
result = expression1 AndAlso expression2  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`result`|Obrigatórios. Qualquer expressão de `Boolean` . O resultado é o `Boolean` resultado da comparação entre as duas expressões.|  
|`expression1`|Obrigatórios. Qualquer expressão de `Boolean` .|  
|`expression2`|Obrigatórios. Qualquer expressão de `Boolean` .|  
  
## <a name="remarks"></a>Comentários  
 Uma operação lógica é chamada de *curto-circuito* se o código compilado puder ignorar a avaliação de uma expressão, dependendo do resultado de outra expressão. Se o resultado da primeira expressão avaliada determinar o resultado final da operação, não será necessário avaliar a segunda expressão, pois ela não pode alterar o resultado final. O curto circuito pode melhorar o desempenho se a expressão ignorada for complexa ou se envolver chamadas de procedimento.  
  
 Se as duas expressões forem avaliadas como `True` , `result` será `True` . A tabela a seguir ilustra como o `result` é determinado.  
  
|Se `expression1` for |E `expression2` é|O valor de `result` é|  
|---|---|---|  
|`True`|`True`|`True`|  
|`True`|`False`|`False`|  
|`False`|(não avaliado)|`False`|  
  
## <a name="data-types"></a>Tipos de dados  
 O `AndAlso` operador é definido somente para o [tipo de dados booliano](../data-types/boolean-data-type.md). Visual Basic converte cada operando conforme necessário `Boolean` antes de avaliar a expressão. Se você atribuir o resultado a um tipo numérico, Visual Basic o converterá de `Boolean` para esse tipo, isso `False` se tornará `0` e `True` se tornará `-1` .
Para obter mais informações, consulte [conversões de tipo booliano](../data-types/boolean-data-type.md#type-conversions).
  
## <a name="overloading"></a>Sobrecarga  
 O [operador and](and-operator.md) e o [Operador IsFalse](isfalse-operator.md) podem ser *sobrecarregados*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Sobrecarregar os `And` operadores e `IsFalse` afeta o comportamento do `AndAlso` operador. Se o seu código usa `AndAlso` em uma classe ou estrutura que sobrecarrega `And` e, certifique-se de `IsFalse` entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `AndAlso` operador para executar uma conjunção lógica em duas expressões. O resultado é um `Boolean` valor que representa se a expressão conjunta inteira é verdadeira. Se a primeira expressão for `False` , a segunda não será avaliada.  
  
 [!code-vb[VbVbalrOperators#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#24)]  
  
 O exemplo anterior produz resultados de `True` , `False` , e `False` , respectivamente. No cálculo de `secondCheck` , a segunda expressão não é avaliada porque a primeira já é `False` . No entanto, a segunda expressão é avaliada no cálculo de `thirdCheck` .  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra um `Function` procedimento que pesquisa um determinado valor entre os elementos de uma matriz. Se a matriz estiver vazia ou se o comprimento da matriz tiver sido excedido, a `While` instrução não testará o elemento de matriz em relação ao valor de pesquisa.  
  
 [!code-vb[VbVbalrOperators#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#25)]  
  
## <a name="see-also"></a>Confira também

- [Operadores lógicos/bit a bit (Visual Basic)](logical-bitwise-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Operador And](and-operator.md)
- [Operador IsFalse](isfalse-operator.md)
- [Operadores lógicos e bit a bit no Visual Basic](../../programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
