---
title: System.ServiceModel.Channels.MsmqMessageRejected
ms.date: 03/30/2017
ms.assetid: 9b7c10a7-2af6-44a2-8b1a-90bba0c7cf26
ms.openlocfilehash: c388a9dc3569e20639de09abc5f4941b73c561ad
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84578045"
---
# <a name="systemservicemodelchannelsmsmqmessagerejected"></a>System.ServiceModel.Channels.MsmqMessageRejected
O MSMQ rejeitou a mensagem.  
  
## <a name="description"></a>Descrição  
 Esse rastreamento indica que uma mensagem MSMQ foi rejeitada.  
  
 As mensagens MSMQ podem ser rejeitadas quando Windows Communication Foundation (WCF) (usado com o NetMsmqBinding ou MsmqIntegrationBinding) não puder processá-las. Essas mensagens são chamadas de mensagens suspeitas. Uma mensagem suspeita é rejeitada quando a `ReceiveErrorHandling` propriedade em NetMsmqBinding ou MsmqIntegrationBinding é definida como `Reject` . Uma mensagem rejeitada é entregue de volta para a [fila de mensagens mortas](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/using-dead-letter-queues-to-handle-message-transfer-failures)do remetente.  
  
 Para obter mais informações sobre quando as mensagens se tornam suspeitas e como configurar seu serviço para tratá-las adequadamente, consulte [manipulação de mensagens suspeitas](../../feature-details/poison-message-handling.md).  
  
 Para obter mais informações sobre o que uma mensagem rejeitada significa no MSMQ, consulte [MQMarkMessageRejected](https://docs.microsoft.com/previous-versions/windows/desktop/msmq/ms707071(v%3dvs.85)).  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
- [Manipulação de mensagens suspeitas](../../feature-details/poison-message-handling.md)
- [MQMarkMessageRejected](https://docs.microsoft.com/previous-versions/windows/desktop/msmq/ms707071(v%3dvs.85))
