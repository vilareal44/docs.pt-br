---
title: <activityStateQuery>do WCF
ms.date: 03/30/2017
ms.assetid: d6cdc04b-6f3a-4097-a623-ee4a1be3b5c4
ms.openlocfilehash: 49c507424e813067e1dad9b08167d9661acef36f
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70991225"
---
# <a name="activitystatequery-of-wcf"></a>\<activityStateQuery>do WCF

Representa uma consulta que é usada para controlar as alterações do ciclo de vida das atividades que compõem uma instância de fluxo de trabalho. Por exemplo, talvez você queira manter o controle sempre que a atividade "enviar email" for concluída em uma instância de fluxo de trabalho. Essa consulta é necessária para um participante de rastreamento para se inscrever em objetos de registro de estado de atividade. Os estados disponíveis para assinar são especificados em ActivityStates.  
  
Para obter mais informações sobre consultas de perfil de rastreamento, consulte [perfis de rastreamento](../../../windows-workflow-foundation/tracking-profiles.md).

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<tracking>**](tracking-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<profiles>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<trackingProfile>**](trackingprofile-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<workflow>**](workflow-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<activityStateQueries>**](activitystatequeries-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<activityStateQuery>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<tracking>
  <profiles>
    <trackingProfile name="Name">
      <workflow>
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
      </workflow>
    </trackingProfile>
  </profiles>
</tracking>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  

As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|activityName|Uma cadeia de caracteres que especifica o nome da atividade para filtrar <xref:System.Activities.Tracking.ActivityStateRecord> instâncias no.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<arguments>](../windows-workflow-foundation/arguments.md)|Uma coleção de argumentos associados a essa consulta de atividade.|  
|[\<states>](../windows-workflow-foundation/states.md)|Uma coleção de elementos de configuração que contêm os estados da atividade inscrito para que um registro de rastreamento deve ser emitido.|  
|[\<states>](../windows-workflow-foundation/states.md)|Uma coleção de variáveis associadas a essa consulta de atividade.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<faultPropagationQuery>](../windows-workflow-foundation/faultpropagationquery.md)|Representa uma lista de elementos de configuração que são usados para controlar solicitações cancelar uma atividade filho pela atividade pai. A consulta é necessária para um participante de rastreamento inscrever-se para Cancelar solicitação objetos de registro.|  
  
## <a name="remarks"></a>Comentários

Um recurso exclusivo de um ActivityStateQuery é a capacidade de extrair dados para controlar a execução de um fluxo de trabalho. Isso fornece contexto adicional ao acessar o rastreamento registra pós execução. Você pode usar os [\<arguments>](../windows-workflow-foundation/arguments.md) [\<states>](../windows-workflow-foundation/states.md) elementos e [\<states>](../windows-workflow-foundation/states.md) para extrair qualquer variável ou argumento de qualquer atividade em um fluxo de trabalho. O exemplo a seguir mostra uma consulta de estado da atividade que extrai variáveis e argumentos quando `Closed` de atividade que controla o registro é emitida. Variáveis e argumentos podem ser extraídos apenas com um ActivityStateRecord e, portanto, são assinados em um perfil de controle usando [\<activityStateQuery>](../windows-workflow-foundation/activitystatequery.md) .  
  
```xml  
<activityStateQuery activityName="SendEmailActivity">
  <states>
    <state name="Closed" />
  </states>
  <variables>
    <variable name="FromAddress" />
  </variables>
  <arguments>
    <argument name="Result" />
  </arguments>
</activityStateQuery>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Activities.Tracking.Configuration.ActivityStateQueryElement>
- <xref:System.Activities.Tracking.ActivityStateQuery>
- [Acompanhamento e rastreamento de fluxo de trabalho](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md)
- [Acompanhando perfis](../../../windows-workflow-foundation/tracking-profiles.md)
