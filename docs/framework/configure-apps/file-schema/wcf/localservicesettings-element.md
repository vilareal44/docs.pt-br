---
title: Elemento <localServiceSettings>
ms.date: 03/30/2017
ms.assetid: 0658549c-3f65-46dd-8c5c-9895441ed734
ms.openlocfilehash: 4883fd563ecf989d67c369085df4fc43d0c5f078
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400311"
---
# <a name="localservicesettings-element"></a>Elemento \<localServiceSettings>
Especifica as configurações de segurança de um serviço local para esta associação.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<localServiceSettings>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<security>
  <localServiceSettings detectReplays="Boolean"
                        inactivityTimeout="TimeSpan"
                        issuedCookieLifeTime="TimeSpan"
                        maxCachedCookies="Integer"
                        maxClockSkew="TimeSpan"
                        maxPendingSessions="Integer"
                        maxStatefulNegotiations="Integer"
                        negotiationTimeout="TimeSpan"
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
|`detectReplays`|Um valor booliano que especifica se os ataques de repetição no canal são detectados e tratados automaticamente. O padrão é `false`.|  
|`inactivityTimeout`|Um positivo <xref:System.TimeSpan> que especifica a duração da inatividade que o canal aguarda antes de atingir o tempo limite. O padrão é "01:00:00".|  
|`issuedCookieLifeTime`|Um <xref:System.TimeSpan> que especifica o tempo de vida emitido para todos os novos cookies de segurança. Os cookies que excederem seu tempo de vida serão reciclados e precisarão ser negociados novamente. O valor padrão é "10:00:00".|  
|`maxCachedCookies`|Um inteiro positivo que especifica o número máximo de cookies que podem ser armazenados em cache. O padrão é 1000.|  
|`maxClockSkew`|Um <xref:System.TimeSpan> valor que especifica a diferença máxima de tempo entre os relógios do sistema das duas partes que se comunicam. O valor padrão é "00:05:00".<br /><br /> Quando esse valor é definido como o padrão, o receptor aceita mensagens com carimbos de hora de tempo de envio de até 5 minutos depois ou antes da hora em que a mensagem foi recebida. As mensagens que não passam no teste de tempo de envio são rejeitadas. Essa configuração é usada em conjunto com o `replayWindow` atributo.|  
|`maxPendingSessions`|Um inteiro positivo que especifica o número máximo de sessões de segurança pendentes às quais o serviço dá suporte. Quando esse limite é atingido, todos os novos clientes recebem falhas de SOAP. O valor padrão é 1000.|  
|`maxStatefulNegotiations`|Um inteiro positivo que especifica o número de negociações de segurança que podem estar ativas simultaneamente. As sessões de negociação que excedem o limite são enfileiradas e só podem ser concluídas quando um espaço abaixo do limite se tornar disponível. O valor padrão é 1.024.|  
|`negotiationTimeout`|Um <xref:System.TimeSpan> que especifica o tempo de vida da política de segurança usada pelo canal. Quando o tempo expira, o canal é renegociado com o cliente para uma nova política de segurança. O valor padrão é "00:02:00".|  
|`reconnectTransportOnFailure`|Um valor booliano que especifica se as conexões que usam o sistema de mensagens WS-Reliable tentarão se reconectar após falhas de transporte. O padrão é `true` , o que significa que tentativas infinitas de reconexão são tentadas. O ciclo é rompido pelo tempo limite de inatividade, o que faz com que o canal lance uma exceção quando não pode ser reconectado.|  
|`replayCacheSize`|Um inteiro positivo que especifica o número de nonces em cache usados para detecção de reprodução. Se esse limite for excedido, o nonce mais antigo será removido e um novo nonce será criado para a nova mensagem. O valor padrão é 500000.|  
|`replayWindow`|Um <xref:System.TimeSpan> que especifica a duração em que os nonces de mensagem individuais são válidos.<br /><br /> Após essa duração, uma mensagem enviada com o mesmo nonce como aquela enviada antes não será aceita. Esse atributo é usado em conjunto com o `maxClockSkew` atributo para evitar ataques de repetição. Um invasor pode repetir uma mensagem depois que sua janela de reprodução tiver expirado. Essa mensagem, no entanto, falharia no `maxClockSkew` teste que rejeitava mensagens com carimbos de data/hora em tempo de envio até um tempo especificado depois ou antes da hora em que a mensagem foi recebida.|  
|`sessionKeyRenewalInterval`|Um <xref:System.TimeSpan> que especifica a duração após a qual o iniciador renovará a chave da sessão de segurança. O padrão é "10:00:00".|  
|`sessionKeyRolloverInterval`|Um <xref:System.TimeSpan> que especifica o intervalo de tempo que uma chave de sessão anterior é válida em mensagens de entrada durante uma renovação de chave. O padrão é "00:05:00".<br /><br /> Durante a renovação da chave, o cliente e o servidor devem sempre enviar mensagens usando a chave mais atual disponível. Ambas as partes aceitarão mensagens de entrada protegidas com a chave de sessão anterior até que o tempo de substituição expire.|  
|`timestampValidityDuration`|Um positivo <xref:System.TimeSpan> que especifica a duração em que um carimbo de data/hora é válido. O padrão é "00:15:00".|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-custombinding.md)|Especifica as opções de segurança para uma associação personalizada.|  
|[\<secureConversationBootstrap>](secureconversationbootstrap.md)|Especifica os valores padrão usados para iniciar um serviço de conversa segura.|  
  
## <a name="remarks"></a>Comentários  
 As configurações são locais porque não são publicadas como parte da política de segurança do serviço e não afetam a associação do cliente.  
  
 Os seguintes atributos do `localServiceSecuritySettings` elemento podem ajudar a mitigar um ataque de segurança de negação de serviço (dos):  
  
- `maxCachedCookies`: controla o número máximo de SecurityContextTokens com limite de tempo que são armazenados em cache pelo servidor após realizar a negociação de SPNEGO ou SSL.  
  
- `issuedCookieLifetime`: controla o tempo de vida do SecurityContextTokens emitido pelo servidor após a negociação de SPNEGO ou SSL. O servidor armazena em cache o SecurityContextTokens por esse período de tempo.  
  
- `maxPendingSessions`: controla o número máximo de conversas seguras que são estabelecidas no servidor, mas para as quais nenhuma mensagem de aplicativo foi processada. Essa cota impede que os clientes estabeleçam conversas seguras no serviço, fazendo com que o serviço Mantenha o estado para cada cliente, mas nunca usá-los.  
  
- `inactivityTimeout`: controla o tempo máximo que o serviço mantém uma conversa segura sem nunca receber uma mensagem de aplicativo nele. Essa cota impede que os clientes estabeleçam conversas seguras no serviço, fazendo com que o serviço Mantenha o estado para cada cliente, mas nunca usá-los.  
  
 Em uma sessão de conversa segura, observe que ambos `inactivityTimeout` e os `receiveTimeout` atributos na associação afetam o tempo limite da sessão. O menor dos dois determina quando ocorre o tempo limite.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.LocalServiceSecuritySettingsElement>
- <xref:System.ServiceModel.Configuration.SecurityElementBase.LocalServiceSettings%2A>
- <xref:System.ServiceModel.Channels.SecurityBindingElement.LocalServiceSettings%2A>
- <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [Como criar uma associação personalizada utilizando o SecurityBindingElement](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [Segurança de associação personalizada](../../../wcf/samples/custom-binding-security.md)
