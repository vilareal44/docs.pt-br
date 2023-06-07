---
title: Tipo de Dados Decimal
ms.date: 07/20/2015
f1_keywords:
- vb.Decimal
helpviewer_keywords:
- literal type characters [Visual Basic], D
- trailing zeros
- real numbers
- trailing 0 characters [Visual Basic]
- Decimal data type
- D literal type character [Visual Basic]
- decimals, Decimal data type
- 0 characters [Visual Basic], trailing
- data types [Visual Basic], assigning
- decimal keyword [Visual Basic]
- numbers [Visual Basic], real
- variable-precision numbers
- zeros, trailing
- '@ identifier type character'
- identifier type characters [Visual Basic], @
ms.assetid: 1d855b45-afe2-45b0-a623-96b6f63a43d5
ms.openlocfilehash: 690c8061b6df1115aa24668520170b44edfa8287
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415641"
---
# <a name="decimal-data-type-visual-basic"></a>Tipo de dados decimal (Visual Basic)

Mantém valores de 128 bits (16 bytes) assinados que representam números inteiros de 96 bits (12 bytes) elevados a uma potência variável de 10. O fator de dimensionamento especifica o número de dígitos à direita do ponto decimal; Ele varia de 0 a 28. Com uma escala de 0 (sem casas decimais), o maior valor possível é +/-79228162514264337593543950335 (+/-7.9228162514264337593543950335E + 28). Com 28 casas decimais, o maior valor é +/-7.9228162514264337593543950335 e o menor valor diferente de zero é +/-0, 1 (+/-1E-28).

## <a name="remarks"></a>Comentários

O `Decimal` tipo de dados fornece o maior número de dígitos significativos para um número. Ele dá suporte a até 29 dígitos significativos e pode representar valores acima de 7,9228 x 10 ^ 28. Ele é particularmente adequado para cálculos, como financeiro, que exigem um grande número de dígitos, mas não podem tolerar erros de arredondamento.

O valor padrão de `Decimal` é 0.

## <a name="programming-tips"></a>Dicas de programação

- **Preciso.** `Decimal`Não é um tipo de dados de ponto flutuante. A `Decimal` estrutura contém um valor inteiro binário, junto com um bit de sinal e um fator de dimensionamento inteiro que especifica qual parte do valor é uma fração decimal. Por isso, os `Decimal` números têm uma representação mais precisa na memória do que os tipos de ponto flutuante ( `Single` e `Double` ).

- **Desempenho.** O `Decimal` tipo de dados é o mais lento de todos os tipos numéricos. Você deve avaliar a importância da precisão em relação ao desempenho antes de escolher um tipo de dados.

- **Ampliação.** O `Decimal` tipo de dados amplia para `Single` ou `Double` . Isso significa que você pode converter `Decimal` para qualquer um desses tipos sem encontrar um <xref:System.OverflowException?displayProperty=nameWithType> erro.

- **Zeros à direita.** Visual Basic não armazena zeros à direita em um `Decimal` literal. No entanto, uma `Decimal` variável preserva os zeros à direita adquiridos de computação. O exemplo a seguir ilustra isto.

  ```vb
  Dim d1, d2, d3, d4 As Decimal
  d1 = 2.375D
  d2 = 1.625D
  d3 = d1 + d2
  d4 = 4.000D
  MsgBox("d1 = " & CStr(d1) & ", d2 = " & CStr(d2) &
        ", d3 = " & CStr(d3) & ", d4 = " & CStr(d4))
  ```

  A saída do `MsgBox` no exemplo anterior é a seguinte:

  ```console
  d1 = 2.375, d2 = 1.625, d3 = 4.000, d4 = 4
  ```

- **Digite os caracteres.** Acrescentar o caractere de tipo literal `D` a um literal o força ao tipo de dados `Decimal`. Acrescentar o caractere de tipo identificador `@` a qualquer identificador o força ao tipo `Decimal`.

- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a estrutura <xref:System.Decimal?displayProperty=nameWithType>.

## <a name="range"></a>Intervalo

 Talvez seja necessário usar o `D` caractere de tipo para atribuir um valor grande a uma `Decimal` variável ou constante. Esse requisito é porque o compilador interpreta um literal como `Long` a menos que um caractere de tipo literal siga o literal, como mostra o exemplo a seguir.

```vb
Dim bigDec1 As Decimal = 9223372036854775807   ' No overflow.
Dim bigDec2 As Decimal = 9223372036854775808   ' Overflow.
Dim bigDec3 As Decimal = 9223372036854775808D  ' No overflow.
```

A declaração para `bigDec1` não produz um estouro porque o valor atribuído a ele está dentro do intervalo de `Long` . O `Long` valor pode ser atribuído à `Decimal` variável.

A declaração para `bigDec2` gera um erro de estouro porque o valor atribuído a ele é muito grande para `Long` . Como o literal numérico não pode ser interpretado primeiro como um `Long` , ele não pode ser atribuído à `Decimal` variável.

Para `bigDec3` , o caractere de tipo literal `D` resolve o problema forçando o compilador a interpretar o literal como um `Decimal` , em vez de como um `Long` .

## <a name="see-also"></a>Confira também

- <xref:System.Decimal?displayProperty=nameWithType>
- <xref:System.Decimal.%23ctor%2A>
- <xref:System.Math.Round%2A?displayProperty=nameWithType>
- [Tipos de dados](index.md)
- [Tipo de Dados Simples](single-data-type.md)
- [Tipo de Dados Duplo](double-data-type.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
