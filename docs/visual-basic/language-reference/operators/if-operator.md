---
title: Operador If
ms.date: 07/20/2015
f1_keywords:
- vb.IfOperator
- IfOperator
helpviewer_keywords:
- ternary operators [Visual Basic]
- conditional execution
- If expressions [Visual Basic]
- conditional operator [Visual Basic]
- If Operator [Visual Basic]
ms.assetid: dd56c9df-7cd4-442c-9ba6-20c70ee44c8f
ms.openlocfilehash: 28fb2afb2c4cf78ffbbb028145de647a8dc512ed
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84371097"
---
# <a name="if-operator-visual-basic"></a>Operador If (Visual Basic)

Usa a avaliação de circuito curto para retornar condicionalmente um dos dois valores. O `If` operador pode ser chamado com três argumentos ou com dois argumentos.

## <a name="syntax"></a>Sintaxe

```vb
If( [argument1,] argument2, argument3 )
```

## <a name="if-operator-called-with-three-arguments"></a>Operador If chamado com três argumentos

Quando `If` é chamado usando três argumentos, o primeiro argumento deve ser avaliado como um valor que pode ser convertido como um `Boolean` . Esse `Boolean` valor determinará quais dos outros dois argumentos serão avaliados e retornados. A lista a seguir se aplica somente quando o `If` operador é chamado usando três argumentos.

### <a name="parts"></a>Partes

|Termo|Definição|
|---|---|
|`argument1`|Obrigatórios. `Boolean`. Determina quais dos outros argumentos a serem avaliados e retornados.|
|`argument2`|Obrigatórios. `Object`. Avaliado e retornado se `argument1` for avaliado como `True` .|
|`argument3`|Obrigatórios. `Object`. Avaliado e retornado se `argument1` for avaliado como `False` ou se `argument1` for uma [Nullable](../../programming-guide/language-features/data-types/nullable-value-types.md) `Boolean` variável anulável que é avaliada como [Nothing](../nothing.md).|

Um `If` operador chamado com três argumentos funciona como uma `IIf` função, exceto pelo fato de usar a avaliação de curto-circuito. Uma `IIf` função sempre avalia todos os três argumentos, enquanto `If` que um operador que tem três argumentos avalia apenas dois deles. O primeiro `If` argumento é avaliado e o resultado é cast como um `Boolean` valor, `True` ou `False` . Se o valor for `True` , `argument2` será avaliado e seu valor será retornado, mas `argument3` não será avaliado. Se o valor da `Boolean` expressão for `False` , `argument3` será avaliado e seu valor será retornado, mas `argument2` não será avaliado. Os exemplos a seguir ilustram o uso de `If` quando três argumentos são usados:

[!code-vb[VbVbalrOperators#100](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class4.vb#100)]

O exemplo a seguir ilustra o valor da avaliação de circuito curto. O exemplo mostra duas tentativas de dividir variável `number` por variável `divisor` , exceto quando `divisor` é zero. Nesse caso, um 0 deve ser retornado e nenhuma tentativa deve ser feita para executar a divisão porque ocorreria um erro em tempo de execução. Como a `If` expressão usa uma avaliação de curto-circuito, ela avalia o segundo ou o terceiro argumento, dependendo do valor do primeiro argumento. Se o primeiro argumento for true, o divisor não será zero e será seguro avaliar o segundo argumento e executar a divisão. Se o primeiro argumento for false, somente o terceiro argumento será avaliado e um 0 será retornado. Portanto, quando o divisor é 0, nenhuma tentativa é feita para executar a divisão e nenhum resultado de erro. No entanto, como o `IIf` não usa a avaliação de curto-circuito, o segundo argumento é avaliado mesmo quando o primeiro argumento é falso. Isso causa um erro de divisão por zero em tempo de execução.

[!code-vb[VbVbalrOperators#101](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class4.vb#101)]

## <a name="if-operator-called-with-two-arguments"></a>Operador If chamado com dois argumentos

O primeiro argumento para `If` pode ser omitido. Isso permite que o operador seja chamado usando apenas dois argumentos. A lista a seguir se aplica somente quando o `If` operador é chamado com dois argumentos.

### <a name="parts"></a>Partes

|Termo|Definição|
|---|---|
|`argument2`|Obrigatórios. `Object`. Deve ser um tipo de valor de referência ou anulável. Avaliado e retornado quando ele é avaliado como algo diferente de `Nothing` .|
|`argument3`|Obrigatórios. `Object`. Avaliado e retornado se `argument2` for avaliado como `Nothing` .|

Quando o `Boolean` argumento é omitido, o primeiro argumento deve ser um tipo de valor de referência ou anulável. Se o primeiro argumento for avaliado como `Nothing` , o valor do segundo argumento será retornado. Em todos os outros casos, o valor do primeiro argumento é retornado. O exemplo a seguir ilustra como essa avaliação funciona:

[!code-vb[VbVbalrOperators#102](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class4.vb#102)]

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Interaction.IIf%2A>
- [Tipos de valor anulável](../../programming-guide/language-features/data-types/nullable-value-types.md)
- [Nada](../nothing.md)
