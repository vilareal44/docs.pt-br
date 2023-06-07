---
title: Operadores condicionais nulos
ms.date: 10/19/2018
helpviewer_keywords:
- null-conditional operators [Visual Basic]
- ?. operator [Visual Basic]
- ?[] operator [C#]
- ?[] operator [Visual Basic]
ms.openlocfilehash: bffbba859968e0a050397cd9e685c142f801798a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401466"
---
# <a name="-and--null-conditional-operators-visual-basic"></a>?. e? () operadores condicionais nulos (Visual Basic)

Testa o valor do operando esquerdo para NULL ( `Nothing` ) antes de executar uma operação de acesso de membro ( `?.` ) ou de índice ( `?()` ); retorna `Nothing` se o operando à esquerda for avaliado como `Nothing` . Observe que em expressões que normalmente retornam tipos de valor, o operador NULL-Conditional retorna um <xref:System.Nullable%601> .

Esses operadores ajudam a escrever menos código para lidar com verificações nulas, especialmente quando decrescentes em estruturas de dados. Por exemplo:

```vb
' Nothing if customers is Nothing
Dim length As Integer? = customers?.Length

' Nothing if customers is Nothing
Dim first As Customer = customers?(0)

' Nothing if customers, the first customer, or Orders is Nothing
Dim count As Integer? = customers?(0)?.Orders?.Count()
```

Para comparação, o código alternativo para a primeira dessas expressões sem um operador condicional nulo é:

```vb
Dim length As Integer
If customers IsNot Nothing Then
   length = customers.Length
End If
```

Às vezes, você precisa tomar uma ação em um objeto que pode ser nulo, com base no valor de um membro booliano nesse objeto (como a propriedade booliana `IsAllowedFreeShipping` no exemplo a seguir):

```vb
Dim customer = FindCustomerByID(123) 'customer will be Nothing if not found.

If customer IsNot Nothing AndAlso customer.IsAllowedFreeShipping Then
  ApplyFreeShippingToOrders(customer)
End If
```

Você pode encurtar seu código e evitar a verificação manual de NULL usando o operador NULL-Conditional da seguinte maneira:

```vb
Dim customer = FindCustomerByID(123) 'customer will be Nothing if not found.

If customer?.IsAllowedFreeShipping Then ApplyFreeShippingToOrders(customer)
```

Os operadores condicionais nulos estão entrando em curto-circuito.  Se uma operação em uma cadeia de acesso de membro condicional e operações de índice forem retornadas `Nothing` , o restante da execução da cadeia será interrompido.  No exemplo a seguir, `C(E)` não é avaliado se `A` , `B` ou `C` é avaliado como `Nothing` .

```vb
A?.B?.C?(E)
```

Outro uso para acesso de membro condicional nulo é invocar delegados de forma segura para thread com muito menos código.  O exemplo a seguir define dois tipos, a `NewsBroadcaster` e a `NewsReceiver` . Os itens de notícias são enviados ao destinatário pelo `NewsBroadcaster.SendNews` delegado.

```vb
Public Module NewsBroadcaster
   Dim SendNews As Action(Of String)

   Public Sub Main()
      Dim rec As New NewsReceiver()
      Dim rec2 As New NewsReceiver()
      SendNews?.Invoke("Just in: A newsworthy item...")
   End Sub

   Public Sub Register(client As Action(Of String))
      SendNews = SendNews.Combine({SendNews, client})
   End Sub
End Module

Public Class NewsReceiver
   Public Sub New()
      NewsBroadcaster.Register(AddressOf Me.DisplayNews)
   End Sub

   Public Sub DisplayNews(newsItem As String)
      Console.WriteLine(newsItem)
   End Sub
End Class
```

Se não houver nenhum elemento na `SendNews` lista de invocação, o `SendNews` delegado lançará um <xref:System.NullReferenceException> . Antes de operadores condicionais NULL, o código como o seguinte garantiu que a lista de invocação de delegado não foi `Nothing` :

```vb
SendNews = SendNews.Combine({SendNews, client})
If SendNews IsNot Nothing Then
   SendNews("Just in...")
End If
```

A nova maneira é muito mais simples:

```vb
SendNews = SendNews.Combine({SendNews, client})
SendNews?.Invoke("Just in...")
```

A nova forma é thread-safe porque o compilador gera código para avaliar `SendNews` somente uma vez, mantendo o resultado em uma variável temporária. Você precisa chamar explicitamente o método `Invoke` porque não há nenhuma sintaxe de invocação de delegado condicional nulo `SendNews?(String)`.

## <a name="see-also"></a>Confira também

- [Operadores (Visual Basic)](index.md)
- [Guia de programação do Visual Basic](../../programming-guide/index.md)
- [Referência de linguagem de Visual Basic](../index.md)
