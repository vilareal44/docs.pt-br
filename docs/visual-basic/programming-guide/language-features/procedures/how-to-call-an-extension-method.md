---
title: Como chamar um método de extensão
ms.date: 07/20/2015
helpviewer_keywords:
- calling extension methods [Visual Basic]
- extension methods [Visual Basic]
ms.assetid: df07750f-40f4-4c07-a79e-1113a27cfbea
ms.openlocfilehash: 54419c99ae08c9ca2e3cfa86993dc99bc02bbb64
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388654"
---
# <a name="how-to-call-an-extension-method-visual-basic"></a>Como chamar um método de extensão (Visual Basic)

Os métodos de extensão permitem adicionar métodos a uma classe existente. Depois que um método de extensão é declarado e colocado no escopo, você pode chamá-lo como um método de instância do tipo que ele estende. Para obter mais informações sobre como escrever um método de extensão, consulte [How to: Write a Extension Method](./how-to-write-an-extension-method.md).

 As instruções a seguir se referem ao método `PrintAndPunctuate` de extensão, que exibirá a instância de cadeia de caracteres que a invoca, seguida por qualquer valor enviado para o segundo parâmetro, `punc` .

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions
    <Extension()>
    Public Sub PrintAndPunctuate(ByVal aString As String,
                                 ByVal punc As String)
        Console.WriteLine(aString & punc)
    End Sub

End Module
```

O método deve estar no escopo quando for chamado.

### <a name="to-call-an-extension-method"></a>Para chamar um método de extensão

1. Declare uma variável que tenha o tipo de dados do primeiro parâmetro do método de extensão. Para `PrintAndPunctuate` , você precisa de uma <xref:System.String> variável:

    ```vb
    Dim example = "Ready"
    ```

2. Essa variável invocará o método de extensão e seu valor será associado ao primeiro parâmetro, `aString` . A seguinte instrução de chamada será exibida `Ready?` .

    ```vb
    example.PrintAndPunctuate("?")
    ```

     Observe que a chamada para esse método de extensão é semelhante a uma chamada para qualquer um dos <xref:System.String> métodos de instância que exigem um parâmetro:

    ```vb
    example.EndsWith("dy")
    example.IndexOf("R")
    ```

3. Declare outra variável de cadeia de caracteres e chame o método novamente para ver que ele funciona com qualquer cadeia de caracteres.

    ```vb
    Dim example2 = " or not"
    example2.PrintAndPunctuate("!!!")
    ```

     O resultado desta vez é: `or not!!!` .

## <a name="example"></a>Exemplo
 O código a seguir é um exemplo completo da criação e uso de um método de extensão simples.

```vb
Imports System.Runtime.CompilerServices
Imports ConsoleApplication1.StringExtensions

Module Module1

    Sub Main()

        Dim example = "Hello"
        example.PrintAndPunctuate(".")
        example.PrintAndPunctuate("!!!!")

        Dim example2 = "Goodbye"
        example2.PrintAndPunctuate("?")
    End Sub

    <Extension()>
    Public Sub PrintAndPunctuate(ByVal aString As String,
                                 ByVal punc As String)
        Console.WriteLine(aString & punc)
    End Sub
End Module

' Output:
' Hello.
' Hello!!!!
' Goodbye?
```

## <a name="see-also"></a>Confira também

- [Como escrever um método de extensão](./how-to-write-an-extension-method.md)
- [Métodos de Extensão](./extension-methods.md)
- [Escopo no Visual Basic](../declared-elements/scope.md)
