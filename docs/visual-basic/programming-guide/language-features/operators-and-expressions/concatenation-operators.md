---
title: Operadores de concatenação
ms.date: 07/20/2015
helpviewer_keywords:
- '& operator [Visual Basic], concatenation'
- concatenation operators [Visual Basic]
- operators [Visual Basic], concatenation
- Visual Basic code, operators
- + operator [Visual Basic], concatenation
- concatenation operators [Visual Basic]
ms.assetid: e59908c3-89e0-41ae-933d-3e8826c16a04
ms.openlocfilehash: c123438a86a2c3293a99770107d970535fcdbdf8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388784"
---
# <a name="concatenation-operators-in-visual-basic"></a>Operadores de concatenação no Visual Basic

Os operadores de concatenação unem várias cadeias de caracteres em uma única cadeia de caracteres. Existem dois operadores de concatenação, `+` e `&`. Ambos realizam a operação de concatenação básica, como mostra o exemplo a seguir.

```vb
Dim x As String = "Mic" & "ro" & "soft"
Dim y As String = "Mic" + "ro" + "soft"
' The preceding statements set both x and y to "Microsoft".
```

Esses operadores também podem concatenar variáveis `String`, como mostra o exemplo a seguir.

[!code-vb[VbVbalrOperators#76](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#76)]

## <a name="differences-between-the-two-concatenation-operators"></a>Diferenças entre os dois operadores de concatenação

O [operador +](../../../language-reference/operators/addition-operator.md) tem a finalidade principal de adicionar dois números. Entretanto, ele pode também concatenar operandos numéricos com operandos de cadeia de caracteres. O operador `+` possui um conjunto complexo de regras que determinam se adicionam, concatenam, sinalizam um erro do compilador ou emitem uma exceção <xref:System.InvalidCastException> de tempo de execução.

O [operador&](../../../language-reference/operators/concatenation-operator.md) é definido somente para `String` operandos e sempre amplia seus operandos para `String` , independentemente da configuração de `Option Strict` . O operador `&` é recomendado para concatenação de cadeia de caracteres por ser definido exclusivamente para cadeias de caracteres e reduz suas chances de gerar uma conversão indesejada.

## <a name="performance-string-and-stringbuilder"></a>Desempenho: String e StringBuilder

Se você realizar um número significativo de manipulações em uma cadeia de caracteres, como concatenações, exclusões e substituições, seu desempenho poderá se beneficiar da classe <xref:System.Text.StringBuilder> no namespace <xref:System.Text>. Ela usa uma instrução extra para criar e inicializar um objeto <xref:System.Text.StringBuilder> e outra instrução para converter seu valor final em uma `String`, mas você pode recuperar esse tempo, pois <xref:System.Text.StringBuilder> pode ser executado com mais rapidez.

## <a name="see-also"></a>Confira também

- [Instrução Option Strict](../../../language-reference/statements/option-strict-statement.md)
- [Tipos de métodos de manipulação da cadeia de caracteres no Visual Basic](../strings/types-of-string-manipulation-methods.md)
- [Operadores aritméticos no Visual Basic](arithmetic-operators.md)
- [Operadores de comparação no Visual Basic](comparison-operators.md)
- [Operadores lógicos e bit a bit no Visual Basic](logical-and-bitwise-operators.md)
