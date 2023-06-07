---
title: Quando usar uma coleção thread-safe
description: Saiba quando usar uma coleção thread-safe no .NET. Há cinco tipos de coleção que são especialmente projetados para dar suporte a operações de adição & de multithread.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- thread-safe collections, when to upgrade
ms.assetid: a9babe97-e457-4ff3-b528-a1bc940d5320
ms.openlocfilehash: 499af6d7b8de1decbcffefe0a3b1420cc548488a
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85326044"
---
# <a name="when-to-use-a-thread-safe-collection"></a>Quando usar uma coleção thread-safe

.NET Framework 4 introduziu cinco tipos de coleção que são especialmente projetados para dar suporte a operações de adição e remoção com vários threads. Para obter a segurança de threads, esses tipos usam vários tipos de mecanismos de bloqueio e de sincronização sem bloqueio eficientes. A sincronização adiciona sobrecarga a uma operação. A quantidade de sobrecarga depende do tipo de sincronização usado, os tipos de operações que são executadas e outros fatores, como o número de threads que estão tentando acessar a coleção simultaneamente.  
  
 Em alguns cenários, a sobrecarga de sincronização é insignificante e permite que o tipo com multithread funcione significativamente mais rápido e ajuste a escala muito melhor do que seu equivalente não thread-safe quando protegido por um bloqueio externo. Em outros cenários, a sobrecarga pode fazer com o tipo thread-safe funcione e ajuste a escala na mesma velocidade ou até mais lentamente do que a versão bloqueada externamente não thread-safe do tipo.  
  
 As seções a seguir fornecem diretrizes gerais sobre quando usar uma coleção thread-safe versus sua equivalente não thread-safe que tem um bloqueio fornecido pelo usuário em torno de suas operações de leitura e gravação. Como o desempenho pode variar dependendo de vários fatores, as diretrizes não são específicas e não são necessariamente válidas em todas as circunstâncias. Se o desempenho for muito importante, a melhor maneira de determinar que tipo de coleção usar é medir o desempenho com base em cargas e configurações de computador representativas. Este documento usa os seguintes termos:  
  
 *Produtor puro – cenário de consumidor*\
 Um determinado thread está adicionando ou removendo elementos, mas não executando as duas ações.  
  
 *Produtor misto – cenário de consumidor*\
 Um determinado thread está tanto adicionando quanto removendo elementos.  
  
 *Velocidade*\
 Desempenho algorítmico mais rápido em relação a outro tipo no mesmo cenário.  
  
 *Escalabilidade*\
 O aumento no desempenho que é proporcional ao número de núcleos no computador. Um algoritmo que ajusta a escala tem um desempenho mais rápido em oito núcleos do que em dois núcleos.  
  
## <a name="concurrentqueuet-vs-queuet"></a>ConcurrentQueue (T) vs. fila (T)  
 Em cenários de produtor-consumidor puros, em que o tempo de processamento para cada elemento é muito pequeno (poucas instruções), <xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType> pode oferecer benefícios de desempenho modestos em relação a um <xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType> que tem um bloqueio externo. Nesse cenário, <xref:System.Collections.Concurrent.ConcurrentQueue%601> funciona melhor quando um thread dedicado está enfileirando e um está retirando da fila. Se você não impuser essa regra, <xref:System.Collections.Generic.Queue%601> poderá até mesmo funcionar um pouco mais rápido do que <xref:System.Collections.Concurrent.ConcurrentQueue%601> em computadores com vários núcleos.  
  
 Quando o tempo de processamento é em torno de 500 FLOPS (operações em ponto flutuante) ou mais, a regra de dois threads não se aplica a <xref:System.Collections.Concurrent.ConcurrentQueue%601>, que tem a escalabilidade muito boa. <xref:System.Collections.Generic.Queue%601> não ajusta a escala bem nesse cenário.  
  
 Em cenários de produtor-consumidor mistos, quando o tempo de processamento é muito pequeno, um <xref:System.Collections.Generic.Queue%601> que tem um bloqueio externo ajusta a escala melhor do que <xref:System.Collections.Concurrent.ConcurrentQueue%601>. No entanto, quando o tempo de processamento é em torno de 500 FLOPS ou mais, o <xref:System.Collections.Concurrent.ConcurrentQueue%601> ajusta a escala melhor.  
  
## <a name="concurrentstack-vs-stack"></a>ConcurrentStack vs. pilha  
 Em cenários de produtor-consumidor puros, quando o tempo de processamento é muito pequeno, <xref:System.Collections.Concurrent.ConcurrentStack%601?displayProperty=nameWithType> e <xref:System.Collections.Generic.Stack%601?displayProperty=nameWithType> que tenham um bloqueio externo provavelmente funcionarão praticamente da mesma forma com um thread de colocação dedicado e um thread de retirada dedicado. No entanto, conforme o número de threads aumenta, os dois tipos ficam mais lentos devido ao aumento da contenção e <xref:System.Collections.Generic.Stack%601> pode funcionar melhor do que <xref:System.Collections.Concurrent.ConcurrentStack%601>. Quando o tempo de processamento é em torno de 500 FLOPS ou mais, os dois tipos ajustam a escala aproximadamente na mesma taxa.  
  
 Em cenários de produtor-consumidor mistos, <xref:System.Collections.Concurrent.ConcurrentStack%601> é mais rápido para cargas de trabalho grandes e pequenas.  
  
 O uso de <xref:System.Collections.Concurrent.ConcurrentStack%601.PushRange%2A> e <xref:System.Collections.Concurrent.ConcurrentStack%601.TryPopRange%2A> pode acelerar muito os tempos de acesso.  
  
## <a name="concurrentdictionary-vs-dictionary"></a>ConcurrentDictionary vs. dicionário  
 Em geral, use uma <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType> em qualquer cenário em que estiver adicionando ou atualizando chaves ou valores simultaneamente de vários threads. Em cenários que envolvem atualizações frequentes e relativamente poucas leituras, o <xref:System.Collections.Concurrent.ConcurrentDictionary%602> geralmente oferece benefícios modestos. Em cenários que envolvem muitas leituras e muitas atualizações, o <xref:System.Collections.Concurrent.ConcurrentDictionary%602> geralmente é significativamente mais rápido em computadores que têm qualquer número de núcleos.  
  
 Em cenários que envolvem atualizações frequentes, você pode aumentar o nível de simultaneidade no <xref:System.Collections.Concurrent.ConcurrentDictionary%602> e, depois, medir para ver se o desempenho aumenta em computadores que têm mais núcleos. Se você alterar o nível de simultaneidade, evite operações globais o máximo possível.  
  
 Se você só estiver lendo chave ou valores, o <xref:System.Collections.Generic.Dictionary%602> será mais rápido porque nenhuma sincronização será necessária se o dicionário não estiver sendo modificado por nenhum thread.  
  
## <a name="concurrentbag"></a>ConcurrentBag  
 Em cenários de produtor-consumidor puros, <xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType> provavelmente funcionará mais lentamente do que os outros tipos de coleção simultâneas.  
  
 Em cenários produtor-consumidor mistos, <xref:System.Collections.Concurrent.ConcurrentBag%601> geralmente é muito mais rápido e mais escalonável do que qualquer outro tipo de coleção simultânea para cargas de trabalho grandes e pequenas.  
  
## <a name="blockingcollection"></a>BlockingCollection  
 Quando forem necessárias semânticas de delimitação e bloqueio, <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> provavelmente funcionará mais rápido do que qualquer implementação personalizada. Ele também dá suporte ao cancelamento, enumeração e tratamento de exceções avançados.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [Coleções com segurança de thread](index.md)
- [Programação paralela](../../parallel-programming/index.md)
