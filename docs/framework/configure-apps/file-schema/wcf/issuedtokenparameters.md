---
title: <issuedTokenParameters>
ms.date: 03/30/2017
ms.assetid: 120b3f37-7331-4816-b712-d6aab39655a4
ms.openlocfilehash: 8432463ff62e4b5e54a491b574cc6a5285efe220
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70397951"
---
# \<issuedTokenParameters>
Especifica os parâmetros para um token de segurança emitido em um cenário de segurança federada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<issuedTokenParameters>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<issuedTokenParameters defaultMessageSecurityVersion="System.ServiceModel.MessageSecurityVersion"
                       inclusionMode="AlwaysToInitiator/AlwaysToRecipient/Never/Once"
                       keySize="Integer"
                       keyType="AsymmetricKey/BearerKey/SymmetricKey"
                       tokenType="String">
  <additionalRequestParameters />
  <claimTypeRequirements>
    <add claimType="URI"
         isOptional="Boolean" />
  </claimTypeRequirements>
  <issuer address="String"
          binding="" />
  <issuerMetadata address="String" />
</issuedTokenParameters>
```  
  
## <a name="type"></a>Type  
 `Type`  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|defaultMessageSecurityVersion|Especifica as versões das especificações de segurança, (WS-Security, WS-Trust, WS-Secure Conversation e WS-Security Policy) que devem ser suportadas pela associação. Esse valor é do tipo <xref:System.ServiceModel.MessageSecurityVersion>.|  
|inclusõesmode|Especifica os requisitos de inclusão de token. Esse atributo é do tipo <xref:System.ServiceModel.Security.Tokens.SecurityTokenInclusionMode> .|  
|keySize|Um inteiro que especifica o tamanho da chave do token. O valor padrão é 256.|  
|keyType|Um valor válido de <xref:System.IdentityModel.Tokens.SecurityKeyType> que especifica o tipo de chave. O padrão é `SymmetricKey`.|  
|tokenType|Uma cadeia de caracteres que especifica o tipo de token. O padrão é "http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAML".|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<additionalRequestParameters>](additionalrequestparameters-element.md)|Uma coleção de elementos de configuração que especificam parâmetros de solicitação adicionais.|  
|[\<claimTypeRequirements>](claimtyperequirements-element.md)|Especifica uma coleção de tipos de declaração necessários.<br /><br /> Em um cenário federado, os serviços atendem aos requisitos de credenciais de entrada. Por exemplo, as credenciais de entrada devem ter um determinado conjunto de tipos de declaração. Cada elemento nessa coleção especifica os tipos de declarações obrigatórias e opcionais que devem aparecer em uma credencial federada.|  
|[\<issuer>](issuer-of-issuedtokenparameters.md)|Um elemento de configuração que especifica o ponto de extremidade que emite o token atual.|  
|[\<issuerMetadata>](issuermetadata-of-issuedtokenparameters.md)|Um elemento de configuração que especifica o endereço do ponto de extremidade dos metadados do emissor do token.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<secureConversationBootstrap>](secureconversationbootstrap.md)|Especifica os valores padrão usados para iniciar um serviço de conversa segura.|  
|[\<security>](security-of-custombinding.md)|Especifica as opções de segurança para uma associação personalizada.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenParameters>
- <xref:System.ServiceModel.Configuration.IssuedTokenParametersElement>
- <xref:System.ServiceModel.Configuration.SecurityElementBase.IssuedTokenParameters%2A>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [Como criar uma associação personalizada utilizando o SecurityBindingElement](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [Segurança de associação personalizada](../../../wcf/samples/custom-binding-security.md)
- [Identidade e autenticação de serviço](../../../wcf/feature-details/service-identity-and-authentication.md)
- [Federação e tokens emitidos](../../../wcf/feature-details/federation-and-issued-tokens.md)
- [Recursos de segurança com associações personalizadas](../../../wcf/feature-details/security-capabilities-with-custom-bindings.md)
- [Federação e tokens emitidos](../../../wcf/feature-details/federation-and-issued-tokens.md)
