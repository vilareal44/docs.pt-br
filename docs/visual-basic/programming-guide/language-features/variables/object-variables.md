---
title: Variáveis de objeto
ms.date: 07/20/2015
helpviewer_keywords:
- object variables [Visual Basic], about object variables
- variables [Visual Basic], object
- objects [Visual Basic], accessing
- object variables [Visual Basic]
ms.assetid: 6169a196-2b13-4ba5-a205-154bc1b87844
ms.openlocfilehash: a5e61f9308d3484dc228a7d09cc2fd30a2f41b35
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410329"
---
# <a name="object-variables-in-visual-basic"></a>Variáveis de objeto no Visual Basic

Além de armazenar valores diretamente, uma variável pode se referir a um objeto. Você atribui um objeto a uma variável pelos mesmos motivos para atribuir qualquer valor a uma variável:

- Um nome de variável é frequentemente mais curto e mais fácil de lembrar do que o caminho completo dos métodos e propriedades necessários para acessar o próprio objeto.

- O uso de uma variável que se refere a um objeto é mais eficiente do que acessar repetidamente o próprio objeto por meio dos métodos ou Propriedades necessários.

- Você pode alterar uma variável para fazer referência a outros objetos enquanto seu código está em execução.

## <a name="making-code-shorter"></a>Tornando o código mais curto

Você pode usar variáveis de objeto para encurtar o código que você precisa digitar. O exemplo a seguir usa o caminho completo de métodos e propriedades para acessar um <xref:System.Windows.Forms.Control> objeto.

```vb
' Assume Me is a valid Form, or replace Me with a valid Form.
Me.ActiveForm.ActiveControl.Text = "Test"
Me.ActiveForm.ActiveControl.Location = New Point(100, 100)
Me.ActiveForm.ActiveControl.Show()
```

Você pode encurtar esse código e acelerar a execução, se você usar uma variável de objeto para o controle. Você deve declarar a variável de objeto com a classe específica que pretende atribuir a ela ( `Control` nesse caso). Depois de atribuir um objeto à variável, você pode tratá-lo exatamente como trata o objeto ao qual ele se refere. Você pode definir ou recuperar as propriedades do objeto ou usar qualquer um de seus métodos. O exemplo a seguir usa uma variável de objeto para simplificar o código no exemplo anterior.

```vb
Dim ctrlActv As System.Windows.Forms.Control = Me.ActiveForm.ActiveControl
ctrlActv.Text = "Test"
ctrlActv.Location = New Point(100, 100)
ctrlActv.Show()
```

## <a name="see-also"></a>Confira também

- [Declaração de Variável](variable-declaration.md)
- [Como agilizar o acesso a um objeto com um caminho de qualificação longo](how-to-speed-up-access-to-an-object-with-a-long-qualification-path.md)
- [Declaração de Variável do Objeto](object-variable-declaration.md)
- [Atribuição de variável do objeto](object-variable-assignment.md)
- [Valores de Variável de Objeto](object-variable-values.md)
