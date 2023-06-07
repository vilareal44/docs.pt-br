---
title: Threads e threading
description: Saiba mais sobre Threading, como processos & threads, quando usar vários threads, & como usar multithread para aumentar a capacidade de resposta ou a taxa de transferência no .NET.
ms.date: 11/08/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- multiple threads
- threading [.NET]
- threading [.NET], multiple threads
ms.assetid: 5baac3aa-e603-4fa6-9f89-0f2c1084e6b1
ms.openlocfilehash: b332db80069e18d3b52cd03eef4995eaad3fda7b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84583395"
---
# <a name="threads-and-threading"></a>Threads e threading

O multithreading permite aumentar a capacidade de resposta do seu aplicativo e, se esse aplicativo é executado em um sistema com vários processadores ou vários núcleos, permite também aumentar sua taxa de transferência.

## <a name="processes-and-threads"></a>Processos e threads

Um *processo* é um programa em execução. Um sistema operacional usa processos para separar os aplicativos que estão sendo executados. Um *thread* é a unidade básica para a qual um sistema operacional aloca tempo do processador. Cada thread tem uma [prioridade de agendamento](scheduling-threads.md) e mantém um conjunto de estruturas que o sistema usa para salvar o contexto do thread quando a execução do thread é colocada em pausa. O contexto de thread inclui todas as informações que o thread precisa para continuar a execução sem interrupções, incluindo o conjunto de registros de CPU e pilha do thread. Vários threads podem ser executados no contexto de um processo. Todos os threads de um processo compartilham seu espaço de endereço virtual. Um thread pode executar qualquer parte do código do programa, incluindo partes que estão sendo executadas no momento por outro thread.

> [!NOTE]
> O .NET Framework fornece uma maneira de isolar aplicativos em um processo com o uso de *domínios de aplicativo*. (Os domínios de aplicativo não estão disponíveis no .NET Core.) Para obter mais informações, consulte a seção [domínios e threads do aplicativo](../../framework/app-domains/application-domains.md#application-domains-and-threads) do artigo [domínios de aplicativo](../../framework/app-domains/application-domains.md) .

Por padrão, um programa .NET é iniciado com um único thread, geralmente chamado de thread *primário*. No entanto, ele pode criar threads adicionais para executar código em paralelo ou simultaneamente com o thread primário. Esses threads costumam ser chamados de threads de *trabalho* .

## <a name="when-to-use-multiple-threads"></a>Quando usar vários threads

Você usa vários threads para aumentar a capacidade de resposta do seu aplicativo e para aproveitar um sistema com vários processadores ou vários núcleos para aumentar a taxa de transferência do aplicativo.

Considere um aplicativo da área de trabalho em que o thread primário é responsável por elementos da interface do usuário e responde às ações do usuário. Use threads de trabalho para executar operações demoradas que, caso contrário, ocupariam o thread primário e fariam com que a interface do usuário parasse de responder. Você também pode usar um thread dedicado para comunicação de rede ou dispositivo para ser mais responsivo a mensagens ou eventos de entrada.

Se o programa realiza operações que podem ser executadas em paralelo, o tempo de execução total pode ser diminuído executando essas operações em threads separados e executando o programa em um sistema com vários processadores ou vários núcleos. Em um sistema desse tipo, o uso de multithreading pode resultar aumentar a taxa de transferência, além de uma maior capacidade de resposta.

## <a name="how-to-use-multithreading-in-net"></a>Como usar multithreading em .NET

Começando com o .NET Framework 4, a maneira recomendada para utilizar o multithreading é usar [TPL (biblioteca de paralelismo de tarefas)](../parallel-programming/task-parallel-library-tpl.md) e [PLINQ (Parallel LINQ)](../parallel-programming/introduction-to-plinq.md). Para obter mais informações, veja [Programação paralela](../parallel-programming/index.md).

Tanto TPL quanto PLINQ contam com os threads <xref:System.Threading.ThreadPool>. A classe <xref:System.Threading.ThreadPool?displayProperty=nameWithType> fornece um pool de threads de trabalho a um aplicativo .NET. Você também pode usar threads de pool de threads. Para obter mais informações, veja [O pool de threads gerenciados](the-managed-thread-pool.md).

Por fim, você pode usar a classe <xref:System.Threading.Thread?displayProperty=nameWithType>, que representa um thread gerenciado. Para obter mais informações, veja [Usando threads e threading](using-threads-and-threading.md).

É possível que vários threads precisem acessar um recurso compartilhado. Para manter o recurso em um estado não corrompido e evitar condições de corrida, você precisa sincronizar o acesso dos threads a ele. Também pode ser conveniente coordenar a interação de vários threads. O .NET fornece uma variedade de tipos que você pode usar para sincronizar o acesso a um recurso compartilhado ou coordenar a interação de thread. Para obter mais informações, veja [Visão geral dos primitivos de sincronização](overview-of-synchronization-primitives.md).

Não se esqueça de tratar as exceções nos threads. Exceções sem tratamento nos threads geralmente encerram o processo. Para obter mais informações, veja [Exceções em threads gerenciados](exceptions-in-managed-threads.md).

## <a name="see-also"></a>Consulte também

- [Objetos e recursos de threading](threading-objects-and-features.md)
- [Práticas recomendadas de threading gerenciado](managed-threading-best-practices.md)
- [Processamento paralelo, simultaneidade e programação assíncrona no .NET](../parallel-processing-and-concurrency.md)
- [Sobre Processos e Threads](/windows/desktop/procthread/about-processes-and-threads)
