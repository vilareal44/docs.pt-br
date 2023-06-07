---
title: Microsoft.Transactions.TransactionBridge.RegisterParticipantFailure
ms.date: 03/30/2017
ms.assetid: 3a4ead79-8550-4037-84e3-fd70ff56e001
ms.openlocfilehash: 1819a269a6775c8541f38f4aa5d733002e3c1e9f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599667"
---
# <a name="microsofttransactionstransactionbridgeregisterparticipantfailure"></a>Microsoft.Transactions.TransactionBridge.RegisterParticipantFailure
Falha do serviço de protocolo WS-AT ao registrar um participante para um protocolo de controle.  
  
## <a name="description"></a>Descrição  
 Rastreado se o MSDTC detectar uma solicitação de registro inválida. Isso pode ser causado por várias solicitações de registro de conclusão e erros internos.  
  
## <a name="troubleshooting"></a>Solução de problemas  
 Não tente se registrar para conclusão mais de uma vez.  Além disso, inspecione a cadeia de caracteres de status na mensagem de rastreamento para determinar se existe algum item acionável.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
