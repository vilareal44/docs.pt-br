---
title: Tutorial de depuração de perda de memória
description: Saiba como depurar um vazamento de memória no .NET Core.
ms.topic: tutorial
ms.date: 04/20/2020
ms.openlocfilehash: ff684f9b9402cb8b7b648e792a1d37ddcc96b399
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86924884"
---
# <a name="debug-a-memory-leak-in-net-core"></a>Depurar um vazamento de memória no .NET Core

**Este artigo aplica-se a:** ✔️ SDK do .net Core 3,1 e versões posteriores

Este tutorial demonstra as ferramentas para analisar um vazamento de memória do .NET Core.

Este tutorial usa um aplicativo de exemplo, que é projetado para vazar a memória intencionalmente. O exemplo é fornecido como um exercício. Você pode analisar um aplicativo que está vazando involuntariamente a memória também.

Neste tutorial, você irá:

> [!div class="checklist"]
>
> - Examine o uso de memória gerenciada com os [contadores dotnet](dotnet-counters.md).
> - Gerar um arquivo de despejo.
> - Analise o uso de memória usando o arquivo de despejo.

## <a name="prerequisites"></a>Pré-requisitos

O tutorial usa:

- [SDK do .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core) ou uma versão posterior.
- [dotnet – rastreamento](dotnet-trace.md) para listar processos.
- [dotnet-contadores](dotnet-counters.md) para verificar o uso de memória gerenciada.
- [dotnet-despejo](dotnet-dump.md) para coletar e analisar um arquivo de despejo.
- Um aplicativo de [destino de depuração de exemplo](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios/) para diagnosticar.

O tutorial pressupõe que o exemplo e as ferramentas estejam instalados e prontos para uso.

## <a name="examine-managed-memory-usage"></a>Examinar o uso de memória gerenciada

Antes de começar a coletar dados de diagnóstico para ajudar a raiz a causar esse cenário, você precisa certificar-se de que está realmente vendo um vazamento de memória (aumento de memória). Você pode usar a ferramenta [dotnet-Counters](dotnet-counters.md) para confirmar isso.

Abra uma janela de console e navegue até o diretório em que você baixou e descompactou o [destino de depuração de exemplo](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios/). Execute o destino:

```dotnetcli
dotnet run
```

Em um console separado, localize a ID do processo usando a ferramenta [dotnet-Trace](dotnet-trace.md) :

```console
dotnet-trace ps
```

A saída deve ser semelhante a:

```console
4807 DiagnosticScena /home/user/git/samples/core/diagnostics/DiagnosticScenarios/bin/Debug/netcoreapp3.0/DiagnosticScenarios
```

Agora, verifique o uso de memória gerenciada com a ferramenta [dotnet-Counters](dotnet-counters.md) . O `--refresh-interval` especifica o número de segundos entre as atualizações:

```console
dotnet-counters monitor --refresh-interval 1 -p 4807
```

A saída ao vivo deve ser semelhante a:

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[System.Runtime]
    # of Assemblies Loaded                           118
    % Time in GC (since last GC)                       0
    Allocation Rate (Bytes / sec)                 37,896
    CPU Usage (%)                                      0
    Exceptions / sec                                   0
    GC Heap Size (MB)                                  4
    Gen 0 GC / sec                                     0
    Gen 0 Size (B)                                     0
    Gen 1 GC / sec                                     0
    Gen 1 Size (B)                                     0
    Gen 2 GC / sec                                     0
    Gen 2 Size (B)                                     0
    LOH Size (B)                                       0
    Monitor Lock Contention Count / sec                0
    Number of Active Timers                            1
    ThreadPool Completed Work Items / sec             10
    ThreadPool Queue Length                            0
    ThreadPool Threads Count                           1
    Working Set (MB)                                  83
```

Concentrando-se nesta linha:

```console
    GC Heap Size (MB)                                  4
```

Você pode ver que a memória de heap gerenciada é de 4 MB logo após a inicialização.

Agora, pressione a URL `https://localhost:5001/api/diagscenario/memleak/20000` .

Observe que o uso de memória cresceu para 30 MB.

```console
    GC Heap Size (MB)                                 30
```

Ao assistir ao uso de memória, você pode dizer com segurança que a memória está crescendo ou vazando. A próxima etapa é coletar os dados certos para análise de memória.

### <a name="generate-memory-dump"></a>Gerar despejo de memória

Ao analisar possíveis vazamentos de memória, você precisa ter acesso ao heap de memória do aplicativo. Em seguida, você pode analisar o conteúdo da memória. Examinando relações entre objetos, você cria teorias sobre o motivo pelo qual a memória não está sendo liberada. Uma fonte de dados de diagnóstico comum é um despejo de memória no Windows ou o dump de núcleo equivalente no Linux. Para gerar um despejo de um aplicativo .NET Core, você pode usar a ferramenta [dotnet-dump)](dotnet-dump.md) .

Usando o [destino de depuração de exemplo](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios/) iniciado anteriormente, execute o seguinte comando para gerar um despejo de núcleo do Linux:

```dotnetcli
dotnet-dump collect -p 4807
```

O resultado é um dump principal localizado na mesma pasta.

```console
Writing minidump with heap to ./core_20190430_185145
Complete
```

### <a name="restart-the-failed-process"></a>Reiniciar o processo com falha

Depois que o despejo for coletado, você deverá ter informações suficientes para diagnosticar o processo com falha. Se o processo com falha estiver em execução em um servidor de produção, agora é o momento ideal para a remediação de curto prazo reiniciando o processo.

Neste tutorial, você está pronto para o destino de [depuração de exemplo](https://docs.microsoft.com/samples/dotnet/samples/diagnostic-scenarios/) e pode fechá-lo. Navegue até o terminal que iniciou o servidor e pressione <kbd>Ctrl + C</kbd>.

### <a name="analyze-the-core-dump"></a>Analisar o dump principal

Agora que você tem um dump principal gerado, use a ferramenta [dotnet-dump](dotnet-dump.md) para analisar o despejo:

```dotnetcli
dotnet-dump analyze core_20190430_185145
```

Em que `core_20190430_185145` é o nome do dump principal que você deseja analisar.

> [!NOTE]
> Se você vir um erro reclamando que *libdl.so* não pode ser encontrado, talvez seja necessário instalar o pacote *libc6-dev* . Para saber mais, confira [Pré-requisitos para o .NET Core no Linux](../install/dependencies.md?pivots=os-linux).

Você verá um prompt onde pode inserir comandos SOS. Normalmente, a primeira coisa que você deseja examinar é o estado geral do heap gerenciado:

```console
> dumpheap -stat

Statistics:
              MT    Count    TotalSize Class Name
...
00007f6c1eeefba8      576        59904 System.Reflection.RuntimeMethodInfo
00007f6c1dc021c8     1749        95696 System.SByte[]
00000000008c9db0     3847       116080      Free
00007f6c1e784a18      175       128640 System.Char[]
00007f6c1dbf5510      217       133504 System.Object[]
00007f6c1dc014c0      467       416464 System.Byte[]
00007f6c21625038        6      4063376 testwebapi.Controllers.Customer[]
00007f6c20a67498   200000      4800000 testwebapi.Controllers.Customer
00007f6c1dc00f90   206770     19494060 System.String
Total 428516 objects
```

Aqui você pode ver que a maioria dos objetos são `String` ou `Customer` .

Você pode usar o `dumpheap` comando novamente com a tabela de método (MT) para obter uma lista de todas as `String` instâncias:

```console
> dumpheap -mt 00007faddaa50f90

         Address               MT     Size
...
00007f6ad09421f8 00007faddaa50f90       94
...
00007f6ad0965b20 00007f6c1dc00f90       80
00007f6ad0965c10 00007f6c1dc00f90       80
00007f6ad0965d00 00007f6c1dc00f90       80
00007f6ad0965df0 00007f6c1dc00f90       80
00007f6ad0965ee0 00007f6c1dc00f90       80

Statistics:
              MT    Count    TotalSize Class Name
00007f6c1dc00f90   206770     19494060 System.String
Total 206770 objects
```

Agora você pode usar o `gcroot` comando em uma `System.String` instância para ver como e por que o objeto tem raiz. Seja paciente porque esse comando leva vários minutos com um heap de 30 MB:

```console
> gcroot -all 00007f6ad09421f8

Thread 3f68:
    00007F6795BB58A0 00007F6C1D7D0745 System.Diagnostics.Tracing.CounterGroup.PollForValues() [/_/src/System.Private.CoreLib/shared/System/Diagnostics/Tracing/CounterGroup.cs @ 260]
        rbx:  (interior)
            ->  00007F6BDFFFF038 System.Object[]
            ->  00007F69D0033570 testwebapi.Controllers.Processor
            ->  00007F69D0033588 testwebapi.Controllers.CustomerCache
            ->  00007F69D00335A0 System.Collections.Generic.List`1[[testwebapi.Controllers.Customer, DiagnosticScenarios]]
            ->  00007F6C000148A0 testwebapi.Controllers.Customer[]
            ->  00007F6AD0942258 testwebapi.Controllers.Customer
            ->  00007F6AD09421F8 System.String

HandleTable:
    00007F6C98BB15F8 (pinned handle)
    -> 00007F6BDFFFF038 System.Object[]
    -> 00007F69D0033570 testwebapi.Controllers.Processor
    -> 00007F69D0033588 testwebapi.Controllers.CustomerCache
    -> 00007F69D00335A0 System.Collections.Generic.List`1[[testwebapi.Controllers.Customer, DiagnosticScenarios]]
    -> 00007F6C000148A0 testwebapi.Controllers.Customer[]
    -> 00007F6AD0942258 testwebapi.Controllers.Customer
    -> 00007F6AD09421F8 System.String

Found 2 roots.
```

Você pode ver que o `String` é mantido diretamente pelo `Customer` objeto e é mantido indiretamente por um `CustomerCache` objeto.

Você pode continuar despejando objetos para ver que a maioria dos `String` objetos segue um padrão semelhante. Neste ponto, a investigação forneceu informações suficientes para identificar a causa raiz em seu código.

Esse procedimento geral permite que você identifique a origem dos principais vazamentos de memória.

## <a name="clean-up-resources"></a>Limpar os recursos

Neste tutorial, você iniciou um servidor Web de exemplo. Esse servidor deve ter sido desligado, conforme explicado na seção [reiniciar o processo com falha](#restart-the-failed-process) .

Você também pode excluir o arquivo de despejo que foi criado.

## <a name="see-also"></a>Veja também

- [dotnet-rastrear](dotnet-trace.md) para listar processos
- [dotnet-contadores](dotnet-counters.md) para verificar o uso de memória gerenciada
- [dotnet-despejo](dotnet-dump.md) para coletar e analisar um arquivo de despejo
- [dotnet/diagnóstico](https://github.com/dotnet/diagnostics/tree/master/documentation/tutorial)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Depurar alta utilização de CPU no .NET Core](debug-highcpu.md)
