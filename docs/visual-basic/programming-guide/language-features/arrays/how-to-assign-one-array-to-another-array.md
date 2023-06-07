---
title: Como atribuir uma matriz a outra matriz
ms.date: 07/20/2015
helpviewer_keywords:
- covariance, arrays
- arrays [Visual Basic], assigning
- arrays [Visual Basic], covariance
ms.assetid: 1ae89ea5-f292-4282-bcfc-e9b06b37fbd5
ms.openlocfilehash: c38def1ba9f3720bc760d6f6e4264510c884c930
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413073"
---
# <a name="how-to-assign-one-array-to-another-array-visual-basic"></a>Como atribuir uma matriz a outra matriz (Visual Basic)

Como as matrizes são objetos, você pode usá-las em instruções de atribuição como outros tipos de objeto. Uma variável de matriz contém um ponteiro para os dados que constituem os elementos de matriz e as informações de classificação e comprimento, e uma atribuição copia apenas esse ponteiro.

### <a name="to-assign-one-array-to-another-array"></a>Para atribuir uma matriz a outra matriz

1. Verifique se as duas matrizes têm a mesma classificação (número de dimensões) e tipos de dados de elemento compatíveis.

2. Use uma instrução de atribuição padrão para atribuir a matriz de origem à matriz de destino. Não siga o nome da matriz com parênteses.

    ```vb
    Dim formArray() As System.Windows.Forms.Form
    Dim controlArray() As System.Windows.Forms.Control
    controlArray = formArray
    ```

Quando você atribui uma matriz a outra, as seguintes regras se aplicam:

- **Classificações iguais.** A classificação (número de dimensões) da matriz de destino deve ser a mesma da matriz de origem.

  Desde que as classificações das duas matrizes sejam iguais, as dimensões não precisam ser iguais. O número de elementos em uma determinada dimensão pode ser alterado durante a atribuição.

- **Tipos de elemento.** Ambas as matrizes devem ter elementos de *tipo de referência* ou ambas as matrizes devem ter elementos de *tipo de valor* . Para obter mais informações, consulte [tipos de valor e tipos de referência](../data-types/value-types-and-reference-types.md).

  - Se ambas as matrizes tiverem elementos de tipo de valor, os tipos de dados de elemento deverão ser exatamente iguais. A única exceção é que você pode atribuir uma matriz de `Enum` elementos a uma matriz do tipo base `Enum` .

  - Se ambas as matrizes tiverem elementos de tipo de referência, o tipo de elemento de origem deverá derivar do tipo de elemento de destino. Quando esse for o caso, as duas matrizes terão a mesma relação de herança que seus elementos. Isso é chamado de *covariância de matriz*.

O compilador relatará um erro se as regras acima forem violadas, por exemplo, se os tipos de dados não forem compatíveis ou se as classificações forem desiguais. Você pode adicionar tratamento de erros ao seu código para certificar-se de que as matrizes são compatíveis antes de tentar uma atribuição. Você também pode usar a palavra-chave do [Operador TryCast](../../../language-reference/operators/trycast-operator.md) se quiser evitar lançar uma exceção.

## <a name="see-also"></a>Confira também

- [Matrizes](index.md)
- [Solução de problemas de matrizes](troubleshooting-arrays.md)
- [Instrução Enum](../../../language-reference/statements/enum-statement.md)
- [Conversões de matriz](../data-types/array-conversions.md)
