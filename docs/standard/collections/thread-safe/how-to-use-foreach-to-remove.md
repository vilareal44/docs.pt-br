---
title: Usar ForEach para remover itens de um BlockingCollection
ms.date: 05/04/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- thread-safe collections, how to enumerate blocking collection
ms.assetid: 2096103c-22f7-420d-b631-f102bc33a6dd
ms.openlocfilehash: 46638d2cd8078fefebc0eacc4b8f7798ffe178ff
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288895"
---
# <a name="use-foreach-to-remove-items-in-a-blockingcollection"></a>Usar ForEach para remover itens de um BlockingCollection

Além de levar itens de um <xref:System.Collections.Concurrent.BlockingCollection%601> usando o <xref:System.Collections.Concurrent.BlockingCollection%601.Take%2A> <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> método e, você também pode usar um [foreach](../../../csharp/language-reference/keywords/foreach-in.md) ([para cada](../../../visual-basic/language-reference/statements/for-each-next-statement.md) em Visual Basic) com o <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType> para remover itens até que a adição seja concluída e a coleção esteja vazia. Isso é chamado de *enumeração de mutação* ou de *mutação consumidora* porque, ao contrário de um loop típico `foreach` (`For Each`), esse enumerador modifica a coleção de origem, removendo itens.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como remover todos os itens em um <xref:System.Collections.Concurrent.BlockingCollection%601> usando um loop `foreach` (`For Each`).

[!code-csharp[CDS_BlockingCollection#03](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example03.cs#03)]
[!code-vb[CDS_BlockingCollection#03](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/enumeratebc.vb#03)]

Este exemplo usa um loop `foreach` com o método <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType> no thread consumidor, que faz com que cada item seja removido da coleção ao ser enumerado. <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> limita o número máximo de itens que estão na coleção a qualquer momento. Enumerar a coleção dessa maneira bloqueará o thread consumidor se nenhum item estiver disponível ou se a coleção estiver vazia. Neste exemplo, o bloqueio não é um problema porque o thread produtor adiciona itens mais rápido do que eles podem ser consumidos.

O <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType> retorna um `IEnumerable<T>` , portanto, a ordem não pode ser garantida. No entanto, internamente um <xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType> é usado como o tipo de coleção subjacente, que removerá os objetos após a ordenação PEPS (primeiro a entrar, primeiro a sair). Se forem feitas chamadas simultâneas <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType> , elas serão concorrentes. Um item consumido (removido da fila) em uma enumeração não pode ser observado no outro.

Para enumerar a coleção sem modificá-la, basta usar `foreach` (`For Each`) sem o método <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A>. No entanto, é importante compreender que esse tipo de enumeração representa um instantâneo da coleção em um ponto preciso no tempo. Se outros threads estiverem adicionando ou removendo itens simultaneamente enquanto você estiver executando o loop, o loop poderá não representar o estado real da coleção.

## <a name="see-also"></a>Veja também

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [Programação paralela](../../parallel-programming/index.md)
