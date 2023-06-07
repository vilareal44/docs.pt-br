---
title: Microsoft.Transactions.TransactionBridge.VolatileOutcomeTimeout
ms.date: 03/30/2017
ms.assetid: 2dbe34c5-57c7-4b64-9257-63021911d03c
ms.openlocfilehash: dd2a0b67ec140aa2e6fe1abad8e85c0206abd8ea
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84579015"
---
# <a name="microsofttransactionstransactionbridgevolatileoutcometimeout"></a>Microsoft.Transactions.TransactionBridge.VolatileOutcomeTimeout
O tempo limite do serviço de protocolo WS-AT expirou ao aguardar uma resposta a uma mensagem de resultado de um participante volátil. O resultado da transação pode estar em dúvida se o participante retornar.  
  
## <a name="description"></a>Descrição  
 Rastreado quando um participante volátil decidiu confirmar ou anular, mas não respondeu a uma solicitação de confirmação ou reversão em um determinado período de tempo.  
  
## <a name="troubleshooting"></a>Solução de problemas  
 Certifique-se de que todos os participantes voláteis sejam capazes de responder dentro do período determinado. O período de tempo padrão é de 180 segundos.  Se isso for insuficiente, aumente a `VolatileOutcomeDelay` política de temporizador para WS-AT.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
