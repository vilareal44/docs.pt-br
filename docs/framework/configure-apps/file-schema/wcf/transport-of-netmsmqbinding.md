---
title: <transport> de <netMsmqBinding>
ms.date: 03/30/2017
ms.assetid: 72e1b338-39f0-4af1-a5d9-7a2fb79f6a0b
ms.openlocfilehash: cc47c01cccc931e81ba57ab37ad9e3accfaa693b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79152925"
---
# <a name="transport-of-netmsmqbinding"></a>\<transport> de \<netMsmqBinding>
Define as configurações de segurança de transporte.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netMsmqBinding>**](netmsmqbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-netmsmqbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<transport>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<netMsmqBinding>
  <binding>
    <security>
      <transport msmqAuthenticationMode="None/WindowsDomain/Certificate"
                 msmqEncryptionAlgorithm="RC4Stream/AES"
                 msmqProtectionLevel="None/Sign/EncryptAndSign"
                 msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />
    </security>
  </binding>
</netMsmqBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|msmqAuthenticationMode|Especifica como a mensagem deve ser autenticada pelo transporte MSMQ. Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: nenhuma autenticação.<br />-WindowsDomain: o mecanismo de autenticação usa Active Directory para recuperar o certificado X. 509 para o identificador de segurança associado à mensagem. Isso é usado para verificar a ACL da fila para garantir que o usuário tenha permissão de gravação para a fila.<br />-Certificate: o canal recupera o certificado do repositório de certificados.<br /><br /> O padrão é `WindowsDomain`.<br /><br /> Se esse atributo for definido como `None` , o `msmqProtectionLevel` atributo também deverá ser definido como `None` . Esse atributo é do tipo<xref:System.ServiceModel.MsmqAuthenticationMode>|  
|msmqEncryptionAlgorithm|Especifica o algoritmo a ser usado para criptografia de mensagem na conexão ao transferir mensagens entre gerenciadores de filas de mensagens. Os valores válidos incluem os seguintes:<br /><br /> - RC4Stream<br />-AES<br />-O valor padrão é `RC4Stream` . Esse atributo é do tipo <xref:System.ServiceModel.MsmqEncryptionAlgorithm> .|  
|msmqProtectionLevel|Especifica a maneira como as mensagens são protegidas no nível do transporte MSMQ. A criptografia garante a integridade da mensagem, enquanto a assinatura e a criptografia garantem a integridade da mensagem e o não-repúdio. Ou seja, a mensagem realmente veio do remetente e o remetente é quem eles dizem. Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: sem proteção.<br />-Sign: as mensagens são assinadas.<br />-EncryptAndSign: as mensagens são criptografadas e assinadas.<br />-O padrão é `Sign` .|  
|msmqSecureHashAlgorithm|Especifica o algoritmo de hash a ser usado para computar o resumo da mensagem. Os valores válidos incluem os seguintes:<br /><br /> -MD5<br />-SHA1<br />-SHA256<br />-SHA512<br /><br /> O padrão é `SHA1`. Esse atributo é do tipo <xref:System.ServiceModel.MsmqSecureHashAlgorithm> .<br>Devido a problemas de colisão com MD5 e SHA1, a Microsoft recomenda SHA256 ou melhor.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-netmsmqbinding.md)|Define as configurações de segurança de transporte para transportes em fila.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement>
- <xref:System.ServiceModel.Configuration.NetMsmqSecurityElement.Transport%2A>
- <xref:System.ServiceModel.NetMsmqSecurity.Transport%2A>
- <xref:System.ServiceModel.MsmqTransportSecurity>
- [Filas no WCF](../../../wcf/feature-details/queues-in-wcf.md)
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
