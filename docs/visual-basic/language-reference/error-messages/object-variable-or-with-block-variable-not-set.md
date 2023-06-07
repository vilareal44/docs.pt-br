---
title: Variável de objeto ou variável com bloco não definida
ms.date: 07/20/2015
f1_keywords:
- vbrID91
ms.assetid: 2f03e611-f0ed-465c-99a2-a816e034faa3
ms.openlocfilehash: d1778e2bb58d32e976f10b3fba1637918278d36e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409277"
---
# <a name="object-variable-or-with-block-variable-not-set"></a>Variável de objeto ou variável com bloco não definida
Uma variável de objeto inválida está sendo referenciada.   Esse erro pode ocorrer por várias razões:

- Uma variável foi declarada sem especificar um tipo. Se uma variável for declarada sem especificar um tipo, o padrão será Type `Object` .

    Por exemplo, uma variável declarada com `Dim x` seria do tipo `Object;` uma variável declarada com `Dim x As String` seria do tipo `String` .

    > [!TIP]
    > A `Option Strict` instrução não permite a digitação implícita que resulta em um `Object` tipo. Se você omitir o tipo, ocorrerá um erro de tempo de compilação. Consulte a [instrução Option Strict](../statements/option-strict-statement.md).

- Você está tentando fazer referência a um objeto que foi definido como `Nothing` .

- Você está tentando acessar um elemento de uma variável de matriz que não foi declarada corretamente.

    Por exemplo, uma matriz declarada como `products() As String` irá disparar o erro se você tentar fazer referência a um elemento da matriz `products(3) = "Widget"` . A matriz não tem elementos e é tratada como um objeto.

- Você está tentando acessar o código dentro de um `With...End With` bloco antes que o bloco tenha sido inicializado.   Um `With...End With` bloco deve ser inicializado executando o `With` ponto de entrada da instrução.

> [!NOTE]
> Em versões anteriores do Visual Basic ou do VBA, esse erro também foi disparado por meio da atribuição de um valor a uma variável sem usar a `Set` palavra-chave ( `x = "name"` em vez de `Set x = "name"` ). A `Set` palavra-chave não é mais válida no Visual Basic .net.

## <a name="to-correct-this-error"></a>Para corrigir este erro

1. Defina `Option Strict` como `On` adicionando o seguinte código ao início do arquivo:

    ```vb
    Option Strict On
    ```

    Quando você executar o projeto, um erro do compilador será exibido no **lista de erros** para qualquer variável que foi especificada sem um tipo.

2. Se você não quiser habilitar `Option Strict` o, pesquise o código em busca de variáveis que foram especificadas sem um tipo ( `Dim x` em vez de `Dim x As String` ) e adicione o tipo pretendido à declaração.

3. Verifique se você não está se referindo a uma variável de objeto que foi definida como `Nothing` .  Pesquise seu código para a palavra-chave `Nothing` e revise seu código para que o objeto não seja definido como `Nothing` até que você o tenha referenciado.

4. Certifique-se de que todas as variáveis de matriz sejam dimensionadas antes de acessá-las. Você pode atribuir uma dimensão quando criar a matriz ( `Dim x(5) As String` em vez de `Dim x() As String` ), ou usar a `ReDim` palavra-chave para definir as dimensões da matriz antes de acessá-la pela primeira vez.

5. Verifique se o `With` bloco foi inicializado executando o `With` ponto de entrada da instrução.

## <a name="see-also"></a>Confira também

- [Declaração de Variável do Objeto](../../programming-guide/language-features/variables/object-variable-declaration.md)
- [Instrução ReDim](../statements/redim-statement.md)
- [Instrução With...End With](../statements/with-end-with-statement.md)
