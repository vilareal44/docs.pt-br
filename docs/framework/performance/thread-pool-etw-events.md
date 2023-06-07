---
title: Eventos ETW de pool de threads
description: Examine os eventos ETW do pool de threads, que coletam informações sobre threads no .NET. Eventos de pool de threads são eventos de pool de threads de trabalho ou eventos de pool de threads de e/s
ms.date: 03/30/2017
helpviewer_keywords:
- thread pool events [.NET Framework]
- ETW, thread pool events (CLR)
ms.assetid: f2a21e3a-3b6c-4433-97f3-47ff16855ecc
ms.openlocfilehash: d3059cec5007c24d41a4a779939d4990f19305ca
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475197"
---
# <a name="thread-pool-etw-events"></a>Eventos ETW de pool de threads
 Esses eventos coletam informações sobre a função de trabalho e os threads de E/S.  
  
 Há dois grupos de eventos de pool de threads:  
  
- [Eventos de pool de threads de trabalho](#worker-thread-pool-events), que fornecem informações sobre como um aplicativo usa o pool de threads e o efeito das cargas de trabalho no controle de simultaneidade.  
  
- [Eventos de pool de threads de E/S](#io-thread-events), que fornecem informações sobre os threads de E/S que são criados, desativados, ativados novamente ou terminados no pool de threads.  

## <a name="worker-thread-pool-events"></a>Eventos de pool de threads de trabalho
 Esses eventos se relacionam ao pool de threads de trabalho do runtime e fornecem notificações de eventos de thread (por exemplo, quando um thread é criado ou interrompido). O pool de threads de trabalho usa um algoritmo adaptável para o controle de simultaneidade, em que o número de threads é calculado com base na taxa de transferência medida. Os eventos de pool de threads de trabalho podem ser usados para entender como um aplicativo está usando o pool de threads, bem como o efeito que determinadas cargas de trabalho podem ter no controle de simultaneidade.  
  
### <a name="threadpoolworkerthreadstart-and-threadpoolworkerthreadstop"></a>ThreadPoolWorkerThreadStart e ThreadPoolWorkerThreadStop  
 A tabela a seguir mostra a palavra-chave e o nível desses eventos. (Para obter mais informações, consulte [Palavras-chaves e níveis CLR ETW](clr-etw-keywords-and-levels.md).)  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Acionado quando|  
|-|-|-|  
|`ThreadPoolWorkerThreadStart`|50|Um thread de trabalho é criado.|  
|`ThreadPoolWorkerThreadStop`|51|Um thread de trabalho é interrompido.|  
|`ThreadPoolWorkerThreadRetirementStart`|52|Um thread de trabalho é desativado.|  
|`ThreadPoolWorkerThreadRetirementStop`|53|Um thread de trabalho desativado fica ativo novamente.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|ActiveWorkerThreadCount|win:UInt32|Número de threads de trabalho disponíveis para processar o trabalho, incluindo aqueles que já estão processando o trabalho.|  
|RetiredWorkerThreadCount|win:UInt32|Número de threads de trabalho que não estão disponíveis para processar o trabalho, mas que estão sendo mantidos em reserva, caso mais threads sejam necessários posteriormente.|  
|ClrInstanceID|Win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
  
### <a name="threadpoolworkerthreadadjustment"></a>ThreadPoolWorkerThreadAdjustment  
 Esses eventos de pool de threads fornecem informações para entender e depurar o comportamento do algoritmo de injeção de threads (controle de simultaneidade). As informações são usadas internamente pelo pool de threads de trabalho.  
  
#### <a name="threadpoolworkerthreadadjustmentsample"></a>ThreadPoolWorkerThreadAdjustmentSample  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Descrição|  
|-----------|--------------|-----------------|  
|`ThreadPoolWorkerThreadAdjustmentSample`|54|Refere-se à coleta de informações para uma amostra; ou seja, uma medida da taxa de transferência com determinado nível de simultaneidade, em um instante.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|Taxa de transferência|win:Double|Número de conclusões por unidade de tempo.|  
|ClrInstanceID|Win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
  
#### <a name="threadpoolworkerthreadadjustmentadjustment"></a>ThreadPoolWorkerThreadAdjustmentAdjustment  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Descrição|  
|-----------|--------------|-----------------|  
|`ThreadPoolWorkerThreadAdjustmentAdjustment`|55|Registra uma alteração no controle, quando o algoritmo de injeção de threads (escalada) determina se uma alteração no nível de simultaneidade está em vigor.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|AverageThroughput|win:Double|Taxa de transferência média de uma amostra de medidas.|  
|NewWorkerThreadCount|win:UInt32|Novo número de threads de trabalho ativos.|  
|Motivo|win:UInt32|Motivo do ajuste.<br /><br /> 0x00 – Aquecimento.<br /><br /> 0x01 – Inicialização.<br /><br /> 0x02 – Movimentação aleatória.<br /><br /> 0x03 – Movimentação ascendente.<br /><br /> 0x04 – Alteração de ponto.<br /><br /> 0x05 – Estabilização.<br /><br /> 0x06 – Privação.<br /><br /> 0x07 – O thread atingiu o tempo limite.|  
|ClrInstanceID|Win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
  
#### <a name="threadpoolworkerthreadadjustmentstats"></a>ThreadPoolWorkerThreadAdjustmentStats  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Descrição|  
|-----------|--------------|-----------------|  
|`ThreadPoolWorkerThreadAdjustmentStats`|56|Coleta de dados no pool de threads.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|Duration|win:Double|Tempo, em segundos, durante o qual essas estatísticas foram coletadas.|  
|Taxa de transferência|win:Double|Número médio de conclusões por segundo durante esse intervalo.|  
|ThreadWave|win:Double|Reservado para uso interno.|  
|ThroughputWave|win:Double|Reservado para uso interno.|  
|ThroughputErrorEstimate|win:Double|Reservado para uso interno.|  
|AverageThroughputErrorEstimate|win:Double|Reservado para uso interno.|  
|ThroughputRatio|win:Double|A melhoria relativa na taxa de transferência causada por variações na contagem de threads de trabalho ativos durante esse intervalo.|  
|Confiança|win:Double|Uma medida da validade do campo ThroughputRatio.|  
|NewcontrolSetting|win:Double|O número de threads de trabalho ativos que servirão como a linha de base para variações futuras na contagem de threads ativos.|  
|NewThreadWaveMagnitude|Win:UInt16|A magnitude das variações futuras na contagem de threads ativos.|  
|ClrInstanceID|Win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  

## <a name="io-thread-events"></a>Eventos de threads de E/S  
 Esses eventos de pool de threads ocorrem em threads do pool de threads de E/S (portas de conclusão), que é assíncrono.  
  
### <a name="iothreadcreate_v1"></a>IOThreadCreate_V1  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Acionado quando|  
|-|-|-|  
|`IOThreadCreate_V1`|44|Um thread de E/S é criado no pool de threads.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|Contagem|win:UInt64|Número de threads de E/S, incluindo o thread recém-criado.|  
|NumRetired|win:UInt64|Número de threads de trabalho desativados.|  
|ClrInstanceID|Win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
  
### <a name="iothreadretire_v1"></a>IOThreadRetire_V1  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Acionado quando|  
|-----------|--------------|-----------------|  
|`IOThreadRetire_V1`|46|Um thread de E/S se torna um candidato para desativação.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|Contagem|win:UInt64|Número de threads de E/S restantes no pool de threads.|  
|NumRetired|win:UInt64|Número de threads de E/S desativados.|  
|ClrInstanceID|Win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
  
### <a name="iothreadunretire_v1"></a>IOThreadUnretire_V1  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Acionado quando|  
|-----------|--------------|-----------------|  
|`IOThreadUnretire_V1`|47|Um thread de E/S é ativado novamente devido à E/S que chega em um período de espera depois que o thread se torna um candidato para desativação.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|Contagem|win:UInt64|Número de threads de E/S no pool de threads, incluindo esse.|  
|NumRetired|win:UInt64|Número de threads de E/S desativados.|  
|ClrInstanceID|Win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
  
### <a name="iothreadterminate"></a>IOThreadTerminate  
 A tabela a seguir mostra a palavra-chave e o nível.  
  
|Palavra-chave para acionar o evento|Nível|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|Informativo (4)|  
  
 A tabela a seguir mostra as informações do evento.  
  
|Evento|ID do evento|Acionado quando|  
|-----------|--------------|-----------------|  
|`IOThreadTerminate`|45|Um thread de e/s é encerrado no pool de threads.|  
  
 A tabela a seguir mostra os dados do evento.  
  
|Nome do campo|Tipo de dados|Descrição|  
|----------------|---------------|-----------------|  
|Contagem|win:UInt64|Número de threads de E/S restantes no pool de threads.|  
|NumRetired|win:UInt64|Número de threads de E/S desativados.|  
|ClrInstanceID|Win:UInt16|ID exclusiva da instância do CLR ou do CoreCLR.|  
  
## <a name="see-also"></a>Consulte também

- [Eventos ETW no CLR](clr-etw-events.md)
