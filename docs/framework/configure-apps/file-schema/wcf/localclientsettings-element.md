---
title: Elemento <localClientSettings>
ms.date: 03/30/2017
ms.assetid: 4680ace5-f4e1-4fcb-b9d8-a4a4af5cd7ae
ms.openlocfilehash: 3ec0394943c030a8866087c98a912682a2a2112e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400315"
---
# <a name="localclientsettings-element"></a>Elemento \<localClientSettings>
Especifica as configurações de segurança de um cliente local para esta associação.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<localClientSettings>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<security>
   <localClientSettings cacheCookies="Boolean"
                        cookieRenewalThresholdPercentage="Integer"
                        detectReplays="Boolean"
                        maxClockSkew="TimeSpan"
                        maxCookieCachingTime="TimeSpan"
                        reconnectTransportOnFailure="Boolean"
                        replayCacheSize="Integer"
                        replayWindow="TimeSpan"
                        sessionKeyRenewalInterval="TimeSpan"
                        sessionKeyRolloverInterval="TimeSpan"
                        timestampValidityDuration="TimeSpan" />
</security>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`cacheCookies`|Um valor booliano que especifica se o Caching de cookie está habilitado. O padrão é `false`.|  
|`cookieRenewalThresholdPercentage`|Um inteiro que especifica a porcentagem máxima de cookies que podem ser renovados. Esse valor deve estar entre 0 e 100, inclusive. O padrão é 90.|  
|`detectReplays`|Um valor booliano que especifica se os ataques de repetição no canal são detectados e tratados automaticamente. O padrão é `false`.|  
|`maxClockSkew`|Um <xref:System.TimeSpan> valor que especifica a diferença máxima de tempo entre os relógios do sistema das duas partes que se comunicam. O valor padrão é "00:05:00".<br /><br /> Quando esse valor é definido como o padrão, o receptor aceita mensagens com carimbos de hora de tempo de envio de até 5 minutos depois ou antes da hora em que a mensagem foi recebida. As mensagens que não passam no teste de tempo de envio são rejeitadas. Essa configuração é usada em conjunto com o `replayWindow` atributo.|  
|`maxCookieCachingTime`|Um <xref:System.TimeSpan> valor que especifica o tempo de vida máximo de cookies. O valor padrão é "10675199.02:48:05.4775807".|  
|`reconnectTransportOnFailure`|Um valor booliano que especifica se as conexões que usam o sistema de mensagens WS-Reliable tentarão se reconectar após falhas de transporte. O padrão é `true` , o que significa que tentativas infinitas de reconexão são tentadas. O ciclo é rompido pelo tempo limite de inatividade, o que faz com que o canal lance uma exceção quando não pode ser reconectado.|  
|`replayCacheSize`|Um inteiro positivo que especifica o número de nonces em cache usados para detecção de reprodução. Se esse limite for excedido, o nonce mais antigo será removido e um novo nonce será criado para a nova mensagem. O valor padrão é 500000.|  
|`replayWindow`|Um <xref:System.TimeSpan> que especifica a duração em que os nonces de mensagem individuais são válidos.<br /><br /> Após essa duração, uma mensagem enviada com o mesmo nonce como aquela enviada antes não será aceita. Esse atributo é usado em conjunto com o `maxClockSkew` atributo para evitar ataques de repetição. Um invasor pode repetir uma mensagem depois que sua janela de reprodução tiver expirado. Essa mensagem, no entanto, falharia no `maxClockSkew` teste que rejeitava mensagens com carimbos de data/hora em tempo de envio até um tempo especificado depois ou antes da hora em que a mensagem foi recebida.|  
|`sessionKeyRenewalInterval`|Um <xref:System.TimeSpan> que especifica a duração após a qual o iniciador renovará a chave da sessão de segurança. O padrão é "10:00:00".|  
|`sessionKeyRolloverInterval`|Um <xref:System.TimeSpan> que especifica o intervalo de tempo que uma chave de sessão anterior é válida em mensagens de entrada durante uma renovação de chave. O padrão é "00:05:00".<br /><br /> Durante a renovação da chave, o cliente e o servidor devem sempre enviar mensagens usando a chave mais atual disponível. Ambas as partes aceitarão mensagens de entrada protegidas com a chave de sessão anterior até que o tempo de substituição expire.|  
|`timestampValidityDuration`|Um positivo <xref:System.TimeSpan> que especifica a duração em que um carimbo de data/hora é válido. O padrão é "00:15:00".|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-custombinding.md)|Especifica as opções de segurança para uma associação personalizada.|  
|[\<secureConversationBootstrap>](secureconversationbootstrap.md)|Especifica os valores padrão usados para iniciar um serviço de conversa segura.|  
  
## <a name="remarks"></a>Comentários  
 As configurações são locais no sentido de que elas não são configurações derivadas da política de segurança do serviço.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.LocalClientSecuritySettingsElement>
- <xref:System.ServiceModel.Configuration.SecurityElementBase.LocalClientSettings%2A>
- <xref:System.ServiceModel.Channels.SecurityBindingElement.LocalClientSettings%2A>
- <xref:System.ServiceModel.Channels.LocalClientSecuritySettings>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [Como criar uma associação personalizada utilizando o SecurityBindingElement](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [Segurança de associação personalizada](../../../wcf/samples/custom-binding-security.md)
