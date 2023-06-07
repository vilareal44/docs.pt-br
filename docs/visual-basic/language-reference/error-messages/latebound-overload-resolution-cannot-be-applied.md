---
title: Não é possível aplicar a resolução de sobrecarga com associação tardia a '<procedurename>' porque a instância de acesso é um tipo de interface
ms.date: 07/20/2015
f1_keywords:
- vbc30933
- bc30933
helpviewer_keywords:
- overload resolution [Visual Basic], with late-bound argument
- BC30933
ms.assetid: 8182eea0-dd34-4d6e-9ca0-41d8713e9dc4
ms.openlocfilehash: 4500a177c7a4729fe5131af1b007fd38e77afe07
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397331"
---
# <a name="latebound-overload-resolution-cannot-be-applied-to-procedurename-because-the-accessing-instance-is-an-interface-type"></a>Não é possível aplicar a resolução de sobrecarga com associação tardia a '\<procedurename>' porque a instância de acesso é um tipo de interface

O compilador está tentando resolver uma referência a uma propriedade ou um procedimento sobrecarregado, mas a referência falha porque um argumento é do tipo `Object` e o objeto de referência tem o tipo de dados de uma interface. O `Object` argumento força o compilador a resolver a referência como associação tardia.

Nessas circunstâncias, o compilador resolve a sobrecarga por meio da classe de implementação em vez de por meio da interface subjacente. Se a classe renomear uma das versões sobrecarregadas, o compilador não considerará que a versão é uma sobrecarga porque seu nome é diferente. Isso, por sua vez, faz com que o compilador ignore a versão renomeada quando ela pode ter sido a opção correta para resolver a referência.

**ID do erro:** BC30933

## <a name="to-correct-this-error"></a>Para corrigir este erro

- Use `CType` para converter o argumento de `Object` para o tipo especificado pela assinatura da sobrecarga que você deseja chamar.

  Observe que ele não ajuda a converter o objeto de referência para a interface subjacente. Você deve converter o argumento para evitar esse erro.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra uma chamada para um procedimento sobrecarregado `Sub` que causa esse erro no momento da compilação.

```vb
Module m1
    Interface i1
        Sub s1(ByVal p1 As Integer)
        Sub s1(ByVal p1 As Double)
    End Interface
    Class c1
        Implements i1
        Public Overloads Sub s1(ByVal p1 As Integer) Implements i1.s1
        End Sub
        Public Overloads Sub s2(ByVal p1 As Double) Implements i1.s1
        End Sub
    End Class
    Sub Main()
        Dim refer As i1 = New c1
        Dim o1 As Object = 3.1415
        ' The following reference is INVALID and causes a compiler error.
        refer.s1(o1)
    End Sub
End Module
```

No exemplo anterior, se o compilador permitisse a chamada para as `s1` gravados, a resolução ocorreria por meio da classe `c1` em vez da interface `i1` . Isso significa que o compilador não consideraria `s2` porque seu nome é diferente no `c1` , embora seja a opção correta, conforme definido pelo `i1` .

Você pode corrigir o erro alterando a chamada para uma das seguintes linhas de código:

```vb
refer.s1(CType(o1, Integer))
refer.s1(CType(o1, Double))
```

Cada uma das linhas de código anteriores converte explicitamente a `Object` variável `o1` em um dos tipos de parâmetro definidos para as sobrecargas.

## <a name="see-also"></a>Confira também

- [Sobrecarga de procedimento](../../programming-guide/language-features/procedures/procedure-overloading.md)
- [Resolução de sobrecarga](../../programming-guide/language-features/procedures/overload-resolution.md)
- [Função CType](../functions/ctype-function.md)
