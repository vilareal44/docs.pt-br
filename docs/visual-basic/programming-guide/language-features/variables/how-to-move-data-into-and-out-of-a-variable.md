---
title: Como inserir e remover dados de uma variável
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], retrieving values
- variables [Visual Basic], storing data
ms.assetid: 93744f46-bf78-4fa0-9640-1de01bc38d9a
ms.openlocfilehash: fe19a6160623aa9ea867becdf7a15b51319abf45
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410432"
---
# <a name="how-to-move-data-into-and-out-of-a-variable-visual-basic"></a>Como inserir e remover dados de uma variável (Visual Basic)

Você armazena um valor em uma variável colocando o nome da variável no lado esquerdo de uma instrução de atribuição.

## <a name="putting-data-in-a-variable"></a>Colocando dados em uma variável

#### <a name="to-store-a-value-in-a-variable"></a>Para armazenar um valor em uma variável

- Use o nome da variável no lado esquerdo de uma instrução de atribuição.

    O exemplo a seguir define o valor da variável `alpha` .

    ```vb
    alpha = (beta * 6.27) / (gamma + 2.1)
    ```

    O valor gerado no lado direito da instrução de atribuição é armazenado na variável.

## <a name="getting-data-from-a-variable"></a>Obtendo dados de uma variável

Você recupera o valor de uma variável, incluindo o nome da variável em uma expressão.

#### <a name="to-retrieve-a-value-from-a-variable"></a>Para recuperar um valor de uma variável

- Use o nome da variável em uma expressão. Você pode usar uma variável em qualquer lugar em que possa usar uma constante ou um literal, exceto em uma expressão que define o valor de uma constante.

  \-ou-

- Use o nome da variável após o sinal de igual ( `=` ) em uma instrução de atribuição.

  O exemplo a seguir lê o valor da variável `startValue` e, em seguida, usa o valor da variável `counter` em uma expressão.

  ```vb
  counter = startValue
  cellValue = (counter + 5) ^ 2
  ```

  O valor da variável participa da expressão exatamente como uma constante e, em seguida, é armazenado na variável ou propriedade no lado esquerdo da instrução de atribuição.

## <a name="see-also"></a>Confira também

- [Variáveis](index.md)
- [Declaração de Variável](variable-declaration.md)
- [Variáveis de Objeto](object-variables.md)
