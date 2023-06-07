---
title: EventCounters no .NET Core
description: Neste artigo, você aprenderá o que é o EventCounters, como implementá-los e como consumi-los.
ms.date: 08/07/2020
ms.openlocfilehash: fc2f945e3de732a81b9ce3fd82eff10e455cae87
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062958"
---
# <a name="eventcounters-in-net-core"></a>EventCounters no .NET Core

**Este artigo aplica-se a: ✔️** SDK do .net Core 3,0 e versões posteriores

EventCounters são APIs do .NET Core usadas para coleta de métrica de desempenho leve, de plataforma cruzada e quase em tempo real. EventCounters foram adicionados como uma alternativa de plataforma cruzada aos "contadores de desempenho" do .NET Framework no Windows. Neste artigo, você aprenderá o que são os EventCounters, como implementá-los e como consumi-los.

O tempo de execução do .NET Core e algumas bibliotecas .NET publicam informações básicas de diagnóstico usando EventCounters a partir do .NET Core 3,0. Além dos EventCounters que são fornecidos pelo tempo de execução do .NET, você pode optar por implementar seu próprio EventCounters. EventCounters pode ser usado para controlar várias métricas.

EventCounters em tempo real como parte de um <xref:System.Diagnostics.Tracing.EventSource> e são automaticamente enviados para ferramentas de ouvinte regularmente. Assim como todos os outros eventos em um <xref:System.Diagnostics.Tracing.EventSource> , eles podem ser consumidos tanto no processo quanto fora do processo por meio de <xref:System.Diagnostics.Tracing.EventListener> e EventPipe. Este artigo se concentra nos recursos de plataforma cruzada do EventCounters e exclui intencionalmente o PerfView e o ETW (rastreamento de eventos para Windows) – embora ambos possam ser usados com o EventCounters.

![Imagem do diagrama EventCounters no proc e fora do processo](media/event-counters.svg)

[!INCLUDE [available-counters](includes/available-counters.md)]

## <a name="eventcounter-api-overview"></a>Visão geral da API do EventCounter

Há duas categorias principais de contadores. Alguns contadores são para valores de "taxa", como o número total de exceções, o número total de GCs e o número total de solicitações. Outros contadores são valores de "instantâneo", como uso de heap, uso de CPU e tamanho de conjunto de trabalho. Dentro de cada uma dessas categorias de contadores, há dois tipos de contadores que variam de acordo com o modo como eles obtêm seu valor. Os contadores de sondagem recuperam seu valor por meio de um retorno de chamada, e os contadores de não sondagem têm seus valores definidos diretamente na instância do contador.

Os contadores são representados pelas seguintes implementações:

* <xref:System.Diagnostics.Tracing.EventCounter>
* <xref:System.Diagnostics.Tracing.IncrementingEventCounter>
* <xref:System.Diagnostics.Tracing.IncrementingPollingCounter>
* <xref:System.Diagnostics.Tracing.PollingCounter>

Um ouvinte de eventos especifica por quanto tempo os intervalos de medição são. Ao final de cada intervalo, um valor é transmitido para o ouvinte para cada contador. As implementações de um contador determinam quais APIs e cálculos são usados para produzir o valor em cada intervalo.

1. O <xref:System.Diagnostics.Tracing.EventCounter> registra um conjunto de valores. O <xref:System.Diagnostics.Tracing.EventCounter.WriteMetric%2A?displayProperty=nameWithType> método adiciona um novo valor ao conjunto. Com cada intervalo, um resumo estatístico do conjunto é calculado, como mín., máx. e média. A ferramenta [dotnet-Counters](dotnet-counters.md) sempre exibirá o valor médio. O <xref:System.Diagnostics.Tracing.EventCounter> é útil para descrever um conjunto discreto de operações. O uso comum pode incluir o monitoramento do tamanho médio em bytes de operações de e/s recentes ou o valor monetário médio de um conjunto de transações financeiras.

1. <xref:System.Diagnostics.Tracing.IncrementingEventCounter>Registra um total acumulado para cada intervalo de tempo. O <xref:System.Diagnostics.Tracing.IncrementingEventCounter.Increment%2A?displayProperty=nameWithType> método adiciona ao total. Por exemplo, se `Increment()` for chamado três vezes durante um intervalo com valores `1` , `2` e `5` , o total acumulado de `8` será relatado como o valor do contador para esse intervalo. A ferramenta [dotnet-Counters](dotnet-counters.md) exibirá a taxa como o total/tempo gravado. O <xref:System.Diagnostics.Tracing.IncrementingEventCounter> é útil para medir a frequência de ocorrência de uma ação, como o número de solicitações processadas por segundo.

1. O <xref:System.Diagnostics.Tracing.PollingCounter> usa um retorno de chamada para determinar o valor que é relatado. Com cada intervalo de tempo, a função de retorno de chamada fornecida pelo usuário é invocada e o valor de retorno é usado como o valor do contador. Um <xref:System.Diagnostics.Tracing.PollingCounter> pode ser usado para consultar uma métrica de uma fonte externa, por exemplo, obter os bytes livres atuais em um disco. Ele também pode ser usado para relatar estatísticas personalizadas que podem ser computadas sob demanda por um aplicativo. Os exemplos incluem relatar o 95 º percentil de latências de solicitação recentes ou a taxa atual de acertos ou de erros de um cache.

1. O <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> usa um retorno de chamada para determinar o valor de incremento relatado. Com cada intervalo de tempo, o retorno de chamada é invocado e, em seguida, a diferença entre a invocação atual e a última invocação é o valor relatado. A ferramenta [dotnet-Counters](dotnet-counters.md) sempre exibirá a diferença como uma taxa, o valor/tempo relatado. Esse contador é útil quando não é viável chamar uma API em cada ocorrência, mas é possível consultar o número total de ocorrências. Por exemplo, você pode relatar o número de bytes gravados em um arquivo por segundo, mesmo sem uma notificação toda vez que um byte é gravado.

## <a name="implement-an-eventsource"></a>Implementar um EventSource

O código a seguir implementa um exemplo <xref:System.Diagnostics.Tracing.EventSource> exposto como o `"Sample.EventCounter.Minimal"` provedor nomeado. Essa origem contém um <xref:System.Diagnostics.Tracing.EventCounter> tempo de processamento de solicitação de representação. Esse contador tem um nome (ou seja, sua ID exclusiva na origem) e um nome de exibição, ambos usados por ferramentas de escuta, como [dotnet-Counter](dotnet-counters.md).

:::code language="csharp" source="snippets/EventCounters/MinimalEventCounterSource.cs":::

Você usa `dotnet-counters ps` para exibir uma lista de processos .NET que podem ser monitorados:

```console
dotnet-counters ps
   1398652 dotnet     C:\Program Files\dotnet\dotnet.exe
   1399072 dotnet     C:\Program Files\dotnet\dotnet.exe
   1399112 dotnet     C:\Program Files\dotnet\dotnet.exe
   1401880 dotnet     C:\Program Files\dotnet\dotnet.exe
   1400180 sample-counters C:\sample-counters\bin\Debug\netcoreapp3.1\sample-counters.exe
```

Passe o <xref:System.Diagnostics.Tracing.EventSource> nome para o `counter_list` comutador para começar a monitorar seu contador:

```console
dotnet-counters monitor --process-id 1400180 Sample.EventCounter.Minimal
```

O exemplo a seguir mostra a saída do monitor:

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[Samples-EventCounterDemos-Minimal]
    Request Processing Time (ms)                            0.445
```

Pressione <kbd>q</kbd> para interromper o comando de monitoramento.

#### <a name="conditional-counters"></a>Contadores condicionais

Ao implementar um <xref:System.Diagnostics.Tracing.EventSource> , os contadores que os contêm podem ser instanciados condicionalmente quando o <xref:System.Diagnostics.Tracing.EventSource.OnEventCommand%2A?displayProperty=nameWithType> método é chamado com um <xref:System.Diagnostics.Tracing.EventCommandEventArgs.Command> valor de `EventCommand.Enable` . Para instanciar com segurança uma instância de contador somente se for `null` , use o [operador de atribuição de União nula](../../csharp/language-reference/operators/null-coalescing-operator.md). Além disso, os métodos personalizados podem avaliar o <xref:System.Diagnostics.DiagnosticSource.IsEnabled%2A> método para determinar se a origem do evento atual está habilitada ou não.

:::code language="csharp" source="snippets/EventCounters/ConditionalEventCounterSource.cs":::

> [!TIP]
> Os contadores condicionais são contadores que são condicionalmente instanciados, uma microotimização. O tempo de execução adota esse padrão para cenários em que os contadores normalmente não são usados, para economizar uma fração de um milissegundo.

### <a name="net-core-runtime-example-counters"></a>Contadores de exemplo de tempo de execução do .NET Core

Há muitas excelentes implementações de exemplo no tempo de execução do .NET Core. Aqui está a implementação de tempo de execução para o contador que controla o tamanho do conjunto de trabalho do aplicativo.

```csharp
var workingSetCounter = new PollingCounter(
    "working-set",
    this,
    () => (double)(Environment.WorkingSet / 1_000_000))
{
    DisplayName = "Working Set",
    DisplayUnits = "MB"
};
```

O <xref:System.Diagnostics.Tracing.PollingCounter> relata a quantidade atual de memória física mapeada para o processo (conjunto de trabalho) do aplicativo, já que ele captura uma métrica em um momento no tempo. O retorno de chamada para sondar um valor é a expressão lambda fornecida, que é apenas uma chamada para a <xref:System.Environment.WorkingSet?displayProperty=fullName> API. <xref:System.Diagnostics.Tracing.DiagnosticCounter.DisplayName>e <xref:System.Diagnostics.Tracing.DiagnosticCounter.DisplayUnits> são propriedades opcionais que podem ser definidas para ajudar o lado do consumidor do contador a exibir o valor mais claramente. Por exemplo, [dotnet-Counters](dotnet-counters.md) usa essas propriedades para exibir a versão mais amigável dos nomes dos contadores.

> [!IMPORTANT]
> As `DisplayName` Propriedades não estão localizadas.

Para o <xref:System.Diagnostics.Tracing.PollingCounter> , e o <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> , nada mais precisa ser feito. Ambos sondam os próprios valores em um intervalo solicitado pelo consumidor.

Aqui está um exemplo de um contador de tempo de execução implementado usando o <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> .

```csharp
var monitorContentionCounter = new IncrementingPollingCounter(
    "monitor-lock-contention-count",
    this,
    () => Monitor.LockContentionCount
)
{
    DisplayName = "Monitor Lock Contention Count",
    DisplayRateTimeScale = TimeSpan.FromSeconds(1)
};
```

O <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> usa a <xref:System.Threading.Monitor.LockContentionCount?displayProperty=nameWithType> API para relatar o incremento da contagem total de contenções de bloqueio. A <xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> propriedade é opcional, mas quando usada, ela pode fornecer uma dica para qual intervalo de tempo o contador é exibido melhor. Por exemplo, a contagem de contenções de bloqueio é melhor exibida como _contagem por segundo_, portanto, ela <xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> é definida como um segundo. A taxa de exibição pode ser ajustada para diferentes tipos de contadores de taxa.

> [!NOTE]
> O <xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> _não_ é usado por [dotnet-Counters](dotnet-counters.md), e ouvintes de eventos não são obrigados a usá-lo.

Há mais implementações de contador a serem usadas como referência no repositório de [tempo de execução do .net](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Private.CoreLib/src/System/Diagnostics/Tracing/RuntimeEventSource.cs) .

## <a name="concurrency"></a>Simultaneidade

> [!TIP]
> A API EventCounters não garante a segurança do thread. Quando os delegados passados para <xref:System.Diagnostics.Tracing.PollingCounter> ou as <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> instâncias são chamados por vários threads, é sua responsabilidade garantir a segurança de threads dos delegados.

Por exemplo, considere o seguinte <xref:System.Diagnostics.Tracing.EventSource> para acompanhar as solicitações.

:::code language="csharp" source="snippets/EventCounters/RequestEventSource.cs":::

O `AddRequest()` método pode ser chamado a partir de um manipulador de solicitação e o `RequestRateCounter` sonda o valor no intervalo especificado pelo consumidor do contador. No entanto, o `AddRequest()` método pode ser chamado por vários threads de uma vez, colocando uma condição de corrida ativada `_requestCount` . Uma maneira alternativa thread-safe de incrementar o `_requestCount` é usar <xref:System.Threading.Interlocked.Increment%2A?displayProperty=nameWithType> .

```csharp
public void AddRequest() => Interlocked.Increment(ref _requestCount);
```

## <a name="consume-eventcounters"></a>Consumir EventCounters

Há duas maneiras principais de consumir EventCounters, seja no processo ou fora do processo. O consumo de EventCounters pode ser diferenciado em três camadas de várias tecnologias consumidas.

- Transportando eventos em um fluxo bruto via ETW ou EventPipe:
  - As APIs do ETW são fornecidas com o sistema operacional Windows, e o EventPipe é acessível como uma [API do .net](https://github.com/dotnet/diagnostics/blob/master/documentation/design-docs/diagnostics-client-library.md#1-attaching-to-a-process-and-dumping-out-all-the-runtime-gc-events-in-real-time-to-the-console)ou o [protocolo IPC](https://github.com/dotnet/diagnostics/blob/master/documentation/design-docs/ipc-protocol.md)de diagnóstico.
- Decodificando o fluxo de eventos binários em eventos:
  - A [biblioteca TraceEvent](https://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent) lida com os formatos de fluxo ETW e EventPipe.
- Ferramentas de linha de comando e GUI:
  - Ferramentas como PerfView (ETW ou EventPipe), dotnet-Counters (somente EventPipe) e dotnet-monitor (somente EventPipe).

### <a name="consume-out-of-proc"></a>Consumir fora do processo

Consumir EventCounters fora do processo é uma abordagem muito comum. Você pode usar os [contadores dotnet](dotnet-counters.md) para consumi-los em uma maneira de plataforma cruzada por meio de um EventPipe. A `dotnet-counters` ferramenta é uma ferramenta global da CLI do dotNet de plataforma cruzada que pode ser usada para monitorar os valores do contador. Para saber como usar o `dotnet-counters` para monitorar seus contadores, consulte [dotnet-Counters](dotnet-counters.md)ou trabalhe no tutorial [medir o desempenho usando o EventCounters](event-counter-perf.md) .

#### <a name="dotnet-trace"></a>dotnet-trace

A `dotnet-trace` ferramenta pode ser usada para consumir os dados do contador por meio de um EventPipe. Aqui está um exemplo `dotnet-trace` de uso para coletar dados do contador.

```console
dotnet-trace collect --process-id <pid> Sample.EventCounter.Minimal:0:0:EventCounterIntervalSec=1
```

Para obter mais informações sobre como coletar valores de contador ao longo do tempo, consulte a documentação do [rastreamento dotnet](dotnet-trace.md#use-dotnet-trace-to-collect-counter-values-over-time) .

#### <a name="azure-application-insights"></a>Azure Application Insights

O EventCounters pode ser consumido por Azure Monitor, especificamente Aplicativo Azure insights. Os contadores podem ser adicionados e removidos, e você é livre para especificar contadores personalizados ou contadores conhecidos. Para obter mais informações, consulte [Personalizando contadores a serem coletados](/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected).

#### <a name="dotnet-monitor"></a>dotnet-monitor

A `dotnet-monitor` ferramenta é uma ferramenta experimental que torna mais fácil obter acesso a informações de diagnóstico em um processo do .net. A ferramenta serve como um superconjunto de todas as ferramentas de diagnóstico. Além dos rastreamentos, ele pode monitorar as métricas, coletar despejos de memória e coletar despejos de GC. Ele é distribuído como uma ferramenta CLI e uma imagem do Docker. Ele expõe uma API REST e a coleção de artefatos de diagnóstico ocorre por meio de chamadas REST.

Para obter mais informações, consulte [Introducing dotnet-monitor, uma ferramenta experimental](https://devblogs.microsoft.com/dotnet/introducing-dotnet-monitor).

### <a name="consume-in-proc"></a>Consumir no processo

Você pode consumir os valores do contador por meio da <xref:System.Diagnostics.Tracing.EventListener> API. Um <xref:System.Diagnostics.Tracing.EventListener> é uma maneira no processo de consumir todos os eventos gravados por todas as instâncias de um <xref:System.Diagnostics.Tracing.EventSource> em seu aplicativo. Para obter mais informações sobre como usar a `EventListener` API, consulte <xref:System.Diagnostics.Tracing.EventListener> .

Primeiro, o <xref:System.Diagnostics.Tracing.EventSource> que produz o valor do contador precisa ser habilitado. Substitua o <xref:System.Diagnostics.Tracing.EventListener.OnEventSourceCreated%2A?displayProperty=nameWithType> método para obter uma notificação quando um <xref:System.Diagnostics.Tracing.EventSource> for criado e, se esse for o correto <xref:System.Diagnostics.Tracing.EventSource> com seu EventCounters, você poderá chamá <xref:System.Diagnostics.Tracing.EventListener.EnableEvents%2A?displayProperty=nameWithType> -lo. Aqui está um exemplo de substituição:

:::code language="csharp" source="snippets/EventCounters/SimpleEventListener.cs" range="16-27":::

#### <a name="sample-code"></a>Código de exemplo

Aqui está uma classe de exemplo <xref:System.Diagnostics.Tracing.EventListener> que imprime todos os nomes de contadores e valores de tempo de execução do .NET <xref:System.Diagnostics.Tracing.EventSource> , para publicar seus contadores internos ( `System.Runtime` ) em algum intervalo.

:::code language="csharp" source="snippets/EventCounters/SimpleEventListener.cs":::

Como mostrado acima, você _deve_ verificar se o `"EventCounterIntervalSec"` argumento está definido no `filterPayload` argumento ao chamar <xref:System.Diagnostics.Tracing.EventListener.EnableEvents%2A> . Caso contrário, os contadores não poderão liberar valores, já que ele não sabe em qual intervalo ele deve ser liberado.

## <a name="see-also"></a>Consulte também

- [dotnet-counters](dotnet-counters.md)
- [dotnet-trace](dotnet-trace.md)
- <xref:System.Diagnostics.Tracing.EventCounter>
- <xref:System.Diagnostics.Tracing.EventListener>
- <xref:System.Diagnostics.Tracing.EventSource>
