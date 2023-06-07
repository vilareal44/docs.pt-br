---
title: Função CType
ms.date: 07/20/2015
f1_keywords:
- vb.CType
helpviewer_keywords:
- expression conversion results
- explicit data type conversions [Visual Basic]
- CType function
- conversions [Visual Basic], expression
ms.assetid: dd4b29e7-6fa1-428c-877e-69955420bb72
ms.openlocfilehash: 88d609146648fe1b0c3124b99a65e85293fc0707
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406424"
---
# <a name="ctype-function-visual-basic"></a>Função CType (Visual Basic)

Retorna o resultado da conversão explícita de uma expressão para um tipo de dados, objeto, estrutura, classe ou interface especificado.

## <a name="syntax"></a>Sintaxe

```vb
CType(expression, typename)
```

## <a name="parts"></a>Partes

`expression`Qualquer expressão válida. Se o valor de `expression` estiver fora do intervalo permitido pelo `typename` , Visual Basic lançará uma exceção.

`typename`Qualquer expressão que seja válida dentro de uma `As` cláusula em uma `Dim` instrução, ou seja, o nome de qualquer tipo de dados, objeto, estrutura, classe ou interface.

## <a name="remarks"></a>Comentários

> [!TIP]
> Você também pode usar as seguintes funções para executar uma conversão de tipo:
>
> - Funções de conversão de tipo, como `CByte` , `CDbl` e `CInt` que executam uma conversão para um tipo de dados específico. Para obter mais informações, consulte [Funções de conversão de tipo](type-conversion-functions.md).
> - [Operador DirectCast](../operators/directcast-operator.md) ou [Operador TryCast](../operators/trycast-operator.md). Esses operadores exigem que um tipo herde ou implemente o outro tipo. Eles podem fornecer um desempenho um pouco melhor do que `CType` ao converter de e para o `Object` tipo de dados.

`CType`é compilado embutido, o que significa que o código de conversão faz parte do código que avalia a expressão. Em alguns casos, o código é executado mais rapidamente porque nenhum procedimento é chamado para executar a conversão.

Se nenhuma conversão for definida de `expression` para `typename` (por exemplo, de `Integer` para `Date` ), Visual Basic exibirá uma mensagem de erro de tempo de compilação.

Se uma conversão falhar em tempo de execução, a exceção apropriada será lançada. Se uma conversão de restrição falhar, um <xref:System.OverflowException> será o resultado mais comum. Se a conversão for indefinida, um <xref:System.InvalidCastException> em será lançado. Por exemplo, isso pode acontecer se `expression` for do tipo `Object` e seu tipo de tempo de execução não tiver nenhuma conversão para `typename` .

Se o tipo de dados de `expression` ou `typename` for uma classe ou estrutura que você definiu, você poderá definir `CType` nessa classe ou estrutura como um operador de conversão. Isso torna o `CType` Act como um *operador sobrecarregado*. Se você fizer isso, poderá controlar o comportamento de conversões de e para sua classe ou estrutura, incluindo as exceções que podem ser geradas.

## <a name="overloading"></a>Sobrecarga

O `CType` operador também pode ser sobrecarregado em uma classe ou estrutura definida fora do seu código. Se o seu código converter para ou de uma classe ou estrutura desse tipo, certifique-se de entender o comportamento de seu `CType` operador. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).

## <a name="converting-dynamic-objects"></a>Convertendo objetos dinâmicos

As conversões de tipo de objetos dinâmicos são executadas por conversões dinâmicas definidas pelo usuário que usam os <xref:System.Dynamic.DynamicObject.TryConvert%2A> <xref:System.Dynamic.DynamicMetaObject.BindConvert%2A> métodos ou. Se você estiver trabalhando com objetos dinâmicos, use o <xref:Microsoft.VisualBasic.Conversion.CTypeDynamic%2A> método para converter o objeto dinâmico.

## <a name="example"></a>Exemplo

O exemplo a seguir usa a `CType` função para converter uma expressão para o `Single` tipo de dados.

[!code-vb[VbVbalrFunctions#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrFunctions/VB/Class1.vb#24)]

Para obter exemplos adicionais, consulte [conversões implícitas e explícitas](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md).

## <a name="see-also"></a>Confira também

- <xref:System.OverflowException>
- <xref:System.InvalidCastException>
- [Funções de conversão do tipo](type-conversion-functions.md)
- [Funções de conversão](conversion-functions.md)
- [Instrução Operator](../statements/operator-statement.md)
- [Como definir um operador de conversão](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
- [Conversão de tipos no .NET Framework](../../../standard/base-types/type-conversion.md)
