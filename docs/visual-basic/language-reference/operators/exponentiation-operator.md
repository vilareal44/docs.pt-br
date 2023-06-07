---
title: Operador ^
ms.date: 07/20/2015
f1_keywords:
- vb.^
helpviewer_keywords:
- raising numbers to powers
- ^ operator [Visual Basic], exponentiation
- square operator [Visual Basic]
- ^ operator [Visual Basic]
- exponentiation operator [Visual Basic]
- exponent
- numbers [Visual Basic], rasing
- powers
- arithmetic operators [Visual Basic], exponentiation
ms.assetid: d89a1ca8-83da-4784-a87b-a9d7dceb3f62
ms.openlocfilehash: e139becf74ae367266a23513e18d0bdbdd24cdea
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371382"
---
# <a name="-operator-visual-basic"></a>Operador ^ (Visual Basic)

Eleva um número à potência de outro número.

## <a name="syntax"></a>Sintaxe

```vb
number ^ exponent
```

## <a name="parts"></a>Partes

`number`\
Obrigatórios. Qualquer expressão numérica.

`exponent`\
Obrigatórios. Qualquer expressão numérica.

## <a name="result"></a>Result

O resultado é `number` elevado à potência de `exponent` , sempre como um `Double` valor.

## <a name="supported-types"></a>Tipos com suporte

`Double`. Os operandos de qualquer tipo diferente são convertidos em `Double` .

## <a name="remarks"></a>Comentários

Visual Basic sempre executa exponenciação no [tipo de dados Double](../data-types/double-data-type.md).

O valor de `exponent` pode ser fracionário, negativo ou ambos.

Quando mais de uma exponenciação é executada em uma única expressão, o `^` operador é avaliado como é encontrado da esquerda para a direita.

> [!NOTE]
> O `^` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando um operando tem o tipo dessa classe ou estrutura. Se o seu código usar esse operador em uma classe ou estrutura desse tipo, certifique-se de entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).

## <a name="example"></a>Exemplo

O exemplo a seguir usa o `^` operador para elevar um número à potência de um expoente. O resultado é o primeiro operando elevado à potência do segundo.

[!code-vb[VbVbalrOperators#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#20)]

O exemplo anterior produz os seguintes resultados:

`exp1`é definido como 4 (2 ao quadrado).

`exp2`é definido como 19683 (3 cúbico, então esse valor cúbico).

`exp3`é definido como-125 (-5 cúbico).

`exp4`é definido como 625 (-5 para a quarta potência).

`exp5`é definido como 2 (a raiz do cubo de 8).

`exp6`é definido como 0,5 (1,0 dividido pela raiz do cubo de 8).

Observe a importância dos parênteses nas expressões no exemplo anterior. Devido à *precedência de operador*, Visual Basic normalmente executa o `^` operador antes de qualquer outro, até mesmo o operador unário `–` . Se `exp4` e `exp6` tiver sido calculado sem parênteses, eles produziram os seguintes resultados:

`exp4 = -5 ^ 4`seria calculado como – (5 para a quarta potência), que resultaria em-625.

`exp6 = 8 ^ -1.0 / 3.0`seria calculado como (8 para a potência – 1 ou 0,125) dividido por 3,0, o que resultaria em 0.041666666666666666666666666666667.

## <a name="see-also"></a>Confira também

- [Operador ^ =](exponentiation-assignment-operator.md)
- [Operadores aritméticos](arithmetic-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Operadores aritméticos no Visual Basic](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
