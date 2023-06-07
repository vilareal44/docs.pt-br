---
title: System.ServiceModel.ManualFlowThrottleLimitReached
ms.date: 03/30/2017
ms.assetid: 9aba851f-1830-493b-8e3e-60f454eb923e
ms.openlocfilehash: bffa6361df5a50748f45aa3004f7a307ad516d7b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84580483"
---
# <a name="systemservicemodelmanualflowthrottlelimitreached"></a>System.ServiceModel.ManualFlowThrottleLimitReached
System.ServiceModel.ManualFlowThrottleLimitReached  
  
## <a name="description"></a>Descrição  
 O sistema atingiu o limite definido para a limitação de ManualFlowControlLimit. O valor de restrição pode ser alterado modificando a propriedade ManualFlowControlLimit no ServiceHost ou InstanceContext, conforme aplicável.  
  
 Esse rastreamento é emitido quando o limite de controle de fluxo manual é reduzido inicialmente para 0. Alterações subsequentes em 0 não são rastreadas. O limite de controle de fluxo no contexto de instância é rastreado uma vez para cada contexto.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
