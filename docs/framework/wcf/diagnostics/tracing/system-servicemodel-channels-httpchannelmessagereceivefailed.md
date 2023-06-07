---
title: System.ServiceModel.Channels.HttpChannelMessageReceiveFailed
ms.date: 03/30/2017
ms.assetid: 9eb311da-fdcc-4dd3-9d85-05b3280dfdda
ms.openlocfilehash: e11b376924ee74e5d0d67da0cac59af41655dc44
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594069"
---
# <a name="systemservicemodelchannelshttpchannelmessagereceivefailed"></a>System.ServiceModel.Channels.HttpChannelMessageReceiveFailed
Falha ao receber uma mensagem por um canal HTTP.  
  
## <a name="description"></a>Descrição  
 Esse rastreamento pode ser emitido como um aviso ou um erro. Em ambos os casos, o rastreamento é emitido quando um ouvinte compatível não é encontrado para uma solicitação HTTP de entrada e a solicitação HTTP é descartada. Isso pode acontecer porque o verbo HTTP da solicitação não foi reconhecido por nenhum ouvinte HTTP ou porque nenhum ouvinte estava escutando no endereço para o qual a solicitação foi destinada. O rastreamento é emitido como um aviso no caso hospedado automaticamente e como um erro quando o serviço é hospedado no IIS.  
  
## <a name="see-also"></a>Consulte também

- [Rastreamento](index.md)
- [Utilizando o rastreamento para solucionar problemas em seu aplicativo](using-tracing-to-troubleshoot-your-application.md)
- [Administração e diagnóstico](../index.md)
