---
title: Os métodos de 'System.Nullable(Of T)' não podem ser usados como operandos do operador 'AddressOf'
ms.date: 07/20/2015
f1_keywords:
- vbc32126
- bc32126
helpviewer_keywords:
- BC32126
ms.assetid: 2325668b-e2ad-40ee-a1ec-30450236c20d
ms.openlocfilehash: 61c6fe7c33b3292066e653304ded43a863413723
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397214"
---
# <a name="methods-of-systemnullableof-t-cannot-be-used-as-operands-of-the-addressof-operator"></a>Os métodos de 'System.Nullable(Of T)' não podem ser usados como operandos do operador 'AddressOf'
Uma instrução usa o `AddressOf` operador com um operando que representa um procedimento da <xref:System.Nullable%601> estrutura.  
  
 **ID do erro:** BC32126  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Substitua o nome do procedimento na `AddressOf` cláusula por um operando que não seja membro de <xref:System.Nullable%601> .  
  
- Escreva uma classe que encapsula o método de <xref:System.Nullable%601> que você deseja usar. No exemplo a seguir, a `NullableWrapper` classe define um novo método chamado `GetValueOrDefault` . Como esse novo método não é um membro de <xref:System.Nullable%601> , ele pode ser aplicado a `nullInstance` uma instância de um tipo anulável para formar um argumento para `AddressOf` .  
  
```vb  
Module Module1  
  
    Delegate Function Deleg() As Integer  
  
    Sub Main()  
        Dim nullInstance As New Nullable(Of Integer)(1)  
  
        Dim del As Deleg  
  
        ' GetValueOrDefault is a method of the Nullable generic  
        ' type. It cannot be used as an operand of AddressOf.  
        ' del = AddressOf nullInstance.GetValueOrDefault  
  
        ' The following line uses the GetValueOrDefault method  
        ' defined in the NullableWrapper class.  
        del = AddressOf (New NullableWrapper(  
            Of Integer)(nullInstance)).GetValueOrDefault  
  
        Console.WriteLine(del.Invoke())  
    End Sub  
  
    Class NullableWrapper(Of T As Structure)  
        Private m_Value As Nullable(Of T)  
  
        Sub New(ByVal Value As Nullable(Of T))  
            m_Value = Value  
        End Sub  
  
        Public Function GetValueOrDefault() As T  
            Return m_Value.Value  
        End Function  
    End Class  
End Module  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.Nullable%601>
- [Operador AddressOf](../operators/addressof-operator.md)
- [Tipos de valor anulável](../../programming-guide/language-features/data-types/nullable-value-types.md)
- [Tipos genéricos no Visual Basic](../../programming-guide/language-features/data-types/generic-types.md)
