---
title: System.ServiceModel.Channels.MsmqMessageDropped
ms.date: 03/30/2017
ms.assetid: 8b6e644d-fa68-4be7-abe9-3659671a37c1
ms.openlocfilehash: 4b016f53a75d9527a5cd1bbadacacd650b7f35b0
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601889"
---
# <a name="systemservicemodelchannelsmsmqmessagedropped"></a>System.ServiceModel.Channels.MsmqMessageDropped
O MSMQ removeu a mensagem.  
  
## <a name="description"></a>Descrição  
 O rastreamento indica que uma mensagem MSMQ foi descartada. As mensagens MSMQ podem ser descartadas quando Windows Communication Foundation (WCF) (usado com o NetMsmqBinding ou MsmqIntegrationBinding) não puder processá-las. Essas mensagens são chamadas de mensagens suspeitas.  
  
 Uma mensagem suspeita é descartada quando a `ReceiveErrorHandling` propriedade em NetMsmqBinding ou MsmqIntegrationBinding é definida como `Drop` . Uma mensagem descartada é removida da fila e não é mais recuperável.  
  
 Para obter mais informações sobre quando as mensagens se tornam suspeitas e como configurar seu serviço para tratá-las adequadamente, consulte [manipulação de mensagens suspeitas](../../feature-details/poison-message-handling.md).  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
- [Manipulação de mensagens suspeitas](../../feature-details/poison-message-handling.md)
