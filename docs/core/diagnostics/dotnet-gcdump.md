---
title: dotnet-gcdump-.NET Core
description: Instalando e usando a ferramenta de linha de comando dotnet-gcdump.
ms.date: 07/26/2020
ms.openlocfilehash: a7b19f4d7487677b975621e7267a17894dae2e3a
ms.sourcegitcommit: c4a15c6c4ecbb8a46ad4e67d9b3ab9b8b031d849
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88656645"
---
# <a name="heap-analysis-tool-dotnet-gcdump"></a>Ferramenta de análise de heap (dotNet-gcdump)

**Este artigo aplica-se a:** ✔️ SDK do .net Core 3,1 e versões posteriores

## <a name="install-dotnet-gcdump"></a>Instalar dotnet-gcdump

Para instalar a versão de lançamento mais recente do `dotnet-gcdump` [pacote NuGet](https://www.nuget.org/packages/dotnet-gcdump), use o comando de [instalação da ferramenta dotnet](../tools/dotnet-tool-install.md) :

```dotnetcli
dotnet tool install -g dotnet-gcdump
```

## <a name="synopsis"></a>Sinopse

```console
dotnet-gcdump [-h|--help] [--version] <command>
```

## <a name="description"></a>Descrição

A `dotnet-gcdump` ferramenta global é uma maneira de coletar despejos de GC (coletor de lixo) de processos do .net em tempo real. Ele usa a tecnologia EventPipe, que é uma alternativa de plataforma cruzada para o ETW no Windows. Os despejos de GC são criados disparando um GC no processo de destino, ativando eventos especiais e regenerando o grafo de raízes de objeto a partir do fluxo de eventos. Esse processo permite que os despejos de GC sejam coletados enquanto o processo está em execução e com sobrecarga mínima. Esses despejos são úteis para vários cenários:

- Comparando o número de objetos no heap em vários pontos no tempo.
- Análise de raízes de objetos (respondendo a perguntas como "o que ainda tem uma referência a esse tipo?").
- Coletando estatísticas gerais sobre as contagens de objetos no heap.

### <a name="view-the-gc-dump-captured-from-dotnet-gcdump"></a>Exibir o despejo de GC capturado de dotnet-gcdump

No Windows, `.gcdump` os arquivos podem ser exibidos no [Perfview](https://github.com/microsoft/perfview) para análise ou no Visual Studio. Atualmente, não há nenhuma maneira de abrir um `.gcdump` em plataformas não Windows.

Você pode coletar vários `.gcdump` s e abri-los simultaneamente no Visual Studio para obter uma experiência de comparação.

## <a name="options"></a>Opções

- **`--version`**

  Exibe a versão do `dotnet-gcdump` Utilitário do.

- **`-h|--help`**

  Mostra a ajuda da linha de comando.

## `dotnet-gcdump collect`

Coleta um despejo de GC de um processo em execução no momento.

### <a name="synopsis"></a>Sinopse

```console
dotnet-gcdump collect [-h|--help] [-p|--process-id <pid>] [-o|--output <gcdump-file-path>] [-v|--verbose] [-t|--timeout <timeout>] [-n|--name <name>]
```

### <a name="options"></a>Opções

- **`-h|--help`**

  Mostra a ajuda da linha de comando.

- **`-p|--process-id <pid>`**

  A ID do processo para coletar o despejo do GC.

- **`-o|--output <gcdump-file-path>`**

  O caminho onde os despejos de GC coletados devem ser gravados. O padrão é *. \\ AAAAMMDD \_ HHMMSS \_ \<pid> . gcdump*.

- **`-v|--verbose`**

  Gere o log durante a coleta do despejo do GC.

- **`-t|--timeout <timeout>`**

  Desistir da coleta do despejo do GC se levar mais tempo do que esse número de segundos. O valor padrão é 30.

- **`-n|--name <name>`**

  O nome do processo do qual coletar o despejo do GC.

## `dotnet-gcdump ps`

Lista os processos dotnet para os quais os despejos de GC podem ser coletados.

### <a name="synopsis"></a>Sinopse

```console
dotnet-gcdump ps
```

## `dotnet-gcdump report <gcdump_filename>`

Gere um relatório de um despejo GC gerado anteriormente ou de um processo em execução e grave no `stdout` .

### <a name="synopsis"></a>Sinopse

```console
dotnet-gcdump report [-h|--help] [-p|--process-id <pid>] [-t|--report-type <HeapStat>]
```

### <a name="options"></a>Opções

- **`-h|--help`**

  Mostra a ajuda da linha de comando.

- **`-p|--process-id <pid>`**

  A ID do processo para coletar o despejo do GC.

- **`-t|--report-type <HeapStat>`**

  O tipo de relatório a ser gerado. Opções disponíveis: heapstat (padrão).

## <a name="troubleshoot"></a>Solucionar problemas

- Não há nenhuma informação de tipo no gcdump.

   Antes do .NET Core 3,1, houve um problema em que um cache de tipo não foi limpo entre gcdumps quando eles eram invocados com EventPipe. Isso resultou nos eventos necessários para determinar as informações de tipo que não estão sendo enviadas para o segundo e o gcdumps subsequente. Isso foi corrigido no .NET Core 3,1-Preview2.

- Os tipos COM e estáticos não estão no despejo de GC.

   Antes do .NET Core 3,1-Preview2, houve um problema em que os tipos static e COM não eram enviados quando o despejo de GC era invocado via EventPipe. Isso foi corrigido no .NET Core 3,1-Preview2.
