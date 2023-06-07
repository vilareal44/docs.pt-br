---
title: Estruturas de dados para programação paralela
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- data structures, multi-threading
ms.assetid: bdc82f2f-4754-45a1-a81e-fe2e9c30cef9
ms.openlocfilehash: f9c130b73044440f24b7b8bbebe9527490a165c1
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288518"
---
# <a name="data-structures-for-parallel-programming"></a>Estruturas de dados para programação paralela
O .NET Framework versão 4 apresenta vários tipos novos que são úteis em programação paralela, incluindo um conjunto de classes de coleção simultâneas, primitivos de sincronização leve e tipos para inicialização lenta. Você pode usar esses tipos com qualquer código de aplicativo multithread, incluindo PLINQ e biblioteca de paralelismo de tarefas.  
  
## <a name="concurrent-collection-classes"></a>Classes de coleção simultâneas  
 As classes de coleção no namespace <xref:System.Collections.Concurrent?displayProperty=nameWithType> fornecem operações de adição e remoção thread-safe que, sempre que possível, evitam bloqueios e usam o bloqueio refinado quando os bloqueios forem necessários. Ao contrário de coleções que foram introduzidas no .NET Framework versões 1.0 e 2.0, uma classe de coleção simultânea não exige que o código de usuário use qualquer bloqueio ao acessar itens. As classes de coleção simultânea podem melhorar consideravelmente o desempenho em tipos como <xref:System.Collections.ArrayList?displayProperty=nameWithType> e <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> (com bloqueio implementado pelo usuário) em cenários nos quais vários threads adicionam e removem itens de uma coleção.  
  
 A tabela a seguir lista as novas classes de coleção simultâneas:  
  
|Tipo|Description|  
|----------|-----------------|  
|<xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType>|Fornece funcionalidades de bloqueio e delimitação para coleções thread-safe que implementam <xref:System.Collections.Concurrent.IProducerConsumerCollection%601?displayProperty=nameWithType>. Os threads de produtor são bloqueados se nenhum slot estiver disponível, ou se a coleção estiver cheia. Threads de consumidor são bloqueados se a coleção estiver vazia. Esse tipo também oferece suporte ao acesso sem bloqueio de produtores e consumidores. <xref:System.Collections.Concurrent.BlockingCollection%601> pode ser usado como uma classe base ou repositório de backup para fornecer bloqueio e limitação a qualquer classe de coleção que ofereça suporte a <xref:System.Collections.Generic.IEnumerable%601>.|  
|<xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType>|Uma implementação de recipiente thread-safe que fornece operações de adição e get escalonáveis.|  
|<xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType>|Um tipo de dicionário simultâneo e escalonável.|  
|<xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType>|Uma fila FIFO simultânea e escalonável.|  
|<xref:System.Collections.Concurrent.ConcurrentStack%601?displayProperty=nameWithType>|Uma pilha LIFO simultânea e escalonável.|  
  
 Para obter mais informações, veja [Coleções thread-safe](../collections/thread-safe/index.md).  
  
## <a name="synchronization-primitives"></a>Primitivos de sincronização  
 Os novos primitivos de sincronização no namespace <xref:System.Threading?displayProperty=nameWithType> habilitam simultaneidade refinada e desempenho mais rápido, evitando mecanismos de bloqueio caros encontrados no código de multithreading herdado. Alguns dos novos tipos, como <xref:System.Threading.Barrier?displayProperty=nameWithType> e <xref:System.Threading.CountdownEvent?displayProperty=nameWithType> não têm correspondentes em versões anteriores do .NET Framework.  
  
 A tabela a seguir lista os novos tipos de sincronização:  
  
|Tipo|Description|  
|----------|-----------------|  
|<xref:System.Threading.Barrier?displayProperty=nameWithType>|Permite que vários threads funcionem em um algoritmo em paralelo fornecendo um ponto em que cada tarefa pode sinalizar sua chegada e, depois, gerar um bloqueio até que algumas ou todas as tarefas tenham chegado. Para saber mais, consulte [Barreira](../threading/barrier.md).|  
|<xref:System.Threading.CountdownEvent?displayProperty=nameWithType>|Simplifica cenários de bifurcação e junção fornecendo um mecanismo fácil de encontro. Para saber mais, confira [CountdownEvent](../threading/countdownevent.md).|  
|<xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType>|Um primitivo de sincronização semelhante a <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType>. <xref:System.Threading.ManualResetEventSlim> é leve, mas só pode ser usado para comunicação entre processos.|  
|<xref:System.Threading.SemaphoreSlim?displayProperty=nameWithType>|Um primitivo de sincronização que limita o número de threads que podem acessar simultaneamente um recurso ou um pool de recursos. Para saber mais, confira [Semaphore e SemaphoreSlim](../threading/semaphore-and-semaphoreslim.md).|  
|<xref:System.Threading.SpinLock?displayProperty=nameWithType>|Um primitivo de bloqueio de exclusão mútua que faz com que o thread que está tentando adquirir o bloqueio aguarde em um loop, ou *rotação*, durante um período antes de gerar seu quantum. Em cenários nos quais a espera pelo bloqueio deve ser curta, <xref:System.Threading.SpinLock> oferece um desempenho melhor do que outras formas de bloqueio. Para saber mais, veja [SpinLock](../threading/spinlock.md).|  
|<xref:System.Threading.SpinWait?displayProperty=nameWithType>|Um tipo de pequeno e leve que girará por um tempo especificado e, no final, colocará o thread em um estado de espera se a contagem de rotações for ultrapassada.  Para saber mais, veja [SpinWait](../threading/spinwait.md).|  
  
 Para obter mais informações, consulte:  
  
- [Como: usar o SpinLock para sincronização de nível baixo](../threading/how-to-use-spinlock-for-low-level-synchronization.md)  
  
- [Como sincronizar operações simultâneas com uma barreira](../threading/how-to-synchronize-concurrent-operations-with-a-barrier.md).  
  
## <a name="lazy-initialization-classes"></a>Classes de inicialização lenta  
 Com a inicialização lenta, a memória de um objeto não é alocada até que seja necessário. A inicialização lenta pode melhorar o desempenho distribuindo as alocações de objeto uniformemente entre o tempo de vida de um programa. Você pode habilitar a inicialização lenta para qualquer tipo personalizado encapsulando o tipo <xref:System.Lazy%601>.  
  
 A tabela a seguir lista os tipos de inicialização lenta:  
  
|Tipo|Description|  
|----------|-----------------|  
|<xref:System.Lazy%601?displayProperty=nameWithType>|Fornece inicialização lenta, leve e thread-safe.|  
|<xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType>|Fornece um valor com inicialização lenta em uma base por thread, com cada thread invocando lentamente a função de inicialização.|  
|<xref:System.Threading.LazyInitializer?displayProperty=nameWithType>|Fornece métodos estáticos que evitam a necessidade de alocar uma instância dedicada de inicialização lenta. Em vez disso, usam referências para garantir que os destinos sejam inicializados conforme são acessados.|  
  
 Para obter mais informações, veja [Inicialização lenta](../../framework/performance/lazy-initialization.md).  
  
## <a name="aggregate-exceptions"></a>Agregar exceções  
 O tipo <xref:System.AggregateException?displayProperty=nameWithType> pode ser usado para capturar várias exceções lançadas simultaneamente em threads separados e retorná-las para o thread associado como uma única exceção. Os tipos <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> e <xref:System.Threading.Tasks.Parallel?displayProperty=nameWithType> e o PLINQ usam <xref:System.AggregateException> extensivamente para essa finalidade. Para saber mais, veja [Tratamento de exceções](exception-handling-task-parallel-library.md) e [Como tratar exceções em uma consulta PLINQ](how-to-handle-exceptions-in-a-plinq-query.md).  
  
## <a name="see-also"></a>Veja também

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- <xref:System.Threading?displayProperty=nameWithType>
- [Programação paralela](index.md)
