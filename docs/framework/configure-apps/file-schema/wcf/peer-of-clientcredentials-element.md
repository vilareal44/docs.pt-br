---
title: <peer>do <clientCredentials> elemento
ms.date: 03/30/2017
ms.assetid: 505bd987-0042-4622-b68e-94f439729d53
ms.openlocfilehash: dce7ef64de1e3eb248e3553c97cbce8e9b205b4c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400093"
---
# <a name="peer-of-clientcredentials-element"></a>\<peer>do \<clientCredentials> elemento
Especifica as credenciais usadas ao autenticar clientes ponto a ponto.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCredentials>**](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<peer>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<peer>
  <certificate />
  <peerAuthentication />
  <messageSenderAuthentication />
</peer>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<certificate>](certificate-element.md)|Especifica um certificado X. 509 a ser usado para assinar e criptografar mensagens para clientes ponto a ponto. .|  
|[\<peerAuthentication>](peerauthentication-element.md)|Especifica opções de autenticação para clientes pares.|  
|[\<messageSenderAuthentication>](messagesenderauthentication-element.md)|Especifica opções de autenticação para remetentes de mensagens.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<clientCredentials>](clientcredentials.md)|Especifica as credenciais usadas para autenticar um cliente para um serviço.|  
  
## <a name="remarks"></a>Comentários  
 Esse elemento de configuração especifica as credenciais que um nó par usa para se autenticar em outros nós na malha, bem como configurações de autenticação que um nó par usa para autenticar outros nós de mesmo nível. Para obter mais informações, consulte [autenticação de mensagens de canal de emparelhamento](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/aa967730(v=vs.90)) e [protegendo os aplicativos de canal de mesmo nível](../../../wcf/feature-details/securing-peer-channel-applications.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ClientCredentialsElement>
- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Configuration.PeerCredentialElement>
- <xref:System.ServiceModel.Configuration.ClientCredentialsElement.Peer%2A>
- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Description.ClientCredentials.Peer%2A>
- <xref:System.ServiceModel.Security.PeerCredential>
- [Rede ponto a ponto](../../../wcf/feature-details/peer-to-peer-networking.md)
- [Protegendo clientes](../../../wcf/securing-clients.md)
- [Autenticação de mensagem de canal par](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/aa967730(v=vs.90))
- [Autenticação personalizada do canal par](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751447(v=vs.90))
- [Protegendo aplicativos de canal par](../../../wcf/feature-details/securing-peer-channel-applications.md)
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
