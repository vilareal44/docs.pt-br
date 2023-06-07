---
title: <etwTracking>
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: cb45c82e-6ea1-4c4d-924c-118a25ae1f35
ms.openlocfilehash: d562bd4e3d46a1bdf41fc4065fee926850a49aa1
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152165"
---
# \<etwTracking>
Um comportamento de serviço que permite que um serviço que utiliza o acompanhamento ETW use um <xref:System.Activities.Tracking.EtwTrackingParticipant>.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.ServiceModel>**](system-servicemodel-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors-of-workflow.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<etwTracking>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<behaviors>
  <serviceBehaviors>
    <behavior name="String">
      <etwTracking profileName="String" />
    </behavior>
  </serviceBehaviors>
</behaviors>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|profileName|Uma cadeia de caracteres que especifica o nome do perfil de rastreamento associado a esse comportamento.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behavior>desse\<serviceBehaviors>](behavior-of-servicebehaviors-of-workflow.md)|Especifica um elemento de comportamento.|  
  
## <a name="remarks"></a>Comentários  
 Quando adicionado à configuração de comportamento do serviço, este elemento de configuração configura um participante de rastreamento em um serviço de fluxo de trabalho.  
  
 Os participantes de rastreamento são usados para obter os dados de rastreamento emissores de fluxo de trabalho e armazená-lo em mídias diferentes. Da mesma forma, qualquer pós-processamento no controle de que registros também podem ser realizados o participante de rastreamento.  
  
## <a name="example"></a>Exemplo  
 O exemplo de configuração a seguir mostra o participante de rastreamento ETW padrão que está sendo configurado no arquivo Web. config.  
  
 A ID do provedor que o participante de rastreamento ETW usa para gravar os registros de rastreamento no ETW é definida na **\<diagnostics>** seção. O participante de rastreamento tem um perfil associado a ele para especificar os registros de rastreamento que tiver assinado. Isso é definido pelo atributo **ProfileName** do **\<add>** elemento. Depois que elas são definidas, o participante de rastreamento é adicionado ao **\<etwTracking>** comportamento do serviço. Isso adicionará os participantes de rastreamento selecionado para extensões da instância de fluxo de trabalho, para que eles começam a receber os registros de rastreamento.  
  
```xml  
<configuration>
  <system.web>
    <compilation targetFrameworkMoniker=".NETFramework,Version=v4.0"/>
  </system.web>
  <system.serviceModel>
    <diagnostics etwProviderId="52A3165D-4AD9-405C-B1E8-7D9A257EAC9F" />
    <tracking>
      <participants>
        <add name="EtwTrackingParticipant"
             type="System.Activities.Tracking.EtwTrackingParticipant, System.Activities, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"
             profileName="HealthMonitoring_Tracking_Profile"/>
      </participants>
    </tracking>
    <behaviors>
      <serviceBehaviors>
        <behavior>
          <etwTracking profileName="Sample Tracking Profile"/>  
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Activities.Description.EtwTrackingBehavior>
- <xref:System.ServiceModel.Activities.Configuration.EtwTrackingBehaviorElement>
- [Acompanhamento e rastreamento de fluxo de trabalho](../../../windows-workflow-foundation/workflow-tracking-and-tracing.md)
- [Acompanhando participantes](../../../windows-workflow-foundation/tracking-participants.md)
