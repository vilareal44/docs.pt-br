---
title: Coleções thread-safe
description: Introdução às coleções com segurança de thread usando o namespace System. Collections. decurrent no .NET, que inclui classes de coleção com segurança de threads e escalonáveis.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- thread-safe collections, overview
ms.assetid: 2e7ca21f-786c-4367-96be-0cf3f3dcc6bd
ms.openlocfilehash: 3a252124ade4c43961c06697367bbc4ca5d0c9cb
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768580"
---
# <a name="thread-safe-collections"></a>Coleções thread-safe
O .NET Framework 4 introduz o namespace <xref:System.Collections.Concurrent?displayProperty=nameWithType>, que inclui várias classes de coleção que são tanto thread-safe quanto escalonáveis. Vários threads podem adicionar ou remover itens dessas coleções de modo seguro e eficiente sem a necessidade de sincronização adicional no código do usuário. Ao escrever um novo código, use as classes de coleção simultâneas sempre que vários threads forem ser gravados simultaneamente nas coleções. Se estiver lendo apenas de uma coleção compartilhada, você poderá usar as classes no namespace <xref:System.Collections.Generic?displayProperty=nameWithType>. É recomendável não usar as classes da coleção 1.0, a menos que você precise ter como destino o runtime do .NET Framework 1.1 ou anterior.  
  
## <a name="thread-synchronization-in-the-net-framework-10-and-20-collections"></a>Sincronização de thread nas coleções do .NET Framework 1.0 e 2.0  
 As coleções introduzidas no .NET Framework 1.0 são encontradas no namespace <xref:System.Collections?displayProperty=nameWithType>. Essas coleções, que incluem os comumente usados <xref:System.Collections.ArrayList> e <xref:System.Collections.Hashtable>, fornecem algum acesso thread-safe por meio da propriedade `Synchronized`, que retorna um wrapper thread-safe em torno da coleção. O wrapper funciona bloqueando toda a coleção a cada operação de adição ou remoção. Portanto, cada thread que está tentando acessar a coleção deve aguardar por sua vez para receber esse bloqueio. Isso não é escalonável e pode causar degradação significativa no desempenho para coleções grandes. Além disso, o design não é totalmente protegido contra condições de corrida. Para saber mais, confira [Synchronization in Generic Collections](https://docs.microsoft.com/archive/blogs/bclteam/synchronization-in-generic-collections-brian-grunkemeyer) (Sincronização em coleções genéricas).  
  
 As classes de coleções introduzidas no .NET Framework 2.0 são encontradas no namespace <xref:System.Collections.Generic?displayProperty=nameWithType>. Entre elas <xref:System.Collections.Generic.List%601>, <xref:System.Collections.Generic.Dictionary%602> e assim por diante. Essas classes fornecem desempenho e segurança de tipos aprimorados em comparação com as classes do .NET Framework 1.0. No entanto, as classes de coleção do .NET Framework 2.0 não fornecem nenhuma sincronização de thread; o código de usuário deve fornecer sincronização quando itens são adicionados ou removidos simultaneamente em vários threads.  
  
 O recomendável são as classes de coleções simultâneas no .NET Framework 4, pois elas fornecem não apenas a segurança de tipos das classes de coleção do .NET Framework 2.0, mas também acesso thread-safe mais eficiente e completo do que o fornecido pelas coleções do .NET Framework 1.0.  
  
## <a name="fine-grained-locking-and-lock-free-mechanisms"></a>Bloqueio refinado e mecanismos sem bloqueio  
 Alguns dos tipos de coleção simultâneos usam mecanismos de sincronização leves como <xref:System.Threading.SpinLock>, <xref:System.Threading.SpinWait>, <xref:System.Threading.SemaphoreSlim> e <xref:System.Threading.CountdownEvent>, que são novos no .NET Framework 4. Esses tipos de sincronização geralmente usam *giro ocupado* por breves períodos antes de colocarem o thread em um verdadeiro estado de espera. Quando espera-se que os tempos de espera sejam muito curtos, girar é muito menos dispendioso em termos de recursos computacionais do que esperar, o que envolve uma transição de kernel que utiliza muitos recursos. Para classes de coleção que usam o giro, essa eficiência significa que vários threads podem adicionar e remover itens em uma taxa muito alta. Para saber mais sobre rotação versus bloqueio, confira [SpinLock](../../threading/spinlock.md) e [SpinWait](../../threading/spinwait.md).  
  
 As classes <xref:System.Collections.Concurrent.ConcurrentQueue%601> e <xref:System.Collections.Concurrent.ConcurrentStack%601> não usam bloqueios. Em vez disso, elas usam operações do <xref:System.Threading.Interlocked> para obter acesso thread-safe.  
  
> [!NOTE]
> Considerando que as classes de coleções simultâneas dão suporte a <xref:System.Collections.ICollection>, elas fornecem implementações para as propriedades <xref:System.Collections.ICollection.IsSynchronized%2A> e <xref:System.Collections.ICollection.SyncRoot%2A>, embora essas propriedades sejam irrelevantes. `IsSynchronized` sempre retorna `false` e `SyncRoot` é sempre `null` (`Nothing` no Visual Basic).  
  
 A tabela a seguir lista os tipos de coleção no namespace <xref:System.Collections.Concurrent?displayProperty=nameWithType>.  
  
|Tipo|Description|  
|----------|-----------------|  
|<xref:System.Collections.Concurrent.BlockingCollection%601>|Fornece funcionalidade de delimitação e bloqueio de qualquer tipo que implemente <xref:System.Collections.Concurrent.IProducerConsumerCollection%601>. Para obter mais informações, veja [Visão geral de BlockingCollection](blockingcollection-overview.md).|  
|<xref:System.Collections.Concurrent.ConcurrentDictionary%602>|Implementação thread-safe de um dicionário de pares chave-valor.|  
|<xref:System.Collections.Concurrent.ConcurrentQueue%601>|Implementação thread-safe de uma fila PEPS (primeiro a entrar, primeiro a sair).|  
|<xref:System.Collections.Concurrent.ConcurrentStack%601>|Implementação thread-safe de uma fila LIFO (último a entrar, primeiro a sair).|  
|<xref:System.Collections.Concurrent.ConcurrentBag%601>|Implementação thread-safe de uma coleção não ordenada de elementos.|  
|<xref:System.Collections.Concurrent.IProducerConsumerCollection%601>|A interface que um tipo deve implementar para uso em um `BlockingCollection`.|  
  
## <a name="related-topics"></a>Tópicos relacionados  
  
|Title|Descrição|  
|-----------|-----------------|  
|[Visão geral de BlockingCollection](blockingcollection-overview.md)|Este tópico descreve a funcionalidade fornecida pelo tipo <xref:System.Collections.Concurrent.BlockingCollection%601>.|  
|[Como Adicionar e Remover Itens de um ConcurrentDictionary](how-to-add-and-remove-items.md)|Descreve como adicionar e remover elementos de um <xref:System.Collections.Concurrent.ConcurrentDictionary%602>|  
|[Como Adicionar e Retirar Itens Individualmente de uma BlockingCollection](how-to-add-and-take-items.md)|Descreve como adicionar e recuperar itens de uma coleta de bloqueio sem usar o enumerador de somente leitura.|  
|[Como Adicionar a Funcionalidade de Delimitação e Bloqueio a uma Coleção](how-to-add-bounding-and-blocking.md)|Descreve como usar qualquer classe de coleção como o mecanismo de armazenamento subjacente para uma coleção <xref:System.Collections.Concurrent.IProducerConsumerCollection%601>.|  
|[Como Usar ForEach para Remover Itens de uma BlockingCollection](how-to-use-foreach-to-remove.md)|Descreve como usar `foreach` (`For Each` no Visual Basic) para remover todos os itens em uma coleção de bloqueios.|  
|[Como usar matrizes de coleções Blocking em um pipeline](how-to-use-arrays-of-blockingcollections.md)|Descreve como usar várias coleções de bloqueio ao mesmo tempo para implementar um pipeline.|  
|[Como criar um pool de objetos usando um ConcurrentBag](how-to-create-an-object-pool.md)|Mostra como usar um recipiente simultâneo para melhorar o desempenho em cenários nos quais, em vez de criar novos objetos continuamente, você pode reutilizá-los.|  
  
## <a name="reference"></a>Referência  
 <xref:System.Collections.Concurrent?displayProperty=nameWithType>
