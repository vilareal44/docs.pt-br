---
title: Sombreamento
ms.date: 07/20/2015
helpviewer_keywords:
- inheritance [Visual Basic], shadowing
- overriding, and shadowing
- shadowing
- duplicate names [Visual Basic]
- shadowing, by inheritance
- declared elements [Visual Basic], referencing
- shadowing, by scope
- declared elements [Visual Basic], hiding
- naming conflicts, shadowing
- declared elements [Visual Basic], shadowing
- shadowing, and overriding
- scope [Visual Basic], shadowing
- Shadows keyword [Visual Basic], about
- objects [Visual Basic], names
- names [Visual Basic], shadowing
ms.assetid: 54bb4c25-12c4-4181-b4a0-93546053964e
ms.openlocfilehash: 7d76e2e7398c2f954ff4274f77ffa350efbd3617
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410741"
---
# <a name="shadowing-in-visual-basic"></a>Sombreamento no Visual Basic
Quando dois elementos de programação compartilham o mesmo nome, um deles pode ocultar ou *sombrear*o outro. Nessa situação, o elemento sombreado não está disponível para referência; em vez disso, quando seu código usa o nome do elemento, o compilador Visual Basic o resolve para o elemento de sombreamento.  
  
## <a name="purpose"></a>Finalidade  
 A principal finalidade do sombreamento é proteger a definição de seus membros de classe. A classe base pode passar por uma alteração que cria um elemento com o mesmo nome de um que você já definiu. Se isso acontecer, o `Shadows` modificador força referências por meio de sua classe a ser resolvida para o membro que você definiu, em vez de para o novo elemento de classe base.  
  
## <a name="types-of-shadowing"></a>Tipos de sombreamento  
 Um elemento pode sombrear outro elemento de duas maneiras diferentes. O elemento de sombreamento pode ser declarado dentro de uma sub-região da região que contém o elemento sombreado; nesse caso, o sombreamento é realizado *por meio do escopo*. Ou uma classe derivada pode redefinir um membro de uma classe base; nesse caso, o sombreamento é feito *por meio de herança*.  
  
### <a name="shadowing-through-scope"></a>Sombreamento por meio de escopo  
 É possível que elementos de programação no mesmo módulo, classe ou estrutura tenham o mesmo nome, mas escopo diferente. Quando dois elementos são declarados dessa maneira e o código se refere ao nome que eles compartilham, o elemento com o escopo mais estreito sombreia o outro elemento (o escopo do bloco é o mais estreito).  
  
 Por exemplo, um módulo pode definir uma `Public` variável chamada `temp` , e um procedimento dentro do módulo pode declarar uma variável local também denominada `temp` . Referências a `temp` de dentro do procedimento acessam a variável local, enquanto `temp` as referências a fora do procedimento acessam a `Public` variável. Nesse caso, a variável Procedure `temp` sombreia a variável module `temp` .  
  
 A ilustração a seguir mostra duas variáveis, ambas nomeadas `temp` . A variável local `temp` sombreia a variável de membro `temp` quando acessada de dentro de seu próprio procedimento `p` . No entanto, a `MyClass` palavra-chave ignora o sombreamento e acessa a variável de membro.  
  
 ![Gráfico que mostra o sombreamento por meio do escopo.](./media/shadowing/shadow-scope-diagram.gif)
  
 Para obter um exemplo de sombreamento por meio de escopo, consulte [como ocultar uma variável com o mesmo nome que a variável](how-to-hide-a-variable-with-the-same-name-as-your-variable.md).  
  
### <a name="shadowing-through-inheritance"></a>Sombreamento por meio de herança  
 Se uma classe derivada redefinir um elemento de programação herdado de uma classe base, o elemento redefinindo sombreia o elemento original. Você pode sombrear qualquer tipo de elemento declarado ou conjunto de elementos sobrecarregados, com qualquer outro tipo. Por exemplo, uma `Integer` variável pode sombrear um `Function` procedimento. Se você sombrear um procedimento com outro procedimento, poderá usar uma lista de parâmetros diferente e um tipo de retorno diferente.  
  
 A ilustração a seguir mostra uma classe base `b` e uma classe derivada `d` que herda de `b` . A classe base define um procedimento chamado `proc` , e a classe derivada a sombreia com outro procedimento de mesmo nome. A primeira `Call` instrução acessa o sombreamento `proc` na classe derivada. No entanto, a `MyBase` palavra-chave ignora o sombreamento e acessa o procedimento sombreado na classe base.  
  
 ![Diagrama gráfico de sombreamento por meio de herança](./media/shadowing/shadowing-inherit-diagram.gif)  
  
 Para obter um exemplo de sombreamento por meio de herança, consulte [como ocultar uma variável com o mesmo nome que a variável](how-to-hide-a-variable-with-the-same-name-as-your-variable.md) e [como ocultar uma variável herdada](how-to-hide-an-inherited-variable.md).  
  
#### <a name="shadowing-and-access-level"></a>Sombra e nível de acesso  
 O elemento de sombreamento nem sempre é acessível do código usando a classe derivada. Por exemplo, ele pode ser declarado `Private` . Nesse caso, o sombreamento é derrotado e o compilador resolve qualquer referência ao mesmo elemento que ele teria se não tivesse ocorrido nenhum sombreamento. Esse elemento é o elemento acessível o menor número de etapas de derivação para trás da classe de sombreamento. Se o elemento sombreado for um procedimento, a resolução será a versão acessível mais próxima com o mesmo nome, lista de parâmetros e tipo de retorno.  
  
 O exemplo a seguir mostra uma hierarquia de herança de três classes. Cada classe define um `Sub` procedimento `display` , e cada classe derivada sombreia o `display` procedimento em sua classe base.  
  
```vb  
Public Class firstClass  
    Public Sub display()  
        MsgBox("This is firstClass")  
    End Sub  
End Class  
Public Class secondClass  
    Inherits firstClass  
    Private Shadows Sub display()  
        MsgBox("This is secondClass")  
    End Sub  
End Class  
Public Class thirdClass  
    Inherits secondClass  
    Public Shadows Sub display()  
        MsgBox("This is thirdClass")  
    End Sub  
End Class  
Module callDisplay  
    Dim first As New firstClass  
    Dim second As New secondClass  
    Dim third As New thirdClass  
    Public Sub callDisplayProcedures()  
        ' The following statement displays "This is firstClass".  
        first.display()  
        ' The following statement displays "This is firstClass".  
        second.display()  
        ' The following statement displays "This is thirdClass".  
        third.display()  
    End Sub  
End Module  
```  
  
 No exemplo anterior, a classe derivada `secondClass` sombreia `display` com um `Private` procedimento. Quando `callDisplay` o módulo `display` chama `secondClass` o, o código de chamada está fora `secondClass` e, portanto, não pode acessar o `display` procedimento privado. O sombreamento é derrotado e o compilador resolve a referência ao procedimento da classe base `display` .  
  
 No entanto, a classe derivada adicional `thirdClass` declara `display` como `Public` , de modo que o código no `callDisplay` pode acessá-la.  
  
## <a name="shadowing-and-overriding"></a>Sombreamento e substituição  
 Não confunda sombreamento com substituição. Ambos são usados quando uma classe derivada herda de uma classe base e ambas redefinem um elemento declarado com outro. Mas há diferenças significativas entre os dois. Para obter uma comparação, consulte [diferenças entre sombreamento e substituição](differences-between-shadowing-and-overriding.md).  
  
## <a name="shadowing-and-overloading"></a>Sombreamento e sobrecarga  
 Se você sombrear o mesmo elemento de classe base com mais de um elemento em sua classe derivada, os elementos de sombreamento se tornarão versões sobrecarregadas desse elemento. Para obter mais informações, consulte [sobrecarga de procedimento](../procedures/procedure-overloading.md).  
  
## <a name="accessing-a-shadowed-element"></a>Acessando um elemento sombreado  
 Quando você acessa um elemento de uma classe derivada, normalmente faz isso por meio da instância atual dessa classe derivada, qualificando o nome do elemento com a `Me` palavra-chave. Se a classe derivada sombreia o elemento na classe base, você pode acessar o elemento da classe base qualificando-o com a `MyBase` palavra-chave.  
  
 Para obter um exemplo de como acessar um elemento sombreado, consulte [como acessar uma variável ocultada por uma classe derivada](how-to-access-a-variable-hidden-by-a-derived-class.md).  
  
### <a name="declaration-of-the-object-variable"></a>Declaração da variável de objeto  
 A maneira como você cria a variável de objeto também pode afetar se a classe derivada acessa um elemento de sombreamento ou o elemento sombreado. O exemplo a seguir cria dois objetos de uma classe derivada, mas um objeto é declarado como a classe base e o outro como a classe derivada.  
  
```vb  
Public Class baseCls  
    ' The following statement declares the element that is to be shadowed.  
    Public z As Integer = 100  
End Class  
Public Class dervCls  
    Inherits baseCls  
    ' The following statement declares the shadowing element.  
    Public Shadows z As String = "*"  
End Class  
Public Class useClasses  
    ' The following statement creates the object declared as the base class.  
    Dim basObj As baseCls = New dervCls()  
    ' Note that dervCls widens to its base class baseCls.  
    ' The following statement creates the object declared as the derived class.  
    Dim derObj As dervCls = New dervCls()  
    Public Sub showZ()
    ' The following statement outputs 100 (the shadowed element).  
        MsgBox("Accessed through base class: " & basObj.z)  
    ' The following statement outputs "*" (the shadowing element).  
        MsgBox("Accessed through derived class: " & derObj.z)  
    End Sub  
End Class  
```  
  
 No exemplo anterior, a variável `basObj` é declarada como a classe base. Atribuir um `dervCls` objeto a ele constitui uma conversão de ampliação e, portanto, é válido. No entanto, a classe base não pode acessar a versão de sombreamento da variável `z` na classe derivada, portanto, o compilador é resolvido `basObj.z` para o valor da classe base original.  
  
## <a name="see-also"></a>Confira também

- [Referências a elementos declarados](references-to-declared-elements.md)
- [Escopo no Visual Basic](scope.md)
- [Conversões de Widening e Narrowing](../data-types/widening-and-narrowing-conversions.md)
- [Sombras](../../../language-reference/modifiers/shadows.md)
- [Substituições](../../../language-reference/modifiers/overrides.md)
- [Me, My, MyBase e MyClass](../../program-structure/me-my-mybase-and-myclass.md)
- [Noções básicas de herança](../objects-and-classes/inheritance-basics.md)
