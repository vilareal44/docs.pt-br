---
title: <security> de <basicHttpBinding>
ms.date: 03/30/2017
ms.assetid: 6432708d-5465-4bd9-bfc2-466742db99cb
ms.openlocfilehash: c8e4f2d000a155eecd2a6c7faaaf4af525b24ca3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738707"
---
# <a name="security-of-basichttpbinding"></a>\<security> de \<basicHttpBinding>
Define os recursos de segurança do [\<basicHttpBinding>](basichttpbinding.md) .  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<basicHttpBinding>**](basichttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<security mode="Message/None/Transport/TransportWithCredential">
  <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
             proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
             realm="string" />
  <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
           clientCredentialType="Certificate/IssuedToken/None/UserName/Windows" />
</security>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|mode|Opcional. Especifica o tipo de segurança que é usado. O padrão é `None`. Esse atributo é do tipo <xref:System.ServiceModel.BasicHttpSecurityMode> .|  
  
## <a name="mode-attribute"></a>Atributo de modo  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Nenhum|-As mensagens não são protegidas durante a transferência.|  
|Transport|A segurança é fornecida usando o transporte HTTPS. As mensagens SOAP são protegidas usando HTTPS. O serviço é autenticado para o cliente usando o certificado X. 509 do serviço. O cliente é autenticado usando o ClientCredentialtype fornecido. Consulte o [\<transport>](transport-of-basichttpbinding.md) .|  
|Mensagem|A segurança é fornecida usando a segurança de mensagem SOAP. Por padrão, o corpo é criptografado e assinado. Para essa associação, o sistema requer que o certificado do servidor seja fornecido para o cliente fora da banda. O único válido `ClientCredentialType` para essa associação é `Certificate` .|  
|TransportWithMessageCredential|A integridade, a confidencialidade e a autenticação do servidor são fornecidas pela segurança do transporte. A autenticação de cliente é fornecida por meio de segurança da mensagem SOAP. Esse modo é relevante quando o usuário está Autenticando usando o nome de usuário/senha e há uma implantação HTTP existente para proteger a transferência de mensagens.|  
|TransportCredentialOnly|Esse modo não fornece confidencialidade e integridade de mensagens. Ele fornece autenticação de cliente baseada em http. Esse modo deve ser usado com cautela. Ele deve ser usado em ambientes em que a segurança de transporte está sendo fornecida por outros meios (como IPSec) e somente a autenticação do cliente é fornecida pela infraestrutura do WCF.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<transport>](transport-of-basichttpbinding.md)|Define as configurações de segurança de transporte para um serviço HTTP básico. Este elemento corresponde a <xref:System.ServiceModel.HttpTransportSecurity> .|  
|[\<message>](message-of-basichttpbinding.md)|Define as configurações de segurança da mensagem para um serviço HTTP básico. Este elemento corresponde a <xref:System.ServiceModel.BasicHttpMessageSecurity> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|associação|O elemento Binding do [\<basicHttpBinding>](basichttpbinding.md) .|  
  
## <a name="remarks"></a>Comentários  
 Por padrão, a mensagem SOAP não é protegida e o cliente não é autenticado. Esse elemento permite que você defina configurações de segurança adicionais para o `basicHttpBinding` elemento.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.BasicHttpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.BasicHttpBindingElement.Security%2A>
- <xref:System.ServiceModel.Configuration.BasicHttpSecurityElement>
- <xref:System.ServiceModel.BasicHttpSecurity>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Selecionando um tipo de credencial](../../../wcf/feature-details/selecting-a-credential-type.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
