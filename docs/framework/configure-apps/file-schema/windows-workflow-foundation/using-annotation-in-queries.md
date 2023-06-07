---
title: Usar anotação em consultas
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: 50855b30-d5fe-49a9-89d3-3f1bfd670958
ms.openlocfilehash: 728408e744bc1eca62158fab1a7a17e985fe3b6c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "69947281"
---
# <a name="using-annotation-in-queries"></a>Usar anotação em consultas
As anotações permitem que você marca arbitrariamente registros de rastreamento com um valor que pode ser configurado após tempo de compilação. Por exemplo, talvez você queira que vários registros de acompanhamento em vários fluxos de trabalho sejam marcados com "servidor de email" = = "Server1 de email". Isso facilita localizar todos os registros com essa marca ao consultar o rastreamento registra posteriormente.  
  
## <a name="adding-annotations"></a>Adicionando anotações  
 Uma anotação pode ser adicionada a uma consulta de controle, conforme mostrado no exemplo a seguir.  
  
```xml  
<activityStateQuery activityName="SendEmailActivity">  
  <states>  
    <state name="Closed"/>  
  </states>  
  <annotations>  
    <annotation name="MailServer" value="Mail Server1"/>  
  </annotations>  
</activityStateQuery>  
```  
  
> [!NOTE]
> Esses elementos de consulta de controle podem ser usados para criar um perfil de rastreamento. Um perfil de rastreamento pode ser criado na configuração ou usando código.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Activities.Tracking.Configuration.ProfileWorkflowElement>
- <xref:System.Activities.Tracking.TrackingProfile>
- [\<participants>](participants.md)
- [Acompanhamento e rastreamento de fluxo de trabalho](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md)
- [Acompanhando perfis](../../../windows-workflow-foundation/tracking-profiles.md)
