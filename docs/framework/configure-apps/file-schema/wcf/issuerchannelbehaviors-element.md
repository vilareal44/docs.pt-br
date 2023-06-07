---
title: Elemento <issuerChannelBehaviors>
ms.date: 03/30/2017
ms.assetid: f7378673-8e9b-45b2-98d1-cf5dccdd8c40
no-loc:
- <system.serviceModel>
- <behaviors>
- <endpointBehaviors>
- <behavior>
- <clientCredentials>
- <issuedToken>
- <issuerChannelBehaviors>
- <dataContractSerializer>
ms.openlocfilehash: cbbfb9d3b5af47a360aa82cf837cd6749f61b641
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70893150"
---
# <a name="issuerchannelbehaviors-element"></a>Elemento \<issuerChannelBehaviors>

Contém uma coleção de comportamentos de ponto de extremidade de cliente do Windows Communication Foundation (WCF) (definida na configuração) a ser usada na comunicação com os serviços de token de serviço especificados. Os comportamentos definidos não podem incluir nenhum [\<clientCredentials>](clientcredentials.md) elemento.

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<system.serviceModel>](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[\<behaviors>](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<endpointBehaviors>](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<behavior>](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<clientCredentials>](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[\<issuedToken>](issuedtoken.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<issuerChannelBehaviors>

## <a name="syntax"></a>Sintaxe

```xml
<issuerChannelBehaviors>
  <add behaviorConfiguration="string"
       issuerAddress="string" />
</issuerChannelBehaviors>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

Nenhum.

### <a name="child-elements"></a>Elementos filho

|Elemento|Descrição|
|-------------|-----------------|
|[\<add>](add-of-issuerchannelbehaviors.md)|Adiciona um comportamento à coleção.|

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|[\<issuedToken>](issuedtoken.md)|Especifica um token personalizado usado para autenticar um cliente para um serviço.|

## <a name="remarks"></a>Comentários

Use este elemento quando quaisquer comportamentos (que não sejam os comportamentos que incluem `<clientCredentials>` elementos) devem ser usados para se comunicar com um serviço. Por exemplo, se um [\<dataContractSerializer>](datacontractserializer-element.md) elemento de comportamento deve ser incluído.

## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.IssuedTokenClientElement.IssuerChannelBehaviors%2A>
- <xref:System.ServiceModel.Configuration.IssuedTokenClientBehaviorsElement>
- <xref:System.ServiceModel.Configuration.IssuedTokenClientBehaviorsElementCollection>
- <xref:System.ServiceModel.Security.IssuedTokenClientCredential.IssuerChannelBehaviors%2A>
- [Identidade e autenticação de serviço](../../../wcf/feature-details/service-identity-and-authentication.md)
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [Federação e tokens emitidos](../../../wcf/feature-details/federation-and-issued-tokens.md)
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Protegendo clientes](../../../wcf/securing-clients.md)
- [Como criar um cliente federado](../../../wcf/feature-details/how-to-create-a-federated-client.md)
- [Como configurar um emissor local](../../../wcf/feature-details/how-to-configure-a-local-issuer.md)
- [Federação e tokens emitidos](../../../wcf/feature-details/federation-and-issued-tokens.md)
