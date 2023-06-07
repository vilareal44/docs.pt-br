---
title: <authentication>do <clientCertificate> elemento
ms.date: 03/30/2017
ms.assetid: 4a55eea2-1826-4026-b911-b7cc9e9c8bfe
ms.openlocfilehash: 99084f6b7afbdd8586ee706cd6ec44b349d81ff2
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70398261"
---
# <a name="authentication-of-clientcertificate-element"></a>\<authentication>do \<clientCertificate> elemento
Especifica comportamentos de autenticação para certificados de cliente usados por um serviço.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCertificate>**](clientcertificate-of-servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<authentication>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<authentication customCertificateValidatorType="namespace.typeName, [,AssemblyName] [,Version=version number] [,Culture=culture] [,PublicKeyToken=token]"
                certificateValidationMode="ChainTrust/None/PeerTrust/PeerOrChainTrust/Custom"
                includeWindowsGroups="Boolean"
                mapClientCertificateToWindowsAccount="Boolean"
                revocationMode="NoCheck/Online/Offline"
                trustedStoreLocation="CurrentUser/LocalMachine" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|customCertificateValidatorType|Cadeia de caracteres opcional. Um tipo e um assembly usados para validar um tipo personalizado. Esse atributo deve ser definido quando `certificateValidationMode` é definido como `Custom` .|  
|certificateValidationMode|Enumeração opcional. Especifica um dos modos usados para validar as credenciais. Esse atributo é do <xref:System.ServiceModel.Security.X509CertificateValidationMode> tipo. Se definido como <xref:System.ServiceModel.Security.X509CertificateValidationMode.Custom?displayProperty=nameWithType> , um `customCertificateValidator` também deverá ser fornecido. O padrão é <xref:System.ServiceModel.Security.X509CertificateValidationMode.ChainTrust?displayProperty=nameWithType>.|  
|includeWindowsGroups|Booliano opcional. Especifica se os grupos do Windows estão incluídos no contexto de segurança. Definir esse atributo como `true` tem um impacto no desempenho, pois ele resulta em uma expansão de grupo completa. Defina esse atributo como `false` se você não precisar estabelecer a lista de grupos aos quais um usuário pertence.|  
|mapClientCertificateToWindowsAccount|Booliano. Especifica se o cliente pode ser mapeado para uma identidade do Windows usando o certificado. Active Directory deve estar habilitado para fazer isso.|  
|rerevocationmode|Enumeração opcional. Um dos modos usados para verificar as listas de certificados revogados (RCL). O padrão é `Online`. Esse valor é ignorado ao usar a segurança de transporte HTTP.|  
|trustedStoreLocation|Enumeração opcional. Um dos dois locais de armazenamento do sistema: `LocalMachine` ou `CurrentUser` . Esse valor é usado quando um certificado de serviço é negociado para o cliente. A validação é executada no repositório de **pessoas confiáveis** no local de armazenamento especificado. O padrão é `CurrentUser`.|  
  
## <a name="customcertificatevalidatortype-attribute"></a>Atributo customCertificateValidatorType  
  
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
|Enumeração|Um dos seguintes valores: NOCHECK, online, offline. Para obter mais informações, consulte [trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md).|  
  
## <a name="trustedstorelocation-attribute"></a>Atributo trustedStoreLocation  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Enumeração|Um dos seguintes valores: `LocalMachine` ou `CurrentUser`. O padrão é `CurrentUser`. Se o aplicativo cliente estiver sendo executado em uma conta do sistema, o certificado geralmente estará abaixo de `LocalMachine` . Se o aplicativo cliente estiver sendo executado sob uma conta de usuário, o certificado normalmente estará em `CurrentUser` .|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<clientCertificate>](clientcertificate-of-servicecredentials.md)|Define um certificado X. 509 usado para autenticar um cliente para um serviço.|  
  
## <a name="remarks"></a>Comentários  
 O `<authentication>` elemento corresponde à <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication> classe. Ele permite que você personalize como os clientes são autenticados. Você pode definir o `certificateValidationMode` atributo como `None` ,,, `ChainTrust` `PeerOrChainTrust` `PeerTrust` ou `Custom` . Por padrão, o nível é definido como `ChainTrust` , que especifica que cada certificado deve ser encontrado em uma hierarquia de certificados que terminam em uma *autoridade raiz* na parte superior da cadeia. Esse é o modo mais seguro. Você também pode definir o valor como `PeerOrChainTrust` , que especifica que os certificados emitidos por conta própria (peer Trust) são aceitos, bem como certificados que estão em uma cadeia confiável. Esse valor é usado ao desenvolver e depurar clientes e serviços porque certificados emitidos por conta própria não precisam ser comprados de uma autoridade confiável. Ao implantar um cliente, use o `ChainTrust` valor em vez disso.  
  
 Você também pode definir o valor como `Custom` . Quando definido como o `Custom` valor, você também deve definir o `customCertificateValidatorType` atributo para um assembly e tipo usado para validar o certificado. Para criar seu próprio validador personalizado, você deve herdar da <xref:System.IdentityModel.Selectors.X509CertificateValidator> classe abstrata. Para obter mais informações, consulte [como: criar um serviço que emprega um validador de certificado personalizado](../../../wcf/extending/how-to-create-a-service-that-employs-a-custom-certificate-validator.md).  
  
## <a name="example"></a>Exemplo  
 O código a seguir especifica um certificado X. 509 e um tipo de validação personalizado no `<authentication>` elemento.  
  
```xml  
<serviceBehaviors>
  <behavior name="myServiceBehavior">
    <clientCertificate>
      <certificate findValue="www.cohowinery.com"
                   storeLocation="CurrentUser"
                   storeName="TrustedPeople"
                   x509FindType="FindByIssuerName" />
      <authentication customCertificateValidatorType="MyTypes.Coho"
                      certificateValidationMode="Custom"
                      revocationMode="Offline"
                      includeWindowsGroups="false"
                      mapClientCertificateToWindowsAccount="true" />
    </clientCertificate>
  </behavior>
</serviceBehaviors>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication>
- <xref:System.ServiceModel.Security.X509CertificateValidationMode>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential.Authentication%2A>
- <xref:System.ServiceModel.Configuration.X509InitiatorCertificateServiceElement.Authentication%2A>
- <xref:System.ServiceModel.Configuration.X509ClientCertificateAuthenticationElement>
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [Como criar um serviço que utiliza um validador de certificado personalizado](../../../wcf/extending/how-to-create-a-service-that-employs-a-custom-certificate-validator.md)
- [Trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md)
