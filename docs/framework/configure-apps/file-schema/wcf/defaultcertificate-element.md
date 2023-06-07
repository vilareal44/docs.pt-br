---
title: Elemento <defaultCertificate>
ms.date: 03/30/2017
ms.assetid: f1ddf364-9a00-45d3-b989-ff381c154ce6
ms.openlocfilehash: cce236bf80fa00f01a3b5f4680d975f83fde0c16
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400423"
---
# <a name="defaultcertificate-element"></a>Elemento \<defaultCertificate>
Especifica um certificado X. 509 a ser usado quando um serviço ou STS não fornecer um por meio de um protocolo de negociação.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCredentials>**](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCertificate>**](servicecertificate-of-clientcredentials-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<defaultCertificate>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<defaultCertificate findValue="String"
                    storeLocation=" CurrentUser/LocalMachine"
                    storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
                    x509FindType="FindByThumbPrint/FindBySubjectName/FindBySubjectDistinguishedName/FindByIssuerName/FindByIssuerDistinguishedName/FindBySerialiNumber/FindByTimeValid/FindByTimeNotYetValid/FindByTimeExpired/FindByTemplateName/FindByApplicationPolicy/FindByCertificatePolicy/FindByExtension/FindByKeyUsage/FindBySubjectKeyIdentifier" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|findValue|Cadeia de caracteres. O valor a ser procurado.|  
|x509FindType|Enumeração. Um dos campos de certificado a serem pesquisados.|  
|storeLocation|Enumeração. Um dos dois locais de armazenamento do sistema para pesquisar.|  
|storeName|Enumeração. Uma das lojas de sistema para pesquisa.|  
  
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
|[\<serviceCertificate>](servicecertificate-of-clientcredentials-element.md)|Especifica um certificado a ser usado ao autenticar um serviço para o cliente.|  
  
## <a name="remarks"></a>Comentários  
 Para associações que usam segurança de mensagem baseada em certificado, o certificado especificado por esse elemento de configuração é usado para criptografar mensagens para o serviço e deve ser usado pelo serviço para assinar respostas para o cliente. Ele armazena um único certificado a ser usado quando nenhum certificado é especificado por um serviço.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir especifica um certificado a ser usado para pontos de extremidade cujo URI começa com `http://www.contoso.com` e um certificado a ser usado para todos os outros pontos de extremidade que não executam a negociação de certificado.  
  
```xml  
<serviceCertificate>
  <defaultCertificate findValue="www.contoso.com"
                      storeLocation="LocalMachine"
                      storeName="TrustedPeople"
                      x509FindType="FindByIssuerDistinguishedName" />
  <scopedCertificates>
    <add targetUri="http://www.contoso.com"
         findValue="www.contoso.com"
         storeLocation="LocalMachine"
         storeName="Root"
         x509FindType="FindByIssuerName" />
  </scopedCertificates>
  <authentication revocationMode="Online"
                  trustedStoreLocation="LocalMachine" />
</serviceCertificate>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.X509DefaultServiceCertificateElement>
- <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential>
- <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.DefaultCertificate%2A>
- [Trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md)
- [\<authentication>](authentication-of-clientcertificate-element.md)
- [Protegendo clientes](../../../wcf/securing-clients.md)
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
