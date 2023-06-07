---
title: Noções básicas de herança
ms.date: 07/20/2015
helpviewer_keywords:
- derived classes [Visual Basic], inheritance
- MyClass keyword [Visual Basic], using
- MyBase keyword [Visual Basic], using
- Inherits statement [Visual Basic], inheritance
- overriding, Overridable keyword
- MustInherit keyword [Visual Basic], using
- Overrides keyword [Visual Basic], using
- inheritance
- MustInherit classes [Visual Basic]
- MustOverride keyword [Visual Basic], using
- classes [Visual Basic], derived
- NotInheritable keyword [Visual Basic], using
- base classes [Visual Basic], extending properties and methods [Visual Basic]
- NotOverridable keyword [Visual Basic], using
- base classes [Visual Basic], inheritance
- abstract classes [Visual Basic], inheritance
- overriding, Overrides keyword
ms.assetid: dfc8deba-f5b3-4d1d-a937-7cb826446fc5
ms.openlocfilehash: 3bf1847f618a642d26df4aa1c5247a4ba2bd3b23
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411773"
---
# <a name="inheritance-basics-visual-basic"></a>Noções básicas de herança (Visual Basic)

A `Inherits` instrução é usada para declarar uma nova classe, chamada *classe derivada*, com base em uma classe existente, conhecida como *classe base*. As classes derivadas herdam e podem estender, as propriedades, os métodos, os eventos, os campos e as constantes definidas na classe base. A seção a seguir descreve algumas das regras de herança e os modificadores que você pode usar para alterar a forma como as classes herdam ou são herdadas:

- Por padrão, todas as classes são herdáveis, a menos que sejam marcadas com a `NotInheritable` palavra-chave. Classes podem herdar de outras classes em seu projeto ou de classes em outros assemblies que seu projeto referencia.

- Ao contrário de linguagens que permitem várias heranças, Visual Basic permite apenas uma única herança em classes; ou seja, as classes derivadas podem ter apenas uma classe base. Embora várias heranças não sejam permitidas em classes, as classes podem implementar várias interfaces, o que pode efetivamente atingir as mesmas extremidades.

- Para evitar a exposição de itens restritos em uma classe base, o tipo de acesso de uma classe derivada deve ser igual ou mais restritivo do que sua classe base. Por exemplo, uma `Public` classe não pode herdar uma `Friend` `Private` classe ou, e uma `Friend` classe não pode herdar uma `Private` classe.

## <a name="inheritance-modifiers"></a>Modificadores de herança

Visual Basic apresenta as seguintes instruções e modificadores de nível de classe para dar suporte à herança:

- `Inherits`instrução — especifica a classe base.

- `NotInheritable`modificador — impede que os programadores usem a classe como uma classe base.

- `MustInherit`modificador — especifica que a classe destina-se ao uso apenas como uma classe base. Instâncias de `MustInherit` classes não podem ser criadas diretamente; elas só podem ser criadas como instâncias de classe base de uma classe derivada. (Outras linguagens de programação, como C++ e C#, usam a *classe abstract* de termo para descrever tal classe.)

## <a name="overriding-properties-and-methods-in-derived-classes"></a>Substituindo propriedades e métodos em classes derivadas

Por padrão, uma classe derivada herda propriedades e métodos de sua classe base. Se uma propriedade ou método herdado tiver que se comportar de forma diferente na classe derivada, ela poderá ser *substituída*. Ou seja, você pode definir uma nova implementação do método na classe derivada. Os seguintes modificadores são usados para controlar como as propriedades e métodos são substituídos:

- `Overridable`— Permite que uma propriedade ou método em uma classe seja substituído em uma classe derivada.

- `Overrides`— Substitui uma `Overridable` propriedade ou método definido na classe base.

- `NotOverridable`— Impede que uma propriedade ou método seja substituído em uma classe de herança. Por padrão, os `Public` métodos são `NotOverridable` .

- `MustOverride`— Requer que uma classe derivada substitua a propriedade ou o método. Quando a `MustOverride` palavra-chave é usada, a definição do método consiste apenas na `Sub` `Function` instrução, ou `Property` . Nenhuma outra instrução é permitida e, especificamente, não há instruções `End Sub` ou `End Function` . `MustOverride`os métodos devem ser declarados em `MustInherit` classes.

Suponha que você queira definir classes para lidar com a folha de pagamento. Você pode definir uma `Payroll` classe genérica que contém um `RunPayroll` método que calcula a folha de pagamento de uma semana típica. Em seguida, você poderia usar `Payroll` como uma classe base para uma `BonusPayroll` classe mais especializada, que poderia ser usada ao distribuir bônus do funcionário.

A `BonusPayroll` classe pode herdar e substituir o `PayEmployee` método definido na `Payroll` classe base.

O exemplo a seguir define uma classe base `Payroll,` e uma classe derivada, `BonusPayroll` , que substitui um método herdado, `PayEmployee` . Um procedimento, `RunPayroll` , cria e, em seguida, passa um `Payroll` objeto e um `BonusPayroll` objeto para uma função, `Pay` , que executa o `PayEmployee` método de ambos os objetos.

[!code-vb[VbVbalrOOP#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#28)]

## <a name="the-mybase-keyword"></a>A palavra-chave MyBase

A `MyBase` palavra-chave se comporta como uma variável de objeto que se refere à classe base da instância atual de uma classe. `MyBase`é frequentemente usado para acessar membros da classe base que são substituídos ou sombreados em uma classe derivada. Em particular, `MyBase.New` é usado para chamar explicitamente um construtor de classe base de um construtor de classe derivada.

Por exemplo, suponha que você esteja criando uma classe derivada que substitua um método herdado da classe base. O método substituído pode chamar o método na classe base e modificar o valor de retorno, conforme mostrado no fragmento de código a seguir:

[!code-vb[VbVbalrOOP#109](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#109)]

A lista a seguir descreve as restrições sobre como usar o `MyBase` :

- `MyBase`refere-se à classe base imediata e seus membros herdados. Ele não pode ser usado para acessar `Private` Membros na classe.

- `MyBase`é uma palavra-chave, não um objeto real. `MyBase`Não pode ser atribuído a uma variável, passada para procedimentos ou usado em uma `Is` comparação.

- O método `MyBase` qualificado não precisa ser definido na classe base imediata; em vez disso, ele pode ser definido em uma classe base indiretamente herdada. Para que uma referência qualificada pelo `MyBase` seja compilada corretamente, uma classe base deve conter um método que corresponda ao nome e aos tipos de parâmetros que aparecem na chamada.

- Você não pode usar `MyBase` para chamar `MustOverride` métodos de classe base.

- `MyBase`Não pode ser usado para se qualificar. Portanto, o código a seguir não é válido:

  `MyBase.MyBase.BtnOK_Click()`

- `MyBase`Não pode ser usado em módulos.

- `MyBase`Não pode ser usado para acessar membros da classe base que são marcados como `Friend` se a classe base estiver em um assembly diferente.

Para obter mais informações e outro exemplo, consulte [como: acessar uma variável ocultada por uma classe derivada](../declared-elements/how-to-access-a-variable-hidden-by-a-derived-class.md).

## <a name="the-myclass-keyword"></a>A palavra-chave MyClass

A `MyClass` palavra-chave se comporta como uma variável de objeto que se refere à instância atual de uma classe como implementada originalmente. `MyClass`é semelhante `Me` , mas cada chamada de método e propriedade em `MyClass` é tratada como se o método ou a propriedade fossem [NotOverridable](../../../language-reference/modifiers/notoverridable.md). Portanto, o método ou a propriedade não é afetada pela substituição em uma classe derivada.

- `MyClass`é uma palavra-chave, não um objeto real. `MyClass`Não pode ser atribuído a uma variável, passada para procedimentos ou usado em uma `Is` comparação.

- `MyClass`refere-se à classe que a contém e seus membros herdados.

- `MyClass`pode ser usado como um qualificador para `Shared` Membros.

- `MyClass`Não pode ser usado dentro de um `Shared` método, mas pode ser usado dentro de um método de instância para acessar um membro compartilhado de uma classe.

- `MyClass`Não pode ser usado em módulos padrão.

- `MyClass`pode ser usado para qualificar um método que é definido em uma classe base e que não tem implementação do método fornecido nessa classe. Tal referência tem o mesmo significado que o `MyBase.` *método*.

O exemplo a seguir compara `Me` e `MyClass` .

```vb
Class baseClass
    Public Overridable Sub testMethod()
        MsgBox("Base class string")
    End Sub
    Public Sub useMe()
        ' The following call uses the calling class's method, even if
        ' that method is an override.
        Me.testMethod()
    End Sub
    Public Sub useMyClass()
        ' The following call uses this instance's method and not any
        ' override.
        MyClass.testMethod()
    End Sub
End Class
Class derivedClass : Inherits baseClass
    Public Overrides Sub testMethod()
        MsgBox("Derived class string")
    End Sub
End Class
Class testClasses
    Sub startHere()
        Dim testObj As derivedClass = New derivedClass()
        ' The following call displays "Derived class string".
        testObj.useMe()
        ' The following call displays "Base class string".
        testObj.useMyClass()
    End Sub
End Class
```

Embora as `derivedClass` substituições `testMethod` , a `MyClass` palavra-chave em `useMyClass` invalidará os efeitos da substituição e o compilador resolve a chamada para a versão da classe base do `testMethod` .

## <a name="see-also"></a>Confira também

- [Instrução Inherits](../../../language-reference/statements/inherits-statement.md)
- [Me, My, MyBase e MyClass](../../program-structure/me-my-mybase-and-myclass.md)
