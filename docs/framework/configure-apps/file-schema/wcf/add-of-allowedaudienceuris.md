---
title: <add> de <allowedAudienceUris>
ms.date: 03/30/2017
ms.assetid: 4e7b7637-e0ea-4a91-988f-6b6ef28d9fc3
ms.openlocfilehash: e15cb2d3e525d39a321bbe9760ddb8d72b02fffa
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70398351"
---
# <a name="add-of-allowedaudienceuris"></a>\<add> de \<allowedAudienceUris>
Adiciona um URI de destino para o qual o <xref:System.IdentityModel.Tokens.SamlSecurityToken> token de segurança pode ser direcionado para ser considerado válido por uma <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator> instância.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<issuedTokenAuthentication>**](issuedtokenauthentication-of-servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<allowedAudienceUris>**](allowedaudienceuris.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<allowedAudienceUris>
  <add allowedAudienceUri="String" />
</allowedAudienceUris>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|allowedAudienceUri|Uma cadeia de caracteres que contém um URI de destino para o qual o <xref:System.IdentityModel.Tokens.SamlSecurityToken> token de segurança pode ser direcionado para ser considerado válido por uma <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator> instância.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<allowedAudienceUris>](allowedaudienceuris.md)|Representa uma coleção de URIs de destino para as quais o <xref:System.IdentityModel.Tokens.SamlSecurityToken> token de segurança pode ser direcionado para ser considerado válido por uma <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator> instância.|  
  
## <a name="remarks"></a>Comentários  
 Você deve usar essa coleção em um aplicativo federado que utiliza um serviço de token de segurança (STS) que emite <xref:System.IdentityModel.Tokens.SamlSecurityToken> tokens de segurança. Quando o STS emite o token de segurança, ele pode especificar o URI dos serviços Web para os quais o token de segurança é destinado adicionando um <xref:System.IdentityModel.Tokens.SamlAudienceRestrictionCondition> ao token de segurança. Isso permite que o <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator> para o serviço Web de destinatário Verifique se o token de segurança emitido destina-se a esse serviço Web, especificando que essa verificação deve acontecer fazendo o seguinte:  
  
- Defina o `audienceUriMode` atributo de `<issuedTokenAuthentication>` como <xref:System.IdentityModel.Selectors.AudienceUriMode.Always> ou <xref:System.IdentityModel.Selectors.AudienceUriMode.BearerKeyOnly> .  
  
- Especifique o conjunto de URIs válidos adicionando os URIs a esta coleção.  
  
 Para obter mais informações, consulte <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator>.  
  
 Para obter mais informações sobre como usar esse elemento de configuração, consulte [How to: Configure Credentials on a serviço de Federação](../../../wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator>
- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator.AllowedAudienceUris%2A>
- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator.AudienceUriMode%2A>
- <xref:System.ServiceModel.Configuration.IssuedTokenServiceElement.AllowedAudienceUris%2A>
- <xref:System.ServiceModel.Configuration.AllowedAudienceUriElementCollection>
- <xref:System.ServiceModel.Configuration.AllowedAudienceUriElement>
- <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.AllowedAudienceUris%2A>
- [\<allowedAudienceUris>](allowedaudienceuris.md)
- [\<issuedTokenAuthentication>](issuedtokenauthentication-of-servicecredentials.md)
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Como configurar credenciais em um serviço de federação](../../../wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md)
