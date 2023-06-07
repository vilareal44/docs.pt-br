---
title: dotnet-contadores-.NET Core
description: Saiba como instalar e usar a ferramenta de linha de comando dotnet-Counter.
ms.date: 02/26/2020
ms.openlocfilehash: 6a4fd92540dbc16173dfa3a10ff9dfaa1f31f7d0
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062893"
---
# <a name="dotnet-counters"></a>dotnet-counters

**Este artigo aplica-se a:** ✔️ SDK do .net Core 3,0 e versões posteriores

## <a name="install-dotnet-counters"></a>Instalar dotnet-contadores

Para instalar a versão de lançamento mais recente do `dotnet-counters` [pacote NuGet](https://www.nuget.org/packages/dotnet-counters), use o comando de [instalação da ferramenta dotnet](../tools/dotnet-tool-install.md) :

```dotnetcli
dotnet tool install --global dotnet-counters
```

## <a name="synopsis"></a>Sinopse

```console
dotnet-counters [-h|--help] [--version] <command>
```

## <a name="description"></a>Descrição

`dotnet-counters`é uma ferramenta de monitoramento de desempenho para monitoramento de integridade ad hoc e investigação de desempenho de primeiro nível. Ele pode observar os valores do contador de desempenho que são publicados por meio da <xref:System.Diagnostics.Tracing.EventCounter> API. Por exemplo, você pode monitorar rapidamente coisas como o uso da CPU ou a taxa de exceções que estão sendo geradas em seu aplicativo .NET Core para ver se há algo suspeito antes de mergulhar em uma investigação de desempenho mais séria usando o `PerfView` ou o `dotnet-trace` .

## <a name="options"></a>Opções

- **`--version`**

  Exibe a versão do utilitário dotnet-Counters.

- **`-h|--help`**

  Mostra a ajuda da linha de comando.

## <a name="commands"></a>Comandos

| Comando                                             |
|-----------------------------------------------------|
| [dotnet – coleta de contadores](#dotnet-counters-collect) |
| [dotnet – lista de contadores](#dotnet-counters-list)       |
| [dotnet – monitor de contadores](#dotnet-counters-monitor) |
| [dotnet-contadores PS](#dotnet-counters-ps)           |

## <a name="dotnet-counters-collect"></a>dotnet – coleta de contadores

Coletar periodicamente os valores de contador selecionados e exportá-los para um formato de arquivo especificado para o pós-processamento.

### <a name="synopsis"></a>Sinopse

```console
dotnet-counters collect [-h|--help] [-p|--process-id] [--refreshInterval] [counter_list] [--format] [-o|--output]
```

### <a name="options"></a>Opções

- **`-p|--process-id <PID>`**

  A ID do processo a ser monitorado.

- **`--refresh-interval <SECONDS>`**

  O número de segundos de atraso entre a atualização dos contadores exibidos

- **`counter_list <COUNTERS>`**

  Uma lista de contadores separados por espaços. Os contadores podem ser especificados `provider_name[:counter_name]` . Se o `provider_name` for usado sem uma qualificação `counter_name` , todos os contadores serão mostrados. Para descobrir os nomes de provedor e contador, use o comando [dotnet-Counters List](#dotnet-counters-list) .

- **`--format <csv|json>`**

  O formato a ser exportado. Atualmente disponível: CSV, JSON.

- **`-o|--output <output>`**

  O nome do arquivo de saída.

### <a name="examples"></a>Exemplos

- Colete todos os contadores em um intervalo de atualização de 3 segundos e gere um CSV como saída:

  ```console
  > dotnet-counters collect --process-id 1902 --refresh-interval 3 --format csv

  counter_list is unspecified. Monitoring all counters by default.
  Starting a counter session. Press Q to quit.
  ```

## <a name="dotnet-counters-list"></a>dotnet – lista de contadores

Exibe uma lista de nomes e descrições de contadores, agrupados por provedor.

### <a name="synopsis"></a>Sinopse

```console
dotnet-counters list [-h|--help]
```

### <a name="example"></a>Exemplo

```console
> dotnet-counters list
Showing well-known counters only. Specific processes may support additional counters.

System.Runtime
    cpu-usage                                    Amount of time the process has utilized the CPU (ms)
    working-set                                  Amount of working set used by the process (MB)
    gc-heap-size                                 Total heap size reported by the GC (MB)
    gen-0-gc-count                               Number of Gen 0 GCs / min
    gen-1-gc-count                               Number of Gen 1 GCs / min
    gen-2-gc-count                               Number of Gen 2 GCs / min
    time-in-gc                                   % time in GC since the last GC
    gen-0-size                                   Gen 0 Heap Size
    gen-1-size                                   Gen 1 Heap Size
    gen-2-size                                   Gen 2 Heap Size
    loh-size                                     LOH Heap Size
    alloc-rate                                   Allocation Rate
    assembly-count                               Number of Assemblies Loaded
    exception-count                              Number of Exceptions / sec
    threadpool-thread-count                      Number of ThreadPool Threads
    monitor-lock-contention-count                Monitor Lock Contention Count
    threadpool-queue-length                      ThreadPool Work Items Queue Length
    threadpool-completed-items-count             ThreadPool Completed Work Items Count
    active-timer-count                           Active Timers Count

Microsoft.AspNetCore.Hosting
    requests-per-second                  Request rate
    total-requests                       Total number of requests
    current-requests                     Current number of requests
    failed-requests                      Failed number of requests
```

> [!NOTE]
> Os `Microsoft.AspNetCore.Hosting` contadores são exibidos quando há processos identificados que dão suporte a esses contadores, por exemplo, quando um aplicativo ASP.NET Core está em execução no computador host.

## <a name="dotnet-counters-monitor"></a>dotnet – monitor de contadores

Exibe a atualização periódica de valores dos contadores selecionados.

### <a name="synopsis"></a>Sinopse

```console
dotnet-counters monitor [-h|--help] [-p|--process-id] [--refreshInterval] [counter_list]
```

### <a name="options"></a>Opções

- **`-p|--process-id <PID>`**

  A ID do processo a ser monitorado.

- **`--refresh-interval <SECONDS>`**

  O número de segundos de atraso entre a atualização dos contadores exibidos

- **`counter_list <COUNTERS>`**

  Uma lista de contadores separados por espaços. Os contadores podem ser especificados `provider_name[:counter_name]` . Se o `provider_name` for usado sem uma qualificação `counter_name` , todos os contadores serão mostrados. Para descobrir os nomes de provedor e contador, use o comando [dotnet-Counters List](#dotnet-counters-list) .

### <a name="examples"></a>Exemplos

- Monitorar todos os contadores de `System.Runtime` em um intervalo de atualização de 3 segundos:

  ```console
  > dotnet-counters monitor --process-id 1902  --refresh-interval 3 System.Runtime

  Press p to pause, r to resume, q to quit.
    System.Runtime:
      CPU Usage (%)                                 24
      Working Set (MB)                            1982
      GC Heap Size (MB)                            811
      Gen 0 GC / second                             20
      Gen 1 GC / second                              4
      Gen 2 GC / second                              1
      Number of Exceptions / sec                     4
  ```

- Monitorar apenas o uso da CPU e o tamanho do heap do GC de `System.Runtime` :

  ```console
  > dotnet-counters monitor --process-id 1902 System.Runtime[cpu-usage,gc-heap-size]

  Press p to pause, r to resume, q to quit.
    System.Runtime:
      CPU Usage (%)                                 24
      GC Heap Size (MB)                            811
  ```

- Monitorar `EventCounter` valores de definidos pelo usuário `EventSource` . Para obter mais informações, consulte [tutorial: medir o desempenho usando o EventCounters no .NET Core](event-counter-perf.md).

  ```console
  > dotnet-counters monitor --process-id 1902 Samples-EventCounterDemos-Minimal

  Press p to pause, r to resume, q to quit.
      request                                      100
  ```
  
## <a name="dotnet-counters-ps"></a>dotnet-contadores PS

Exibe uma lista de processos dotnet que podem ser monitorados.

### <a name="synopsis"></a>Sinopse

```console
dotnet-counters ps [-h|--help]
```

### <a name="example"></a>Exemplo

```console
> dotnet-counters ps
  
  15683 WebApi     /home/suwhang/repos/WebApi/WebApi
  16324 dotnet     /usr/local/share/dotnet/dotnet
```
