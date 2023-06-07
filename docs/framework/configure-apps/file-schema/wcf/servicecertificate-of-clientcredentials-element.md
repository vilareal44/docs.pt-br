---
title: <serviceCertificate>do <clientCredentials> elemento
ms.date: 03/30/2017
ms.assetid: e50c0ac5-f0df-4c90-b54b-fc602c1f84ea
ms.openlocfilehash: 4c7489a171bdd5cb4b747ca99f1b7ff6dd65517b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399679"
---
# <a name="servicecertificate-of-clientcredentials-element"></a>\<serviceCertificate>do \<clientCredentials> elemento
Especifica um certificado a ser usado ao autenticar um serviço para o cliente.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCredentials>**](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceCertificate>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<serviceCertificate />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<defaultCertificate>](defaultcertificate-element.md)|Especifica um certificado X. 509 a ser usado quando um serviço ou STS não fornecer um por meio de um protocolo de negociação.|  
|[\<scopedCertificates>](scopedcertificates-element.md)|Representa uma coleção de certificados X. 509 fornecidos por serviços específicos (com escopo) para autenticação. Normalmente, essa coleção é usada para especificar os certificados de serviço para serviços de token de segurança em um cenário federado.|  
|[\<authentication>](authentication-of-servicecertificate-element.md)|Especifica comportamentos de autenticação para certificados de serviço usados por um cliente.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<clientCredentials>](clientcredentials.md)|Especifica as credenciais usadas pelo cliente para se autenticar em um serviço.|  
  
## <a name="remarks"></a>Comentários  
 Esse elemento de configuração especifica as configurações usadas pelo cliente para validar o certificado apresentado pelo serviço usando a autenticação SSL. Também contém um certificado para o serviço que é explicitamente configurado no cliente a ser usado para criptografar mensagens para o serviço usando a segurança de mensagem.  
  
 Os atributos do `serviceCertificate` elemento são idênticos aos atributos do [\<clientCertificate>](clientcertificate-of-clientcredentials-element.md) .  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ClientCredentialsElement>
- <xref:System.ServiceModel.Configuration.ClientCredentialsElement.ServiceCertificate%2A>
- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Description.ClientCredentials.ServiceCertificate%2A>
- <xref:System.ServiceModel.Configuration.X509RecipientCertificateClientElement>
- <xref:System.ServiceModel.Security.X509CertificateRecipientClientCredential>
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [Protegendo clientes](../../../wcf/securing-clients.md)
- [Trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md)
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
