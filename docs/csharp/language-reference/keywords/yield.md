---
title: Palavra-chave contextual yield – referência de C#
ms.date: 07/20/2015
f1_keywords:
- yield
- yield_CSharpKeyword
helpviewer_keywords:
- yield keyword [C#]
ms.assetid: 1089194f-9e53-46a2-8642-53ccbe9d414d
ms.openlocfilehash: ec1058d1590d64fa8d8786b3118ecf9733c55d6f
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063140"
---
# <a name="yield-c-reference"></a>yield (Referência de C#)

Ao usar a  [palavra-chave contextual](index.md#contextual-keywords)`yield` em uma instrução, você indica que o método, o operador ou o acessador `get` em que ela é exibida é um iterador. Usar `yield` para definir um iterador elimina a necessidade de uma classe adicional explícita (a classe que mantém o estado de uma enumeração, consulte <xref:System.Collections.Generic.IEnumerator%601> para obter um exemplo) ao implementar o padrão <xref:System.Collections.IEnumerable> e <xref:System.Collections.IEnumerator> para um tipo de coleção personalizado.

O exemplo a seguir mostra as duas formas de instrução `yield`.

```csharp
yield return <expression>;
yield break;
```

## <a name="remarks"></a>Comentários

Você usa uma instrução `yield return` para retornar cada elemento individualmente.

A sequência retornada de um método iterador pode ser consumida usando uma instrução [foreach](foreach-in.md) ou uma consulta LINQ. Cada iteração do loop `foreach` chama o método iterador. Quando uma instrução `yield return` é atingida no método iterador, `expression` é retornado e o local atual no código é retido. A execução será reiniciada desse local na próxima vez que a função iteradora for chamada.

Você pode usar uma instrução `yield break` para terminar a iteração.

Para obter mais informações sobre iteradores, consulte [Iteradores](../../iterators.md).

## <a name="iterator-methods-and-get-accessors"></a>Métodos de iterador e acessadores get

A declaração de um iterador deve atender aos seguintes requisitos:

- O tipo de retorno deve ser <xref:System.Collections.IEnumerable>, <xref:System.Collections.Generic.IEnumerable%601>, <xref:System.Collections.IEnumerator> ou <xref:System.Collections.Generic.IEnumerator%601>.

- A declaração não pode ter os parâmetros [in, ](in-parameter-modifier.md) [ref](ref.md) nem [out](out-parameter-modifier.md).

O tipo `yield` de um iterador que retorna <xref:System.Collections.IEnumerable> ou <xref:System.Collections.IEnumerator> é `object`.  Se o iterador retornar <xref:System.Collections.Generic.IEnumerable%601> ou <xref:System.Collections.Generic.IEnumerator%601>, uma conversão implícita deverá existir do tipo da expressão na instrução `yield return` para o parâmetro de tipo genérico.

Você não pode incluir uma instrução `yield return` ou `yield break` em:

- [Expressões lambda](../operators/lambda-expressions.md) e [métodos anônimos](../operators/delegate-operator.md).

- Métodos que contêm blocos inseguros. Para obter mais informações, consulte [unsafe](unsafe.md).

## <a name="exception-handling"></a>Tratamento de exceções

Uma instrução `yield return` não pode estar localizada em um bloco try-catch. Uma instrução `yield return` pode estar localizada no bloco try de uma instrução try-finally.

Uma instrução `yield break` pode estar localizada em um bloco try ou em um bloco catch, mas não em um bloco finally.

Se o corpo `foreach` (fora do método iterador) acionar uma exceção, um bloco `finally` no método iterador será executado.

## <a name="technical-implementation"></a>Implementação técnica

O código a seguir retorna uma `IEnumerable<string>` de um método iterador e itera através de seus elementos.

```csharp
IEnumerable<string> elements = MyIteratorMethod();
foreach (string element in elements)
{
   ...
}
```

A chamada a `MyIteratorMethod` não executa o corpo do método. Em vez disso, a chamada retorna `IEnumerable<string>` na variável `elements`.

Em uma iteração do loop `foreach`, o método <xref:System.Collections.IEnumerator.MoveNext%2A> é chamado para `elements`. Essa chamada executará o corpo de `MyIteratorMethod` até que a próxima instrução `yield return` seja atingida. A expressão retornada pela instrução `yield return` determina não apenas o valor da variável `element` para o consumo do corpo do loop, mas também a propriedade <xref:System.Collections.Generic.IEnumerator%601.Current%2A> de `elements`, que é uma `IEnumerable<string>`.

Em cada iteração subsequente do loop `foreach`, a execução do corpo do iterador continuará de onde parou, parando novamente quando atingir uma instrução `yield return`. O loop `foreach` é concluído quando o fim do método iterador ou uma instrução `yield break` é atingida.

## <a name="example"></a>Exemplo

O exemplo a seguir contém uma instrução `yield return` dentro de um loop `for`. Cada iteração do corpo da instrução `foreach` no método `Main` cria uma chamada à função iteradora `Power`. Cada chamada à função iteradora prossegue para a próxima execução da instrução `yield return` que ocorre durante a próxima iteração do loop `for`.

O tipo de retorno do método iterador é <xref:System.Collections.IEnumerable> que é um tipo de interface de iterador. Quando o método iterador é chamado, ele retorna um objeto enumerável que contém as potências de um número.

[!code-csharp[csrefKeywordsContextual#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#5)]

## <a name="example"></a>Exemplo

O exemplo a seguir demonstra um acessador `get` que é um iterador. No exemplo, cada instrução `yield return` retorna uma instância de uma classe definida pelo usuário.

[!code-csharp[csrefKeywordsContextual#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#21)]

## <a name="c-language-specification"></a>Especificação da linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Consulte também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [foreach, in](foreach-in.md)
- [Iterators](../../iterators.md)
