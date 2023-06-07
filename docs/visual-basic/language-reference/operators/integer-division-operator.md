---
title: Operador \
ms.date: 07/20/2015
f1_keywords:
- vb.\
- '\'
helpviewer_keywords:
- division operator [Visual Basic], integer
- integer division operator [Visual Basic]
- zero, division by zero
- arithmetic operators [Visual Basic], division
- division [Visual Basic], by zero
- backslash (\) [Visual Basic]
- '\ operator [Visual Basic]'
- integer quotient
- math operators [Visual Basic]
- quotients, integer
- truncation [Visual Basic], integer division
ms.assetid: 4b0ee347-950c-45c9-8e23-54bc85df208e
ms.openlocfilehash: 1f8095afc5f096928b946607adc715af49827022
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84370876"
---
# <a name="-operator-visual-basic"></a>Operador \ (Visual Basic)
Divide dois números e retorna um resultado de número inteiro.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
expression1 \ expression2  
```  
  
## <a name="parts"></a>Partes  
 `expression1`  
 Obrigatórios. Qualquer expressão numérica.  
  
 `expression2`  
 Obrigatórios. Qualquer expressão numérica.  
  
## <a name="supported-types"></a>Tipos com suporte  
 Todos os tipos numéricos, incluindo os tipos de ponto flutuante e não assinados e `Decimal` .  
  
## <a name="result"></a>Result  
 O resultado é o quociente inteiro de `expression1` dividido por `expression2` , que descarta qualquer restante e retém apenas a parte inteira. Isso é conhecido como *truncamento*.  
  
 O tipo de dados de resultado é um tipo numérico apropriado para os tipos de dados de `expression1` e `expression2` . Consulte as tabelas "aritmética de inteiros" em [tipos de dados de resultados do operador](data-types-of-operator-results.md).  
  
 O [operador/(Visual Basic)](floating-point-division-operator.md) retorna o quociente completo, que retém o restante na parte fracionária.  
  
## <a name="remarks"></a>Comentários  
 Antes de executar a divisão, Visual Basic tenta converter qualquer expressão numérica de ponto flutuante para `Long` . Se `Option Strict` for `On` , ocorrerá um erro do compilador. Se `Option Strict` for `Off` , um <xref:System.OverflowException> será possível se o valor estiver fora do intervalo do [tipo de dados Long](../data-types/long-data-type.md). A conversão em `Long` também está sujeita ao *arredondamento do banco*. Para obter mais informações, consulte "partes fracionárias" em [funções de conversão de tipo](../functions/type-conversion-functions.md).  
  
 Se `expression1` ou `expression2` for avaliado como [Nothing](../nothing.md), ele será tratado como zero.  
  
## <a name="attempted-division-by-zero"></a>Tentativa de divisão por zero  
 Se `expression2` for avaliada como zero, o `\` operador lançará uma <xref:System.DivideByZeroException> exceção. Isso é verdadeiro para todos os tipos de dados numéricos dos operandos.  
  
> [!NOTE]
> O `\` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Se o seu código usar esse operador em uma classe ou estrutura desse tipo, certifique-se de entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `\` operador para executar a divisão de inteiro. O resultado é um inteiro que representa o quociente inteiro dos dois operandos, com o restante Descartado.  
  
 [!code-vb[VbVbalrOperators#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#18)]  
  
 As expressões no exemplo anterior retornam valores de 2, 3, 33 e-22, respectivamente.  
  
## <a name="see-also"></a>Confira também

- [\\= Operador](integer-division-assignment-operator.md)
- [Operador/(Visual Basic)](floating-point-division-operator.md)
- [Instrução Option Strict](../statements/option-strict-statement.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Operadores aritméticos no Visual Basic](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
