---
title: Depurar alto uso da CPU-.NET Core
description: Um tutorial que orienta você pela depuração de alto uso da CPU no .NET Core.
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 93076bbce3baf3a219b25c927d2aba3d2d57456f
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557796"
---
# <a name="debug-high-cpu-usage-in-net-core"></a>Depurar alto uso da CPU no .NET Core

**Este artigo aplica-se a: ✔️** SDK do .net Core 3,1 e versões posteriores

Neste tutorial, você aprenderá a depurar um cenário de uso excessivo da CPU. Usando o exemplo fornecido ASP.NET Core repositório de código-fonte do [aplicativo Web](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios) , você pode causar um deadlock intencionalmente. O ponto de extremidade passará por uma falha e acumulação de threads. Você aprenderá como é possível usar várias ferramentas para diagnosticar esse cenário com várias partes importantes dos dados de diagnóstico.

Neste tutorial, você irá:

> [!div class="checklist"]
>
> - Investigar o alto uso da CPU
> - Determinar o uso da CPU com [dotnet-contadores](dotnet-counters.md)
> - Usar [dotnet-rastreamento](dotnet-trace.md) para geração de rastreamento
> - Desempenho do perfil no PerfView
> - Diagnosticar e resolver o uso excessivo da CPU

## <a name="prerequisites"></a>Pré-requisitos

O tutorial usa:

- [SDK do .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core) ou uma versão posterior.
- [Exemplo de destino de depuração](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios) para disparar o cenário.
- [dotnet – rastreamento](dotnet-trace.md) para listar processos e gerar um perfil.
- [dotnet-contadores](dotnet-counters.md) para monitorar o uso da CPU.

## <a name="cpu-counters"></a>Contadores de CPU

Antes de tentar coletar dados de diagnóstico, você precisa observar uma alta condição de CPU. Execute o [aplicativo de exemplo](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios) usando o comando a seguir no diretório raiz do projeto.

```dotnetcli
dotnet run
```

Para localizar a ID do processo, use o seguinte comando:

```dotnetcli
dotnet-trace ps
```

Anote a ID do processo da saída do comando. Nossa ID de processo era `22884` , mas a sua será diferente. Para verificar o uso atual da CPU, use o comando de ferramenta [dotnet-Counters](dotnet-counters.md) :

```dotnetcli
dotnet-counters monitor --refresh-interval 1 -p 22884
```

O `refresh-interval` é o número de segundos entre o contador sondando valores de CPU. A saída deve ser semelhante ao seguinte:

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[System.Runtime]
    % Time in GC since last GC (%)                         0
    Allocation Rate / 1 sec (B)                            0
    CPU Usage (%)                                          0
    Exception Count / 1 sec                                0
    GC Heap Size (MB)                                      4
    Gen 0 GC Count / 60 sec                                0
    Gen 0 Size (B)                                         0
    Gen 1 GC Count / 60 sec                                0
    Gen 1 Size (B)                                         0
    Gen 2 GC Count / 60 sec                                0
    Gen 2 Size (B)                                         0
    LOH Size (B)                                           0
    Monitor Lock Contention Count / 1 sec                  0
    Number of Active Timers                                1
    Number of Assemblies Loaded                          140
    ThreadPool Completed Work Item Count / 1 sec           3
    ThreadPool Queue Length                                0
    ThreadPool Thread Count                                7
    Working Set (MB)                                      63
```

Com o aplicativo Web em execução, imediatamente após a inicialização, a CPU não está sendo consumida e é relatada em `0%` . Navegue até a `api/diagscenario/highcpu` rota com `60000` como o parâmetro de rota:

`https://localhost:5001/api/diagscenario/highcpu/60000`

Agora, execute novamente o comando [dotnet-Counters](dotnet-counters.md) . Para monitorar apenas o `cpu-usage` , especifique `System.Runtime[cpu-usage]` como parte do comando.

```dotnetcli
dotnet-counters monitor System.Runtime[cpu-usage] -p 22884 --refresh-interval 1
```

Você deve ver um aumento no uso da CPU, conforme mostrado abaixo:

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[System.Runtime]
    CPU Usage (%)                                         25
```

Ao longo da duração da solicitação, o uso da CPU focaliza 25%. Dependendo do computador host, espere o uso de CPU variável.

> [!TIP]
> Para visualizar um uso de CPU ainda maior, você pode exercitar esse ponto de extremidade em várias guias do navegador simultaneamente.

Neste ponto, você pode dizer com segurança que a CPU está em execução mais alta do que o esperado.

## <a name="trace-generation"></a>Geração de rastreamento

Ao analisar uma solicitação lenta, você precisa de uma ferramenta de diagnóstico que possa fornecer informações sobre o que o código está fazendo. A opção usual é um criador de perfil, e há diferentes opções de criador de perfil a serem escolhidas.

### <a name="linux"></a>[Linux](#tab/linux)

A `perf` ferramenta pode ser usada para gerar perfis de aplicativo .NET Core. Saia da instância anterior do [destino de depuração de exemplo](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios).

Defina a `COMPlus_PerfMapEnabled` variável de ambiente para fazer com que o aplicativo .NET Core crie um `map` arquivo no `/tmp` diretório. Esse `map` arquivo é usado pelo `perf` para mapear o endereço de CPU para funções geradas por JIT por nome. Para obter mais informações, consulte [Write perf MAP](../run-time-config/debugging-profiling.md#write-perf-map).

Execute o [destino de depuração de exemplo](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios) na mesma sessão de terminal.

```dotnetcli
export COMPlus_PerfMapEnabled=1
dotnet run
```

Exerça o ponto de extremidade de API de CPU alta ( `https://localhost:5001/api/diagscenario/highcpu/60000` ) novamente. Enquanto estiver em execução dentro da solicitação de 1 minuto, execute o `perf` comando com a ID do processo:

```bash
sudo perf record -p 2266 -g
```

O `perf` comando inicia o processo de coleta de desempenho. Deixe que ele seja executado por cerca de 20-30 segundos e pressione <kbd>Ctrl + C</kbd> para sair do processo de coleta. Você pode usar o mesmo `perf` comando para ver a saída do rastreamento.

```bash
sudo perf report -f
```

Você também pode gerar um _elemento chama-grafo_ usando os seguintes comandos:

```bash
git clone --depth=1 https://github.com/BrendanGregg/FlameGraph
sudo perf script | FlameGraph/stackcollapse-perf.pl | FlameGraph/flamegraph.pl > flamegraph.svg
```

Esse comando gera um `flamegraph.svg` que você pode exibir no navegador para investigar o problema de desempenho:

[![Imagem SVG do grafo chama](media/flamegraph.jpg)](media/flamegraph.jpg#lightbox)

### <a name="windows"></a>[Windows](#tab/windows)

No Windows, você pode usar a ferramenta [dotnet-Trace](dotnet-trace.md) como um criador de perfil. Usando o [destino de depuração de exemplo](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios)anterior, exerça novamente o ponto de extremidade de CPU alta ( `https://localhost:5001/api/diagscenario/highcpu/60000` ). Enquanto estiver em execução dentro da solicitação de 1 minuto, use o `collect` comando da seguinte maneira:

```dotnetcli
dotnet-trace collect -p 22884 --providers Microsoft-DotNETCore-SampleProfiler
```

Deixe que o [rastreamento dotnet](dotnet-trace.md) seja executado por cerca de 20-30 segundos e, em seguida, pressione <kbd>Enter</kbd> para sair da coleção. O resultado é um `nettrace` arquivo localizado na mesma pasta. Os `nettrace` arquivos são uma ótima maneira de usar as ferramentas de análise existentes no Windows.

Abra o `nettrace` com [`PerfView`](https://github.com/microsoft/perfview/blob/master/documentation/Downloading.md) conforme mostrado abaixo.

[![Imagem PerfView](media/perfview.jpg)](media/perfview.jpg#lightbox)

---

## <a name="see-also"></a>Confira também

- [dotnet-rastrear](dotnet-trace.md) para listar processos
- [dotnet-contadores](dotnet-counters.md) para verificar o uso de memória gerenciada
- [dotnet-despejo](dotnet-dump.md) para coletar e analisar um arquivo de despejo
- [dotnet/diagnóstico](https://github.com/dotnet/diagnostics/tree/master/documentation/tutorial)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Depurar um deadlock no .NET Core](debug-deadlock.md)
