---
title: Como gravar um loop Parallel.ForEach com variáveis locais de partição
description: Veja um exemplo de como escrever um loop. ForEach paralelo que usa variáveis de partição local no .NET.
ms.date: 06/26/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel foreach loop, how to use local state
ms.assetid: 24b10041-b30b-45cb-aa65-66cf568ca76d
ms.openlocfilehash: f598955fb2d6800f81bce050bdf474fc63bfb554
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599770"
---
# <a name="how-to-write-a-parallelforeach-loop-with-partition-local-variables"></a>Como gravar um loop Parallel.ForEach com variáveis locais de partição

O exemplo a seguir mostra como gravar um método <xref:System.Threading.Tasks.Parallel.ForEach%2A> que usa variáveis locais de partição. Quando um loop <xref:System.Threading.Tasks.Parallel.ForEach%2A> é executado, ele divide a coleção de origem em diversas partições. Cada partição tem sua própria cópia da variável local de partição. Uma variável local de partição é semelhante a uma [variável local de thread](xref:System.Threading.ThreadLocal%601), com a diferença de que várias partições podem ser executadas em um único thread.

O código e os parâmetros desse exemplo parecem com o método <xref:System.Threading.Tasks.Parallel.For%2A> correspondente. Para saber mais, confira [Como escrever um loop Parallel.For com variáveis locais de thread](how-to-write-a-parallel-for-loop-with-thread-local-variables.md).

Para usar uma variável local de partição em um loop <xref:System.Threading.Tasks.Parallel.ForEach%2A>, você deve chamar uma das sobrecargas de método que usa dois parâmetros de tipo. O primeiro parâmetro de tipo, `TSource`, especifica o tipo do elemento de origem. Já o segundo, `TLocal`, especifica o tipo da variável local de partição.

## <a name="example"></a>Exemplo

O exemplo a seguir chama a sobrecarga <xref:System.Threading.Tasks.Parallel.ForEach%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%601%7D%2CSystem.Func%7B%60%600%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%601%2C%60%601%7D%2CSystem.Action%7B%60%601%7D%29?displayProperty=nameWithType> para calcular a soma de uma matriz com um milhão de elementos. Essa sobrecarga tem quatro parâmetros:

- `source`, que é a fonte de dados. Ela deve implementar <xref:System.Collections.Generic.IEnumerable%601>. A fonte de dados do nosso exemplo é um objeto `IEnumerable<Int32>` membro de um milhão retornado pelo método <xref:System.Linq.Enumerable.Range%2A?displayProperty=nameWithType>.

- `localInit`, ou a função que inicia a variável local de partição. Essa função é chamada uma vez para cada partição onde a operação <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> é executada. Nosso exemplo inicia a variável local de partição como zero.

- `body`, um <xref:System.Func%604> que é invocado pelo loop paralelo em cada iteração do loop. Sua assinatura é `Func\<TSource, ParallelLoopState, TLocal, TLocal>`. Você fornece o código para o representante e o loop passa pelos parâmetros de entrada, que são:

  - O elemento atual de <xref:System.Collections.Generic.IEnumerable%601>.

  - Uma variável <xref:System.Threading.Tasks.ParallelLoopState> que pode ser usada no código do representante para avaliar o estado do loop.

  - A variável local de partição.

  Seu delegado retorna a variável local de partição, que é passada para a próxima iteração do loop que executa nessa partição específica. Cada partição do loop mantém uma instância separada dessa variável.

  Nesse exemplo, o delegado adiciona o valor de cada inteiro à variável local de partição, que mantém um total de valores dos elementos inteiros dessa partição em execução.

- `localFinally`, um representante `Action<TLocal>` que o <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> invoca quando são concluídas as operações de loop de cada partição. O método <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> passa o valor final da variável local de partição dessa partição de loop para seu delegado `Action<TLocal>`, e você fornece o código que executa a ação necessária para combinar o resultado dessa partição aos resultados de outras partições. Esse representante pode ser invocado ao mesmo tempo por diversas tarefas. Por isso, o exemplo usa o método <xref:System.Threading.Interlocked.Add%28System.Int32%40%2CSystem.Int32%29?displayProperty=nameWithType> para sincronizar o acesso à variável `total`. Como o tipo de representante é um <xref:System.Action%601>, não há valor retornado.

[!code-csharp[TPL_Parallel#04](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/foreachthreadlocal.cs#04)]
[!code-vb[TPL_Parallel#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/foreachthreadlocal.vb#04)]

## <a name="see-also"></a>Consulte também

- [Paralelismo de dados](data-parallelism-task-parallel-library.md)
- [Como: Gravar um loop Parallel.For com variáveis locais de thread](how-to-write-a-parallel-for-loop-with-thread-local-variables.md)
- [Expressões lambda em PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md)
