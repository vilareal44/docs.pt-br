---
title: <serviceCredentials>
ms.date: 03/30/2017
ms.assetid: 96db336c-4f7a-4193-81a5-910b8ffd804f
ms.openlocfilehash: 90a34a4a52b4c7a2e67d733fecba132818cac4fc
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399631"
---
# \<serviceCredentials>
Especifica a credencial a ser usada na autenticação do serviço e nas configurações relacionadas à validação da credencial do cliente.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceCredentials>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<serviceCredentials type="String">
  <clientCertificate>
  </clientCertificate>
  <issuedTokenAuthentication>
  </issuedTokenAuthentication>
  <peer>
  </peer>
  <secureConversationAuthentication>
  </secureConversationAuthentication>
  <serviceCertificate>
  </serviceCertificate>
  <userNameAuthentication>
  </userNameAuthentication>
  <windowsAuthentication>
  </windowsAuthentication>
</serviceCredentials>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`type`|Uma cadeia de caracteres que especifica o tipo deste elemento de configuração.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<clientCertificate>](clientcertificate-of-servicecredentials.md)|Especifica o certificado a ser usado quando o certificado do cliente está disponível fora de banda. Esse elemento também especifica as configurações de validação de certificado do cliente. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.X509InitiatorCertificateServiceElement> .|  
|[\<issuedTokenAuthentication>](issuedtokenauthentication-of-servicecredentials.md)|Especifica o token emitido atual para este serviço. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.IssuedTokenServiceElement> .|  
|[\<peer>](peer-of-servicecredentials.md)|Especifica as credenciais atuais para um nó par. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.PeerCredentialElement> .|  
|[\<secureConversationAuthentication>](secureconversationauthentication-of-servicecredential.md)|Especifica as credenciais atuais para uma conversa segura. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.SecureConversationServiceElement> .|  
|[\<serviceCertificate>](servicecertificate-of-servicecredentials.md)|Especifica um certificado usado por um serviço para se identificar. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.X509RecipientCertificateServiceElement> .|  
|[\<userNameAuthentication>](usernameauthentication.md)|Especifica as configurações para validação de senha de nome de usuário. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.UserNameServiceElement> .|  
|[\<windowsAuthentication>](windowsauthentication-of-servicecredentials.md)|Especifica as configurações para validação de credenciais do Windows. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.WindowsServiceElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|Especifica um elemento de comportamento.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ServiceCredentialsElement>
- <xref:System.ServiceModel.Description.ServiceCredentials>
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
