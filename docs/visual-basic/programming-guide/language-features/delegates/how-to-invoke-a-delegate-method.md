---
title: Como invocar um método delegado
ms.date: 07/20/2015
ms.assetid: b56866ae-abf9-4a5a-a855-486359455e9c
ms.openlocfilehash: f319727c007b93c7b334af0598f1b9f7c034144d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410715"
---
# <a name="how-to-invoke-a-delegate-method-visual-basic"></a>Como invocar um método delegado (Visual Basic)

Este exemplo mostra como associar um método a um delegado e, em seguida, chamar esse método por meio do delegado.

### <a name="create-the-delegate-and-matching-procedures"></a>Criar o delegado e os procedimentos correspondentes

1. Crie um delegado chamado `MySubDelegate` .

    ```vb
    Delegate Sub MySubDelegate(ByVal x As Integer)
    ```

2. Declare uma classe que contém um método com a mesma assinatura do delegado.

    ```vb
    Class class1
        Sub Sub1(ByVal x As Integer)
            MsgBox("The value of x is: " & CStr(x))
        End Sub
    End Class
    ```

3. Defina um método que cria uma instância do delegado e invoca o método associado ao delegado chamando o `Invoke` método interno.

    ```vb
    Protected Sub DelegateTest()
        Dim c1 As New class1
        ' Create an instance of the delegate.
        Dim msd As MySubDelegate = AddressOf c1.Sub1
        ' Call the method.
        msd.Invoke(10)
    End Sub
    ```

## <a name="see-also"></a>Confira também

- [Instrução Delegate](../../../language-reference/statements/delegate-statement.md)
- [Delegados](index.md)
- [Eventos](../events/index.md)
- [Aplicativos multi-threaded](../../../../standard/threading/using-threads-and-threading.md)
