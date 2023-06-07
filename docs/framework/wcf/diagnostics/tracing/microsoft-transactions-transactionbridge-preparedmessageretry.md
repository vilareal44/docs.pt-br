---
title: Microsoft.Transactions.TransactionBridge.PreparedMessageRetry
ms.date: 03/30/2017
ms.assetid: 2194292d-bf5f-4aef-9336-cd3c4795cb71
ms.openlocfilehash: 50dd3070525bbc6d36956b25dee587ec7864d3ff
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594303"
---
# <a name="microsofttransactionstransactionbridgepreparedmessageretry"></a>Microsoft.Transactions.TransactionBridge.PreparedMessageRetry
Uma nova tentativa de mensagem preparada foi enviada a um coordenador sem resposta.  
  
## <a name="description"></a>Descrição  
 Rastreado se o Gerenciador de transações local precisar reenviar uma mensagem preparada para um coordenador superior porque ele não recebeu uma resposta em um determinado período/  
  
## <a name="troubleshooting"></a>Solução de problemas  
 Investigue possíveis problemas de rede ou de produtos que impedem que a resposta seja entregue no prazo.  Se muitas dessas mensagens forem vistas, isso poderá indicar problemas de infraestrutura ou tempos de resposta muito longos. Os dois problemas reduzem drasticamente a taxa de transferência das transações no sistema.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
