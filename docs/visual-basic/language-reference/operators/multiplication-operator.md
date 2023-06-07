---
title: '* Operador'
ms.date: 07/20/2015
f1_keywords:
- vb.*
helpviewer_keywords:
- arithmetic operators [Visual Basic], multiplication
- operators [Visual Basic], multiplication
- '* operator [Visual Basic]'
- multiplication operator [Visual Basic], syntax
- math operators [Visual Basic]
ms.assetid: 2b210382-99da-4195-89ba-b1d06f5e89ad
ms.openlocfilehash: f1a7653fb3006ab3c9736ec168a8c5ea028f4763
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409304"
---
# <a name="-operator-visual-basic"></a>Operador * (Visual Basic)
Multiplica dois números.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
number1 * number2  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`number1`|Obrigatórios. Qualquer expressão numérica.|  
|`number2`|Obrigatórios. Qualquer expressão numérica.|  
  
## <a name="result"></a>Result  
 O resultado é o produto do `number1` e do `number2` .  
  
## <a name="supported-types"></a>Tipos com suporte  
 Todos os tipos numéricos, incluindo os tipos de ponto flutuante e não assinados e `Decimal` .  
  
## <a name="remarks"></a>Comentários  
 O tipo de dados do resultado depende dos tipos dos operandos. A tabela a seguir mostra como o tipo de dados do resultado é determinado.  
  
|Tipos de dados do operando|Tipo de dados de resultado|  
|---|---|  
|Ambas as expressões são tipos de dados integral ([SByte](../data-types/sbyte-data-type.md), [byte](../data-types/byte-data-type.md), [Short](../data-types/short-data-type.md), [UShort](../data-types/ushort-data-type.md), [Integer](../data-types/integer-data-type.md), [UInteger](../data-types/uinteger-data-type.md), [Long](../data-types/long-data-type.md), [ULONG](../data-types/ulong-data-type.md))|Um tipo de dados numérico apropriado para os tipos de dados do `number1` e do `number2` . Consulte as tabelas "aritmética de inteiros" em [tipos de dados de resultados do operador](data-types-of-operator-results.md).|  
|As duas expressões são [decimal](../data-types/decimal-data-type.md)|`Decimal`|  
|Ambas as expressões são [únicas](../data-types/single-data-type.md)|`Single`|  
|Uma das expressões é um tipo de dados de ponto flutuante ( `Single` ou [duplo](../data-types/double-data-type.md)), mas não ambos `Single` (Observe que `Decimal` não é um tipo de dados de ponto flutuante)|`Double`|  
  
 Se uma expressão for avaliada como [Nothing](../nothing.md), ela será tratada como zero.  
  
## <a name="overloading"></a>Sobrecarga  
 O `*` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Se o seu código usar esse operador em uma classe ou estrutura desse tipo, certifique-se de entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa o `*` operador para multiplicar dois números. O resultado é o produto dos dois operandos.  
  
 [!code-vb[VbVbalrOperators#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#4)]  
  
## <a name="see-also"></a>Confira também

- [Operador * =](multiplication-assignment-operator.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Operadores aritméticos no Visual Basic](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
