---
title: Como controlar a disponibilidade de uma variável
ms.date: 07/20/2015
helpviewer_keywords:
- access levels, declared elements
- Private keyword [Visual Basic], accessing variables
- access levels, variables
- Public keyword [Visual Basic], accessing variables
- Friend keyword [Visual Basic], accessing variables
- variables [Visual Basic], access level
- declared elements [Visual Basic], access level
- Protected keyword [Visual Basic], accessing variables
ms.assetid: eaf4f073-7922-43ce-ae1e-90ff376ae947
ms.openlocfilehash: 0bfa7fa2bdac4746827884c1dad62734c549a48e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84357381"
---
# <a name="how-to-control-the-availability-of-a-variable-visual-basic"></a>Como controlar a disponibilidade de uma variável (Visual Basic)
Você controla a disponibilidade de uma variável especificando seu *nível de acesso*. O nível de acesso determina qual código tem permissão para ler ou gravar na variável.  
  
- As *variáveis de membro* (definidas em nível de módulo e fora de qualquer procedimento) assumem o padrão de acesso público, o que significa que qualquer código que possa vê-las pode acessá-las. Você pode alterar isso especificando um modificador de acesso.  
  
- As *variáveis locais* (definidas dentro de um procedimento) nominalmente têm acesso público, embora apenas o código dentro de seu procedimento possa acessá-las. Você não pode alterar o nível de acesso de uma variável local, mas pode alterar o nível de acesso do procedimento que a contém.  
  
 Para obter mais informações, consulte [níveis de acesso em Visual Basic](access-levels.md).  
  
## <a name="private-and-public-access"></a>Acesso público e privado  
  
#### <a name="to-make-a-variable-accessible-only-from-within-its-module-class-or-structure"></a>Para tornar uma variável acessível somente de dentro de seu módulo, classe ou estrutura  
  
1. Coloque a [instrução Dim](../../../language-reference/statements/dim-statement.md) para a variável dentro do módulo, da classe ou da estrutura, mas fora de qualquer procedimento.  
  
2. Inclua a palavra-chave [Private](../../../language-reference/modifiers/private.md) na `Dim` instrução.  
  
     Você pode ler ou gravar na variável de qualquer lugar dentro do módulo, da classe ou da estrutura, mas não de fora dela.  
  
#### <a name="to-make-a-variable-accessible-from-any-code-that-can-see-it"></a>Para tornar uma variável acessível de qualquer código que possa vê-la  
  
1. Para uma variável de membro, coloque a `Dim` instrução para a variável dentro de um módulo, classe ou estrutura, mas fora de qualquer procedimento.  
  
2. Inclua a palavra-chave [Public](../../../language-reference/modifiers/public.md) na `Dim` instrução.  
  
     Você pode ler ou gravar na variável de qualquer código que interopere com seu assembly.  
  
 -ou-  
  
1. Para uma variável local, coloque a `Dim` instrução para a variável dentro de um procedimento.  
  
2. Não inclua a `Public` palavra-chave na `Dim` instrução.  
  
     Você pode ler ou gravar na variável de qualquer lugar dentro do procedimento, mas não de fora dela.  
  
## <a name="protected-and-friend-access"></a>Acesso protegido e amigo  
 Você pode limitar o nível de acesso de uma variável à sua classe e a todas as classes derivadas ou a seu assembly. Você também pode especificar a União dessas limitações, que permite o acesso a partir do código em qualquer classe derivada ou em qualquer outro lugar no mesmo assembly. Você especifica essa União combinando as `Protected` `Friend` palavras-chave e na mesma declaração.  
  
#### <a name="to-make-a-variable-accessible-only-from-within-its-class-and-any-derived-classes"></a>Para tornar uma variável acessível somente de dentro de sua classe e de quaisquer classes derivadas  
  
1. Coloque a `Dim` instrução para a variável dentro de uma classe, mas fora de qualquer procedimento.  
  
2. Inclua a palavra-chave [Protected](../../../language-reference/modifiers/protected.md) na `Dim` instrução.  
  
     Você pode ler ou gravar na variável de qualquer lugar dentro da classe, bem como de dentro de qualquer classe derivada dela, mas não de fora de nenhuma classe na cadeia de derivação.  
  
#### <a name="to-make-a-variable-accessible-only-from-within-the-same-assembly"></a>Para tornar uma variável acessível somente de dentro do mesmo assembly  
  
1. Coloque a `Dim` instrução para a variável dentro de um módulo, classe ou estrutura, mas fora de qualquer procedimento.  
  
2. Inclua a palavra-chave [Friend](../../../language-reference/modifiers/friend.md) na `Dim` instrução.  
  
     Você pode ler ou gravar na variável de qualquer lugar dentro do módulo, da classe ou da estrutura, bem como de qualquer código no mesmo assembly, mas não de fora do assembly.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra declarações de variáveis `Public` com `Protected` `Friend` os níveis de acesso,,, `Protected Friend` e `Private` . Observe que, quando a `Dim` instrução especifica um nível de acesso, você não precisa incluir a `Dim` palavra-chave.  
  
```vb  
Public Class classForEverybody  
Protected Class classForMyHeirs  
Friend stringForThisProject As String  
Protected Friend stringForProjectAndHeirs As String  
Private numberForMeOnly As Integer  
```  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Quanto mais restritiva for o nível de acesso de uma variável, menor será a probabilidade de que o código mal-intencionado possa usar o uso impróprio.  
  
## <a name="see-also"></a>Confira também

- [Níveis de acesso no Visual Basic](access-levels.md)
- [Instrução Dim](../../../language-reference/statements/dim-statement.md)
- [Pública](../../../language-reference/modifiers/public.md)
- [Protected](../../../language-reference/modifiers/protected.md)
- [Público](../../../language-reference/modifiers/friend.md)
- [Privada](../../../language-reference/modifiers/private.md)
