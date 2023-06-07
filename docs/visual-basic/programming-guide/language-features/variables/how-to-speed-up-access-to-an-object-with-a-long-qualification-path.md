---
title: Como agilizar o acesso a um objeto com um caminho de qualificação longo
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], accessing
- variables [Visual Basic], object
- With statement [Visual Basic]
- With block
- object variables [Visual Basic], accessing
ms.assetid: 3eb7657f-c9fe-4e05-8bc3-4bb14d5ae585
ms.openlocfilehash: fe93e7bac2a21f1060d1f93765eb35e1ad0c7eb0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410406"
---
# <a name="how-to-speed-up-access-to-an-object-with-a-long-qualification-path-visual-basic"></a>Como agilizar o acesso a um objeto com um demarcador de qualificação longo (Visual Basic)

Se você acessar com frequência um objeto que requer um caminho de qualificação de vários métodos e propriedades, você pode acelerar seu código não repetindo o caminho de qualificação.

Há duas maneiras de evitar a repetição do caminho de qualificação. Você pode atribuir o objeto a uma variável ou pode usá-lo em um `With` bloco... `End With` .

### <a name="to-speed-up-access-to-a-heavily-qualified-object-by-assigning-it-to-a-variable"></a>Para acelerar o acesso a um objeto muito qualificado atribuindo-o a uma variável

1. Declare uma variável do tipo do objeto que você está acessando com frequência. Especifique o caminho de qualificação na parte de inicialização da declaração.

    ```vb
    Dim ctrlActv As Control = someForm.ActiveForm.ActiveControl
    ```

2. Use a variável para acessar os membros do objeto.

    ```vb
    ctrlActv.Text = "Test"
    ctrlActv.Location = New Point(100, 100)
    ctrlActv.Show()
    ```

### <a name="to-speed-up-access-to-a-heavily-qualified-object-by-using-a-withend-with-block"></a>Para acelerar o acesso a um objeto muito qualificado usando um com... Terminar com bloco

1. Coloque o caminho de qualificação em uma `With` instrução.

    ```vb
    With someForm.ActiveForm.ActiveControl
    ```

2. Acesse os membros do objeto dentro do `With` bloco, antes da `End With` instrução.

    ```vb
        .Text = "Test"
        .Location = New Point(100, 100)
        .Show()
    End With
    ```

## <a name="see-also"></a>Confira também

- [Variáveis de Objeto](object-variables.md)
- [Instrução With...End With](../../../language-reference/statements/with-end-with-statement.md)
