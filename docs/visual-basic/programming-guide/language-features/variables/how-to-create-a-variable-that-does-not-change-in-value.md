---
title: Como criar uma variável que não se altera no valor
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], read-only
- variables [Visual Basic], constant value
ms.assetid: 86b59266-25df-4635-ae15-9b59c411d036
ms.openlocfilehash: 04e08784b5cfbdeb6db73b9b00fe9afa201bd06d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410510"
---
# <a name="how-to-create-a-variable-that-does-not-change-in-value-visual-basic"></a>Como criar uma variável que não se altera no valor (Visual Basic)

A noção de uma variável que não altera seu valor pode parecer ser contraditória. Mas há situações em que uma constante não é viável e é útil ter uma variável com um valor fixo. Nesse caso, você pode definir uma variável de membro com a palavra-chave [ReadOnly](../../../language-reference/modifiers/readonly.md) .

Você não pode usar a [instrução const](../../../language-reference/statements/const-statement.md) para declarar e atribuir um valor constante nas seguintes circunstâncias:

- A `Const` instrução não aceita o tipo de dados que você deseja usar

- Você não conhece o valor no tempo de compilação

- Não é possível calcular o valor constante no tempo de compilação

### <a name="to-create-a-variable-that-does-not-change-in-value"></a>Para criar uma variável que não é alterada no valor

1. Em nível de módulo, declare uma variável de membro com a [instrução Dim](../../../language-reference/statements/dim-statement.md)e inclua a palavra-chave [ReadOnly](../../../language-reference/modifiers/readonly.md) .

    ```vb
    Dim ReadOnly timeStarted
    ```

    Você pode especificar `ReadOnly` somente em uma variável de membro. Isso significa que você deve definir a variável no nível de módulo, fora de qualquer procedimento.

2. Se você puder calcular o valor em uma única instrução em tempo de compilação, use uma cláusula de inicialização na `Dim` instrução. Siga a [cláusula as](../../../language-reference/statements/as-clause.md) com um sinal de igual ( `=` ), seguido por uma expressão. Verifique se o compilador pode avaliar essa expressão como um valor constante.

    ```vb
    Dim ReadOnly timeStarted As Date = Now
    ```

    Você pode atribuir um valor a uma `ReadOnly` variável apenas uma vez. Depois de fazer isso, nenhum código pode alterar seu valor.

    Se você não souber o valor em tempo de compilação, ou não puder computar em tempo de compilação em uma única instrução, ainda poderá atribuí-lo em tempo de execução em um construtor. Para fazer isso, você deve declarar a `ReadOnly` variável no nível de classe ou estrutura. No construtor para essa classe ou estrutura, calcule o valor fixo da variável e atribua-o à variável antes de retornar do construtor.

## <a name="see-also"></a>Confira também

- [WriteOnly](../../../language-reference/modifiers/writeonly.md)
- [Instrução Const](../../../language-reference/statements/const-statement.md)
