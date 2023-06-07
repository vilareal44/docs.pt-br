---
title: <authentication>do <serviceCertificate> elemento
ms.date: 03/30/2017
ms.assetid: 733b67b4-08a1-4d25-9741-10046f9357ef
ms.openlocfilehash: 29170f032469b4d55b50f57ca06ce403a5aeaf2c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70398223"
---
# <a name="authentication-of-servicecertificate-element"></a>\<authentication>do \<serviceCertificate> elemento
Especifica as configurações usadas pelo proxy do cliente para autenticar certificados de serviço que são obtidos usando a negociação SSL/TLS.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCredentials>**](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCertificate>**](servicecertificate-of-clientcredentials-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<authentication>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<authentication customCertificateValidatorType="String"
                certificateValidationMode="None/PeerTrust/ChainTrust/PeerOrChainTrust/Custom"
                revocationMode="NoCheck/Online/Offline"
                trustedStoreLocation="LocalMachine/CurrentUser" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|customCertificateValidatorType|Cadeia de caracteres. Um tipo e um assembly usados para validar um tipo personalizado.|  
|certificateValidationMode|Especifica um dos três modos usados para validar as credenciais. Se definido como `Custom` , um customCertificateValidator também deverá ser fornecido. O padrão é `ChainTrust`.|  
|rerevocationmode|Um dos modos usados para verificar se há listas de certificados revogados (CRL). O padrão é `Online`.|  
|trustedStoreLocation|Um dos dois locais de armazenamento do sistema: `LocalMachine` ou `CurrentUser` . Esse valor é usado quando um certificado de serviço é negociado para o cliente. A validação é executada no repositório de **pessoas confiáveis** no local de armazenamento especificado. O padrão é `CurrentUser`.|  
  
## <a name="customcertificatevalidator-attribute"></a>Atributo customCertificateValidator  
  
|Valor|Descrição|  
|-----------|-----------------|  
|String|Especifica o nome do tipo e o assembly e outros dados usados para localizar o tipo.|  
  
## <a name="certificatevalidationmode-attribute"></a>Atributo certificateValidationMode  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Enumeração|Um dos seguintes valores: None, PeerTrust, ChainTrust, PeerOrChainTrust, Custom.<br /><br /> Para obter mais informações, consulte [trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md).|  
  
## <a name="revocationmode-attribute"></a>Atributo rerevocationmode  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Enumeração|Um dos seguintes valores: NOCHECK, online, offline.<br /><br /> Para obter mais informações, consulte [trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md).|  
  
## <a name="trustedstorelocation-attribute"></a>Atributo trustedStoreLocation  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Enumeração|Um dos seguintes valores: LocalMachine ou CurrentUser. O padrão é CurrentUser. Se o aplicativo cliente estiver sendo executado em uma conta do sistema, o certificado normalmente estará em LocalMachine. Se o aplicativo cliente estiver sendo executado em uma conta de usuário, o certificado normalmente será em CurrentUser.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<serviceCertificate>](servicecertificate-of-clientcredentials-element.md)|Especifica um certificado a ser usado ao autenticar um serviço para o cliente.|  
  
## <a name="remarks"></a>Comentários  
 O `certificateValidationMode` atributo desse elemento de configuração especifica o nível de confiança usado para autenticar certificados. Por padrão, o nível é definido como `ChainTrust` , que especifica que cada certificado deve ser encontrado em uma hierarquia de certificados que termina em uma autoridade de certificação confiável na parte superior da cadeia. Esse é o modo mais seguro. Você também pode definir o valor como `PeerOrChainTrust` , que especifica que os certificados emitidos por conta própria (peer Trust) são aceitos, bem como certificados que estão em uma cadeia confiável. Esse valor é usado ao desenvolver e depurar clientes e serviços porque certificados emitidos por conta própria não precisam ser comprados de uma autoridade confiável. Ao implantar um cliente, use o `ChainTrust` valor em vez disso. Você também pode definir o valor para `Custom` ou `None` . Para usar o valor `Custom`, você também deverá definir o atributo `customCertificateValidator` para um assembly e um tipo usados para validar o certificado. Para criar seu próprio validador personalizado, você deve herdar da classe abstrata X509CertificateValidator. Para obter mais informações, consulte [como: criar um serviço que emprega um validador de certificado personalizado](../../../wcf/extending/how-to-create-a-service-that-employs-a-custom-certificate-validator.md).  
  
 O `revocationMode` atributo especifica como os certificados são verificados para revogação. O padrão é `online` que indica que os certificados serão verificados automaticamente para revogação. Para obter mais informações, consulte [trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir executa duas tarefas. Ele especifica primeiro um certificado de serviço para o cliente usar ao se comunicar com pontos de extremidade cujo nome de domínio está `www.contoso.com` sobre o protocolo http. Em segundo lugar, ele especifica o modo de revogação e o local de armazenamento usados durante a autenticação.  
  
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

- <xref:System.ServiceModel.Configuration.X509RecipientCertificateClientElement>
- <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential>
- <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential.Authentication%2A>
- <xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication>
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [Trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md)
- [Como criar um serviço que utiliza um validador de certificado personalizado](../../../wcf/extending/how-to-create-a-service-that-employs-a-custom-certificate-validator.md)
- [\<authentication>](authentication-of-clientcertificate-element.md)
- [Protegendo clientes](../../../wcf/securing-clients.md)
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
