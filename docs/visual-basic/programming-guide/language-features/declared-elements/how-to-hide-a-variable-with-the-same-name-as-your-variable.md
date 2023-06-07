---
title: Como ocultar uma variável com o mesmo nome que a variável
ms.date: 07/20/2015
helpviewer_keywords:
- qualification [Visual Basic], of element names
- declarations [Visual Basic], elements
- element names [Visual Basic], qualification
- references [Visual Basic], declared elements
- declaration statements [Visual Basic], declared elements
- declaring elements [Visual Basic]
- referencing declared elements [Visual Basic]
- declared elements [Visual Basic], referencing
- declared elements [Visual Basic], about declared elements
ms.assetid: e39c0752-f19f-4d2e-a453-00df1b5fc7ee
ms.openlocfilehash: c1f4c2fbf339358be77e76468b1db94616bf04a2
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84357226"
---
# <a name="how-to-hide-a-variable-with-the-same-name-as-your-variable-visual-basic"></a>Como ocultar uma variável com o mesmo nome que a variável (Visual Basic)

Você pode ocultar uma variável *sombreando* -a, ou seja, redefinindo-a com uma variável de mesmo nome. Você pode sombrear a variável que deseja ocultar de duas maneiras:

- **Sombreamento por meio do escopo.** Você pode sombrear a ti por meio do escopo, redeclarando-a dentro de uma sub-região da região que contém a variável que você deseja ocultar.

- **Sombreamento por meio de herança.** Se a variável que você deseja ocultar estiver definida no nível de classe, você poderá sombrear a ti por meio da herança redeclarando-a com a palavra-chave [Shadows](../../../language-reference/modifiers/shadows.md) em uma classe derivada.

## <a name="two-ways-to-hide-a-variable"></a>Duas maneiras de ocultar uma variável

#### <a name="to-hide-a-variable-by-shadowing-it-through-scope"></a>Para ocultar uma variável Sombreando-a por escopo

1. Determine a região que define a variável que você deseja ocultar e determine uma sub-região na qual redefini-la com sua variável.

    |Região da variável|Subregião permitida para redefini-la|
    |-----------------------|-------------------------------------------|
    |Módulo|Uma classe dentro do módulo|
    |Classe|Uma subclasse dentro da classe<br /><br /> Um procedimento dentro da classe|

    Você não pode redefinir uma variável de procedimento em um bloco dentro desse procedimento, por exemplo, em uma `If` construção... `End If` ou em um `For` loop.

2. Crie a subregião se ela ainda não existir.

3. Dentro da sub-região, escreva uma [instrução Dim](../../../language-reference/statements/dim-statement.md) declarando a variável de sombreamento.

    Quando o código dentro da sub-região refere-se ao nome da variável, o compilador resolve a referência à variável de sombreamento.

    O exemplo a seguir ilustra o sombreamento por meio do escopo, bem como uma referência que ignora o sombreamento.

    ```vb
    Module shadowByScope
        ' The following statement declares num as a module-level variable.
        Public num As Integer
        Sub show()
            ' The following statement declares num as a local variable.
            Dim num As Integer
            ' The following statement sets the value of the local variable.
            num = 2
            ' The following statement displays the module-level variable.
            MsgBox(CStr(shadowByScope.num))
        End Sub
        Sub useModuleLevelNum()
            ' The following statement sets the value of the module-level variable.
            num = 1
            show()
        End Sub
    End Module
    ```

    O exemplo anterior declara a variável `num` no nível do módulo e no nível do procedimento (no procedimento `show` ). A variável local `num` sombreia a variável em nível de módulo `num` dentro `show` , portanto, a variável local é definida como 2. No entanto, não há nenhuma variável local para sombrear `num` no `useModuleLevelNum` procedimento. Portanto, `useModuleLevelNum` define o valor da variável em nível de módulo como 1.

    A `MsgBox` chamada dentro `show` ignora o mecanismo de sombreamento qualificando `num` com o nome do módulo. Portanto, ele exibe a variável em nível de módulo em vez da variável local.

#### <a name="to-hide-a-variable-by-shadowing-it-through-inheritance"></a>Para ocultar uma variável Sombreando-a por meio de herança

1. Certifique-se de que a variável que você deseja ocultar seja declarada em uma classe e no nível de classe (fora de qualquer procedimento). Caso contrário, não será possível sombrear a ti por meio de herança.

2. Defina uma classe derivada da classe da variável, caso ainda não exista uma.

3. Dentro da classe derivada, escreva uma `Dim` instrução que declare sua variável. Inclua a palavra-chave [Shadows](../../../language-reference/modifiers/shadows.md) na declaração.

    Quando o código na classe derivada se refere ao nome da variável, o compilador resolve a referência à sua variável.

    O exemplo a seguir ilustra o sombreamento por meio de herança. Ele faz duas referências, uma que acessa a variável de sombreamento e outra que ignora o sombreamento.

    ```vb
    Public Class shadowBaseClass
        Public shadowString As String = "This is the base class string."
    End Class
    Public Class shadowDerivedClass
        Inherits shadowBaseClass
        Public Shadows shadowString As String = "This is the derived class string."
        Public Sub showStrings()
            Dim s As String = "Unqualified shadowString: " & shadowString &
                 vbCrLf & "MyBase.shadowString: " & MyBase.shadowString
            MsgBox(s)
        End Sub
    End Class
    ```

    O exemplo anterior declara a variável `shadowString` na classe base e a sombreia na classe derivada. O procedimento `showStrings` na classe derivada exibe a versão de sombreamento da cadeia de caracteres quando o nome `shadowString` não é qualificado. Em seguida, ele exibe a versão sombreada quando `shadowString` é qualificado com a `MyBase` palavra-chave.

## <a name="robust-programming"></a>Programação robusta

O sombreamento apresenta mais de uma versão de uma variável com o mesmo nome. Quando uma instrução de código se refere ao nome da variável, a versão para a qual o compilador resolve a referência depende de fatores como o local da instrução do código e a presença de uma cadeia de caracteres de qualificação. Isso pode aumentar o risco de se referir a uma versão não intencional de uma variável sombreada. Você pode reduzir esse risco Qualificando totalmente todas as referências a uma variável sombreada.

## <a name="see-also"></a>Confira também

- [Referências a elementos declarados](references-to-declared-elements.md)
- [Sombreamento no Visual Basic](shadowing.md)
- [Diferenças entre sombreamento e sobreposição](differences-between-shadowing-and-overriding.md)
- [Como ocultar uma variável herdada](how-to-hide-an-inherited-variable.md)
- [Como acessar uma variável oculta por uma classe derivada](how-to-access-a-variable-hidden-by-a-derived-class.md)
- [Substituições](../../../language-reference/modifiers/overrides.md)
- [Me, My, MyBase e MyClass](../../program-structure/me-my-mybase-and-myclass.md)
- [Noções básicas de herança](../objects-and-classes/inheritance-basics.md)
