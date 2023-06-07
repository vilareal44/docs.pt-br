---
title: <add> de <issuerChannelBehaviors>
ms.date: 03/30/2017
ms.assetid: 50710506-e28f-45dd-ab7e-bff6f44173db
ms.openlocfilehash: cf7ac2691ad1c641352a8047373ced538b19e983
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70398326"
---
# <a name="add-of-issuerchannelbehaviors"></a>\<add> de \<issuerChannelBehaviors>

Adiciona um comportamento de ponto de extremidade a ser usado ao se comunicar com um STS.

> [!NOTE]
> Se qualquer comportamento de ponto de extremidade contiver um [\<clientCredentials>](clientcredentials.md) elemento, uma exceção será lançada.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCredentials>**](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<issuedToken>**](issuedtoken.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<issuerChannelBehaviors>**](issuerchannelbehaviors-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  

## <a name="syntax"></a>Sintaxe

```xml
<add issuerAddress="string"
     behaviorConfiguration="string" />
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|issuerAddress|O URI do emissor do token de segurança com o qual se comunicar.|
|behaviorConfiguration|O nome de um comportamento de ponto de extremidade definido no mesmo arquivo de configuração.|

### <a name="child-elements"></a>Elementos filho

Nenhum.

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|[\<issuerChannelBehaviors>](issuerchannelbehaviors-element.md)|Contém uma coleção de comportamentos de ponto de extremidade de cliente do Windows Communication Foundation (WCF) a ser usada ao se comunicar com os serviços de token de serviço especificados.|

## <a name="remarks"></a>Comentários

`issuerAddress`contém o URI do serviço de token de segurança com o qual o cliente deseja se comunicar. `behaviorConfiguration`aponta para um comportamento de ponto de extremidade que o aplicativo usa nos canais criados por Windows Communication Foundation (WCF) para obter os tokens emitidos dos serviços de token de segurança.

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
- [\<issuerChannelBehaviors>](issuerchannelbehaviors-element.md)
