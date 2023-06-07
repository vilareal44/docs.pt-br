---
title: Operador Mod
ms.date: 04/24/2018
f1_keywords:
- vb.Mod
helpviewer_keywords:
- remainder (Mod operator)
- division operator [Visual Basic], Mod operator
- modulus operator [Visual Basic], Visual Basic
- Mod operator [Visual Basic]
- operators [Visual Basic], division
- arithmetic operators [Visual Basic], Mod
- math operators [Visual Basic]
ms.assetid: 6ff7e40e-cec8-4c77-bff6-8ddd2791c25b
ms.openlocfilehash: 32065567799b023556a018ae2f5ba338796e0b49
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401505"
---
# <a name="mod-operator-visual-basic"></a>Operador Mod (Visual Basic)

Divide dois números e retorna apenas o resto.

## <a name="syntax"></a>Sintaxe

```vb
result = number1 Mod number2
```

## <a name="parts"></a>Partes

`result` \
Obrigatórios. Qualquer variável numérica ou propriedade.

`number1` \
Obrigatórios. Qualquer expressão numérica.

`number2` \
Obrigatórios. Qualquer expressão numérica.

## <a name="supported-types"></a>Tipos com suporte

Todos os tipos numéricos. Isso inclui os tipos de ponto flutuante e não assinados e `Decimal` .

## <a name="result"></a>Result

O resultado é o resto após `number1` ser dividido por `number2` . Por exemplo, a expressão `14 Mod 4` é avaliada como 2.

> [!NOTE]
> Há uma diferença entre *resto* e *módulo* em matemática, com resultados diferentes para números negativos. O `Mod` operador em Visual Basic, o operador de .NET Framework `op_Modulus` e a instrução de Il de [REM](<xref:System.Reflection.Emit.OpCodes.Rem>) subjacente All executam uma operação pendente.

O resultado de uma `Mod` operação retém o sinal do dividendo, `number1` e, portanto, pode ser positivo ou negativo. O resultado está sempre no intervalo (- `number2` , `number2` ), exclusivo. Por exemplo:

```vb
Public Module Example
   Public Sub Main()
      Console.WriteLine($" 8 Mod  3 = {8 Mod 3}")
      Console.WriteLine($"-8 Mod  3 = {-8 Mod 3}")
      Console.WriteLine($" 8 Mod -3 = {8 Mod -3}")
      Console.WriteLine($"-8 Mod -3 = {-8 Mod -3}")
   End Sub
End Module
' The example displays the following output:
'       8 Mod  3 = 2
'      -8 Mod  3 = -2
'       8 Mod -3 = 2
'      -8 Mod -3 = -2
```

## <a name="remarks"></a>Comentários

Se `number1` ou `number2` for um valor de ponto flutuante, o restante de ponto flutuante da divisão será retornado. O tipo de dados do resultado é o menor tipo de dados que pode conter todos os valores possíveis que resultam da divisão com os tipos de dados de `number1` e `number2` .

Se `number1` ou `number2` for avaliado como [Nothing](../nothing.md), ele será tratado como zero.

Os operadores relacionados incluem o seguinte:

- O [operador \ (Visual Basic)](integer-division-operator.md) retorna o quociente inteiro de uma divisão. Por exemplo, a expressão `14 \ 4` é avaliada como 3.

- O [operador/(Visual Basic)](floating-point-division-operator.md) retorna o quociente completo, incluindo o resto, como um número de ponto flutuante. Por exemplo, a expressão `14 / 4` é avaliada como 3,5.

## <a name="attempted-division-by-zero"></a>Tentativa de divisão por zero

Se `number2` for avaliada como zero, o comportamento do `Mod` operador dependerá do tipo de dados dos operandos:

- Uma divisão integral gera uma <xref:System.DivideByZeroException> exceção se `number2` não puder ser determinada em tempo de compilação e gerará um erro em tempo de compilação `BC30542 Division by zero occurred while evaluating this expression` se `number2` for avaliado como zero em tempo de compilação.
- Uma divisão de ponto flutuante retorna <xref:System.Double.NaN?displayProperty=nameWithType> .

## <a name="equivalent-formula"></a>Fórmula equivalente

A expressão `a Mod b` é equivalente a qualquer uma das seguintes fórmulas:

`a - (b * (a \ b))`

`a - (b * Fix(a / b))`

## <a name="floating-point-imprecision"></a>Irprecisão de ponto flutuante

Quando você trabalha com números de ponto flutuante, lembre-se de que eles nem sempre têm uma representação decimal precisa na memória. Isso pode levar a resultados inesperados de determinadas operações, como comparação de valor e o `Mod` operador. Para obter mais informações, consulte [Solucionando problemas de tipos de dados](../../programming-guide/language-features/data-types/troubleshooting-data-types.md).

## <a name="overloading"></a>Sobrecarga

O `Mod` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento. Se o seu código se aplicar `Mod` a uma instância de uma classe ou estrutura que inclua tal sobrecarga, certifique-se de entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).

## <a name="example"></a>Exemplo

O exemplo a seguir usa o `Mod` operador para dividir dois números e retornar apenas o resto. Se um dos números for um número de ponto flutuante, o resultado será um número de ponto flutuante que representa o restante.

[!code-vb[VbVbalrOperators#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#31)]

## <a name="example"></a>Exemplo

O exemplo a seguir demonstra a possível imprecisão de operandos de ponto flutuante. Na primeira instrução, os operandos são `Double` e 0,2 é uma fração binária infinitamente repetida com um valor armazenado de 0.20000000000000001. Na segunda instrução, o caractere de tipo literal `D` força ambos os operandos para `Decimal` e 0,2 tem uma representação precisa.

[!code-vb[VbVbalrOperators#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#32)]

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Conversion.Int%2A>
- <xref:Microsoft.VisualBasic.Conversion.Fix%2A>
- [Operadores aritméticos](arithmetic-operators.md)
- [Precedência do operador no Visual Basic](operator-precedence.md)
- [Operadores Listados por Funcionalidade](operators-listed-by-functionality.md)
- [Solução de problemas de tipos de dados](../../programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [Operadores aritméticos no Visual Basic](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
- [Operador \ (Visual Basic)](integer-division-operator.md)
