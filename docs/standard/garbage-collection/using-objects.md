---
title: Usando objetos que implementam IDisposable
description: Saiba como usar objetos que implementam a interface IDisposable no .NET. Os tipos que usam recursos não gerenciados implementam IDisposable para permitir a recuperação de recursos.
ms.date: 05/13/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Dispose method
- try/finally block
- garbage collection, encapsulating resources
ms.assetid: 81b2cdb5-c91a-4a31-9c83-eadc52da5cf0
ms.openlocfilehash: 7d5d4080f22aab6870a230d495b4a4b9ebcb3b96
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599848"
---
# <a name="using-objects-that-implement-idisposable"></a>Usando objetos que implementam IDisposable

O coletor de lixo do Common Language Runtime recupera a memória usada por objetos gerenciados, mas os tipos que usam recursos não gerenciados implementam a <xref:System.IDisposable> interface para permitir que os recursos necessários a esses recursos não gerenciados sejam recuperados. Após terminar de usar um objeto que implementa <xref:System.IDisposable>, você deverá chamar a implementação de <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> do objeto. É possível fazer isso de duas formas:

- Com a `using` instrução C# ( `Using` em Visual Basic).
- Implementando um `try/finally` bloco e chamando o <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> no `finally` .

## <a name="the-using-statement"></a>A instrução using

A [ `using` instrução](../../csharp/language-reference/keywords/using-statement.md) em C# e a [ `Using` instrução](../../visual-basic/language-reference/statements/using-statement.md) em Visual Basic simplificam o código que você deve escrever para limpar um objeto. A instrução `using` obtém um ou mais recursos, executa as instruções que você especifica e descarta o objeto automaticamente. Entretanto, a instrução `using` é útil apenas para os objetos que são usados no escopo do método no qual eles são criados.

O exemplo a seguir usa a instrução `using` para criar e liberar um objeto <xref:System.IO.StreamReader?displayProperty=nameWithType>.

[!code-csharp[Conceptual.Disposable#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/using1.cs#1)]
[!code-vb[Conceptual.Disposable#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/using1.vb#1)]

Embora a <xref:System.IO.StreamReader> classe implemente a <xref:System.IDisposable> interface, que indica que ela usa um recurso não gerenciado, o exemplo não chama explicitamente o <xref:System.IO.StreamReader.Dispose%2A?displayProperty=nameWithType> método. Quando o compilador do C# ou do Visual Basic encontra a instrução `using`, ele emite a IL (linguagem intermediária) que equivale ao seguinte código que contém explicitamente um bloco `try/finally`.

[!code-csharp[Conceptual.Disposable#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/using3.cs#3)]
[!code-vb[Conceptual.Disposable#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/using3.vb#3)]

A instrução `using` do C# também permite que você adquira vários recursos em uma única instrução, o que é internamente equivalente a instruções `using` aninhadas. O exemplo a seguir cria instancia dois objetos <xref:System.IO.StreamReader> para ler o conteúdo de dois arquivos diferentes.

[!code-csharp[Conceptual.Disposable#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/using4.cs#4)]

## <a name="tryfinally-block"></a>Bloco Try/finally

Em vez de incluir um bloco `try/finally` em uma instrução `using`, você pode optar por implementar diretamente o bloco `try/finally`. Pode ser seu estilo de codificação pessoal ou você pode desejar fazer isso por um dos seguintes motivos:

- Para incluir um `catch` bloco para tratar as exceções lançadas no `try` bloco. Caso contrário, as exceções geradas na `using` instrução não serão manipuladas.

- Para criar uma instância de um objeto que implementa <xref:System.IDisposable> cujo escopo não é local para o bloco dentro do qual é declarado.

O exemplo a seguir é semelhante ao exemplo anterior, exceto que ele usa um bloco `try/catch/finally` para criar uma instância, usar e descartar um objeto <xref:System.IO.StreamReader> e também para manipular todas as exceções lançadas pelo construtor <xref:System.IO.StreamReader> e pelo método <xref:System.IO.StreamReader.ReadToEnd%2A>. O código no `finally` bloco verifica se o objeto que implementa <xref:System.IDisposable> não está `null` antes de chamar o <xref:System.IDisposable.Dispose%2A> método. Não fazer isso poderá levar a uma exceção de <xref:System.NullReferenceException> em tempo de execução.

[!code-csharp[Conceptual.Disposable#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/using5.cs#6)]
[!code-vb[Conceptual.Disposable#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/using5.vb#6)]

Você poderá seguir esse padrão básico se optar por implementar ou precisar implementar um bloco `try/finally`, pois a linguagem de programação não oferece suporte a uma instrução `using`, mas permite chamadas diretas para o método <xref:System.IDisposable.Dispose%2A>.

## <a name="idisposable-instance-members"></a>Membros da instância IDisposable

Se uma classe mantém uma <xref:System.IDisposable> implementação como um membro de instância, seja um campo ou uma propriedade, a classe também deve implementar <xref:System.IDisposable> . Para obter mais informações, consulte [implementar uma disposição em cascata](implementing-dispose.md#cascade-dispose-calls).

## <a name="see-also"></a>Consulte também

- [Limpando recursos não gerenciados](unmanaged.md)
- [Instrução using (referência C#)](../../csharp/language-reference/keywords/using-statement.md)
- [Instrução Using (Visual Basic)](../../visual-basic/language-reference/statements/using-statement.md)
