---
title: <security> de <netMsmqBinding>
ms.date: 03/30/2017
ms.assetid: 001d11a9-7439-498c-b09d-fca20eaf8cd3
ms.openlocfilehash: 7877fd59aff581eee5b62a1ca224dbf51c956069
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738671"
---
# <a name="security-of-netmsmqbinding"></a>\<security> de \<netMsmqBinding>
Define as configurações de segurança para uma associação MSMQ. Especifica se a segurança de transporte ou de SOAP está habilitada e, em caso afirmativo, qual modo de autenticação e níveis de proteção estão em uso.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netMsmqBinding>**](netmsmqbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<security mode="None/Transport/Message/Both">
  <transport msmqAuthenticationMode="None/WindowsDomain/Certificate"
             msmqEncryptionAlgorithm="RC4Stream/AES"
             msmqProtectionLevel="None/Sign/EncryptAndSign"
             msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />
    <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
             clientCredentialType="None/Windows/UserName/Certificate/CardSpace" />
</security>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|mode|Especifica o tipo de segurança que controla a integridade, a confidencialidade e a autenticação. Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: isso desabilita a segurança.<br />-Transporte: a proteção e a autenticação são oferecidas pelo transporte. Isso se aplica à segurança de mensagem entre os dois gerenciadores de fila. Não há nenhuma segurança oferecida entre o Gerenciador de aplicativos e filas. Os aplicativos MSMQ existentes são funcionalmente equivalentes a esse tipo de modo de segurança.<br />-Mensagem: especifica a segurança de aplicativo final. Não há nenhuma segurança oferecida na camada de transporte. Isso é semelhante à segurança oferecida por outras associações padrão.<br />-Both: oferece segurança na camada de mensagens de transporte e SOAP. A mesma credencial é necessária nos dois níveis.<br /><br /> O valor padrão é transporte. Esse atributo é do tipo <xref:System.ServiceModel.NetMsmqSecurityMode> .|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<message>](message-of-netmsmqbinding.md)|Define as configurações de segurança da mensagem SOAP. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.MessageSecurityOverMsmqElement> .|  
|[\<transport>](transport-of-netmsmqbinding.md)|Define as configurações de segurança para o transporte MSMQ. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|associação|O elemento Binding da[\<netMsmqBinding>](netmsmqbinding.md)|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.NetMsmqSecurityElement>
- <xref:System.ServiceModel.NetMsmqBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.NetMsmqBindingElement.Security%2A>
- <xref:System.ServiceModel.NetMsmqSecurity>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
- [Filas no WCF](../../../wcf/feature-details/queues-in-wcf.md)
