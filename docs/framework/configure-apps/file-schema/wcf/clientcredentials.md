---
title: <clientCredentials>
ms.date: 03/30/2017
ms.assetid: 1e6eef0d-a34e-4d74-b0f7-f65d2181858d
ms.openlocfilehash: f295fe48e194611c80b78c0c23ab3e66ea1c0b64
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400507"
---
# \<clientCredentials>
Especifica as credenciais usadas para autenticar o cliente para um serviço.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<clientCredentials>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<clientCredentials type="String"
                   supportInteractive="Boolean" >
  <clientCertificate>
  </clientCertificate>
  <digest>
  </digest>
  <isuedToken>
  </isuedToken>
  <peer>
  </peer>
  <serviceCertificate>
  </serviceCertificate>
  <windowsAuthentication>
  </windowsAuthentication>
</clientCredentials>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`supportInteractive`|Um valor booliano que especifica se um usuário interativo pode estar envolvido na seleção de uma credencial de cliente em tempo de execução. O valor padrão é `true`.|  
|`type`|Uma cadeia de caracteres que especifica o tipo deste elemento de configuração.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<clientCertificate>](clientcertificate-of-clientcredentials-element.md)|Especifica o certificado usado para autenticar o cliente para o serviço. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.X509InitiatorCertificateClientElement> .|  
|[\<httpDigest>](httpdigest-element.md)|Especifica um resumo usado para autenticar o cliente para o serviço. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.HttpDigestClientElement> .|  
|[\<issuedToken>](issuedtoken.md)|Especifica um tipo de token personalizado usado para autenticar o cliente para um serviço de token seguro (STS). Esse elemento é do tipo <xref:System.ServiceModel.Configuration.IssuedTokenClientElement> .|  
|[\<peer>](peer-of-clientcredentials-element.md)|Especifica uma credencial par atual. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.PeerCredentialElement> .|  
|[\<serviceCertificate>](servicecertificate-of-clientcredentials-element.md)|Especifica o certificado usado para autenticar o serviço para o cliente e fornece uma estrutura para definir as opções de certificado. Esse certificado deve ser fornecido fora de banda do serviço para o cliente. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.X509RecipientCertificateClientElement> .|  
|[\<windows>](windows-of-clientcredentials-element.md)|Especifica uma credencial do Windows. O padrão é a credencial do thread atual. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.WindowsClientElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|Especifica um comportamento de ponto de extremidade.|  
  
## <a name="remarks"></a>Comentários  
 As credenciais do cliente são usadas para autenticar o cliente para serviços em casos em que a autenticação mútua é necessária. Esta seção de configuração também pode ser usada para especificar certificados de serviço para cenários em que o cliente deve proteger mensagens para um serviço com o certificado do serviço.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ClientCredentialsElement>
- <xref:System.ServiceModel.Description.ClientCredentials>
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [Protegendo clientes](../../../wcf/securing-clients.md)
