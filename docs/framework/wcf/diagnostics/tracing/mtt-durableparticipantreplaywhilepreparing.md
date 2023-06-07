---
title: Microsoft.Transactions.TransactionBridge.DurableParticipantReplayWhilePreparing
ms.date: 03/30/2017
ms.assetid: 10ef3876-6f8e-4d4e-8444-f47847b64795
ms.openlocfilehash: 31fb8d466c76c7490aa80dfcab089332af4036a2
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84589126"
---
# <a name="microsofttransactionstransactionbridgedurableparticipantreplaywhilepreparing"></a>Microsoft.Transactions.TransactionBridge.DurableParticipantReplayWhilePreparing
O serviço de protocolo WS-AT recebeu uma mensagem de reprodução de um participante durável, que não respondeu à preparação. Consequentemente, a transação foi anulada.  
  
## <a name="description"></a>Descrição  
 Rastreado se uma mensagem de reprodução for recebida enquanto um participante durável ainda estiver se preparando. Esta é uma mensagem ilegal para esse Estado e a transação será anulada.  
  
## <a name="troubleshooting"></a>Solução de problemas

Receber esse erro pode indicar que um participante durável (incluindo TransactionManagers subordinados) se recuperou de uma falha; no entanto, não há certeza do resultado da transação e solicita seu status.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
