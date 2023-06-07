---
title: System.ServiceModel.Channels.FailedAcceptFromPool
ms.date: 03/30/2017
ms.assetid: d535b1b5-ee58-45e8-b400-7d9570f4eb9a
ms.openlocfilehash: 5bfa31d0eaf3b00017512eddc60bfa99eda619e3
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84582576"
---
# <a name="systemservicemodelchannelsfailedacceptfrompool"></a>System.ServiceModel.Channels.FailedAcceptFromPool
Falha ao tentar reutilizar uma conexão em pool. Outra tentativa é feita dentro do período de tempo limite especificado.  
  
## <a name="description"></a>Descrição  
 Esse rastreamento informativo indica que ocorreu um erro ao tentar reutilizar uma conexão em pool. Isso pode acontecer porque a conexão em pool não era compatível, pronta ou ainda está ativa. As tentativas adicionais de reutilização de outra conexão em pool são feitas dentro de um determinado tempo limite e uma nova conexão é criada caso não sejam encontradas conexões utilizáveis.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
