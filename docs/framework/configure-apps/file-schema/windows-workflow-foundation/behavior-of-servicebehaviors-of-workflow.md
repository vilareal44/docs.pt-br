---
title: <behavior>do <serviceBehaviors> fluxo de trabalho
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: 6a4b718a-1b40-4957-935a-f6122819ab3c
ms.openlocfilehash: 071cff8e9f6ec3fa0546a07d19160869d8b43f60
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152314"
---
# <a name="behavior-of-servicebehaviors-of-workflow"></a>\<behavior>do \<serviceBehaviors> fluxo de trabalho
O elemento **Behavior** contém uma coleção de configurações para o comportamento de um serviço. Cada comportamento é indexado por seu **nome**. Os serviços podem vincular a cada comportamento por meio desse nome usando o atributo **behaviorConfiguration** do [\<endpoint>](../wcf/endpoint-element.md) elemento. Isso permite que os pontos de extremidade compartilhem configurações comuns de comportamento sem redefinir as configurações.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.ServiceModel>**](system-servicemodel-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<behavior>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.ServiceModel>  
  <behaviors>  
    <serviceBehaviors>  
      <behavior name="String">
        <bufferReceive maxPendingMessagesPerChannel="Integer" />
        <etwTracking profileName="String" />
        <sendMessageChannelCache allowUnsafeCaching="Boolean">
          <channelSettings idleTimeout="TimeSpan"
                           leaseTimeout="TimeSpan"
                           maxItemsInCache="Integer" />
          <factorySettings idleTimeout="TimeSpan"
                           leaseTimeout="TimeSpan"
                           maxItemsInCache="Integer" />
        </sendMessageChannelCache>
        <sqlWorkflowInstanceStore connectionStringName="String"
                                  hostLockRenewalPeriod="TimeSpan"
                                  instanceCompletionAction="DeleteNothing/DeleteAll"
                                  instanceEncodingAction="None/GZip"
                                  instanceLockedExceptionAction="NoRetry/BasicRetry/AggressiveRetry"
                                  runnableInstancesDetectionPeriod="TimeSpan" />
        <workflowIdle timeToPersist="TimeSpan"
                      timeToUnload="TimeSpan" />
        <workflowUnhandledException action="Abandon/AbandonAndSuspend/Cancel/Terminate" />
      </behavior>
    </serviceBehaviors>  
  </behaviors>  
</system.ServiceModel>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|name|Uma cadeia de caracteres exclusiva que contém o nome da configuração do comportamento. Esse valor é uma cadeia de caracteres definida pelo usuário que deve ser exclusiva, pois ele atua como a cadeia de caracteres de identificação para o elemento.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<bufferReceive>](bufferreceive.md)|Um comportamento de serviço que permite que um serviço a ser usado em buffer recebe o processamento, que permite que um serviço de fluxo de trabalho processar mensagens de fora de ordem.|  
|[\<routing>](../wcf/routing-of-servicebehavior.md)|Um comportamento de serviço que permite que um serviço que utiliza o acompanhamento ETW use um <xref:System.Activities.Tracking.EtwTrackingParticipant>.|  
|[\<sendMessageChannelCache>](sendmessagechannelcache.md)|Um comportamento de serviço que permite a personalização do cache do compartilhamento níveis, as configurações de cache da fábrica de canal e as configurações de cache do canal para fluxos de trabalho que enviam mensagens a pontos de extremidade de serviço usando atividades de mensagem de envio.|  
|[\<sqlWorkflowInstanceStore>](sqlworkflowinstancestore.md)|Um comportamento de serviço que permite configurar o recurso <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore>, que dá suporte a informações de estado persistentes para instâncias de serviço de fluxo de trabalho em um banco de dados do SQL Server 2005 ou do SQL Server 2008.|  
|[\<workflowIdle>](workflowidle.md)|Um comportamento de serviço que controla quando instâncias de fluxo de trabalho ocioso são descarregadas e persistidas.|  
|[\<workflowInstanceManagement>](workflowinstancemanagement.md)|Um comportamento de serviço que permite que você especifique as configurações que controlam como as instâncias de fluxo de trabalho são executadas, incluindo persistência, o comportamento de exceção sem tratamento e o comportamento ocioso.|  
|[\<workflowUnhandledException>](workflowunhandledexception.md)|Um comportamento de serviço que permite que você especifique a ação a ser executada quando ocorre uma exceção sem tratamento em um serviço de fluxo de trabalho.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<serviceBehaviors>](servicebehaviors-of-workflow.md)|Uma coleção de elementos de comportamento de serviço.|
