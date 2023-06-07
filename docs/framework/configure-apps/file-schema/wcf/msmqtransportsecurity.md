---
title: <msmqTransportSecurity>
ms.date: 03/30/2017
ms.assetid: 092e911b-ab1b-4069-a26e-6134c3299e06
ms.openlocfilehash: 5899c609b3cf52c4a275ba6fb10c5826fcf37f1e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79153003"
---
# \<msmqTransportSecurity>
Especifica as configurações de segurança de transporte MSMQ para uma associação personalizada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<msmqIntegration>**](msmqintegration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<msmqTransportSecurity>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<msmqTransportSecurity msmqAuthenticationMode="None/Windows/Certificate"
                       msmqEncryptionAlgorithm="RC4Stream/AES"
                       msmqProtectionLevel="None/Sign/EncryptAndSign"
                       msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />
</msmqTransportSecurity>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`msmqAuthenticationMode`|Especifica como a mensagem deve ser autenticada pelo transporte MSMQ. Se for definido como `None` , o valor do `msmqProtectionLevel` atributo também deverá ser definido como `None` .<br /><br /> Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: nenhuma autenticação.<br />-Windows: o mecanismo de autenticação usa Active Directory para obter o certificado X. 509 para o SID associado à mensagem. Isso é usado para verificar a ACL da fila para garantir que o usuário tenha permissão para gravar na fila.<br />-Certificate: o canal Obtém o certificado do repositório de certificados.<br /><br /> O valor padrão é Windows. Esse atributo é do tipo <xref:System.ServiceModel.MsmqAuthenticationMode> .|  
|`msmqEncryptionAlgorithm`|Especifica o algoritmo a ser usado para criptografia de mensagem na conexão ao transferir mensagens entre gerenciadores de filas de mensagens. Os valores válidos incluem os seguintes:<br /><br /> - RC4Stream<br />-AES<br /><br /> O valor padrão é RC4Stream. Esse atributo é do tipo <xref:System.ServiceModel.MsmqEncryptionAlgorithm> .|  
|`msmqProtectionLevel`|Especifica como a mensagem é protegida no nível do transporte MSMQ. A criptografia garante a integridade da mensagem enquanto o EncryptAndSign garante a integridade e o não-repúdio da mensagem; ou seja, a mensagem é realmente proveniente do remetente e o remetente é quem dizem eles. Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: sem proteção.<br />-Sign: as mensagens são assinadas.<br />-EncryptAndSign: as mensagens são criptografadas e assinadas.<br /><br /> O valor padrão é Sign. Esse atributo é do tipo <xref:System.Net.Security.ProtectionLevel> .|  
|`msmqSecureHashAlgorithm`|Especifica o algoritmo a ser usado na computação do resumo como parte das assinaturas. Os valores válidos incluem os seguintes:<br /><br /> -MD5<br />-SHA1<br />-SHA256<br />-SHA512<br /><br /> O valor padrão é SHA1. Esse atributo é do tipo <xref:System.ServiceModel.MsmqSecureHashAlgorithm> .<br>Devido a problemas de colisão com MD5 e SHA1, a Microsoft recomenda SHA256 ou melhor.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<msmqIntegration>](msmqintegration.md)|Especifica as configurações necessárias para a interação com um remetente ou receptor de enfileiramento de mensagens (MSMQ).|  
|[\<msmqTransport>](msmqtransport.md)|Especifica as propriedades de comunicação de filas para um serviço de Windows Communication Foundation (WCF) que usa o protocolo MSMQ nativo.|  
  
## <a name="remarks"></a>Comentários  
 Para obter mais informações sobre segurança de transporte, consulte [segurança de transporte](../../../wcf/feature-details/transport-security.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationSecurity>
- <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Filas no WCF](../../../wcf/feature-details/queues-in-wcf.md)
- [Transportes](../../../wcf/feature-details/transports.md)
- [Selecionando um transporte](../../../wcf/feature-details/choosing-a-transport.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [Segurança de transporte](../../../wcf/feature-details/transport-security.md)
