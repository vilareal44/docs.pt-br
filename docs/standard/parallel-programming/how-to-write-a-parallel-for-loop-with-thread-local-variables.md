---
title: 'Como: Gravar um loop Parallel.For com variáveis locais de thread'
description: Veja um exemplo de como escrever um loop. for paralelo no .NET que usa variáveis de thread local, que armazenam e recuperam o estado em cada tarefa separada no loop.
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel for loops, how to use local state
ms.assetid: 68384064-7ee7-41e2-90e3-71f00bde01bb
ms.openlocfilehash: 9cff507757aab2e5676df2fabb02a237a2172c17
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599783"
---
# <a name="how-to-write-a-parallelfor-loop-with-thread-local-variables"></a>Como: Gravar um loop Parallel.For com variáveis locais de thread
Este exemplo mostra como usar variáveis de thread local para armazenar e recuperar o estado em cada tarefa separada criada por um loop <xref:System.Threading.Tasks.Parallel.For%2A>. Usando dados de thread local, você pode evitar a sobrecarga de sincronizar um grande número de acessos com estado compartilhado. Em vez de gravar em um recurso compartilhado em cada iteração, você computa e armazena o valor até todas as iterações da tarefas serem concluídas. Em seguida, é possível gravar o resultado final uma vez no recurso compartilhado ou passá-lo para outro método.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir chama o método <xref:System.Threading.Tasks.Parallel.For%60%601%28System.Int32%2CSystem.Int32%2CSystem.Func%7B%60%600%7D%2CSystem.Func%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%600%2C%60%600%7D%2CSystem.Action%7B%60%600%7D%29> para calcular a soma dos valores em uma matriz com um milhão de elementos. O valor de cada elemento é igual a seu índice.  
  
 [!code-csharp[TPL_Parallel#05](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/forandforeach_simple.cs#05)]
 [!code-vb[TPL_Parallel#05](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/forwiththreadlocal.vb#05)]  
  
 Os dois primeiros parâmetros de todo método <xref:System.Threading.Tasks.Parallel.For%2A> especificam os valores de iteração iniciais e finais. Nessa sobrecarga do método, o terceiro parâmetro é onde o estado local é inicializado. Nesse contexto, estado local significa uma variável cujo tempo de vida se prolonga um pouco antes da primeira iteração do loop no thread atual até pouco depois da última iteração.  
  
 O tipo do terceiro parâmetro é um <xref:System.Func%601>, em que `TResult` é o tipo da variável que armazenará o estado de thread local. Seu tipo é definido pelo argumento de tipo genérico fornecido durante a chamada do método <xref:System.Threading.Tasks.Parallel.For%60%601%28System.Int32%2CSystem.Int32%2CSystem.Func%7B%60%600%7D%2CSystem.Func%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%600%2C%60%600%7D%2CSystem.Action%7B%60%600%7D%29> genérico, que, neste caso é <xref:System.Int64>. O argumento de tipo informa ao compilador o tipo da variável temporária que será usada para armazenar o estado de thread local. Neste exemplo, a expressão `() => 0` (ou `Function() 0` no Visual Basic) inicializa a variável de thread local em zero. Se o argumento de tipo genérico fosse um tipo de referência ou um tipo de valor definido pelo usuário, a expressão seria assim:  
  
```csharp  
() => new MyClass()  
```  
  
```vb  
Function() new MyClass()  
```  
  
 O quarto parâmetro define a lógica do loop. Ele deve ser uma expressão representante ou lambda, cuja assinatura é `Func<int, ParallelLoopState, long, long>` no C# ou `Func(Of Integer, ParallelLoopState, Long, Long)` no Visual Basic. O primeiro parâmetro é o valor do contador de loops para essa iteração do loop. O segundo é um objeto <xref:System.Threading.Tasks.ParallelLoopState> que pode ser usado para interromper o loop. Esse objeto é fornecido pela classe <xref:System.Threading.Tasks.Parallel> para cada ocorrência do loop. O terceiro parâmetro é a variável de thread local. O último parâmetro é o tipo de retorno. Neste caso, o tipo é <xref:System.Int64> porque esse é o tipo que especificamos no argumento do tipo <xref:System.Threading.Tasks.Parallel.For%2A>. Essa variável se chama `subtotal` e é retornada pela expressão lambda. O valor retornado é usado para inicializar `subtotal` em toda iteração subsequente do loop. Também é possível considerar esse último parâmetro como um valor passado para cada iteração e, em seguida, passado para o representante `localFinally` quando a última iteração está concluída.  
  
 O quinto parâmetro define o método chamado uma vez, depois que todas as iterações em um determinado thread foram concluídas. O tipo de argumento de entrada mais uma vez corresponde ao argumento de tipo do método <xref:System.Threading.Tasks.Parallel.For%60%601%28System.Int32%2CSystem.Int32%2CSystem.Func%7B%60%600%7D%2CSystem.Func%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%600%2C%60%600%7D%2CSystem.Action%7B%60%600%7D%29> e ao tipo retornado pela expressão lambda do corpo. Neste exemplo, o valor é adicionado a uma variável no escopo da classe em thread-safe chamando o método <xref:System.Threading.Interlocked.Add%2A?displayProperty=nameWithType>. Usando uma variável de thread local, evitamos a gravação nessa variável de classe em toda iteração do loop.  
  
 Para obter mais informações sobre como usar expressões lambda, confira [Expressões lambda no PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md).  
  
## <a name="see-also"></a>Consulte também

- [Paralelismo de dados](data-parallelism-task-parallel-library.md)
- [Programação paralela](index.md)
- [Biblioteca de tarefas paralelas (TPL)](task-parallel-library-tpl.md)
- [Expressões lambda em PLINQ e TPL](lambda-expressions-in-plinq-and-tpl.md)
