---
title: Instrução using – Referência de C#
ms.date: 05/29/2020
helpviewer_keywords:
- using statement [C#]
ms.assetid: afc355e6-f0b9-4240-94dd-0d93f17d9fc3
ms.openlocfilehash: b889d2fcbdf854dbe8948744810f9b74e9f0dac2
ms.sourcegitcommit: 5280b2aef60a1ed99002dba44e4b9e7f6c830604
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84307040"
---
# <a name="using-statement-c-reference"></a>Instrução using (Referência de C#)

Fornece uma sintaxe conveniente que garante o uso correto de objetos <xref:System.IDisposable>. A partir do C# 8,0, a `using` instrução garante o uso correto de <xref:System.IAsyncDisposable> objetos.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como usar a instrução `using`.

:::code language="csharp" source="snippets/usings.cs" id="SnippetFirstExample":::

A partir do C# 8,0, você pode usar a seguinte sintaxe alternativa para a `using` instrução que não exige chaves:

:::code language="csharp" source="snippets/usings.cs" id="SnippetModernUsing":::

## <a name="remarks"></a>Comentários

<xref:System.IO.File> e <xref:System.Drawing.Font> são exemplos de tipos gerenciados que acessam recursos não gerenciados (nesse caso, identificadores de arquivo de caso e contextos de dispositivo). Há muitos outros tipos de recursos não gerenciados e tipos de biblioteca de classes que os encapsula. Todos esses tipos devem implementar a <xref:System.IDisposable> interface ou a <xref:System.IAsyncDisposable> interface.

Quando o tempo de vida de um objeto `IDisposable` é limitado a um único método, você deve declará-lo e instanciá-lo na instrução `using`. A instrução `using` chama o método <xref:System.IDisposable.Dispose%2A> no objeto da forma correta e (quando você o usa como mostrado anteriormente) ele também faz com que o objeto em si saia do escopo assim que <xref:System.IDisposable.Dispose%2A> é chamado. Dentro do `using` bloco, o objeto é somente leitura e não pode ser modificado ou reatribuído. Se o objeto for implementado `IAsyncDisposable` em vez de `IDisposable` , a `using` instrução chamará o <xref:System.IAsyncDisposable.DisposeAsync%2A> e `awaits` o retornado <xref:System.Threading.Tasks.ValueTask> . Para obter mais informações sobre <xref:System.IAsyncDisposable> o, consulte [implementar um método DisposeAsync](../../../standard/garbage-collection/implementing-disposeasync.md).

A `using` instrução garante que <xref:System.IDisposable.Dispose%2A> (ou <xref:System.IAsyncDisposable.DisposeAsync%2A> ) seja chamado mesmo se ocorrer uma exceção dentro do `using` bloco. Você pode obter o mesmo resultado colocando o objeto dentro de um `try` bloco e, em seguida <xref:System.IDisposable.Dispose%2A> , chamando (ou <xref:System.IAsyncDisposable.DisposeAsync%2A> ) em um `finally` bloco; na verdade, é assim que a `using` instrução é convertida pelo compilador. O exemplo de código anterior se expande para o seguinte código em tempo de compilação (observe as chaves extras para criar o escopo limitado para o objeto):

:::code language="csharp" source="snippets/usings.cs" id="SnippetTryFinallyExample":::

A sintaxe mais recente da `using` instrução é convertida em código semelhante. O `try` bloco é aberto onde a variável é declarada. O `finally` bloco é adicionado ao fechamento do bloco delimitador, normalmente no final de um método.

Para obter mais informações sobre a `try` - `finally` instrução, consulte o artigo [Experimente-finally](try-finally.md) .

Várias instâncias de um tipo podem ser declaradas em uma única `using` instrução, conforme mostrado no exemplo a seguir. Observe que você não pode usar variáveis de tipo implícito ( `var` ) ao declarar várias variáveis em uma única instrução:

:::code language="csharp" source="snippets/usings.cs" id="SnippetDeclareMultipleVariables":::

Você pode combinar várias declarações do mesmo tipo usando a nova sintaxe introduzida com C# 8 também, conforme mostrado no exemplo a seguir:

:::code language="csharp" source="snippets/usings.cs" id="SnippetModernMultipleVariables":::

Você pode instanciar o objeto de recurso e, em seguida, passar a variável para a `using` instrução, mas essa não é uma prática recomendada. Nesse caso, após o controle sair do bloco `using`, o objeto permanecerá no escopo, mas provavelmente não terá acesso a seus recursos não gerenciados. Em outras palavras, ele não é mais totalmente inicializado. Se você tentar usar o objeto fora do bloco `using`, corre o risco de causar o lançamento de uma exceção. Por esse motivo, é melhor instanciar o objeto na `using` instrução e limitar seu escopo ao `using` bloco.

:::code language="csharp" source="snippets/usings.cs" id="SnippetDeclareBeforeUsing":::

Para obter mais informações sobre como descartar objetos `IDisposable`, veja [Usando objetos que implementam IDisposable](../../../standard/garbage-collection/using-objects.md).

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, consulte [A instrução using](~/_csharplang/spec/statements.md#the-using-statement) na [Especificação da linguagem C#](/dotnet/csharp/language-reference/language-specification/introduction). A especificação da linguagem é a fonte definitiva para a sintaxe e o uso de C#.

## <a name="see-also"></a>Veja também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [Diretiva de uso](using-directive.md)
- [Coleta de lixo](../../../standard/garbage-collection/index.md)
- [Usando objetos que implementam IDisposable](../../../standard/garbage-collection/using-objects.md)
- [Interface IDisposable](xref:System.IDisposable)
- [instrução using em C# 8,0](~/_csharplang/proposals/csharp-8.0/using.md)
