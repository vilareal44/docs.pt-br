---
title: 'Como: Chamar uma função do Windows que use tipos não assinados'
ms.date: 07/20/2015
helpviewer_keywords:
- Windows functions [Visual Basic], calling
- unsigned data types [Visual Basic]
- UShort data type [Visual Basic], using
- functions [Visual Basic], calling Windows functions
- ULong data type [Visual Basic], using
- UInteger data type [Visual Basic], using
- data types [Visual Basic], using
- unsigned types [Visual Basic]
- data types [Visual Basic], unsigned
- data types [Visual Basic], numeric
- unsigned types [Visual Basic], using
ms.assetid: c2c0e712-8dc2-43b9-b4c6-345fbb02e7ce
ms.openlocfilehash: f30b78a2f0c38f233796e18006c889438dce4c58
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396824"
---
# <a name="how-to-call-a-windows-function-that-takes-unsigned-types-visual-basic"></a>Como chamar uma função do Windows que use tipos não assinados (Visual Basic)

Se você estiver consumindo uma classe, módulo ou estrutura que tem membros de tipos inteiros não assinados, poderá acessar esses membros com Visual Basic.

## <a name="to-call-a-windows-function-that-takes-an-unsigned-type"></a>Para chamar uma função do Windows que usa um tipo sem sinal

1. Use uma [instrução Declare](../../language-reference/statements/declare-statement.md) para informar Visual Basic qual biblioteca contém a função, qual nome está nessa biblioteca, qual é a sequência de chamada e como converter cadeias de caracteres ao chamá-la.

2. Na `Declare` instrução, use `UInteger` ,, `ULong` `UShort` ou `Byte` conforme apropriado para cada parâmetro com um tipo não assinado.

3. Consulte a documentação da função do Windows que você está chamando para localizar os nomes e valores das constantes que ele usa. Muitos deles são definidos no arquivo WinUser. h.

4. Declare as constantes necessárias no seu código. Muitas constantes do Windows são valores não assinados de 32 bits e você deve declará-los `As UInteger` .

5. Chame a função da maneira normal. O exemplo a seguir chama a função do Windows `MessageBox` , que usa um argumento inteiro sem sinal.

    ```vb
    Public Class windowsMessage
        Private Declare Auto Function mb Lib "user32.dll" Alias "MessageBox" (
            ByVal hWnd As Integer,
            ByVal lpText As String,
            ByVal lpCaption As String,
            ByVal uType As UInteger) As Integer
        Private Const MB_OK As UInteger = 0
        Private Const MB_ICONEXCLAMATION As UInteger = &H30
        Private Const IDOK As UInteger = 1
        Private Const IDCLOSE As UInteger = 8
        Private Const c As UInteger = MB_OK Or MB_ICONEXCLAMATION
        Public Function messageThroughWindows() As String
            Dim r As Integer = mb(0, "Click OK if you see this!",
                "Windows API call", c)
            Dim s As String = "Windows API MessageBox returned " &
                 CStr(r)& vbCrLf & "(IDOK = " & CStr(IDOK) &
                 ", IDCLOSE = " & CStr(IDCLOSE) & ")"
            Return s
        End Function
    End Class
    ```

     Você pode testar a função `messageThroughWindows` com o código a seguir.

    ```vb
    Public Sub consumeWindowsMessage()
        Dim w As New windowsMessage
        w.messageThroughWindows()
    End Sub
    ```

    > [!CAUTION]
    > Os `UInteger` tipos de dados,, e `ULong` não fazem `UShort` `SByte` parte da [independência de linguagem e dos componentes independentes de linguagem](../../../standard/language-independence-and-language-independent-components.md) (CLS), portanto, o código em conformidade com CLS não pode consumir um componente que os utilize.

    > [!IMPORTANT]
    > Fazer uma chamada para código não gerenciado, como a API (interface de programação de aplicativo) do Windows, expõe seu código a possíveis riscos de segurança.

    > [!IMPORTANT]
    > Chamar a API do Windows requer permissão de código não gerenciado, o que pode afetar sua execução em situações de confiança parcial. Para obter mais informações, consulte <xref:System.Security.Permissions.SecurityPermission> e [permissões de acesso ao código](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/h846e9b3(v=vs.100)).

## <a name="see-also"></a>Confira também

- [Tipos de dados](../../language-reference/data-types/index.md)
- [Tipo de Dados Integer](../../language-reference/data-types/integer-data-type.md)
- [Tipo de Dados UInteger](../../language-reference/data-types/uinteger-data-type.md)
- [Instrução Declare](../../language-reference/statements/declare-statement.md)
- [Passo a passo: Fazer chamadas de APIs do Windows](walkthrough-calling-windows-apis.md)
