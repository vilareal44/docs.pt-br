---
title: <trackingProfile>
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: 154830ff-ddd3-4397-a3b5-5b334907777f
ms.openlocfilehash: 8985da7e1223ac117cf1b68227140634f9c85d3a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79151883"
---
# \<trackingProfile>
Representa uma seção de configuração para criar uma assinatura para controlar os registros em um participante de rastreamento de fluxo de trabalho. Um perfil de rastreamento contém consultas de rastreamento que permitem um participante de rastreamento assinar eventos de fluxo de trabalho que são emitidos quando o estado de uma instância de fluxo de trabalho é alterado em runtime. As consultas definidas no perfil de rastreamento seção definem os tipos de eventos que são retornados pela assinatura.  
  
 Para obter mais informações sobre o rastreamento de fluxo de trabalho e sua configuração, consulte rastreamento de [fluxo de trabalho e rastreamento](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md) e [perfis de rastreamento](../../../windows-workflow-foundation/tracking-profiles.md).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.ServiceModel>**](system-servicemodel-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<tracking>**](tracking.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<trackingProfile>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <tracking>
    <profiles>
      <participants>
        <add name="String"
             profileName="String"
             type="String" />
      </participants>
      <trackingProfile name="String">
        <workflow activityDefinitionId="String">
          <activityScheduledQueries>
            <activityScheduledQuery activityName="String"
                                    childActivityName="String"/>
          </activityScheduledQueries>
          <activityStateQueries>
            <activityStateQuery activityName="String" />
            <arguments>
              <argument name="String" />
            </arguments>
            <states>
              <state name="String"  />
            </states>
            <variables>
              <variable name="String" />
            </variables>
          </activityStateQueries>
          <bookmarkResumptionQueries>
            <bookmarkResumptionQuery name="String" />
          </bookmarkResumptionQueries>
          <cancelRequestQueries>
            <cancelRequestQuery activityName="String"
                                childActivityName="String"/>
          </cancelRequestQueries>
          <customTrackingQueries>
            <customTrackingQuery activityName="String"
                                 name="String"/>
          </customTrackingQueries>
          <faultPropagationQueries>
            <faultPropagationQuery activityName="String"
                                   faultHandlerActivityName="String" />
          </faultPropagationQueries>
          <workflowInstanceQueries>
            <workflowInstanceQuery>
              <states>
                <state name="String" />
              </states>
            </workflowInstanceQuery>
          </workflowInstanceQueries>
        </workflow>
      </trackingProfile>
    </profiles>
  </tracking>
</system.serviceModel>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|name|Uma cadeia de caracteres que especifica o nome do perfil de rastreamento.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<participants>](participants.md)|Um elemento de configuração que contém todas as consultas de um fluxo de trabalho específico identificado pelo <xref:System.ServiceModel.Activities.Tracking.Configuration.ProfileWorkflowElement.ActivityDefinitionId%2A?displayProperty=nameWithType> propriedade.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<tracking>](tracking.md)|Representa uma seção de configuração para definir configurações de controle para um serviço de fluxo de trabalho.|  
  
## <a name="remarks"></a>Comentários  
 Perfis de rastreamento contém consultas de rastreamento que permitem um participante de rastreamento assinar eventos de fluxo de trabalho que são emitidos quando o estado de uma instância de fluxo de trabalho é alterado em runtime. Dependendo dos requisitos de monitoramento que você pode escrever um perfil que é muito simples, que assina a um pequeno conjunto de alterações de estado de alto nível em um fluxo de trabalho. Por outro lado, você pode criar um perfil muito específico cujos eventos resultantes são ricos reconstruir um fluxo de execução detalhado mais adiante.  
  
 Controlando os perfis são estruturados como as assinaturas declarativas para controlar os registros que permitem que você possa ver o runtime de fluxo de trabalho para o controle específico registro. Há alguns tipos de consulta que permitem que você assine diferentes classes de <xref:System.Activities.Tracking.TrackingRecord> objetos. Para obter uma lista completa de consultas, consulte [\<participants>](participants.md) e [perfis de rastreamento](../../../windows-workflow-foundation/tracking-profiles.md)..  
  
 O exemplo a seguir mostra um perfil de controle em um arquivo de configuração que permite que um participante de controle assine os `Started` `Completed` eventos de fluxo de trabalho e.  
  
```xml  
<system.serviceModel>  
  <tracking>
    <trackingProfile name="Sample Tracking Profile">  
      <workflow activityDefinitionId="*">  
         <workflowInstanceQueries>  
            <workflowInstanceQuery>  
            <states>  
              <state name="Started"/>  
              <state name="Completed"/>  
            </states>  
          </workflowInstanceQuery>  
        </workflowInstanceQueries>  
      </workflow>  
    </trackingProfile>
   </profiles>  
  </tracking>  
</system.serviceModel>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Activities.Tracking.Configuration.ProfileElement>
- <xref:System.Activities.Tracking.TrackingProfile>
- [Acompanhamento e rastreamento de fluxo de trabalho](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md)
- [Acompanhando perfis](../../../windows-workflow-foundation/tracking-profiles.md)
