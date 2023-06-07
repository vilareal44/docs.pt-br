---
title: Não é possível inferir o(s) tipo(s) de dados do(s) parâmetro(s) de tipo a partir destes argumentos
ms.date: 07/20/2015
f1_keywords:
- bc36644
- bc36647
- vbc36647
- vbc36644
helpviewer_keywords:
- BC36644
- BC36647
ms.assetid: 0e0050f2-2039-4311-b260-f0ebfde84189
ms.openlocfilehash: b278dcc10a8a4e9e67aacdb16dca2588d2ebd0f1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409772"
---
# <a name="data-types-of-the-type-parameters-cannot-be-inferred-from-these-arguments"></a>Não é possível inferir o(s) tipo(s) de dados do(s) parâmetro(s) de tipo a partir destes argumentos

Não é possível inferir os tipos de dados dos parâmetros de tipo com base nesses argumentos. A especificação explícita dos tipos de dados pode corrigir esse erro.

Esse erro ocorre quando a resolução de sobrecarga falhou. Ele ocorre como uma mensagem subordinada que declara por que um determinado candidato de sobrecarga foi eliminado. A mensagem de erro explica que o compilador não pode usar a inferência de tipos para localizar tipos de dados para os parâmetros de tipo.

> [!NOTE]
> Ao especificar argumentos não é uma opção (por exemplo, para operadores de consulta em expressões de consulta), a mensagem de erro é exibida sem a segunda sentença.

O código a seguir demonstra o erro.

```vb
Module Module1

    Sub Main()

        '' Not Valid.
        'OverloadedGenericMethod("Hello", "World")

    End Sub

    Sub OverloadedGenericMethod(Of T)(ByVal x As String,
                                      ByVal y As InterfaceExample(Of T))
    End Sub

    Sub OverloadedGenericMethod(Of T, R)(ByVal x As T,
                                         ByVal y As InterfaceExample(Of R))
    End Sub

End Module

Interface InterfaceExample(Of T)
End Interface
```

**ID do erro:** BC36647 e BC36644

## <a name="to-correct-this-error"></a>Para corrigir este erro

Você pode especificar um tipo de dados para o parâmetro de tipo ou parâmetros em vez de depender da inferência de tipos.

## <a name="see-also"></a>Confira também

- [Conversão de delegado reduzida](../../programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
- [Procedimentos genéricos no Visual Basic](../../programming-guide/language-features/data-types/generic-procedures.md)
- [Conversões de tipo no Visual Basic](../../programming-guide/language-features/data-types/type-conversions.md)
