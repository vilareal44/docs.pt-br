---
title: Estação de trabalho vs. coleta de lixo do servidor (GC)
description: Saiba mais sobre a coleta de lixo de servidor e estação de trabalho no .NET.
ms.date: 04/21/2020
helpviewer_keywords:
- garbage collection, workstation
- garbage collection, server
- workstation garbage collection
- server garbage collection
ms.openlocfilehash: 640b5f42c1f841c2537284e4721e827248e3d300
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87917012"
---
# <a name="workstation-and-server-garbage-collection"></a>Coleta de lixo de estação de trabalho ou de servidor

O coletor de lixo tem autoajuste e pode trabalhar em uma ampla variedade de cenários. No entanto, você pode [definir o tipo de coleta de lixo](../../core/run-time-config/garbage-collector.md#flavors-of-garbage-collection) com base nas características da carga de trabalho. O CLR fornece os seguintes tipos de coleta de lixo:

- Coleta de lixo de estação de trabalho (GC), que é projetada para aplicativos cliente. É o tipo de GC padrão para aplicativos autônomos. Para aplicativos hospedados, por exemplo, aqueles hospedados por ASP.NET, o host determina o tipo GC padrão.

  A coleta de lixo da estação de trabalho pode ser simultânea ou não simultânea. A coleta de lixo simultânea (ou *em segundo plano*) permite que os threads gerenciados continuem as operações durante uma coleta de lixo. A [coleta de lixo em segundo plano](background-gc.md) substitui a [coleta de lixo simultânea](background-gc.md#concurrent-garbage-collection) no .NET Framework 4 e versões posteriores.

- A coleta de lixo de servidor, que serve para aplicativos de servidor que precisam de escalabilidade e taxa de transferência altas.

  - No .NET Core, a coleta de lixo do servidor pode ser não simultânea ou em segundo plano.

  - No .NET Framework 4,5 e versões posteriores, a coleta de lixo do servidor pode ser não simultânea ou em segundo plano. No .NET Framework 4 e versões anteriores, a coleta de lixo do servidor é não simultânea.

A ilustração a seguir mostra os threads dedicados que executam a coleta de lixo em um servidor:

![Threads de coleta de lixo do servidor](media/gc-server.png)

## <a name="performance-considerations"></a>Considerações sobre o desempenho

### <a name="workstation-gc"></a>GC da estação de trabalho

Veja a seguir considerações de desempenho e de threading para a coleta de lixo da estação de trabalho:

- A coleção ocorre no thread do usuário que disparou a coleta de lixo e permanece com a mesma prioridade. Como os threads de usuário normalmente são executados com prioridade normal, o coletor de lixo (que é executado em um thread de prioridade normal) deve disputam competir outros threads por tempo da CPU. (Os threads que executam código nativo não são suspensos na coleta de lixo do servidor ou da estação de trabalho.)

- A coleta de lixo de estação de trabalho é sempre usada em um computador que tem apenas um processador, independentemente da [definição de configuração](../../core/run-time-config/garbage-collector.md#workstation-vs-server).

### <a name="server-gc"></a>GC do servidor

Veja a seguir considerações de desempenho e de threading para a coleta de lixo de servidor:

- A coleta ocorre em vários threads dedicados que estão em execução no nível de prioridade `THREAD_PRIORITY_HIGHEST`.

- Fornece-se um heap e um thread dedicado para executar a coleta de lixo para cada CPU, e os heaps são coletados ao mesmo tempo. Cada heap contém um heap de objeto pequeno e um heap de objeto grande, e todos os heaps podem ser acessados pelo código do usuário. Objetos em heaps diferentes podem fazer referência entre si.

- Como vários threads de coleta de lixo funcionam em conjunto, a coleta de lixo de servidor é mais rápida do que a coleta de lixo de estação de trabalho no heap de mesmo tamanho.

- Geralmente, a coleta de lixo de servidor tem segmentos maiores. No entanto, essa é apenas uma generalização: o tamanho do segmento é específico da implementação e está sujeito a alterações. Não faça suposições sobre o tamanho dos segmentos alocados pelo coletor de lixo ao ajustar seu aplicativo.

- A coleta de lixo de servidor pode usar bastante recursos. Por exemplo, imagine que haja 12 processos que usam GC de servidor em execução em um computador com quatro processadores. Se todos os processos ocorrerem para coletar lixo ao mesmo tempo, eles interferirão uns com os outros, pois haveria 12 threads agendados no mesmo processador. Se os processos estiverem ativos, não é uma boa ideia fazer com que todos usem o GC do servidor.

Se você estiver executando centenas de instâncias de um aplicativo, considere usar a coleta de lixo da estação de trabalho com a coleta de lixo simultânea desabilitada. Isso resultará na redução da comutação de contexto, o que pode melhorar o desempenho.

## <a name="see-also"></a>Consulte também

- [Coleta de lixo em segundo plano](background-gc.md)
- [Opções de configuração de tempo de execução para coleta de lixo](../../core/run-time-config/garbage-collector.md)
