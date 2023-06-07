---
title: Microsoft.Transactions.TransactionBridge.EnlistTransactionFailure
ms.date: 03/30/2017
ms.assetid: 1b9f5139-e122-4716-9ef7-2f38e1813993
ms.openlocfilehash: 742e80a3115e8f8caa728e0d8c460ee8b964ddc9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84588711"
---
# <a name="microsofttransactionstransactionbridgeenlisttransactionfailure"></a>Microsoft.Transactions.TransactionBridge.EnlistTransactionFailure
Falha do serviço de protocolo WS-AT ao inscrever-se em uma transação usando o contexto de coordenação fornecido.  
  
## <a name="description"></a>Descrição  
 Rastreado quando o MSDTC não pode se inscrever em uma transação para um determinado protocolo 2PC.  Isso pode acontecer porque a transação não existe mais, a lista de inlistagem não é mais permitida ou muitas inlistagens já estão presentes.  
  
## <a name="troubleshooting"></a>Solução de problemas  
 Inspecione a cadeia de caracteres de status na mensagem de rastreamento para determinar se existe algum item acionável.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
