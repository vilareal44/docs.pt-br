---
title: <channelSettings>
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: 94a4457f-f43f-458d-a47e-2d11103ee75e
ms.openlocfilehash: f6a57e2cc1e7c5e114fd38ee3534ab7c4e629b36
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152268"
---
# \<channelSettings>
Especifica as configurações de cache do canal.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.ServiceModel>**](system-servicemodel-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<sendMessageChannelCache>**](sendmessagechannelcache.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<channelSettings>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<behaviors>
  <serviceBehaviors>
    <behavior name="String">
      <sendMessageChannelCache allowUnsafeCaching="Boolean">
        <channelSettings idleTimeout="TimeSpan"
                         leaseTimeout="TimeSpan"
                         maxItemsInCache="Integer" />
      </sendMessageChannelCache>
    </behavior>
  </serviceBehaviors>
</behaviors>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|idleTimeout|Um valor de TimeSpan que especifica o intervalo máximo de tempo para o qual o objeto pode permanecer ocioso no cache antes de ser descartado.|  
|leaseTimeout|Um valor de TimeSpan que especifica o intervalo de tempo após o qual um objeto é removido do cache.|  
|maxItemsInCache|Um inteiro que especifica o número máximo de objetos que podem ser no cache.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<sendMessageChannelCache>](sendmessagechannelcache.md)|Um comportamento de serviço que permite a personalização do cache do compartilhamento níveis, as configurações de cache da fábrica de canal e as configurações de cache do canal para fluxos de trabalho que enviam mensagens a pontos de extremidade de serviço usando atividades de mensagem de envio.|  
  
## <a name="remarks"></a>Comentários  
 Esse comportamento de serviço destina-se a fluxos de trabalho que enviam mensagens a pontos de extremidade de serviço. Esses fluxos de trabalho normalmente são fluxos de trabalho do cliente, mas também pode ser o serviços de fluxo de trabalho que são hospedados em um <xref:System.ServiceModel.WorkflowServiceHost>.  
  
 Por padrão, em um fluxo de trabalho hospedado por um <xref:System.ServiceModel.WorkflowServiceHost>, o cache usado pelo <xref:System.ServiceModel.Activities.Send> atividades de mensagem é compartilhada entre todas as instâncias de fluxo de trabalho no <xref:System.ServiceModel.WorkflowServiceHost> (nível de host de cache). Um fluxo de trabalho de cliente não é hospedado por um <xref:System.ServiceModel.WorkflowServiceHost>, o cache está disponível somente para a instância de fluxo de trabalho (cache de nível de instância). O cache é desabilitado por padrão para qualquer atividade de envio do fluxo de trabalho que tem pontos de extremidade definidos na configuração.  
  
 Para obter mais informações sobre como alterar os níveis de compartilhamento de cache padrão e as configurações de cache para a fábrica de canais e o cache de canal, consulte [alterando os níveis de compartilhamento de cache para atividades de envio](../../../wcf/feature-details/changing-the-cache-sharing-levels-for-send-activities.md).  
  
## <a name="example"></a>Exemplo  
 Em um serviço hospedado de fluxo de trabalho, você pode especificar os cache e o canal cache configurações de fábrica no arquivo de configuração do aplicativo. Para fazer isso, adicionar um comportamento de serviço que contém as configurações de cache para cache de fábrica e o canal e adicionar esse comportamento de serviço ao seu serviço. O exemplo a seguir mostra o conteúdo de um arquivo de configuração que contém o `MyChannelCacheBehavior` comportamento do serviço com o cache de fábrica personalizado e as configurações de cache do canal. Esse comportamento de serviço é adicionado ao serviço por meio do `behaviorConfiguration` atributo.  
  
```xml  
<configuration>
  <system.serviceModel>  
    <!-- List of other config sections here -->
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="MyChannelCacheBehavior">  
          <sendMessageChannelCache allowUnsafeCaching ="false" >  
            <!-- Control only the host level settings -->
            <factorySettings maxItemsInCache = "8" idleTimeout = "00:05:00" leaseTimeout="10:00:00" />  
            <channelSettings maxItemsInCache = "32" idleTimeout = "00:05:00" leaseTimeout="00:06:00" />  
          </sendMessageChannelCache>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <services>  
      <service name="MyService" behaviorConfiguration="MyChannelCacheBehavior" />  
    </services>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Activities.SendMessageChannelCache>
- <xref:System.ServiceModel.Activities.Configuration.SendMessageChannelCacheElement>
- <xref:System.ServiceModel.Activities.Send>
- <xref:System.ServiceModel.Activities.ChannelCacheSettings>
- [Alterar o nível de compartilhamento de cache para atividades de envio](../../../wcf/feature-details/changing-the-cache-sharing-levels-for-send-activities.md)
