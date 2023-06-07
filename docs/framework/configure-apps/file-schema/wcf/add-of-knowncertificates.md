---
title: <add> de <knownCertificates>
ms.date: 03/30/2017
ms.assetid: 128aaabe-3f1a-4c3b-b59f-898d0f02910f
ms.openlocfilehash: 29b067e6ec20992084f9ab3bab087222bdd56da2
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400616"
---
# <a name="add-of-knowncertificates"></a>\<add> de \<knownCertificates>
Adiciona um certificado X. 509 à coleção de certificados conhecidos.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<issuedTokenAuthentication>**](issuedtokenauthentication-of-servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<knownCertificates>**](knowncertificates.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<knownCertificates>
   <add findValue="String"
      storeLocation="CurrentUser/LocalMachine"
      storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
      x509FindType="FindByThumbprint/FindBySubjectName/FindBySubjectDistinguishedName/FindByIssuerName/FindByIssuerDistinguishedName/FindBySerialNumber/FindByTimeValid/FindByTimeNotYetValid/FindBySerialNumber/FindByTimeExpired/FindByTemplateName/FindByApplicationPolicy/FindByCertificatePolicy/FindByExtension/FindByKeyUsage/FindBySubjectKeyIdentifier"/>
</knownCertificates>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|findValue|Cadeia de caracteres. O valor a ser procurado.|  
|storeLocation|Enumeração. Um dos dois locais de armazenamento para pesquisar.|  
|storeName|Enumeração. Uma das lojas de sistema para pesquisa.|  
|x509FindType|Enumeração. Um dos campos de certificado a serem pesquisados.|  
  
## <a name="findvalue-attribute"></a>Atributo FindValue  
  
|Valor|Descrição|  
|-----------|-----------------|  
|String|O valor depende do campo (especificado pelo atributo X509FindType) que está sendo pesquisado. Por exemplo, se estiver procurando uma impressão digital, o valor deverá ser uma cadeia de caracteres de números hexadecimais.|  
  
## <a name="x509findtype-attribute"></a>Atributo x509FindType  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Enumeração|Os valores incluem: FindByThumbprint, FindBySubjectName, FindBySubjectDistinguishedName, FindByIssuerName, FindByIssuerDistinguishedName, FindBySerialNumber, FindByTimeValid, FindByTimeNotYetValid, FindBySerialNumber, FindByTimeExpired, FindByTemplateName, FindByApplicationPolicy, FindByCertificatePolicy, FindByExtension, FindByKeyUsage, FindBySubjectKeyIdentifier.|  
  
## <a name="storelocation-attribute"></a>Atributo storeLocation  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Enumeração|CurrentUser ou LocalMachine.|  
  
## <a name="storename-attribute"></a>Atributo storeName  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Enumeração|Os valores incluem: catálogo, AuthRoot, CertificateAuthority, não permitido, My, root, TrustedPeople e TrustedPublisher.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<knownCertificates>](knowncertificates.md)|Representa uma coleção de certificados X. 509 que são fornecidos por um serviço de token de segurança (STS) para validação de tokens de segurança.|  
  
## <a name="remarks"></a>Comentários  
 O cenário de token emitido tem três estágios. No primeiro estágio, um cliente que tenta acessar um serviço é chamado de um *serviço de token seguro*. O serviço de token seguro autentica o cliente e subsequentemente emite um token do cliente, normalmente um token SAML (Security Assertion Markup Language). O cliente retorna ao serviço com o token. O serviço examina o token de dados que permite que o serviço autentique o token e, portanto, o cliente. Para autenticar o token, o certificado que o serviço de token seguro usa deve ser conhecido pelo serviço.  
  
 O [\<issuedTokenAuthentication>](issuedtokenauthentication-of-servicecredentials.md) elemento é o repositório para qualquer certificado de serviço de token seguro. Para adicionar certificados, use o [\<knownCertificates>](knowncertificates.md) . Insira um [ \<add> \<knownCertificates> elemento de elemento](add-of-knowncertificates.md) para cada certificado, conforme mostrado no exemplo a seguir.  
  
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
  
 Para revisar as condições necessárias para um cliente ser autenticado por um serviço federado, bem como mais informações sobre como usar esse elemento de configuração, consulte [como configurar credenciais em um serviço de Federação](../../../wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md). Para obter mais informações sobre cenários federados, consulte [Federação e tokens emitidos](../../../wcf/feature-details/federation-and-issued-tokens.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir adiciona o certificado ao repositório para qualquer certificado STS.  
  
```xml  
<serviceBehaviors>
  <behavior name="myServiceBehavior">
    <serviceCredentials>
      <issuedTokenAuthentication>
        <knownCertificates>
          <add findValue="www.contoso.com"
               storeLocation="LocalMachine"
               storeName="CertificateAuthority"
               x509FindType="FindByIssuerName" />
        </knownCertificates>
      </issuedTokenAuthentication>
    </serviceCredentials>
  </behavior>
</serviceBehaviors>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator>
- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator.AllowedAudienceUris%2A>
- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator.AudienceUriMode%2A>
- <xref:System.ServiceModel.Configuration.IssuedTokenServiceElement.KnownCertificates%2A>
- <xref:System.ServiceModel.Configuration.X509CertificateTrustedIssuerElementCollection>
- <xref:System.ServiceModel.Configuration.X509CertificateTrustedIssuerElement>
- <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A>
- [\<knownCertificates>](knowncertificates.md)
- [Trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md)
- [Federação e tokens emitidos](../../../wcf/feature-details/federation-and-issued-tokens.md)
- [Como configurar credenciais em um serviço de federação](../../../wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md)
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
