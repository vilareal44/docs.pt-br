---
title: Como criar uma nova variável
ms.date: 07/20/2015
helpviewer_keywords:
- Dim statement [Visual Basic]
- variables [Visual Basic], creating
ms.assetid: 35300be3-77b0-4bef-a156-034d3cdedde0
ms.openlocfilehash: 501c8f3aff4c5b204d231bdc26213e13d125a7cf
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410523"
---
# <a name="how-to-create-a-new-variable-visual-basic"></a>Como criar uma nova variável (Visual Basic)

Você cria uma variável com uma [instrução Dim](../../../language-reference/statements/dim-statement.md).

### <a name="to-create-a-new-variable"></a>Para criar uma nova variável

1. Declare a variável em uma `Dim` instrução.

    ```vb
    Dim newCustomer
    ```

2. Inclua especificações para as características da variável, como [privado](../../../language-reference/modifiers/private.md), [estático](../../../language-reference/modifiers/static.md), [sombras](../../../language-reference/modifiers/shadows.md)ou [WithEvents](../../../language-reference/modifiers/withevents.md). Para obter mais informações, consulte [características de elemento declaradas](../declared-elements/declared-element-characteristics.md).

    ```vb
    Public Static newCustomer
    ```

    Você não precisará da `Dim` palavra-chave se usar outras palavras-chaves na declaração.

3. Siga as especificações com o nome da variável, que deve seguir Visual Basic regras e convenções. Para obter mais informações, consulte [nomes de elementos declarados](../declared-elements/declared-element-names.md).

    ```vb
    Public Static newCustomer
    ```

4. Siga o nome com a [cláusula as](../../../language-reference/statements/as-clause.md) para especificar o tipo de dados da variável.

    ```vb
    Public Static newCustomer As Customer
    ```

    Se você não especificar o tipo de dados, ele usará o padrão: `Object` .

5. Siga a `As` cláusula com um sinal de igual ( `=` ) e siga o sinal de igual com o valor inicial da variável.

    Visual Basic atribui o valor especificado à variável toda vez que ele executa a `Dim` instrução. Se você não especificar um valor inicial, Visual Basic atribuirá o valor inicial padrão para o tipo de dados da variável quando ele entrar pela primeira vez no código que contém a `Dim` instrução.

    Se a variável for um tipo de referência, você poderá criar uma instância de sua classe, incluindo a palavra-chave [New Operator](../../../language-reference/operators/new-operator.md) na `As` cláusula. Se você não usar `New` , o valor inicial da variável será [Nothing](../../../language-reference/nothing.md).

    ```vb
    Public Static newCustomer As New Customer
    ```

## <a name="see-also"></a>Confira também

- [Variáveis](index.md)
- [Declaração de Variável](variable-declaration.md)
- [Nomes de elementos declarados](../declared-elements/declared-element-names.md)
- [Características do Elemento Declarado](../declared-elements/declared-element-characteristics.md)
- [Tipos de valor e referência](../data-types/value-types-and-reference-types.md)
- [Instruções](../../../language-reference/statements/index.md)
- [Inferência de Tipo de Variável Local](local-type-inference.md)
- [Instrução Option Infer](../../../language-reference/statements/option-infer-statement.md)
