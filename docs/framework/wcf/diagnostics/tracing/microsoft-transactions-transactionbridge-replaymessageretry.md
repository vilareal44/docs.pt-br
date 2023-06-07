---
title: Microsoft.Transactions.TransactionBridge.ReplayMessageRetry
ms.date: 03/30/2017
ms.assetid: e5b820ae-504d-405a-926a-9effa41d2369
ms.openlocfilehash: 162452d5d12859571d78ef19cf1d838953ee7acd
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596201"
---
# <a name="microsofttransactionstransactionbridgereplaymessageretry"></a>Microsoft.Transactions.TransactionBridge.ReplayMessageRetry
Uma nova tentativa de mensagem de reprodução foi enviada a um coordenador sem resposta.  
  
## <a name="description"></a>Descrição  
 Rastreado se o Gerenciador de transações local precisar reenviar uma mensagem de reprodução para um coordenador superior porque ele não recebeu uma resposta em um determinado período de tempo.  
  
## <a name="troubleshooting"></a>Solução de problemas  
 Investigue possíveis problemas de rede ou de produtos que impedem que a resposta seja entregue no prazo.  Se muitas dessas mensagens forem vistas, isso poderá indicar problemas de infraestrutura ou tempos de resposta muito longos. Os dois problemas reduzem drasticamente a taxa de transferência das transações no sistema.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
