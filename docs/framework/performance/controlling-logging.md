---
title: Controlando o registro em log no .NET Framework
description: Use o ETW (rastreamento de eventos para Windows) para controlar o log do .NET e registrar eventos de Common Language Runtime (CLR). Use ferramentas como logman, Tracerpt e Xperf.
ms.date: 03/30/2017
helpviewer_keywords:
- CLR ETW events, logging
ms.assetid: ce13088e-3095-4f0e-9f6b-fad30bbd3d41
ms.openlocfilehash: 45d9244eb11b914fd203f24057e1b65c6bef18c2
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309580"
---
# <a name="controlling-net-framework-logging"></a>Controlando o registro em log no .NET Framework

Você pode usar o ETW (Rastreamento de Eventos para Windows) para registrar eventos de CLR (Common Language Runtime). Você pode criar e exibir rastros usando as seguintes ferramentas:

- As ferramentas de linha de comando [Logman](/windows-server/administration/windows-commands/logman) e [Tracerpt](/windows-server/administration/windows-commands/tracerpt_1), ambas incluídas no sistema operacional Windows.

- As ferramentas [Xperf](/windows-hardware/test/wpt/xperf-command-line-reference) no [Windows Performance Toolkit](/windows-hardware/test/wpt/). Para obter mais informações sobre o Xperf, visite o [blog de Desempenho do Windows](https://docs.microsoft.com/archive/blogs/pigscanfly/).

Para capturar informações de eventos de CLR, o provedor de CLR deve estar instalado em seu computador. Para confirmar que o provedor está instalado, digite `logman query providers` no prompt de comando. Uma lista de provedores é exibida. Essa lista deve conter uma entrada para o provedor CLR, conforme mostrado a seguir.

```output
Provider                                 GUID
-------------------------------------------------------------------------------
.NET Common Language Runtime    {E13C0D23-CCBC-4E12-931B-D9CC2EEE27E4}.
```

Se o provedor CLR não estiver listado, você poderá instalá-lo no Windows Vista e em sistemas operacionais posteriores usando a ferramenta de linha de comando [Wevtutil](/windows-server/administration/windows-commands/wevtutil) do Windows. Abra uma janela de prompt de comando como administrador. Altere o diretório de prompts para a pasta .NET Framework 4 (%WINDIR%\Microsoft.NET\Framework [64] \v4. \<.NET version> \ ). Esta pasta contém o arquivo CLR-ETW.man. No prompt de comando, digite o seguinte comando para instalar o provedor de CLR:

`wevtutil im CLR-ETW.man`

## <a name="capturing-clr-etw-events"></a>Capturando eventos de CLR com o ETW

Use as ferramentas de linha de comando [Logman](/windows-server/administration/windows-commands/logman) e [Xperf](/windows-hardware/test/wpt/xperf-command-line-reference) para capturar eventos ETW e as ferramentas [Tracerpt](/windows-server/administration/windows-commands/tracerpt_1) e [Xperf](/windows-hardware/test/wpt/xperf-command-line-reference) para decodificar eventos de rastreamento.

Para ativar o log, um usuário deve especificar três coisas:

- O provedor com o qual a comunicação será feita.

- Um número de 64 bits que representa um conjunto de palavra-chave. Cada palavra-chave representa um conjunto de eventos que o provedor pode ativar. O número representa um conjunto combinado de palavras-chave para ativar.

- Um número pequeno representando o nível (detalhamento) em que o registro será feito. O nível 1 é o menos detalhado, enquanto que o nível 5 é o mais detalhado. O nível 0 é um padrão cujo significado é específico do provedor.

### <a name="to-capture-clr-etw-events-using-logman"></a>Para capturar eventos ETW de CLR usando o Logman

1. No prompt de comando, digite:

     `logman start clrevents -p {e13c0d23-ccbc-4e12-931b-d9cc2eee27e4} 0x1CCBD 0x5 -ets -ct perf`

     onde:

    - O parâmetro `-p` identifica o GUID do provedor.

    - `0x1CCBD` especifica as categorias de eventos que serão geradas.

    - `0x5` define o nível do log (nesse caso, detalhado (5)).

    - O parâmetro `-ets` instrui o Logman a enviar comandos para a seções de rastreamento de eventos.

    - O parâmetro `-ct perf` especifica que a função `QueryPerformanceCounter` será usada para registrar o carimbo de data/hora para cada evento.

2. Para parar de registrar os eventos, digite:

     `logman stop clrevents -ets`

     Este comando cria um arquivo binário de rastreamento chamado clrevents.etl.

### <a name="to-capture-clr-etw-events-using-xperf"></a>Para capturar eventos CLR ETW usando o Xperf

1. No prompt de comando, digite:

     `xperf -start clr -on e13c0d23-ccbc-4e12-931b-d9cc2eee27e4:0x1CCBD:5 -f clrevents.etl`

     Onde o GUID é o GUID do provedor ETW de CLR, e `0x1CCBD:5` rastreia tudo no nível 5 (detalhado) e nos níveis inferiores.

2. Para parar o rastreamento, digite:

     `Xperf -stop clr`

     Este comando cria um arquivo de rastreamento chamado clrevents.etl.

## <a name="viewing-clr-etw-events"></a>Exibindo eventos ETW de CLR

Use os comandos listados abaixo para exibir os eventos ETW de CLR. Para obter uma descrição dos eventos, consulte [Eventos CLR ETW](clr-etw-events.md).

### <a name="to-view-clr-etw-events-using-tracerpt"></a>Para exibir eventos ETW de CLR usando o Tracerpt

- No prompt de comando, digite:

     `tracerpt clrevents.etl`

     Este comando cria dois arquivos: dumpfile.xml e summary.txt. O arquivo dumpfile.xml lista todos os eventos, enquanto que summary.txt fornece um resumo dos eventos.

### <a name="to-view-clr-etw-events-using-xperf"></a>Para exibir eventos ETW de CLR usando Xperf

- No prompt de comando, digite:

     `xperf clrevents.etl`

     Este comando abre o visualizador de arquivos Xperf ETL. Nesse visualizador, os eventos CLR são mostrados na exibição **Eventos Genéricos**. Para exibir uma grade de dados de eventos categorizados por tipo, selecione uma região de tempo nessa exibição e, em seguida, clique com o botão direito do mouse e selecione **Resumo**.

### <a name="to-convert-the-etl-file-to-a-comma-separated-value-file"></a>Para converter o arquivo .etl em um arquivo de valores separados por vírgula.

- No prompt de comando, digite:

     `xperf -i clrevents.etl -f clrevents.csv`

     Este comando faz com que XPerf despeje os eventos na forma de um arquivo de valores separados por vírgula (CSV) que você pode abrir. Porque eventos diferentes possuem campos diferentes, esse arquivo CSV contém mais de uma linha de cabeçalho antes dos dados. O primeiro campo de cada linha é o tipo de evento, que indica qual cabeçalho deve ser usado para determinar o restante dos campos.

## <a name="see-also"></a>Confira também

- [Windows Performance Toolkit](/windows-hardware/test/wpt/)
- [Eventos ETW no Common Language Runtime](etw-events-in-the-common-language-runtime.md)
