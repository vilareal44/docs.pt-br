---
title: <trackingProfile>do WCF
ms.date: 10/08/2018
ms.assetid: 09b651c2-c0d2-4850-a101-b0e009a1dc3a
ms.openlocfilehash: c5df03d63653e658a23a36e8943c06f156d2ae00
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70854944"
---
# <a name="trackingprofile-of-wcf"></a>\<trackingProfile>do WCF
Representa uma seção de configuração para criar uma assinatura para controlar os registros em um participante de rastreamento de fluxo de trabalho. Um perfil de rastreamento contém consultas de rastreamento que permitem um participante de rastreamento assinar eventos de fluxo de trabalho que são emitidos quando o estado de uma instância de fluxo de trabalho é alterado em runtime. As consultas definidas no perfil de rastreamento seção definem os tipos de eventos que são retornados pela assinatura.  
  
Para obter mais informações sobre o rastreamento de fluxo de trabalho e sua configuração, consulte rastreamento de [fluxo de trabalho e rastreamento](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md) e [perfis de rastreamento](../../../windows-workflow-foundation/tracking-profiles.md).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<tracking>**](tracking-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<profiles>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<trackingProfile>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <tracking>
    <profiles>
      <trackingProfile name="String">
        <workflow activityDefinitionId="String">
          <activityScheduledQueries>
            <activityScheduledQuery activityName="String"
                                    childActivityName="String" />
          </activityScheduledQueries>
          <activityStateQueries>
            <activityStateQuery activityName="String">
              <arguments>
                <argument name="String" />
              </arguments>
              <states>
                <state name="String" />
              </states>
              <variables>
                <variable name="String" />
              </variables>
            </activityStateQuery>
          </activityStateQueries>
          <bookmarkResumptionQueries>
            <bookmarkResumptionQuery name="String" />
          </bookmarkResumptionQueries>
          <cancelRequestedQueries>
            <cancelRequestedQuery activityName="String"
                                  childActivityName="String" />
          </cancelRequestedQueries>
          <customTrackingQueries>
            <customTrackingQuery activityName="String"
                                 name="String" />
          </customTrackingQueries>
          <faultPropagationQueries>
            <faultPropagationQuery faultSourceActivityName="String"
                                   faultHandlerActivityName="String" />
          </faultPropagationQueries>
          <stateMachineStateQueries>
            <stateMachineStateQuery activityName="String" />
          </stateMachineStateQueries>
          <workflowInstanceQueries>
            <workflowInstanceQuery>
              <states>
                <state name="String"/>
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
|[\<participants>](../windows-workflow-foundation/participants.md)|Um elemento de configuração que contém todas as consultas de um fluxo de trabalho específico identificado pelo <xref:System.ServiceModel.Activities.Tracking.Configuration.ProfileWorkflowElement.ActivityDefinitionId?displayProperty=nameWithType> propriedade.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<tracking>](../windows-workflow-foundation/tracking.md)|Representa uma seção de configuração para definir configurações de controle para um serviço de fluxo de trabalho.|  
  
## <a name="remarks"></a>Comentários  
 Perfis de rastreamento contém consultas de rastreamento que permitem um participante de rastreamento assinar eventos de fluxo de trabalho que são emitidos quando o estado de uma instância de fluxo de trabalho é alterado em runtime. Dependendo dos requisitos de monitoramento que você pode escrever um perfil que é muito simples, que assina a um pequeno conjunto de alterações de estado de alto nível em um fluxo de trabalho. Por outro lado, você pode criar um perfil muito específico cujos eventos resultantes são ricos reconstruir um fluxo de execução detalhado mais adiante.  
  
 Controlando os perfis são estruturados como as assinaturas declarativas para controlar os registros que permitem que você possa ver o runtime de fluxo de trabalho para o controle específico registro. Há alguns tipos de consulta que permitem que você assine diferentes classes de <xref:System.Activities.Tracking.TrackingRecord> objetos. Para obter uma lista completa de consultas, consulte [\<participants>](../windows-workflow-foundation/participants.md) e [perfis de rastreamento](../../../windows-workflow-foundation/tracking-profiles.md).
  
O exemplo a seguir mostra um perfil de controle em um arquivo de configuração que permite que um participante de controle assine os `Started` `Completed` eventos de fluxo de trabalho e.  
  
```xml  
<system.serviceModel>
  <tracking>
    <profiles>
      <trackingProfile name="Sample Tracking Profile">
        <workflow activityDefinitionId="*">
          <workflowInstanceQueries>
            <workflowInstanceQuery>
              <states>
                <state name="Started" />
                <state name="Completed" />
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
