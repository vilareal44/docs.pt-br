---
title: Procedimento principal
ms.date: 07/20/2015
f1_keywords:
- vb.Main
helpviewer_keywords:
- Main procedure
- Main method [Visual Basic]
- main function
ms.assetid: f0db283e-f283-4464-b521-b90858cc1b44
ms.openlocfilehash: cf6003206566dfe8f70a7f75cd4d7ec7565794a5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403168"
---
# <a name="main-procedure-in-visual-basic"></a>Procedimento principal no Visual Basic
Cada aplicativo de Visual Basic deve conter um procedimento chamado `Main` . Esse procedimento serve como o ponto de partida e o controle geral para seu aplicativo. O .NET Framework chama o `Main` procedimento quando ele carregou seu aplicativo e está pronto para passar o controle para ele. A menos que você esteja criando um aplicativo Windows Forms, você deve escrever o `Main` procedimento para aplicativos que são executados por conta própria.

 `Main`contém o código que é executado primeiro. No `Main` , você pode determinar qual formulário deve ser carregado primeiro quando o programa for iniciado, descobrir se uma cópia do seu aplicativo já está em execução no sistema, estabelecer um conjunto de variáveis para seu aplicativo ou abrir um banco de dados que o aplicativo requer.

## <a name="requirements-for-the-main-procedure"></a>Requisitos para o procedimento principal
 Um arquivo que é executado por conta própria (normalmente com extensão. exe) deve conter um `Main` procedimento. Uma biblioteca (por exemplo, com extensão. dll) não é executada por conta própria e não requer um `Main` procedimento. Os requisitos para os diferentes tipos de projetos que você pode criar são os seguintes:

- Os aplicativos de console são executados por conta própria e você deve fornecer pelo menos um `Main` procedimento.

- Windows Forms aplicativos são executados por conta própria. No entanto, o compilador de Visual Basic gera automaticamente um `Main` procedimento em tal aplicativo, e você não precisa escrever um.

- As bibliotecas de classes não exigem um `Main` procedimento. Isso inclui bibliotecas de controle do Windows e bibliotecas de controle da Web. Os aplicativos Web são implantados como bibliotecas de classes.

## <a name="declaring-the-main-procedure"></a>Declarando o procedimento principal
 Há quatro maneiras de declarar o `Main` procedimento. Ele pode usar argumentos ou não, e pode retornar um valor ou não.

> [!NOTE]
> Se você declarar `Main` em uma classe, deverá usar a `Shared` palavra-chave. Em um módulo, `Main` não precisa ser `Shared` .

- A maneira mais simples é declarar um `Sub` procedimento que não use argumentos nem retornar um valor.

    ```vb
    Module mainModule
        Sub Main()
            MsgBox("The Main procedure is starting the application.")
            ' Insert call to appropriate starting place in your code.
            MsgBox("The application is terminating.")
        End Sub
    End Module
    ```

- `Main`também pode retornar um `Integer` valor, que o sistema operacional usa como o código de saída para seu programa. Outros programas podem testar esse código examinando o valor ERRORLEVEL do Windows. Para retornar um código de saída, você deve declarar `Main` como um `Function` procedimento em vez de um `Sub` procedimento.

    ```vb
    Module mainModule
        Function Main() As Integer
            MsgBox("The Main procedure is starting the application.")
            Dim returnValue As Integer = 0
            ' Insert call to appropriate starting place in your code.
            ' On return, assign appropriate value to returnValue.
            ' 0 usually means successful completion.
            MsgBox("The application is terminating with error level " &
                 CStr(returnValue) & ".")
            Return returnValue
        End Function
    End Module
    ```

- `Main`também pode pegar uma `String` matriz como um argumento. Cada cadeia de caracteres na matriz contém um dos argumentos de linha de comando usados para invocar seu programa. Você pode executar ações diferentes dependendo de seus valores.

    ```vb
    Module mainModule
        Function Main(ByVal cmdArgs() As String) As Integer
            MsgBox("The Main procedure is starting the application.")
            Dim returnValue As Integer = 0
            ' See if there are any arguments.
            If cmdArgs.Length > 0 Then
                For argNum As Integer = 0 To UBound(cmdArgs, 1)
                    ' Insert code to examine cmdArgs(argNum) and take
                    ' appropriate action based on its value.
                Next
            End If
            ' Insert call to appropriate starting place in your code.
            ' On return, assign appropriate value to returnValue.
            ' 0 usually means successful completion.
            MsgBox("The application is terminating with error level " &
                 CStr(returnValue) & ".")
            Return returnValue
        End Function
    End Module
    ```

- Você pode declarar `Main` para examinar os argumentos de linha de comando, mas não retornar um código de saída, da seguinte maneira.

    ```vb
    Module mainModule
        Sub Main(ByVal cmdArgs() As String)
            MsgBox("The Main procedure is starting the application.")
            Dim returnValue As Integer = 0
            ' See if there are any arguments.
            If cmdArgs.Length > 0 Then
                For argNum As Integer = 0 To UBound(cmdArgs, 1)
                    ' Insert code to examine cmdArgs(argNum) and take
                    ' appropriate action based on its value.
                Next
            End If
            ' Insert call to appropriate starting place in your code.
            MsgBox("The application is terminating.")
        End Sub
    End Module
    ```
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A>
- <xref:System.Array.Length%2A>
- <xref:Microsoft.VisualBasic.Information.UBound%2A>
- [Estrutura de um programa Visual Basic](structure-of-a-visual-basic-program.md)
- [-main](../../reference/command-line-compiler/main.md)
- [Compartilhado](../../language-reference/modifiers/shared.md)
- [Instrução Sub](../../language-reference/statements/sub-statement.md)
- [Instrução Function](../../language-reference/statements/function-statement.md)
- [Tipo de Dados Integer](../../language-reference/data-types/integer-data-type.md)
- [Tipo de dados da cadeia de caracteres](../../language-reference/data-types/string-data-type.md)
