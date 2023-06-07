---
title: Migrando aplicativos remotos .NET para o WCF
ms.date: 03/30/2017
helpviewer_keywords:
- ',NET remoting [WCF]'
ms.assetid: 24793465-65ae-4308-8c12-dce4fd12a583
ms.openlocfilehash: d12583904e4a025a8de1103f0fb48f4656d6855e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598769"
---
# <a name="migrating-net-remoting-applications-to-wcf"></a>Migrando aplicativos remotos .NET para o WCF
**Este tópico é específico para uma tecnologia herdada que é mantida para compatibilidade com versões anteriores com aplicativos existentes e não é recomendada para desenvolvimento novo. Agora, os aplicativos distribuídos devem ser desenvolvidos usando o WCF.**  
  
 Há duas maneiras de aproveitar o WCF com os aplicativos existentes de comunicação remota do .NET: integração e migração. A integração permite que você tenha o .NET Remoting 2,0 e o WCF em execução lado a lado, permitindo que você exponha os mesmos objetos de negócios em ambas as tecnologias simultaneamente, sem a necessidade de modificar seu código .NET Remoting 2,0 existente. A integração requer que você esteja executando o .NET Framework 2,0 ou superior. Se você quiser aproveitar os recursos do WCF e não precisar de compatibilidade de conexão com os sistemas de comunicação remota 2,0, poderá migrar todo o serviço para o WCF. A migração do .NET Remoting 2,0 para o WCF requer alterações na interface do objeto remoto e em suas definições de configuração. Esses dois tópicos são abordados na [comunicação remota com o Windows Communication Foundation](https://docs.microsoft.com/previous-versions/aa730857(v=vs.80)).  
  
## <a name="see-also"></a>Consulte também

- [Visão geral conceitual](../conceptual-overview.md)
