---
title: <transport> de <msmqIntegrationBinding>
ms.date: 03/30/2017
ms.assetid: 054579e3-7fdd-47df-99ca-952706ba5c8e
ms.openlocfilehash: 1cb165fed9266307335482166116c4c1d62efe7e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152951"
---
# <a name="transport-of-msmqintegrationbinding"></a>\<transport> de \<msmqIntegrationBinding>
Define as configurações de segurança para o transporte de integração do enfileiramento de mensagens.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<msmqIntegrationBinding>**](msmqintegrationbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-msmqintegrationbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<transport>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<security>
  <transport msmqAuthenticationMode="None/WindowsDomain/Certificate"
             msmqEncryptionAlgorithm="RC4Stream/AES"
             msmqProtectionLevel="None/Sign/EncryptAndSign"
             msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />
</security>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`msmqAuthenticationMode`|Especifica como a mensagem deve ser autenticada pelo transporte MSMQ. Se for definido como `None` , o valor do `msmqProtectionLevel` atributo também deverá ser definido como `None` .<br /><br /> Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: nenhuma autenticação.<br />-WindowsDomain: o mecanismo de autenticação usa Active Directory para obter o certificado X. 509 para o SID associado à mensagem. Isso é usado para verificar a ACL da fila para garantir que o usuário tenha permissão para gravar na fila.<br />-Certificate: o canal Obtém o certificado do repositório de certificados.<br /><br /> O valor padrão é WindowsDomain. Esse atributo é do tipo <xref:System.ServiceModel.MsmqAuthenticationMode> .|  
|`msmqEncryptionAlgorithm`|Especifica o algoritmo a ser usado para criptografia de mensagem na conexão ao transferir mensagens entre gerenciadores de filas de mensagens. Os valores válidos incluem os seguintes:<br /><br /> - RC4Stream<br />-AES<br /><br /> O valor padrão é RC4Stream. Esse atributo é do tipo <xref:System.ServiceModel.MsmqEncryptionAlgorithm> .|  
|`msmqProtectionLevel`|Especifica como a mensagem é protegida no nível do transporte MSMQ. A criptografia garante a integridade da mensagem enquanto o EncryptAndSign garante a integridade e o não-repúdio da mensagem; ou seja, a mensagem é realmente proveniente do remetente e o remetente é quem dizem eles.<br /><br /> -Os valores válidos incluem o seguinte:<br />-Nenhum: sem proteção.<br />-Sign: as mensagens são assinadas.<br />-EncryptAndSign: as mensagens são criptografadas e assinadas.<br /><br /> O valor padrão é Sign. Esse atributo é do tipo ProtectionLevel.|  
|`msmqSecureHashAlgorithm`|-Especifica o algoritmo a ser usado na computação do resumo como parte das assinaturas. Os valores válidos incluem os seguintes:<br />-MD5<br />-SHA1<br />-SHA256<br />-SHA512<br /><br /> O valor padrão é SHA1. Esse atributo é do tipo <xref:System.ServiceModel.MsmqSecureHashAlgorithm> .<br>Devido a problemas de colisão com MD5 e SHA1, a Microsoft recomenda SHA256 ou melhor.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-basichttpbinding.md)|Define as configurações de segurança para uma associação MSMQ.|  
  
## <a name="remarks"></a>Comentários  
 Esse elemento encapsula as configurações de segurança para o transporte de integração do enfileiramento de mensagens. As configurações são as mesmas para a integração do enfileiramento de mensagens e para os transportes em fila. Ele permite que você defina o modo de autenticação, o algoritmo de criptografia, o algoritmo de hash seguro e o nível de proteção.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement>
- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationSecurity.Transport%2A>
- <xref:System.ServiceModel.Configuration.MsmqIntegrationSecurityElement.Transport%2A>
- <xref:System.ServiceModel.MsmqTransportSecurity>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
