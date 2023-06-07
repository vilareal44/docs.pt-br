---
title: <security> de <netTcpBinding>
ms.date: 03/30/2017
ms.assetid: 286cd191-4fd5-4c4e-a223-9c71cf7fdead
ms.openlocfilehash: aa01e906ddd2f15007c72bfc2a45122cfb15ba2c
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73736377"
---
# <a name="security-of-nettcpbinding"></a>\<security> de \<netTcpBinding>
Define as configurações de segurança para uma associação.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netTcpBinding>**](nettcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<security mode="Message/None/Transport/TransportWithCredential">
  <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
             protectionLevel="None/Sign/EncryptAndSign" />
  <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
           clientCredentialType="Certificate/IssuedToken/None/UserName/Windows" />
</security>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|mode|Opcional. Especifica o tipo de segurança que é aplicado. Os valores válidos são mostrados abaixo. O valor padrão é `Transport`.<br /><br /> Esse atributo é do tipo <xref:System.ServiceModel.SecurityMode> .|  
  
## <a name="mode-attribute"></a>Atributo de modo  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Nenhum|A segurança é desabilitada.|  
|Transport|A segurança de transporte é fornecida usando TLS sobre TCP ou SPNego. O serviço pode precisar ser configurado com certificados SSL. É possível controlar o nível de proteção com esse modo.|  
|Mensagem|A segurança é fornecida usando a segurança de mensagem SOAP. Por padrão, o corpo SOAP é criptografado e assinado. Esse modo oferece uma variedade de recursos, como se as credenciais de serviço estão disponíveis no cliente fora da banda, o pacote de algoritmos a ser usado e qual nível de proteção aplicar ao corpo da mensagem. A autenticação do cliente é executada uma vez por sessão e os resultados da autenticação são armazenados em cache durante a sessão.|  
|TransportWithMessageCredential|A segurança de transporte é associada à segurança da mensagem. A segurança de transporte é fornecida pelo TLS sobre TCP ou SPNego e garante a integridade, a confidencialidade e a autenticação do servidor. A segurança de mensagem SOAP fornece autenticação de cliente. Por padrão, a autenticação do cliente é executada uma vez por sessão e os resultados da autenticação são armazenados em cache durante a sessão.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<transport>](transport-of-nettcpbinding.md)|Define as configurações de segurança para o transporte. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.TcpTransportSecurityElement> .|  
|[\<message>](message-element-of-nettcpbinding.md)|Define as configurações de segurança para a mensagem. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.MessageSecurityOverTcpElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|associação|O elemento Binding do [\<netTcpBinding>](nettcpbinding.md) .|  
  
## <a name="remarks"></a>Comentários  
 Cada uma das associações padrão fornece parâmetros para controlar os requisitos de segurança de transferência. Esses parâmetros normalmente incluem o modo de segurança que especificou se a segurança no nível de mensagem ou de transporte é usada e a escolha do tipo de credencial do cliente. Com base na escolha das opções que esses parâmetros apresentam, uma pilha de canais é construída com a segurança apropriada.  
  
 As associações fornecidas pelo sistema fornecidas pelo Windows Communication Foundation (WCF) são um conjunto projetado para atender a alguns dos requisitos de cenário mais comuns. Cada uma dessas associações permite a especificação de requisitos de segurança para alguns cenários específicos de destino.  
  
 Este elemento de configuração fornece as especificações de segurança para o `netTcpBinding` . Essa é uma associação segura, confiável e otimizada adequada para comunicação entre computadores. Por padrão, ele gera uma pilha de comunicação de tempo de execução que dá suporte a TCP para entrega de mensagens e segurança do Windows para segurança e autenticação de mensagens, WS-ReliableMessaging para confiabilidade e codificação de mensagens binárias.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.NetTcpSecurity>
- <xref:System.ServiceModel.NetTcpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.NetTcpBindingElement.Security%2A>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
