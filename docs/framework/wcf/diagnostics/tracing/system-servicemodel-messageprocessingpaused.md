---
title: System.ServiceModel.MessageProcessingPaused
ms.date: 03/30/2017
ms.assetid: 36b5302a-93cc-478a-9bb2-8a1601fba1df
ms.openlocfilehash: 85bec8255e0d20d6e76ea354e5b8c42b83d7d8e6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598145"
---
# <a name="systemservicemodelmessageprocessingpaused"></a>System.ServiceModel.MessageProcessingPaused
System.ServiceModel.MessageProcessingPaused  
  
## <a name="description"></a>Descrição  
 Os threads foram alternados durante o processamento de uma mensagem.  
  
 O processamento de mensagens pode ser pausado pelos seguintes motivos:  
  
- ConcurrencyMode é Single ou reentrante e o serviço está processando outra mensagem.  
  
- A transação está habilitada e o serviço está processando outra transação.  
  
- O contexto de sincronização não é atual.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
