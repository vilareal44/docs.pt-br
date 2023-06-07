---
title: Microsoft.Transactions.TransactionBridge.PrepareMessageRetry
ms.date: 03/30/2017
ms.assetid: ada4baa5-b60d-46b8-ad46-4d69f8d8a9fa
ms.openlocfilehash: 0c53c1617f3aa3c5f16ba16e8ec548e46ce22137
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594329"
---
# <a name="microsofttransactionstransactionbridgepreparemessageretry"></a>Microsoft.Transactions.TransactionBridge.PrepareMessageRetry
Uma repetição de mensagem de preparação foi enviada a um participante sem resposta.  
  
## <a name="description"></a>Descrição  
 Rastreado se o Gerenciador de transações local precisar reenviar uma mensagem de preparação a um participante subordinado, pois ele não recebeu uma resposta em um determinado período de tempo.  
  
## <a name="troubleshooting"></a>Solução de problemas  
 Investigue possíveis problemas de rede ou de produtos que impedem que a resposta seja entregue no prazo.  Se muitas dessas mensagens forem vistas, isso poderá indicar problemas de infraestrutura ou tempos de resposta muito longos. Ambos os problemas podem reduzir drasticamente a taxa de transferência de transações no sistema.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
