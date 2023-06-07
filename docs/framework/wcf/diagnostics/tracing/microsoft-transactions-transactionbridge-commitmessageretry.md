---
title: Microsoft.Transactions.TransactionBridge.CommitMessageRetry
ms.date: 03/30/2017
ms.assetid: 4abe01f0-6398-4fba-b2f3-c054b7f7e971
ms.openlocfilehash: f37bc252e12aef94d77c0745d36b5c8232a169eb
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599731"
---
# <a name="microsofttransactionstransactionbridgecommitmessageretry"></a>Microsoft.Transactions.TransactionBridge.CommitMessageRetry
Uma nova tentativa de mensagem de confirmação foi enviada a um participante sem resposta.  
  
## <a name="description"></a>Descrição  
 Rastreado se o Gerenciador de transações local precisar reenviar uma mensagem de confirmação para um participante subordinado porque ele não recebeu uma resposta em um determinado período de tempo.  
  
## <a name="troubleshooting"></a>Solução de problemas  
 Investigue possíveis problemas de rede ou de produtos que impedem que a resposta seja entregue no prazo.  Se muitas dessas mensagens forem vistas, isso poderá indicar problemas de infraestrutura ou tempos de resposta muito longos. Os dois problemas reduzem drasticamente a taxa de transferência das transações no sistema.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
