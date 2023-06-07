---
title: <issuedTokenAuthentication> de <serviceCredentials>
ms.date: 03/30/2017
ms.assetid: 5c2e288f-f603-4d13-839a-0fd6d1981bec
ms.openlocfilehash: 6d468a27ee05fb4dd8cf087d10e5d170783d3454
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400352"
---
# <a name="issuedtokenauthentication-of-servicecredentials"></a>\<issuedTokenAuthentication> de \<serviceCredentials>
Especifica um token personalizado emitido como uma credencial de serviço.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<issuedTokenAuthentication>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<issuedTokenAuthentication allowUntrustedRsaIssuers="Boolean"
                           audienceUriMode="Always/BearerKeyOnly/Never"
                           customCertificateValidatorType="namespace.typeName, [,AssemblyName] [,Version=version number] [,Culture=culture] [,PublicKeyToken=token]"
                           certificateValidationMode="ChainTrust/None/PeerTrust/PeerOrChainTrust/Custom"
                           revocationMode="NoCheck/Online/Offline"
                           samlSerializer="String"
                           trustedStoreLocation="CurrentUser/LocalMachine">
  <allowedAudienceUris>
    <add allowedAudienceUri="String" />
  </allowedAudienceUris>
  <knownCertificates>
    <add findValue="String"
         storeLocation="CurrentUser/LocalMachine"
         storeName=" CurrentUser/LocalMachine"
         x509FindType="FindByThumbprint/FindBySubjectName/FindBySubjectDistinguishedName/FindByIssuerName/FindByIssuerDistinguishedName/FindBySerialNumber/FindByTimeValid/FindByTimeNotYetValid/FindBySerialNumber/FindByTimeExpired/FindByTemplateName/FindByApplicationPolicy/FindByCertificatePolicy/FindByExtension/FindByKeyUsage/FindBySubjectKeyIdentifier" />
  </knownCertificates>
</issuedTokenAuthentication>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`allowedAudienceUris`|Obtém o conjunto de URIs de destino para o qual o <xref:System.IdentityModel.Tokens.SamlSecurityToken> token de segurança pode ser direcionado para ser considerado válido por uma <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator> instância. Para obter mais informações sobre como usar esse atributo, consulte <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator.AllowedAudienceUris%2A> .|  
|`allowUntrustedRsaIssuers`|Um valor booliano que especifica se os emissores de certificado RSA não confiáveis são permitidos.<br /><br /> Os certificados são assinados por autoridades de certificação (CAs) para verificar a autenticidade. Um emissor não confiável é uma AC que não é especificada para ser confiável para assinar certificados.|  
|`audienceUriMode`|Obtém um valor que especifica se o <xref:System.IdentityModel.Tokens.SamlSecurityToken> token de segurança <xref:System.IdentityModel.Tokens.SamlAudienceRestrictionCondition> deve ser validado. Esse valor é do tipo <xref:System.IdentityModel.Selectors.AudienceUriMode>. Para obter mais informações sobre como usar esse atributo, consulte <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator.AudienceUriMode%2A> .|  
|`certificateValidationMode`|Define o modo de validação de certificado. Um dos valores válidos de <xref:System.ServiceModel.Security.X509CertificateValidationMode> . Se definido como `Custom` , um `customCertificateValidator` também deverá ser fornecido. O padrão é `ChainTrust`.|  
|`customCertificateValidatorType`|Cadeia de caracteres opcional. Um tipo e um assembly usados para validar um tipo personalizado. Esse atributo deve ser definido quando `certificateValidationMode` é definido como `Custom` .|  
|`revocationMode`|Define o modo de revogação que especifica se uma verificação de revogação ocorre e se é executada online ou offline. Esse atributo é do tipo <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> .|  
|`samlSerializer`|Um atributo de cadeia de caracteres opcional que especifica o tipo de SamlSerializer que é usado para a credencial de serviço. O padrão é uma cadeia de caracteres vazia.|  
|`trustedStoreLocation`|Enumeração opcional. Um dos dois locais de armazenamento do sistema: `LocalMachine` ou `CurrentUser` .|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`knownCertificates`|Especifica uma coleção de <xref:System.ServiceModel.Configuration.X509CertificateTrustedIssuerElement> elementos que especifica emissores confiáveis para a credencial de serviço.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<serviceCredentials>](servicecredentials.md)|Especifica a credencial a ser usada na autenticação do serviço e as configurações relacionadas à validação da credencial do cliente.|  
  
## <a name="remarks"></a>Comentários  
 O cenário de token emitido tem três estágios. No primeiro estágio, um cliente que tenta acessar um serviço é chamado de um *serviço de token seguro*. O serviço de token seguro autentica o cliente e subsequentemente emite um token do cliente, normalmente um token SAML (Security Assertion Markup Language). O cliente retorna ao serviço com o token. O serviço examina o token de dados que permite que o serviço autentique o token e, portanto, o cliente. Para autenticar o token, o certificado que o serviço de token seguro usa deve ser conhecido pelo serviço.  
  
 Esse elemento é o repositório para qualquer um desses certificados de serviço de token seguro. Para adicionar certificados, use o [\<knownCertificates>](knowncertificates.md) . Insira um [\<add>](add-of-knowncertificates.md) para cada certificado, conforme mostrado no exemplo a seguir.  
  
```xml  
<issuedTokenAuthentication>
  <knownCertificates>
    <add findValue="www.contoso.com"
         storeLocation="LocalMachine"
         storeName="My"
         X509FindType="FindBySubjectName" />
  </knownCertificates>
</issuedTokenAuthentication>
```  
  
 Por padrão, os certificados devem ser obtidos de um serviço de token seguro. Esses certificados "conhecidos" garantem que somente clientes legítimos possam acessar um serviço.  
  
 Para obter mais informações sobre como usar esse elemento de configuração, consulte [How to: Configure Credentials on a serviço de Federação](../../../wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator>
- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator.AllowedAudienceUris%2A>
- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator.AudienceUriMode%2A>
- <xref:System.ServiceModel.Configuration.ServiceCredentialsElement.IssuedTokenAuthentication%2A>
- <xref:System.ServiceModel.Configuration.IssuedTokenServiceElement>
- <xref:System.ServiceModel.Description.ServiceCredentials.IssuedTokenAuthentication%2A>
- <xref:System.ServiceModel.Security.IssuedTokenServiceCredential>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Como configurar credenciais em um serviço de federação](../../../wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md)
