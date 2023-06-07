---
title: Como controlar o escopo de uma variável
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], scope
- declared elements [Visual Basic], scope
- visibility [Visual Basic], declared elements
- variables [Visual Basic], visibility
- scope [Visual Basic], declared elements
- scope [Visual Basic], variables
- scope [Visual Basic], Visual Basic
- declared elements [Visual Basic], visibility
- visibility [Visual Basic], variables
ms.assetid: 44b7f62a-cb5c-4d50-bce9-60ae68f87072
ms.openlocfilehash: 8b21f22edea84448e3f2969c3e4b07c08a17a338
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84357342"
---
# <a name="how-to-control-the-scope-of-a-variable-visual-basic"></a>Como controlar o escopo de uma variável (Visual Basic)
Normalmente, uma variável está no *escopo*ou visível para referência, em toda a região em que você a declara. Em alguns casos, o nível de *acesso* da variável pode influenciar seu escopo.  
  
 Para obter mais informações, consulte [escopo em Visual Basic](scope.md).  
  
## <a name="scope-at-block-or-procedure-level"></a>Escopo no nível de bloco ou procedimento  
  
#### <a name="to-make-a-variable-visible-only-within-a-block"></a>Para tornar uma variável visível somente dentro de um bloco  
  
- Coloque a [instrução Dim](../../../language-reference/statements/dim-statement.md) para a variável entre as instruções de declaração de inicialização e término desse bloco, por exemplo, entre as `For` `Next` instruções e de um `For` loop.  
  
     Você pode se referir à variável somente de dentro do bloco.  
  
#### <a name="to-make-a-variable-visible-only-within-a-procedure"></a>Para tornar uma variável visível somente dentro de um procedimento  
  
- Coloque a `Dim` instrução para a variável dentro do procedimento, mas fora de qualquer bloco (como um `With` bloco... `End With` ).  
  
     Você pode se referir à variável somente de dentro do procedimento, incluindo dentro de qualquer bloco contido no procedimento.  
  
## <a name="scope-at-module-or-namespace-level"></a>Escopo no nível de namespace ou módulo  
 Para sua conveniência, o *nível de módulo* de termo único aplica-se igualmente a módulos, classes e estruturas. O nível de acesso de uma variável de nível de módulo determina seu escopo. O namespace que contém o módulo, a classe ou a estrutura também influencia o escopo.  
  
#### <a name="to-make-a-variable-visible-throughout-a-module-class-or-structure"></a>Para tornar uma variável visível em um módulo, classe ou estrutura  
  
1. Coloque a `Dim` instrução para a variável dentro do módulo, da classe ou da estrutura, mas fora de qualquer procedimento.  
  
2. Inclua a palavra-chave [Private](../../../language-reference/modifiers/private.md) na `Dim` instrução.  
  
3. Você pode consultar a variável de qualquer lugar dentro do módulo, da classe ou da estrutura, mas não de fora dela.  
  
#### <a name="to-make-a-variable-visible-throughout-a-namespace"></a>Para tornar uma variável visível em um namespace  
  
1. Coloque a `Dim` instrução para a variável dentro do módulo, da classe ou da estrutura, mas fora de qualquer procedimento.  
  
2. Inclua a palavra-chave [Friend](../../../language-reference/modifiers/friend.md) ou [Public](../../../language-reference/modifiers/public.md) na `Dim` instrução.  
  
3. Você pode consultar a variável de qualquer lugar dentro do namespace que contém o módulo, a classe ou a estrutura.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir declara uma variável no nível do módulo e limita sua visibilidade ao código dentro do módulo.  
  
```vb  
Module demonstrateScope  
    Private strMsg As String  
    Sub initializePrivateVariable()  
        strMsg = "This variable cannot be used outside this module."  
    End Sub  
    Sub usePrivateVariable()  
        MsgBox(strMsg)  
    End Sub  
End Module  
```  
  
 No exemplo anterior, todos os procedimentos definidos no módulo `demonstrateScope` podem se referir à `String` variável `strMsg` . Quando o `usePrivateVariable` procedimento é chamado, ele exibe o conteúdo da variável de cadeia de caracteres `strMsg` em uma caixa de diálogo.  
  
 Com a seguinte alteração no exemplo anterior, a variável de cadeia de caracteres `strMsg` pode ser referenciada pelo código em qualquer lugar no namespace de sua declaração.  
  
```vb  
Public strMsg As String  
```  
  
## <a name="robust-programming"></a>Programação robusta  
 Quanto mais estreita for o escopo de uma variável, menos oportunidades você terá para se referir acidentalmente a ela em vez de outra variável com o mesmo nome. Você também pode minimizar problemas de correspondência de referência.  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Quanto mais estreita for o escopo de uma variável, menor será a probabilidade de que o código mal-intencionado possa fazer uso impróprio dela.  
  
## <a name="see-also"></a>Confira também

- [Escopo no Visual Basic](scope.md)
- [Tempo de vida no Visual Basic](lifetime.md)
- [Níveis de acesso no Visual Basic](access-levels.md)
- [Variáveis](../variables/index.md)
- [Declaração de Variável](../variables/variable-declaration.md)
- [Instrução Dim](../../../language-reference/statements/dim-statement.md)
