---
title: <security> de <wsHttpBinding>
ms.date: 03/30/2017
ms.assetid: 8658b162-2ddf-4162-a869-aa517a42288a
ms.openlocfilehash: b66b5228cab9dbc35502a13a2d0fe56ce4c6a18d
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738586"
---
# <a name="security-of-wshttpbinding"></a>\<security> de \<wsHttpBinding>
Representa os recursos de segurança do [\<wsHttpBinding>](wshttpbinding.md) .  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<wsHttpBinding>**](wshttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<security mode="Message/None/Transport/TransportWithMessageCredential">
  <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
             proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
             realm="String"
             defaultClientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
             defaultProxyCredentialType="Basic/Digest/None/Ntlm/Windows"
             defaultRealm="String" />
  <message clientCredentialType="Certificate/IssuedToken/None/UserName/Windows"
           algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
           establishSecurityContext="Boolean"
           negotiateServiceCredential="Boolean" />
</security>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|mode|Adicional. Especifica o tipo de segurança que é aplicado. O padrão é `Message`.<br />-Este atributo é do tipo <xref:System.ServiceModel.SecurityMode> .|  
  
## <a name="mode-attribute"></a>Atributo Mode  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Nenhum|A segurança é desabilitada.|  
|Transport|A proteção é fornecida usando HTTPS. O serviço precisa ser configurado com certificados SSL. A mensagem é totalmente protegida usando HTTPS e é autenticada pelo cliente usando o certificado SSL do serviço. A autenticação do cliente é controlada por meio do `ClientCredentials` atributo. do [\<transport>](transport-of-wshttpbinding.md) .|  
|Mensagem|A segurança é fornecida usando a segurança de mensagem SOAP. Por padrão, o corpo SOAP é criptografado e assinado. Esse modo oferece uma variedade de recursos, como se as credenciais de serviço estão disponíveis no cliente fora da banda, o pacote de algoritmos a ser usado e qual nível de proteção aplicar ao corpo da mensagem por meio da propriedade Security. Message. A autenticação do cliente é executada uma vez por sessão e os resultados da autenticação são armazenados em cache durante a sessão.|  
|TransportWithMessageCredential|Nesse modo, o HTTPS fornece integridade, confidencialidade e autenticação de servidor, e a segurança de mensagem SOAP fornece autenticação de cliente. Por padrão, a autenticação do cliente é executada uma vez por sessão e os resultados da autenticação são armazenados em cache durante a sessão.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<transport>](transport-of-wshttpbinding.md)|Define as configurações de segurança de transporte. Este elemento corresponde ao <xref:System.ServiceModel.Configuration.HttpTransportSecurityElement> tipo.|  
|[\<message>](message-of-wshttpbinding.md)|Define as configurações de segurança para a mensagem. Este elemento corresponde ao <xref:System.ServiceModel.Configuration.MessageSecurityOverHttpElement> tipo.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<wsHttpBinding>](wshttpbinding.md)|Uma associação segura para aplicativos de transporte HTTP.|  
  
## <a name="remarks"></a>Comentários  
 A classe WSHttpBinding foi projetada para interoperação com serviços que implementam especificações WS-*. A segurança de transporte para essa associação é protocolo SSL (SSL) sobre HTTP ou HTTPS.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.WSHttpSecurity>
- <xref:System.ServiceModel.WSHttpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.WSHttpBindingElement.Security%2A>
- <xref:System.ServiceModel.Configuration.WSHttpSecurityElement>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
