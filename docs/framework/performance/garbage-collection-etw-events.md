---
title: Eventos ETW de coleta de lixo
description: Exiba informações detalhadas sobre eventos ETW de coleta de lixo. Os eventos cobertos incluem GCStart_V1, GCEnd_V1, GCHeapStats_V1, GCCreateSegment_V1 e muito mais.
ms.date: 03/30/2017
helpviewer_keywords:
- GC events
- garbage collection events [.NET Framework]
- ETW, garbage collection events (CLR)
ms.assetid: f14b6fd7-0966-4d87-bc89-54ef3a44a94a
ms.openlocfilehash: 58ad874ef6a12c18c404640aa66577c391573534
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309736"
---
# <a name="garbage-collection-etw-events"></a>Eventos ETW de coleta de lixo

 Esses eventos coletam informações referentes à coleta de lixo. Eles ajudam no diagnóstico e na depuração, inclusive determinando quantas vezes a coleta de lixo foi executada, a quantidade de memória liberada durante a coleta de lixo e assim por diante.

Esta categoria consiste nos seguintes eventos:

- [Evento GCStart_V1](#gcstart_v1-event)
- [Evento GCEnd_V1](#gcend_v1-event)
- [Evento GCHeapStats_V1](#gcheapstats_v1-event)
- [Evento GCCreateSegment_V1](#gccreatesegment_v1-event)
- [Evento GCFreeSegment_V1](#gcfreesegment_v1-event)
- [Evento GCRestartEEBegin_V1](#gcrestarteebegin_v1-event)
- [Evento GCRestartEEEnd_V1](#gcrestarteeend_v1-event)
- [Evento GCSuspendEE_V1](#gcsuspendee_v1-event)
- [Evento GCSuspendEEEnd_V1](#gcsuspendeeend_v1-event)
- [Evento GCAllocationTick_V2](#gcallocationtick_v2-event)
- [Evento GCFinalizersBegin_V1](#gcfinalizersbegin_v1-event)
- [Evento GCFinalizersEnd_V1](#gcfinalizersend_v1-event)
- [Evento GCCreateConcurrentThread_V1](#gccreateconcurrentthread_v1-event)
- [Evento GCTerminateConcurrentThread_V1](#gcterminateconcurrentthread_v1-event)

## <a name="gcstart_v1-event"></a>Evento GCStart_V1  

A tabela a seguir mostra a palavra-chave e o nível. Para obter mais informações, consulte [palavras-chave e níveis do ETW do CLR](clr-etw-keywords-and-levels.md).

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCStart_V1`|1|Uma coleta de lixo foi iniciada.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|Contagem|win:UInt32|A *n*° de coleta de lixo.|
|Profundidade|win:UInt32|A geração que está sendo coletada.|
|Motivo|win:UInt32|Motivo do gatilho da coleta de lixo:<br /><br /> 0x0 – Alocação de heap de objetos pequenos.<br /><br /> 0x1 – Induzido.<br /><br /> 0x2 – Memória insuficiente.<br /><br /> 0x3 – Vazio.<br /><br /> 0x4 – Alocação de heap de objetos grandes.<br /><br /> 0x5 – Espaço insuficiente (para heap de objetos pequenos).<br /><br /> 0x6 – Espaço insuficiente (para heap de objetos grandes).<br /><br /> 0x7 – Induzido, mas não forçado como bloqueio.|
|Tipo|win:UInt32|0x0 – A coleta de lixo de bloqueio ocorreu fora da coleta de lixo em segundo plano.<br /><br /> 0x1 – Coleta de lixo em segundo plano.<br /><br /> 0x2 – A coleta de lixo de bloqueio ocorreu durante a coleta de lixo em segundo plano.|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

## <a name="gcend_v1-event"></a>Evento GCEnd_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCEnd_V1`|2|A coleta de lixo foi encerrada.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|Contagem|win:UInt32|A *n*° de coleta de lixo.|
|Profundidade|win:UInt32|A geração que foi coletada.|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

## <a name="gcheapstats_v1-event"></a>Evento GCHeapStats_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Descrição|
|-----------|--------------|-----------------|
|`GCHeapStats_V1`|4|Mostra as estatísticas de heap no final de cada coleta de lixo.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|GenerationSize0|win:UInt64|O tamanho, em bytes, da memória de geração 0.|
|TotalPromotedSize0|win:UInt64|O número de bytes que são promovidos da geração 0 para a geração 1.|
|GenerationSize1|win:UInt64|O tamanho, em bytes, da memória de geração 1.|
|TotalPromotedSize1|win:UInt64|O número de bytes que são promovidos da geração 1 para a geração 2.|
|GenerationSize2|win:UInt64|O tamanho, em bytes, da memória de geração 2.|
|TotalPromotedSize2|win:UInt64|O número de bytes que sobreviveram na geração 2 após a última coleta.|
|GenerationSize3|win:UInt64|O tamanho, em bytes, do heap de objetos grandes.|
|TotalPromotedSize3|win:UInt64|O número de bytes que sobreviveram no heap de objetos grandes após a última coleta.|
|FinalizationPromotedSize|win:UInt64|O tamanho total, em bytes, dos objetos que estão prontos para finalização.|
|FinalizationPromotedCount|win:UInt64|O número de objetos que estão prontos para finalização.|
|PinnedObjectCount|win:UInt32|O número de objetos (imóveis) fixados.|
|SinkBlockCount|win:UInt32|O número de blocos de sincronização em uso.|
|GCHandleCount|win:UInt32|O número de manipuladores de coleta de lixo em uso.|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|
  
## <a name="gccreatesegment_v1-event"></a>Evento GCCreateSegment_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCCreateSegment_V1`|5|Um novo segmento de coleta de lixo foi criado. Além disso, quando o rastreamento é habilitado em um processo que já está em execução, esse evento é acionado para cada segmento existente.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|Endereço|win:UInt64|O endereço do segmento.|
|Tamanho|win:UInt64|O tamanho do segmento.|
|Tipo|win:UInt32|0x0 – Heap de objetos pequenos.<br /><br /> 0x1 – Heap de objetos grandes.<br /><br /> 0x2 – Heap somente leitura.|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

Observe que o tamanho de segmentos alocados pelo coletor de lixo é específico à implementação e está sujeito a alterações a qualquer momento, incluindo em atualizações periódicas. Seu aplicativo nunca deve fazer suposições sobre o tamanho de um segmento em particular nem depender dele, tampouco deve tentar configurar a quantidade de memória disponível para alocações de segmento.

## <a name="gcfreesegment_v1-event"></a>Evento GCFreeSegment_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCFreeSegment_V1`|6|Um segmento de coleta de lixo foi liberado.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|Endereço|win:UInt64|O endereço do segmento.|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

## <a name="gcrestarteebegin_v1-event"></a>Evento GCRestartEEBegin_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCRestartEEBegin_V1`|7|A continuidade da suspensão do Common Language Runtime foi iniciada.|

Nenhum dado do evento.

## <a name="gcrestarteeend_v1-event"></a>Evento GCRestartEEEnd_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCRestartEEEnd_V1`|3|A continuidade da suspensão do Common Language Runtime foi encerrada.|

Nenhum dado do evento.

## <a name="gcsuspendee_v1-event"></a>Evento GCSuspendEE_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCSuspendEE_V1`|9|Início da suspensão do mecanismo de execução da coleta de lixo.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|Motivo|win:UInt16|0x0 – Outros.<br /><br /> 0x1 – Coleta de lixo.<br /><br /> 0x2 – Desligamento do domínio do aplicativo.<br /><br /> 0x3 – Densidade do código.<br /><br /> 0x4 – Desligamento.<br /><br /> 0x5 – Depurador.<br /><br /> 0x6 – Preparação para a coleta de lixo.|
|Contagem|win:UInt32|A contagem de GC no momento. Normalmente, você verá um evento de Início de GC posterior depois disso e sua Contagem será essa Contagem + 1, conforme aumentamos o Índice de GC durante uma coleta de lixo.|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

## <a name="gcsuspendeeend_v1-event"></a>Evento GCSuspendEEEnd_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCSuspendEEEnd_V1`|8|Fim da suspensão do mecanismo de execução da coleta de lixo.|

Nenhum dado do evento.

## <a name="gcallocationtick_v2-event"></a>Evento GCAllocationTick_V2

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCAllocationTick_V2`|10|A cada vez, aproximadamente 100 KB são alocados.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|AllocationAmount|win:UInt32|O tamanho de alocação, em bytes. Esse valor é preciso para alocações menores do que o tamanho de um ULONG (4.294.967.295 bytes). Se a alocação for maior, esse campo conterá um valor truncado. Use `AllocationAmount64` para alocações muito grandes.|
|AllocationKind|win:UInt32|0x0 – Alocação de objeto pequeno (a alocação está no heap de objetos pequenos).<br /><br /> 0x1 – Alocação de objeto grande (a alocação está no heap de objetos grandes).|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|
|AllocationAmount64|win:UInt64|O tamanho de alocação, em bytes. Esse valor é preciso para alocações muito grandes.|
|TypeId|win:Pointer|O endereço da MethodTable. Quando há vários tipos de objetos que foram alocados durante esse evento, esse é o endereço da MethodTable que corresponde ao último objeto alocado (o objeto que fez com que o limite de 100 KB fosse excedido).|
|TypeName|win:UnicodeString|O nome do tipo que foi alocado. Quando há vários tipos de objetos que foram alocados durante esse evento, esse é o tipo do último objeto alocado (o objeto que fez com que o limite de 100 KB fosse excedido).|
|HeapIndex|win:UInt32|O heap em que o objeto foi alocado. Esse valor é 0 (zero) durante a execução da coleta de lixo da estação de trabalho.|

## <a name="gcfinalizersbegin_v1-event"></a>Evento GCFinalizersBegin_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCFinalizersBegin_V1`|14|O início da execução dos finalizadores.|

Nenhum dado do evento.

## <a name="gcfinalizersend_v1-event"></a>Evento GCFinalizersEnd_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCFinalizersEnd_V1`|13|O fim da execução dos finalizadores.|

A seguinte tabela mostra os dados do evento:

|Nome do campo|Tipo de dados|Descrição|
|----------------|---------------|-----------------|
|Contagem|win:UInt32|O número de finalizadores que foram executados.|
|ClrInstanceID|win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|

## <a name="gccreateconcurrentthread_v1-event"></a>Evento GCCreateConcurrentThread_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|
|`ThreadingKeyword` (0x10000)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCCreateConcurrentThread_V1`|11|O thread da coleta de lixo simultânea foi criado.|

Nenhum dado do evento.

## <a name="gcterminateconcurrentthread_v1-event"></a>Evento GCTerminateConcurrentThread_V1

A seguinte tabela mostra a palavra-chave e o nível:

|Palavra-chave para acionar o evento|Level|
|-----------------------------------|-----------|
|`GCKeyword` (0x1)|Informativo (4)|
|`ThreadingKeyword` (0x10000)|Informativo (4)|

A seguinte tabela mostra as informações do evento:

|Evento|ID do evento|Acionado quando|
|-----------|--------------|-----------------|
|`GCTerminateConcurrentThread_V1`|12|O thread da coleta de lixo simultânea foi terminado.|

Nenhum dado do evento.

## <a name="see-also"></a>Confira também

- [Eventos ETW no CLR](clr-etw-events.md)
