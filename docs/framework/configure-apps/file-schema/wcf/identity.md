---
title: <identity>
ms.date: 03/30/2017
ms.assetid: c1d2ae56-e231-4a07-9c3f-9f13381dc0d8
ms.openlocfilehash: 15c9e38a141fc294c47863b1a932711444ac079a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855146"
---
# \<identity>
O elemento Identity permite que um desenvolvedor cliente especifique em tempo de design a identidade esperada do serviço. No processo de handshake entre o cliente e o serviço, a infraestrutura Windows Communication Foundation (WCF) garantirá que a identidade do serviço esperado corresponda aos valores desse elemento e, portanto, possa ser autenticada. Para obter mais informações, consulte [identidade de serviço e autenticação](../../../wcf/feature-details/service-identity-and-authentication.md).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<client>**](client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpoint>**](endpoint-of-client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<identity>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<identity>
  <certificate encodedValue="String" />
  <certificateReference findValue="String"
                        isChainIncluded="Boolean"
                        storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
                        storeLocation="LocalMachine/CurrentUser"
                        X509FindType="Enumeration" />
  <dns value="String" />
  <rsa value="String" />
  <servicePrincipalName value="String" />
  <userPrincipalName value="String" />
</identity>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|certificado|Especifica as configurações de um certificado X. 509. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.CertificateElement> . Ele contém um atributo `encodedValue` que é uma cadeia de caracteres, que especifica o valor codificado por esse certificado.|  
|certificateReference|Especifica as configurações para a validação de certificado X. 509. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.CertificateReferenceElement> .|  
|dns|Especifica o DNS de um certificado X. 509 usado para autenticar um serviço. Esse elemento contém um atributo `value` que é uma cadeia de caracteres e contém a identidade real.|  
|rsa|Especifica o valor do campo RSA de um certificado X. 509 usado para autenticar um serviço para um cliente. Esse elemento contém um atributo `value` que é uma cadeia de caracteres e contém a identidade real|  
|servicePrincipalName|Especifica uma identidade de SPN (nome principal do servidor), que é o nome principal usado por um cliente para identificar exclusivamente uma instância de um serviço. Esse elemento contém um atributo `value` que é uma cadeia de caracteres e contém o nome da entidade de segurança real. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.ServicePrincipalNameElement> .|  
|userPrincipalName|Especifica uma identidade de UPN (nome principal do usuário), que é o tipo de nome de logon de um usuário em uma rede. O nome principal do usuário consiste no nome do objeto de usuário usado em Active Directory, seguido pelo símbolo arroba ( \@ ) e, em seguida, normalmente, o domínio pai do sistema de nome de domínio. Por exemplo, Jeff na árvore de domínio Fabrikam.com pode ter o nome principal do usuário [jeff@fabrikam.com](mailto:jeffsmith@fabrikam.com) .  Esse elemento contém um atributo `value` que é uma cadeia de caracteres e contém o nome da entidade de segurança real. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.UserPrincipalNameElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<custom>](custom.md)|Especifica um resolvedor de pares personalizado para um netPeerTcpBinding.|  
|[\<endpoint>](endpoint-element.md)|Configura os pontos de extremidade de serviço.|  
|[\<endpoint>desse\<client>](endpoint-of-client.md)|Configura os pontos de extremidade do canal.|  
|[\<issuer>](issuer.md)|Especifica o serviço de token de segurança (STS) para o serviço federado.|  
|[\<issuerMetadata>](issuermetadata.md)|Especifica o ponto de extremidade de metadados para o serviço de token de segurança (STS) de um serviço federado.|  
|[\<issuedTokenParameters>](issuedtokenparameters.md)|Define parâmetros para um token emitido em uma associação personalizada.|  
|[\<localIssuer>](localissuer.md)|Especifica um serviço de token de segurança (STS) local.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.IdentityElement>
- <xref:System.ServiceModel.EndpointAddress>
- <xref:System.ServiceModel.EndpointAddress.Identity%2A>
- [Identidade e autenticação de serviço](../../../wcf/feature-details/service-identity-and-authentication.md)
- [Pontos de extremidade: endereços, associações e contratos](../../../wcf/feature-details/endpoints-addresses-bindings-and-contracts.md)
