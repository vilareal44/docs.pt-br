---
title: <transport> de <netTcpBinding>
ms.date: 03/30/2017
ms.assetid: 49462e0a-66e1-463f-b3e1-c83a441673c6
ms.openlocfilehash: 4ef08ad73a03dea21d27217364a7bacb46a3848e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73735924"
---
# <a name="transport-of-nettcpbinding"></a>\<transport> de \<netTcpBinding>
Define o tipo de requisitos de segurança no nível de mensagem para um ponto de extremidade configurado com o [\<netTcpBinding>](nettcpbinding.md) .  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netTcpBinding>**](nettcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-nettcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<transport>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<netTcpBinding>
  <binding>
    <security mode="None|Transport|Message|TransportWithMessageCredential">
      <transport clientCredentialType="None|Windows|Certificate"
                 protectionLevel="None|Sign|EncryptAndSign"
                 sslProtocols="Tls|Tls11|Tls12">
        <extendedProtectionPolicy policyEnforcement="Never|WhenSupported|Always"
                                  protectionScenario="TransportSelected|TrustedProxy">
          <customServiceNames>
          </customServiceNames>
        </extendedProtectionPolicy>
      </transport>
    </security>
  </binding>
</netTcpBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|clientCredentialType|Opcional. Especifica o tipo de credencial a ser usado ao executar a autenticação de cliente usando a segurança de transporte.<br /><br /> -O valor padrão é `Windows` .<br />-Este atributo é do tipo <xref:System.ServiceModel.TcpClientCredentialType> .|  
|protectionLevel|Opcional. Define a segurança no nível do transporte TCP. A assinatura de mensagens reduz o risco de violação de terceiros com a mensagem enquanto ela está sendo transferida. A criptografia fornece privacidade no nível de dados durante o transporte.<br /><br /> O valor padrão é `EncryptAndSign`.|  
|sslProtocols|Um valor de sinalizador de enumeração SslProtocols que especifica quais SslProtocols têm suporte. O padrão é TLS&#124;Tls11&#124;Tls12.|  
|policyEnforcement|Essa enumeração especifica quando o <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> deve ser imposto.<br /><br /> 1. nunca – a política nunca é imposta (a proteção estendida está desabilitada).<br />2. WhenSupported – a política será imposta somente se o cliente oferecer suporte à proteção estendida.<br />3. sempre – a política é sempre imposta. Os clientes que não dão suporte à proteção estendida não serão autenticados.|  
  
## <a name="clientcredentialtype-attribute"></a>Atributo clientCredentialtype  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Nenhum|O cliente é anônimo. Isso requer um certificado para o serviço.|  
|Windows|Especifica a autenticação do Windows do cliente usando a negociação do SP (negociação Kerberos).|  
|Certificado|O cliente é autenticado usando um certificado. Isso usa a negociação SSL e requer um certificado para o serviço.|  
  
## <a name="protectionlevel-attribute"></a>Atributo protectionLevel  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Nenhum|Sem proteção.|  
|Assinar|As mensagens são assinadas.|  
|EncryptAndSign|-As mensagens são criptografadas e assinadas.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-nettcpbinding.md)|Especifica os recursos de segurança do [\<netTcpBinding>](nettcpbinding.md) .|  
  
## <a name="remarks"></a>Comentários  
 Use a segurança de transporte para integridade e confidencialidade da mensagem SOAP e da autenticação mútua. Se esse modo de segurança for selecionado em uma associação, a pilha de canais será configurada usando um transporte seguro e as mensagens SOAP serão protegidas usando segurança de transporte, como Windows (Negotiate) ou SSL sobre TCP.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.TcpTransportSecurity>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement.Transport%2A>
- <xref:System.ServiceModel.NetTcpSecurity.Transport%2A>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
