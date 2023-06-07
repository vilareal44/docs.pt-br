---
title: dotnet-ferramenta de rastreamento-.NET Core
description: Instalando e usando a ferramenta de linha de comando dotnet-Trace.
ms.date: 11/21/2019
ms.openlocfilehash: 25178a0e59ce9edb69d15ee761c1b9e56aa5eb3a
ms.sourcegitcommit: b4f8849c47c1a7145eb26ce68bc9f9976e0dbec3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517302"
---
# <a name="dotnet-trace-performance-analysis-utility"></a>dotnet-utilitário de análise de desempenho de rastreamento

**Este artigo aplica-se a:** ✔️ SDK do .net Core 3,0 e versões posteriores

## <a name="install-dotnet-trace"></a>Instalar dotnet-Trace

Instale `dotnet-trace` o [pacote NuGet](https://www.nuget.org/packages/dotnet-trace) com o comando de [instalação da ferramenta dotnet](../tools/dotnet-tool-install.md) :

```dotnetcli
dotnet tool install --global dotnet-trace
```

## <a name="synopsis"></a>Sinopse

```console
dotnet-trace [-h, --help] [--version] <command>
```

## <a name="description"></a>Descrição

A `dotnet-trace` ferramenta:

* É uma ferramenta .NET Core de plataforma cruzada.
* Habilita a coleção de rastreamentos do .NET Core de um processo em execução sem um criador de perfil nativo.
* O é criado em relação à tecnologia de plataforma cruzada `EventPipe` do tempo de execução do .NET Core.
* Oferece a mesma experiência no Windows, Linux ou macOS.

## <a name="options"></a>Opções

- **`-h|--help`**

  Mostra a ajuda da linha de comando.

- **`--version`**

  Exibe a versão do utilitário dotnet-Trace.

## <a name="commands"></a>Comandos

| Comando                                                   |
|-----------------------------------------------------------|
| [dotnet-coleta de rastreamento](#dotnet-trace-collect)             |
| [dotnet-conversão de rastreamento](#dotnet-trace-convert)             |
| [dotnet-rastrear PS](#dotnet-trace-ps)                       |
| [dotnet-lista de rastreamento-perfis](#dotnet-trace-list-profiles) |

## <a name="dotnet-trace-collect"></a>dotnet-coleta de rastreamento

Coleta um rastreamento de diagnóstico de um processo em execução.

### <a name="synopsis"></a>Sinopse

```console
dotnet-trace collect [--buffersize <size>] [--clreventlevel <clreventlevel>] [--clrevents <clrevents>]
    [--format <Chromium|NetTrace|Speedscope>] [-h|--help]
    [-n, --name <name>]  [-o|--output <trace-file-path>] [-p|--process-id <pid>]
    [--profile <profile-name>] [--providers <list-of-comma-separated-providers>]
```

### <a name="options"></a>Opções

- **`--buffersize <size>`**

  Define o tamanho do buffer circular na memória, em megabytes. 256 MB padrão.

- **`--clreventlevel <clreventlevel>`**

  Detalhes dos eventos CLR a serem emitidos.

- **`--clrevents <clrevents>`**

  Lista de eventos de tempo de execução CLR a serem emitidos.

- **`--format {Chromium|NetTrace|Speedscope}`**

  Define o formato de saída para a conversão do arquivo de rastreamento. O padrão é `NetTrace`.

- **`-n, --name <name>`**

  O nome do processo do qual coletar o rastreamento.

- **`-o|--output <trace-file-path>`**

  O caminho de saída para os dados de rastreamento coletados. Se não for especificado, o padrão será `trace.nettrace` .

- **`-p|--process-id <PID>`**

  A ID do processo da qual coletar o rastreamento.

- **`--profile <profile-name>`**

  Um conjunto nomeado predefinido de configurações de provedor que permite que cenários de rastreamento comuns sejam especificados de forma sucinta.

- **`--providers <list-of-comma-separated-providers>`**

  Uma lista separada por vírgulas de `EventPipe` provedores a serem habilitados. Esses provedores complementam todos os provedores implícitos pelo `--profile <profile-name>` . Se houver alguma inconsistência para um provedor específico, essa configuração terá precedência sobre a configuração implícita do perfil.

  Esta lista de provedores está no formato:

  - `Provider[,Provider]`
  - `Provider`está no formato: `KnownProviderName[:Flags[:Level][:KeyValueArgs]]` .
  - `KeyValueArgs`está no formato: `[key1=value1][;key2=value2]` .

## <a name="dotnet-trace-convert"></a>dotnet-conversão de rastreamento

Converte `nettrace` rastreamentos em formatos alternativos para uso com ferramentas de análise de rastreamento alternativas.

### <a name="synopsis"></a>Sinopse

```console
dotnet-trace convert [<input-filename>] [--format <Chromium|NetTrace|Speedscope>] [-h|--help] [-o|--output <output-filename>]
```

### <a name="arguments"></a>Argumentos

- **`<input-filename>`**

  Arquivo de rastreamento de entrada a ser convertido. O padrão é *trace. NetTrace*.

### <a name="options"></a>Opções

- **`--format <Chromium|NetTrace|Speedscope>`**

  Define o formato de saída para a conversão do arquivo de rastreamento.

- **`-o|--output <output-filename>`**

  Nome de arquivo de saída. A extensão do formato de destino será adicionada.

## <a name="dotnet-trace-ps"></a>dotnet-rastrear PS

 Lista os processos dotnet dos quais os rastreamentos podem ser coletados.

### <a name="synopsis"></a>Sinopse

```console
dotnet-trace ps [-h|--help]
```

## <a name="dotnet-trace-list-profiles"></a>dotnet-lista de rastreamento-perfis

Lista os perfis de rastreamento pré-criados com uma descrição de quais provedores e filtros estão em cada perfil.

### <a name="synopsis"></a>Sinopse

```console
dotnet-trace list-profiles [-h|--help]
```

## <a name="collect-a-trace-with-dotnet-trace"></a>Coletar um rastreamento com dotnet-Trace

Para coletar rastreamentos usando `dotnet-trace` :

- Obtenha o identificador do processo (PID) do aplicativo .NET Core do qual coletar rastreamentos.

  - No Windows, você pode usar o Gerenciador de tarefas ou o `tasklist` comando, por exemplo.
  - No Linux, por exemplo, o `ps` comando.
  - [dotnet-rastrear PS](#dotnet-trace-ps)

- Execute o comando a seguir:

  ```console
  dotnet-trace collect --process-id <PID>
  ```

  O comando anterior gera uma saída semelhante à seguinte:

  ```console
  Press <Enter> to exit...
  Connecting to process: <Full-Path-To-Process-Being-Profiled>/dotnet.exe
  Collecting to file: <Full-Path-To-Trace>/trace.nettrace
  Session Id: <SessionId>
  Recording trace 721.025 (KB)
  ```

- Pare a coleta pressionando a `<Enter>` tecla. `dotnet-trace`encerrará eventos de log no arquivo *trace. NetTrace* .

## <a name="view-the-trace-captured-from-dotnet-trace"></a>Exibir o rastreamento capturado de dotnet-Trace

No Windows, os arquivos *. NetTrace* podem ser exibidos em [Perfview](https://github.com/microsoft/perfview) para análise: para rastreamentos coletados em outras plataformas, o arquivo de rastreamento pode ser movido para um computador Windows para ser exibido em Perfview.

No Linux, o rastreamento pode ser exibido alterando o formato de saída de `dotnet-trace` para `speedscope` . O formato do arquivo de saída pode ser alterado usando a `-f|--format` opção- `-f speedscope` fará com que o `dotnet-trace` produza um `speedscope` arquivo. Você pode escolher entre `nettrace` (a opção padrão) e `speedscope` . `Speedscope`os arquivos podem ser abertos em <https://www.speedscope.app> .

> [!NOTE]
> O tempo de execução do .NET Core gera rastreamentos no `nettrace` formato. Os rastreamentos são convertidos em speedscope (se especificado) após a conclusão do rastreamento. Como algumas conversões podem resultar em perda de dados, o arquivo original `nettrace` é preservado ao lado do arquivo convertido.

## <a name="use-dotnet-trace-to-collect-counter-values-over-time"></a>Usar o rastreamento dotnet para coletar valores de contador ao longo do tempo

`dotnet-trace`podem

* Use `EventCounter` para o monitoramento de integridade básico em ambientes sensíveis ao desempenho. Por exemplo, em produção.
* Colete rastreamentos para que eles não precisem ser exibidos em tempo real.

Por exemplo, para coletar valores de contador de desempenho de tempo de execução, use o seguinte comando:

```console
dotnet-trace collect --process-id <PID> --providers System.Runtime:0:1:EventCounterIntervalSec=1
```

O comando anterior informa os contadores de tempo de execução para relatar uma vez a cada segundo para monitoramento de integridade leve. Substituir `EventCounterIntervalSec=1` por um valor mais alto (por exemplo, 60) permite a coleta de um rastreamento menor com menos granularidade nos dados do contador.

O comando a seguir reduz a sobrecarga e o tamanho do rastreamento mais do que o anterior:

```console
dotnet-trace collect --process-id <PID> --providers System.Runtime:0:1:EventCounterIntervalSec=1,Microsoft-Windows-DotNETRuntime:0:1,Microsoft-DotNETCore-SampleProfiler:0:1
```

O comando anterior desabilita os eventos de tempo de execução e o profiler de pilha gerenciado.

## <a name="net-providers"></a>Provedores .NET

O tempo de execução do .NET Core dá suporte aos provedores .NET a seguir. O .NET Core usa as mesmas palavras-chave para habilitar `Event Tracing for Windows (ETW)` os `EventPipe` rastreamentos e.

| Nome do provedor                            | Informações do |
|------------------------------------------|-------------|
| `Microsoft-Windows-DotNETRuntime`        | [O provedor de runtime](../../framework/performance/clr-etw-providers.md#the-runtime-provider)<br>[Palavras-chave de tempo de execução CLR](../../framework/performance/clr-etw-keywords-and-levels.md#runtime) |
| `Microsoft-Windows-DotNETRuntimeRundown` | [O provedor de encerramento](../../framework/performance/clr-etw-providers.md#the-rundown-provider)<br>[Palavras-chave de resumo do CLR](../../framework/performance/clr-etw-keywords-and-levels.md#rundown) |
| `Microsoft-DotNETCore-SampleProfiler`    | Habilita o criador de perfil de exemplo. |
