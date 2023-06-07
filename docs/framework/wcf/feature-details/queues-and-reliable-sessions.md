---
title: Sessões confiáveis e filas
ms.date: 03/30/2017
ms.assetid: 7e794d03-141c-45ed-b6b1-6c0e104c1464
ms.openlocfilehash: af45fd86f673d0cc296f6593d9d5709d3e2b616e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596741"
---
# <a name="queues-and-reliable-sessions"></a>Sessões confiáveis e filas
As filas e as sessões confiáveis são os recursos de Windows Communication Foundation (WCF) que implementam mensagens confiáveis. Os tópicos contidos nesta seção discutem os recursos de mensagens confiáveis do WCF.  
  
 O sistema de mensagens confiável é como uma fonte de mensagens confiável (chamada de origem) transfere mensagens de forma confiável para um destino de mensagem confiável (chamado de destino).  
  
 O sistema de mensagens confiável tem os seguintes aspectos principais:  
  
- Transferir garantias para mensagens enviadas de uma origem para um destino, independentemente da falha de transferência de mensagem ou falhas de transporte.  
  
- Separação da origem e do destino umas das outras, o que fornece falha independente e recuperação da origem e do destino, bem como transferência confiável e entrega de mensagens, mesmo que a origem ou o destino não estejam disponíveis.  
  
 As mensagens confiáveis frequentemente são o custo de alta latência. Latência é o tempo que leva para a mensagem alcançar o destino da origem. O WCF, portanto, fornece os seguintes tipos de mensagens confiáveis:  
  
- [Sessões confiáveis](reliable-sessions.md), que oferecem transferência confiável sem o custo de alta latência  
  
- [Filas no WCF](queues-in-wcf.md), que oferecem transferências confiáveis e separação entre a origem e o destino.  
  
## <a name="reliable-sessions"></a>Sessões confiáveis  
 As sessões confiáveis fornecem transferência de mensagens confiável de ponta a ponta entre uma origem e um destino usando o protocolo WS-ReliableMessaging, independentemente do número ou tipo de intermediários que separam os pontos de extremidade de mensagens (origem e destino). Isso inclui os intermediários de transporte que não usam SOAP (por exemplo, proxies HTTP) ou intermediários que usam SOAP (por exemplo, roteadores ou pontes baseados em SOAP) que são necessários para que as mensagens fluam entre os pontos de extremidade. As sessões confiáveis usam uma janela de transferência na memória para mascarar falhas de nível de mensagem SOAP e restabelecer conexões no caso de falhas de transporte.  
  
 As sessões confiáveis fornecem transferências de mensagens confiáveis de baixa latência. Eles fornecem mensagens SOAP sobre quaisquer proxies ou intermediários, equivalentes ao que o TCP fornece para pacotes em pontes IP. Para obter mais informações sobre sessões confiáveis, consulte [sessões confiáveis](reliable-sessions.md).  
  
## <a name="queues"></a>Filas  
 As filas no WCF fornecem transferências confiáveis de mensagens e separação entre origens e destinos ao custo de alta latência. A comunicação na fila do WCF é criada com base no enfileiramento de mensagens (também conhecido como MSMQ).  
  
 O MSMQ é fornecido como uma opção com o Windows que é executado como um serviço NT. Ele captura mensagens para transmissão em uma fila de transmissão em nome da origem e a entrega a uma fila de destino. A fila de destino aceita mensagens em nome do destino para entrega posterior sempre que o destino solicita mensagens. Os gerenciadores de filas do MSMQ implementam um protocolo de transferência de mensagens confiável para que as mensagens não sejam perdidas na transmissão. O protocolo pode ser baseado em SOAP ou nativo, como o protocolo SRMP (SOAP Reliable Messaging Protocol).  
  
 A separação, combinada com transferências de mensagens confiáveis entre filas, permite que os aplicativos que são livremente acoplados se comuniquem de forma confiável. Ao contrário de sessões confiáveis, a origem e o destino não precisam estar em execução ao mesmo tempo. Isso habilita implicitamente cenários em que as filas são, em vigor, usadas como um mecanismo de nivelamento de carga quando há uma incompatibilidade entre a taxa de produção de mensagem pela origem e a taxa de consumo de mensagem pelo destino. Para obter mais informações sobre filas, consulte [filas no WCF](queues-in-wcf.md).  
  
## <a name="see-also"></a>Consulte também

- [Filas no WCF](queues-in-wcf.md)
- [Enfileiramento no WCF](queuing-in-wcf.md)
- [Sessões confiáveis](reliable-sessions.md)
- [Visão geral de sessões confiáveis](reliable-sessions-overview.md)
