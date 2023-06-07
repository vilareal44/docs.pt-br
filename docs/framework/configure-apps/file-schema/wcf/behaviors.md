---
title: <behaviors>
ms.date: 03/30/2017
ms.assetid: 0e5da4e6-1aa5-466c-924e-f10efee57f0b
ms.openlocfilehash: bcdd26f038b343040d81b0add83bf166a5e3151f
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74139695"
---
# \<behaviors>
Esse elemento define duas coleções filho chamadas `endpointBehaviors` e `serviceBehaviors` .  Cada coleção define elementos de comportamento consumidos por pontos de extremidade e serviços, respectivamente. Cada elemento do comportamento é identificado por seu exclusivo `name` atributo. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<behaviors>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<behaviors>
  <serviceBehaviors>
  </serviceBehaviors>
  <endpointBehaviors>
  </endpointBehaviors>
</behaviors>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<endpointBehaviors>](endpointbehaviors.md)|Esta seção de configuração representa todos os comportamentos definidos para um ponto de extremidade específico.|  
|[\<serviceBehaviors>](servicebehaviors.md)|Esta seção de configuração representa todos os comportamentos definidos para um serviço específico.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<system.serviceModel>](system-servicemodel.md)|O elemento raiz de todos os elementos de configuração de Windows Communication Foundation (WCF).|  
  
## <a name="remarks"></a>Comentários  
 Você pode usar o `<remove>` elemento para remover um comportamento específico da coleção. Para fazer isso, basta fornecer o nome do comportamento a ser removido no `name` atributo do `<remove>` elemento.  Você também pode usar o `<clear>` elemento para garantir que uma coleção de comportamentos comece vazia limpando todo o conteúdo da coleção.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.BehaviorsSection>
- <xref:System.ServiceModel.Configuration.EndpointBehaviorElementCollection>
- <xref:System.ServiceModel.Configuration.EndpointBehaviorElement>
- <xref:System.ServiceModel.Configuration.ServiceBehaviorElementCollection>
- <xref:System.ServiceModel.Configuration.ServiceBehaviorElement>
- [Configurando e estendendo o runtime com comportamentos](../../../wcf/extending/configuring-and-extending-the-runtime-with-behaviors.md)
- [Configurando comportamentos do cliente](../../../wcf/configuring-client-behaviors.md)
- [Especificando o comportamento em tempo de execução do cliente](../../../wcf/specifying-client-run-time-behavior.md)
- [Especificando o comportamento em tempo de execução do serviço](../../../wcf/specifying-service-run-time-behavior.md)
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
