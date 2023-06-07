---
title: Medir o desempenho usando o EventCounters no .NET Core
description: Neste tutorial, você aprenderá a medir o desempenho usando o EventCounters.
ms.date: 08/07/2020
ms.topic: tutorial
ms.openlocfilehash: 7b4940e17d01e7ec5a50d11e3c818ecdec2d48cf
ms.sourcegitcommit: 1e6439ec4d5889fc08cf3bfb4dac2b91931eb827
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2020
ms.locfileid: "88024992"
---
# <a name="tutorial-measure-performance-using-eventcounters-in-net-core"></a>Tutorial: medir o desempenho usando o EventCounters no .NET Core

**Este artigo aplica-se a: ✔️** SDK do .net Core 3,0 e versões posteriores

Neste tutorial, você aprenderá como um <xref:System.Diagnostics.Tracing.EventCounter> pode ser usado para medir o desempenho com uma alta frequência de eventos. Você pode usar os [contadores disponíveis](event-counters.md#available-counters) publicados por vários pacotes oficiais do .NET Core, provedores de terceiros ou criar suas próprias métricas para monitoramento.

Neste tutorial, você irá:

> [!div class="checklist"]
>
> - Implemente um <xref:System.Diagnostics.Tracing.EventSource> .
> - Monitorar contadores com [dotnet-contadores](dotnet-counters.md).

## <a name="prerequisites"></a>Pré-requisitos

O tutorial usa:

- [SDK do .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core) ou uma versão posterior.
- [dotnet-contadores](dotnet-counters.md) para monitorar os contadores de eventos.
- Um aplicativo de [destino de depuração de exemplo](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios) para diagnosticar.

## <a name="get-the-source"></a>Obter a origem

O aplicativo de exemplo será usado como base para o monitoramento. O [repositório de ASP.NET Core de exemplo](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios) está disponível no navegador de exemplos. Baixe o arquivo zip, extraia-o depois de baixado e abra-o em seu IDE favorito. Compile e execute o aplicativo para garantir que ele funcione corretamente e, em seguida, interrompa o aplicativo.

## <a name="implement-an-eventsource"></a>Implementar um EventSource

Para eventos que ocorrem a cada poucos milissegundos, você desejará que a sobrecarga por evento seja baixa (menos de um milissegundo). Caso contrário, o impacto no desempenho será significativo. O registro em log de um evento significa que você vai escrever algo em disco. Se o disco não for rápido o suficiente, você perderá eventos. Você precisa de uma solução diferente de registrar o evento em si.

Ao lidar com um grande número de eventos, conhecer a medida por evento também não é útil. Na maioria das vezes, tudo o que você precisa é de apenas algumas estatísticas. Portanto, você pode obter as estatísticas dentro do próprio processo e, em seguida, escrever um evento uma vez em um momento para relatar as estatísticas, o que <xref:System.Diagnostics.Tracing.EventCounter> fará.

Veja abaixo um exemplo de como implementar um <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> . Crie um novo arquivo chamado *MinimalEventCounterSource.cs* e use o trecho de código como sua fonte:

:::code language="csharp" source="snippets/EventCounters/MinimalEventCounterSource.cs":::

A <xref:System.Diagnostics.Tracing.EventSource.WriteEvent%2A?displayProperty=nameWithType> linha é a <xref:System.Diagnostics.Tracing.EventSource> parte e não faz parte do <xref:System.Diagnostics.Tracing.EventCounter> , ela foi escrita para mostrar que você pode registrar uma mensagem junto com o contador de eventos.

## <a name="add-an-action-filter"></a>Adicionar um filtro de ação

O código-fonte de exemplo é um projeto ASP.NET Core. Você pode adicionar um [filtro de ação](/aspnet/core/mvc/controllers/filters#action-filters) globalmente, que registrará o tempo total de solicitação. Crie um novo arquivo chamado *LogRequestTimeFilterAttribute.cs*e use o código a seguir:

```csharp
using System.Diagnostics;
using Microsoft.AspNetCore.Http.Extensions;
using Microsoft.AspNetCore.Mvc.Filters;

namespace DiagnosticScenarios
{
    public class LogRequestTimeFilterAttribute : ActionFilterAttribute
    {
        readonly Stopwatch _stopwatch = new Stopwatch();

        public override void OnActionExecuting(ActionExecutingContext context) => _stopwatch.Start();

        public override void OnActionExecuted(ActionExecutedContext context)
        {
            _stopwatch.Stop();

            MinimalEventCounterSource.Log.Request(
                context.HttpContext.Request.GetDisplayUrl(), _stopwatch.ElapsedMilliseconds);
        }
    }
}
```

O filtro de ação inicia a <xref:System.Diagnostics.Stopwatch> à medida que a solicitação começa e para quando ela é concluída, capturando o tempo decorrido. O total de milissegundos é registrado na `MinimalEventCounterSource` instância singleton. Para que esse filtro seja aplicado, você precisa adicioná-lo à coleção de filtros. No arquivo *Startup.cs* , atualize o `ConfigureServices` método em incluir este filtro.

```csharp
public void ConfigureServices(IServiceCollection services) =>
    services.AddControllers(options => options.Filters.Add<LogRequestTimeFilterAttribute>())
            .AddNewtonsoftJson();
```

## <a name="monitor-event-counter"></a>Monitorar contador de eventos

Com a implementação em um <xref:System.Diagnostics.Tracing.EventSource> e o filtro de ação personalizada, compile e inicie o aplicativo.
Você registrou a métrica no <xref:System.Diagnostics.Tracing.EventCounter> , mas, a menos que você acesse as estatísticas de dele, isso não é útil. Para obter as estatísticas, você precisa habilitar o <xref:System.Diagnostics.Tracing.EventCounter> criando um temporizador que é acionado sempre que você deseja os eventos, bem como um ouvinte para capturar os eventos. Para fazer isso, você pode usar o [dotnet-Counters](dotnet-counters.md).

Use o comando [dotnet-Counters PS](dotnet-counters.md#dotnet-counters-ps) para exibir uma lista de processos .NET que podem ser monitorados.

```console
dotnet-counters ps
```

Usando o identificador de processo da saída do `dotnet-counters ps` comando, você pode começar a monitorar o contador de eventos com o seguinte `dotnet-counters monitor` comando:

```console
dotnet-counters monitor --process-id 2196 Sample.EventCounter.Minimal Microsoft.AspNetCore.Hosting[total-requests,requests-per-second] System.Runtime[cpu-usage]
```

Enquanto o `dotnet-counters monitor` comando estiver em execução, mantenha <kbd>F5</kbd> no navegador para começar a emitir solicitações contínuas para o `https://localhost:5001/api/values` ponto de extremidade. Depois de alguns segundos, pare pressionando <kbd>q</kbd>

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[Microsoft.AspNetCore.Hosting]
    Request Rate / 1 sec                               9
    Total Requests                                   134
[System.Runtime]
    CPU Usage (%)                                     13
[Sample.EventCounter.Minimal]
    Request Processing Time (ms)                      34.5
```

O `dotnet-counters monitor` comando é ótimo para o monitoramento ativo. No entanto, talvez você queira coletar essas métricas de diagnóstico para o pós-processamento e a análise. Para isso, use o `dotnet-counters collect` comando. O `collect` comando switch é semelhante ao `monitor` comando, mas aceita alguns parâmetros adicionais. Você pode especificar o nome e o formato do arquivo de saída desejado. Para um arquivo JSON chamado *diagnostics.jsem* use o seguinte comando:

```console
dotnet-counters collect --process-id 2196 --format json -o diagnostics.json Sample.EventCounter.Minimal Microsoft.AspNetCore.Hosting[total-requests,requests-per-second] System.Runtime[cpu-usage]
```

Novamente, enquanto o comando estiver em execução, mantenha <kbd>F5</kbd> no navegador para começar a emitir solicitações contínuas para o `https://localhost:5001/api/values` ponto de extremidade. Depois de alguns segundos, pare pressionando <kbd>q</kbd>. O *diagnostics.jsno* arquivo é gravado. No entanto, o arquivo JSON que é gravado não é recuado; para facilitar a leitura, ele é recuado aqui.

```json
{
  "TargetProcess": "DiagnosticScenarios",
  "StartTime": "8/5/2020 3:02:45 PM",
  "Events": [
    {
      "timestamp": "2020-08-05 15:02:47Z",
      "provider": "System.Runtime",
      "name": "CPU Usage (%)",
      "counterType": "Metric",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:47Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Request Rate / 1 sec",
      "counterType": "Rate",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:47Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Total Requests",
      "counterType": "Metric",
      "value": 134
    },
    {
      "timestamp": "2020-08-05 15:02:47Z",
      "provider": "Sample.EventCounter.Minimal",
      "name": "Request Processing Time (ms)",
      "counterType": "Metric",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:47Z",
      "provider": "System.Runtime",
      "name": "CPU Usage (%)",
      "counterType": "Metric",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:48Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Request Rate / 1 sec",
      "counterType": "Rate",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:48Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Total Requests",
      "counterType": "Metric",
      "value": 134
    },
    {
      "timestamp": "2020-08-05 15:02:48Z",
      "provider": "Sample.EventCounter.Minimal",
      "name": "Request Processing Time (ms)",
      "counterType": "Metric",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:48Z",
      "provider": "System.Runtime",
      "name": "CPU Usage (%)",
      "counterType": "Metric",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:50Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Request Rate / 1 sec",
      "counterType": "Rate",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:50Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Total Requests",
      "counterType": "Metric",
      "value": 134
    },
    {
      "timestamp": "2020-08-05 15:02:50Z",
      "provider": "Sample.EventCounter.Minimal",
      "name": "Request Processing Time (ms)",
      "counterType": "Metric",
      "value": 0
    }
  ]
}
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [EventCounters](event-counters.md)
