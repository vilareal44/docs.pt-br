---
title: <security> de <msmqIntegrationBinding>
ms.date: 03/30/2017
ms.assetid: ae5c68a8-14a2-4c6e-b9e0-3e94e3e9135e
ms.openlocfilehash: 2268bf48a2b86c3b3b25db006e6f8f55ea33af73
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738688"
---
# <a name="security-of-msmqintegrationbinding"></a>\<security> de \<msmqIntegrationBinding>
Define as configurações de segurança de transporte para o canal de integração do serviço de enfileiramento de mensagens (MSMQ).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<msmqIntegrationBinding>**](msmqintegrationbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<msmqIntegrationBinding>
  <binding>
    <security mode="None/Transport">
      <transport msmqAuthenticationMode="None/Windows/Certificate"
                 msmqEncryptionAlgorithm="RC4Stream/AES"
                 msmqProtectionLevel="None/Sign/EncryptAndSign"
                 msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />
      <message algorithmSuite="Aes128/Aes192/Aes256/Rsa15Aes128/ Rsa15Aes256/TripleDes"
               clientCredentialType="None/Windows/UserName/Certificate/CardSpace"
               defaultProtectionLevel="None/Sign/EncryptAndSign" />
    </security>
  </binding>
</msmqIntegrationBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|mode|Especifica o tipo de segurança que controla a integridade, a confidencialidade e a autenticação com o canal de integração do enfileiramento de mensagens. Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: isso desabilita a segurança.<br />-Transporte: a proteção e a autenticação são oferecidas pelo transporte. Isso se aplica à segurança de mensagem entre os dois gerenciadores de fila. Não há nenhuma segurança oferecida entre o Gerenciador de aplicativos e filas. Os aplicativos MSMQ existentes são funcionalmente equivalentes a esse tipo de modo de segurança.<br /><br /> O valor padrão é `Transport`. Esse atributo é do tipo <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationSecurityMode> .|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<transport>](transport-of-msmqintegrationbinding.md)|Define as configurações de segurança para o transporte de integração do enfileiramento de mensagens. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|O elemento Binding do [\<msmqIntegrationBinding>](msmqintegrationbinding.md) .|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.MsmqIntegrationSecurityElement>
- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.MsmqIntegrationBindingElement.Security%2A>
- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationSecurity>
- [Filas no WCF](../../../wcf/feature-details/queues-in-wcf.md)
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
- [\<msmqIntegrationBinding>](msmqintegrationbinding.md)
