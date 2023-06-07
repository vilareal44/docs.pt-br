---
title: System.ServiceModel.TxFailedToNegotiateOleTx
ms.date: 03/30/2017
ms.assetid: 3f0f0b4b-a1ad-4704-8329-455daf54892d
ms.openlocfilehash: 2c9ea77cdd76d4593c2ee5b09a4b917677b8925f
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601407"
---
# <a name="systemservicemodeltxfailedtonegotiateoletx"></a>System.ServiceModel.TxFailedToNegotiateOleTx
A negociação do protocolo OleTransactions não pôde ser concluída para o contexto de coordenação especificado.  
  
## <a name="description"></a>Descrição  
 Rastreado quando uma transação de entrada com informações de OleTx não é capaz de usar as informações de OleTx anexadas e voltará a usar WS-AT em vez disso.  
  
## <a name="troubleshooting"></a>Solução de problemas  
 Indica um possível problema com a comunicação de RPC do MSDTC entre os computadores. Se muitos desses rastreamentos aparecerem no log, uma diminuição drástica no desempenho poderá resultar.  Se OleTx não for desejado, defina `OleTxUpgradeEnabled` como 0 na configuração do registro WS-AT.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
