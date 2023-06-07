---
title: <resolver>
ms.date: 03/30/2017
ms.assetid: 0c00200c-f135-4e5c-a024-76b72bcbc021
ms.openlocfilehash: 0dc667f392595d895bd4f2773ab69777d7369446
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738732"
---
# \<resolver>
Especifica um resolvedor de pares que é usado para resolver uma ID de malha de mesmo nível para um conjunto de endereços de nó par que representa vários nós que participam da malha.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netPeerTcpBinding>**](netpeertcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<resolver>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<resolver mode="Auto/Custom/Pnrp"
          referralPolicy="DoNotShare/Service/Share">
</resolver>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`mode`|Uma cadeia de caracteres que especifica se a instância do resolvedor de pares associada a esse serviço é específica do PNRP, um resolvedor personalizado ou determinado automaticamente. Esse atributo é do tipo <xref:System.ServiceModel.PeerResolvers.PeerResolverMode> .|  
|`referralPolicy`|Uma cadeia de caracteres que especifica a maneira como as referências são compartilhadas entre os pares. Esse atributo é do tipo <xref:System.ServiceModel.PeerResolvers.PeerReferralPolicy> .|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<headers>](headers.md)|Especifica as configurações para um serviço resolvedor de pares personalizado.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação do [\<netPeerTcpBinding>](netpeertcpbinding.md) .|  
  
## <a name="remarks"></a>Comentários  
 Um resolvedor de nome de par é um serviço de descoberta usado por canais de mesmo nível para localizar nós de mesmo nível que participam de uma malha de mesmo nível. Ele também é usado para "registrar" um nó com uma malha de mesmo nível, o mecanismo pelo qual o nó par se torna conhecido e disponível na malha de pares. Para obter mais informações sobre resolvedores de pares, consulte [resolvedores de pares](../../../wcf/feature-details/peer-resolvers.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.PeerResolver>
- <xref:System.ServiceModel.PeerResolvers.PeerResolverSettings>
- <xref:System.ServiceModel.NetPeerTcpBinding.Resolver%2A>
- <xref:System.ServiceModel.Configuration.NetPeerTcpBindingElement.Resolver%2A>
- <xref:System.ServiceModel.Configuration.PeerResolverElement>
- [Resolvedores pares](../../../wcf/feature-details/peer-resolvers.md)
- [Adicionando um resolvedor personalizado a um aplicativo PeerChannel](https://docs.microsoft.com/previous-versions/ms730105(v=vs.90))
