---
title: <faultPropagationQueries>do WCF
ms.date: 03/30/2017
ms.assetid: d85f66a7-e7b0-4dbb-83cc-89fa06fc9161
ms.openlocfilehash: 709c2c6907b4d0d28118f9de12edb047aa16d741
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855271"
---
# <a name="faultpropagationqueries-of-wcf"></a>\<faultPropagationQueries>do WCF

Representa uma coleção de consultas que são usadas para rastrear a manipulação de falhas que ocorrem em uma atividade.  Esse evento ocorre toda vez que um FaultHandler processa uma falha. Você deve usar essa consulta para controlar o tratamento de falhas que ocorrem dentro de uma atividade. A consulta é necessária para um participante de rastreamento assinar os registros de propagação de falhas.  
  
Para obter mais informações sobre consultas de perfil de rastreamento, consulte [perfis de rastreamento](../../../windows-workflow-foundation/tracking-profiles.md).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<tracking>**](tracking-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<profiles>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<trackingProfile>**](trackingprofile-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<workflow>**](workflow-of-wcf.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<faultPropagationQueries>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<tracking>
  <profiles>
    <trackingProfile name="Name">
      <workflow>
        <faultPropagationQueries>
          <faultPropagationQuery faultSourceActivityName="String"
                                 faultHandlerActivityName="String"/>
        </faultPropagationQueries>
      </workflow>
    </trackingProfile>
  </profiles>
</tracking>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.
  
### <a name="attributes"></a>Atributos

Nenhum.
  
### <a name="child-elements"></a>Elementos filho

|Elemento|Descrição|  
|-------------|-----------------|  
|[\<faultPropagationQuery>](faultpropagationquery-of-wcf.md)|Uma consulta que é usada para rastrear a manipulação de falhas que ocorrem em uma atividade.  Esse evento ocorre toda vez que um FaultHandler processa uma falha.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<workflow>](../windows-workflow-foundation/workflow.md)|Um elemento de configuração que contém todas as consultas de um fluxo de trabalho específico identificado pelo `activityDefinitionId` propriedade.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Activities.Tracking.Configuration.FaultPropagationQueryElementCollection?displayProperty=nameWithType>
- <xref:System.Activities.Tracking.FaultPropagationQuery?displayProperty=nameWithType>
- [Acompanhamento e rastreamento de fluxo de trabalho](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md)
- [Acompanhando perfis](../../../windows-workflow-foundation/tracking-profiles.md)
