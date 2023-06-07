---
title: Como ocultar uma variável herdada
ms.date: 07/20/2015
helpviewer_keywords:
- qualification [Visual Basic], of element names
- element names [Visual Basic], qualification
- references [Visual Basic], declared elements
- declaration statements [Visual Basic], declared elements
- referencing declared elements [Visual Basic]
- declared elements [Visual Basic], referencing
- declared elements [Visual Basic], about declared elements
- variables [Visual Basic], hiding inherited
ms.assetid: 765728d9-7351-4a30-999d-b5f34f024412
ms.openlocfilehash: f49bba0497f9f4f2774b01284c815bba9aaed119
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84357264"
---
# <a name="how-to-hide-an-inherited-variable-visual-basic"></a>Como ocultar uma variável herdada (Visual Basic)

Uma classe derivada herda todas as definições de sua classe base. Se você quiser definir uma variável usando o mesmo nome de um elemento da classe base, poderá ocultar ou *sombrear*esse elemento da classe base ao definir a variável na classe derivada. Se você fizer isso, o código na classe derivada acessa sua variável, a menos que ela ignore explicitamente o mecanismo de sombreamento.

Outro motivo para você querer ocultar uma variável herdada é proteger-se contra a revisão da classe base. A classe base pode passar por uma alteração que altera o elemento que você está herdando. Se isso acontecer, o `Shadows` modificador força referências da classe derivada a serem resolvidas para a variável, em vez de para o elemento de classe base.

## <a name="to-hide-an-inherited-variable"></a>Para ocultar uma variável herdada

1. Certifique-se de que a variável que você deseja ocultar seja declarada no nível de classe (fora de qualquer procedimento). Caso contrário, você não precisa ocultá-lo.
  
2. Dentro de sua classe derivada, escreva uma [instrução Dim](../../../language-reference/statements/dim-statement.md) declarando sua variável. Use o mesmo nome que a variável herdada.

3. Inclua a palavra-chave [Shadows](../../../language-reference/modifiers/shadows.md) na declaração.

     Quando o código na classe derivada se refere ao nome da variável, o compilador resolve a referência à sua variável.

     O exemplo a seguir ilustra a sombra de uma variável herdada:
  
    ```vb  
    Public Class ShadowBaseClass  
        Public shadowString As String = "This is the base class string."  
    End Class  
    Public Class ShadowDerivedClass  
        Inherits ShadowBaseClass  
        Public Shadows shadowString As String = "This is the derived class string."  
        Public Sub ShowStrings()  
            Dim s As String = $"Unqualified shadowString: {shadowString}{vbCrLf}MyBase.shadowString: {MyBase.shadowString}"
            MsgBox(s)  
        End Sub  
    End Class  
    ```  
  
     O exemplo anterior declara a variável `shadowString` na classe base e a sombreia na classe derivada. O procedimento `ShowStrings` na classe derivada exibe a versão de sombreamento da cadeia de caracteres quando o nome `shadowString` não é qualificado. Em seguida, ele exibe a versão sombreada quando `shadowString` é qualificado com a `MyBase` palavra-chave.  
  
## <a name="robust-programming"></a>Programação robusta

O sombreamento apresenta mais de uma versão de uma variável com o mesmo nome. Quando uma instrução de código se refere ao nome da variável, a versão para a qual o compilador resolve a referência depende de fatores como o local da instrução do código e a presença de uma cadeia de caracteres de qualificação. Isso pode aumentar o risco de se referir a uma versão não intencional de uma variável sombreada. Você pode reduzir esse risco Qualificando totalmente todas as referências a uma variável sombreada.

## <a name="see-also"></a>Confira também

- [Referências a elementos declarados](references-to-declared-elements.md)
- [Sombreamento no Visual Basic](shadowing.md)
- [Diferenças entre sombreamento e sobreposição](differences-between-shadowing-and-overriding.md)
- [Como ocultar uma variável com o mesmo nome que a variável](how-to-hide-a-variable-with-the-same-name-as-your-variable.md)
- [Como acessar uma variável oculta por uma classe derivada](how-to-access-a-variable-hidden-by-a-derived-class.md)
- [Substituições](../../../language-reference/modifiers/overrides.md)
- [Me, My, MyBase e MyClass](../../program-structure/me-my-mybase-and-myclass.md)
- [Noções básicas de herança](../objects-and-classes/inheritance-basics.md)
