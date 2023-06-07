---
title: <state>do WCF,<workflowInstanceQuery>
ms.date: 03/30/2017
ms.assetid: 40f21055-766c-4be9-86c4-d1d899007098
ms.openlocfilehash: 80f7532f3c51680a2e34713b526dc43822db61b9
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70854956"
---
# <a name="state-of-wcf-workflowinstancequery"></a>\<state>do WCF,\<workflowInstanceQuery>
Representa uma coleção de estados inscritos da instância do fluxo de trabalho controladas quando os registros de rastreamento são criados.  
  
 Para obter mais informações sobre consultas de perfil de rastreamento, consulte [perfis de rastreamento](../../../windows-workflow-foundation/tracking-profiles.md)  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<tracking>**](tracking-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<profiles>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<trackingProfile>**](trackingprofile-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<workflow>**](workflow-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<workflowInstanceQueries>**](workflowinstancequeries-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<workflowInstanceQuery>**](workflowinstancequery-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<states>**](states-of-wcf-workflowinstancequery.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<state>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<tracking>
  <profiles>
    <trackingProfile name="Name">
      <workflow>
        <workflowInstanceQueries>
          <workflowInstanceQuery>
            <states>
              <state name="Name" />
            </states>
          </workflowInstanceQuery>
        </workflowInstanceQueries>
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
|`name`|Uma cadeia de caracteres que especifica um estado inscrito da instância do fluxo de trabalho controladas quando o registro de rastreamento é criado.|  
  
### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|  
|-------------|-----------------|  
|[\<states>](states-of-wcf-workflowinstancequery.md)|Uma coleção de estados inscritos da instância do fluxo de trabalho controladas quando os registros de rastreamento são criados.|  
  
## <a name="remarks"></a>Comentários  

Os registros retornados são filtrados por estados nesta coleção.  
  
Os valores de estado possíveis são descritos na tabela a seguir:
  
|Estado|Descrição|  
|-----------|-----------------|  
|Anulado|A instância de fluxo de trabalho será anulada.|  
|Concluído|A instância de fluxo de trabalho for concluída.|  
|Excluído|A instância de fluxo de trabalho é excluída.|  
|Idle|A instância de fluxo de trabalho está ociosa.|  
|Persistentes|A instância de fluxo de trabalho é mantida.|  
|Retomada|A instância de fluxo de trabalho é retomada.|  
|Iniciado|A instância de fluxo de trabalho é iniciada.|  
|UnhandledException|A instância de fluxo de trabalho encontrou uma exceção sem tratamento.|  
|Descarregado|A instância de fluxo de trabalho é descarregada.|  
|Canceled|A instância de fluxo de trabalho é cancelada.|  
|Suspenso|A instância de fluxo de trabalho é suspensa.|  
|Encerrada|A instância de fluxo de trabalho é encerrada.|  
|Não suspenso|A instância de fluxo de trabalho é unsuspended.|  
  
## <a name="example"></a>Exemplo

A configuração a seguir assina o fluxo de trabalho em nível de instância registros de controle para o `Started` estado da instância usando esta consulta.  
  
```xml  
<workflowInstanceQueries>
  <workflowInstanceQuery>
    <states>
      <state name="Started" />
    </states>
  </workflowInstanceQuery>
</workflowInstanceQueries>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Activities.Tracking.Configuration.WorkflowInstanceQueryElement?displayProperty=nameWithType>
- <xref:System.ServiceModel.Activities.Tracking.Configuration.StateElement?displayProperty=nameWithType>
- <xref:System.Activities.Tracking.WorkflowInstanceQuery?displayProperty=nameWithType>
- [Acompanhamento e rastreamento de fluxo de trabalho](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md)
- [Acompanhando perfis](../../../windows-workflow-foundation/tracking-profiles.md)
