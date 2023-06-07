---
title: <security> de <customBinding>
ms.date: 03/30/2017
ms.assetid: 243a5148-bbd1-447f-a8a5-6e7792c0a3f1
ms.openlocfilehash: 454113f66007ddd69f8455bb532e9cbd12fcefb7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738702"
---
# <a name="security-of-custombinding"></a>\<security> de \<customBinding>
Especifica as opções de segurança para uma associação personalizada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<security allowSerializedSigningTokenOnReply="Boolean"
          authenticationMode="AuthenticationMode"
          defaultAlgorithmSuite="SecurityAlgorithmSuite"
          includeTimestamp="Boolean"
          requireDerivedKeys="Boolean"
          keyEntropyMode="ClientEntropy/ServerEntropy/CombinedEntropy"
          messageProtectionOrder="SignBeforeEncrypt/SignBeforeEncryptAndEncryptSignature/EncryptBeforeSign"
          messageSecurityVersion="WSSecurityJan2004/WSSecurityXXX2005"
          requireDerivedKeys="Boolean"
          requireSecurityContextCancellation="Boolean"
          requireSignatureConfirmation="Boolean"
          securityHeaderLayout="Strict/Lax/LaxTimestampFirst/LaxTimestampLast">
   <issuedTokenParameters />
   <localClientSettings />
   <localServiceSettings />
   <secureConversationBootstrap />
</security>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|allowSerializedSigningTokenOnReply|Opcional. Um valor booliano que especifica se um token serializado pode ser usado na resposta. O valor padrão é `false`. Ao usar uma associação dupla, a configuração padronizada como `true` e qualquer configuração feita será ignorada.|  
|authenticationMode|Opcional. Especifica o modo de autenticação usado entre o iniciador e o respondente. Veja abaixo todos os valores.<br /><br /> O padrão é `sspiNegotiated`.|  
|defaultAlgorithmSuite|Opcional. Define a criptografia de mensagem e os algoritmos de encapsulamento de chaves. Os algoritmos e os tamanhos de chave são determinados pela <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> classe. Esses algoritmos são mapeados para aqueles especificados na especificação do WS-SecurityPolicy (Security Policy Language).<br /><br /> Os valores possíveis são mostrados abaixo. O valor padrão é `Basic256`.<br /><br /> Esse atributo é usado ao trabalhar com uma plataforma diferente que opta por um conjunto de algoritmos diferente do padrão. Você deve estar ciente dos pontos fortes e fracos dos algoritmos relevantes ao fazer modificações nessa configuração. Esse atributo é do tipo <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> .|  
|includeTimestamp|Um valor booliano que especifica se os carimbos de data/hora são incluídos em cada mensagem. O padrão é `true`.|  
|keyentropiamode|Especifica a maneira como as chaves para proteger mensagens são computadas. As chaves podem ser baseadas somente no material da chave do cliente, somente no material da chave de serviço ou em uma combinação de ambos. Os valores válidos são<br /><br /> -   `ClientEntropy`: A chave de sessão é baseada nos dados de chave fornecidos pelo cliente.<br />-   `ServerEntropy`: A chave de sessão é baseada nos dados de chave fornecidos pelo servidor.<br />-   `CombinedEntropy`: A chave de sessão é baseada nos dados de chave fornecidos pelo cliente e serviço.<br /><br /> O padrão é `CombinedEntropy`.<br /><br /> Esse atributo é do tipo <xref:System.ServiceModel.Security.SecurityKeyEntropyMode> .|  
|messageProtectionOrder|Define a ordem na qual os algoritmos de segurança de nível de mensagem são aplicados à mensagem. Os valores válidos incluem os seguintes:<br /><br /> -   `SignBeforeEncrypt`: Assinar primeiro e depois criptografar.<br />-   `SignBeforeEncryptAndEncryptSignature`: Assinar primeiro, criptografar e, em seguida, criptografar a assinatura.<br />-   `EncryptBeforeSign`: Criptografar primeiro e depois assinar.<br /><br /> O valor padrão depende da versão do WS-Security que está sendo usada. O valor padrão é `SignBeforeEncryptAndEncryptSignature` ao usar o WS-Security 1,1. O valor padrão é `SignBeforeEncrypt` ao usar o WS-Security 1,0.<br /><br /> Esse atributo é do tipo <xref:System.ServiceModel.Security.MessageProtectionOrder> .|  
|messageSecurityVersion|Opcional. Define a versão do WS-Security que é usada. Os valores válidos incluem os seguintes:<br /><br /> - WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11<br />- WSSecurity10WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10<br />- WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10<br /><br /> O padrão é WSSecurity11WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11 e pode ser expresso no XML como de simples `Default` . Esse atributo é do tipo <xref:System.ServiceModel.MessageSecurityVersion> .|  
|requireDerivedKeys|Um valor booliano que especifica se as chaves podem ser derivadas das chaves de prova originais. O padrão é `true`.|  
|requireSecurityContextCancellation|Opcional. Um valor booliano que especifica se o contexto de segurança deve ser cancelado e encerrado quando não for mais necessário. O padrão é `true`.|  
|requireSignatureConfirmation|Opcional. Um valor booliano que especifica se a confirmação de assinatura do WS-Security está habilitada. Quando definido como `true` , as assinaturas de mensagens são confirmadas pelo respondente.  Quando a associação personalizada é configurada para certificados mútuos ou é configurada para usar tokens emitidos (associações do WSS 1,1), esse atributo usa como padrão `true` . Caso contrário, o padrão é `false`.<br /><br /> A confirmação de assinatura é usada para confirmar que o serviço está respondendo no reconhecimento total de uma solicitação.|  
|securityHeaderLayout|Opcional. Especifica a ordenação dos elementos no cabeçalho de segurança. Os valores válidos são<br /><br /> -   `Strict`: Os itens são adicionados ao cabeçalho de segurança de acordo com o princípio geral de "declare antes de usar".<br />-   `Lax`: Os itens são adicionados ao cabeçalho de segurança em qualquer ordem que confirme ao WSS: SOAP Message Security.<br />-   `LaxWithTimestampFirst`: Os itens são adicionados ao cabeçalho de segurança em qualquer ordem que confirme a segurança de mensagem do WSS: SOAP, exceto que o primeiro elemento no cabeçalho de segurança deve ser um elemento wsse: timestamp.<br />-   `LaxWithTimestampLast`: Os itens são adicionados ao cabeçalho de segurança em qualquer ordem que confirme o WSS: SOAP Message Security, exceto que o último elemento no cabeçalho de segurança deve ser um elemento wsse: timestamp.<br /><br /> O padrão é `Strict`.<br /><br /> Esse elemento é do tipo <xref:System.ServiceModel.Channels.SecurityHeaderLayout> .|  
  
## <a name="authenticationmode-attribute"></a>Atributo AuthenticationMode  
  
|Valor|Descrição|  
|-----------|-----------------|  
|String|`AnonymousForCertificate`<br /><br /> `AnonymousForSslNegotiated`<br /><br /> `CertificateOverTransport`<br /><br /> `IssuedToken`<br /><br /> `IssuedTokenForCertificate`<br /><br /> `IssuedTokenForSslNegotiated`<br /><br /> `IssuedTokenOverTransport`<br /><br /> `Kerberos`<br /><br /> `KerberosOverTransport`<br /><br /> `MutualCertificate`<br /><br /> `MutualCertificateDuplex`<br /><br /> `MutualSslNegotiated`<br /><br /> `SecureConversation`<br /><br /> `SspiNegotiated`<br /><br /> `UserNameForCertificate`<br /><br /> `UserNameForSslNegotiated`<br /><br /> `UserNameOverTransport`<br /><br /> `SspiNegotiatedOverTransport`|  
  
## <a name="defaultalgorithm-attribute"></a>Atributo defaultalgorithm  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Basic128|Use criptografia Aes128, SHA1 para síntese de mensagem e rsa-oaep-mgf1p para encapsulamento de chave.|  
|Basic192|Use a criptografia Aes192, SHA1 para Message Digest, rsa-oaep-mgf1p para quebra automática de chave.|  
|Basic256|Use a criptografia Aes256, SHA1 para Message Digest, rsa-oaep-mgf1p para quebra automática de chave.|  
|Basic256Rsa15|Use Aes256 para criptografia de mensagem, SHA1 para Message Digest e Rsa15 para Key Wrap.|  
|Basic192Rsa15|Use Aes192 para criptografia de mensagem, SHA1 para Message Digest e Rsa15 para Key Wrap.|  
|TripleDes|Use a criptografia TripleDes, SHA1 para Message Digest, rsa-oaep-mgf1p para quebra automática de chave.|  
|Basic128Rsa15|Use Aes128 para criptografia de mensagem, SHA1 para Message Digest e Rsa15 para Key Wrap.|  
|TripleDesRsa15|Use a criptografia TripleDes, SHA1 para síntese de mensagem e Rsa15 para o encapsulamento de chave.|  
|Basic128Sha256|Use Aes256 para criptografia de mensagem, SHA256 para síntese de mensagem e rsa-oaep-mgf1p para quebra automática de chave.|  
|Basic192Sha256|Use Aes192 para criptografia de mensagem, SHA256 para síntese de mensagem e rsa-oaep-mgf1p para encapsulamento de chave.|  
|Basic256Sha256|Use Aes256 para criptografia de mensagem, SHA256 para síntese de mensagem e rsa-oaep-mgf1p para quebra automática de chave.|  
|TripleDesSha256|Use TripleDes para criptografia de mensagem, SHA256 para síntese de mensagem e rsa-oaep-mgf1p para encapsulamento de chave.|  
|Basic128Sha256Rsa15|Use Aes128 para criptografia de mensagem, SHA256 para síntese de mensagem e Rsa15 para quebra de chave.|  
|Basic192Sha256Rsa15|Use Aes192 para criptografia de mensagem, SHA256 para síntese de mensagem e Rsa15 para quebra de chave.|  
|Basic256Sha256Rsa15|Use Aes256 para criptografia de mensagem, SHA256 para síntese de mensagem e Rsa15 para quebra automática de chave.|  
|TripleDesSha256Rsa15|Use TripleDes para criptografia de mensagem, SHA256 para síntese de mensagem e Rsa15 para quebra de chave.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<issuedTokenParameters>](issuedtokenparameters.md)|Especifica um token emitido atual. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.IssuedTokenParametersElement> .|  
|[\<localClientSettings>](localclientsettings-element.md)|Especifica as configurações de segurança de um cliente local para esta associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.LocalClientSecuritySettingsElement> .|  
|[\<localServiceSettings>](localservicesettings-element.md)|Especifica as configurações de segurança de um serviço local para esta associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.LocalServiceSecuritySettingsElement> .|  
|[\<secureConversationBootstrap>](secureconversationbootstrap.md)|Especifica os valores padrão usados para iniciar um serviço de conversa segura.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação da associação personalizada.|  
  
## <a name="remarks"></a>Comentários  
 Para obter mais informações sobre como usar esse elemento, consulte [modos de autenticação SecurityBindingElement](../../../wcf/feature-details/securitybindingelement-authentication-modes.md) e [como criar uma associação personalizada usando o SecurityBindingElement](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra como configurar a segurança usando uma associação personalizada. Ele mostra como usar uma associação personalizada para habilitar a segurança em nível de mensagem com um transporte seguro. Isso é útil quando um transporte seguro é necessário para transmitir as mensagens entre o cliente e o serviço e simultaneamente as mensagens devem ser seguras no nível de mensagem. Não há suporte para essa configuração por associações fornecidas pelo sistema.  
  
 A configuração de serviço define uma associação personalizada que dá suporte à comunicação TCP protegida usando o protocolo TLS/SSL e a segurança de mensagem do Windows. A associação personalizada usa um certificado de serviço para autenticar o serviço no nível de transporte e para proteger as mensagens durante a transmissão entre o cliente e o serviço. Isso é feito pelo [\<sslStreamSecurity>](sslstreamsecurity.md) elemento Binding. O certificado do serviço é configurado usando um comportamento de serviço.  
  
 Além disso, a associação personalizada usa segurança de mensagem com o tipo de credencial do Windows – esse é o tipo de credencial padrão. Isso é feito pelo elemento de associação de [segurança](security-of-custombinding.md) . O cliente e o serviço são autenticados usando a segurança no nível da mensagem se o mecanismo de autenticação Kerberos estiver disponível. Se o mecanismo de autenticação Kerberos não estiver disponível, a autenticação NTLM será usada. O NTLM autentica o cliente para o serviço, mas não autentica o serviço para o cliente. O elemento de associação de [segurança](security-of-custombinding.md) é configurado para usar `SecureConversation` authenticationType, o que resulta na criação de uma sessão de segurança no cliente e no serviço. Isso é necessário para permitir que o contrato duplex do serviço funcione. Para obter mais informações sobre como executar este exemplo, consulte [segurança de associação personalizada](../../../wcf/samples/custom-binding-security.md).  
  
```xml  
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">
        <host>
          <baseAddresses>
            <!-- use following base address -->
            <add baseAddress="net.tcp://localhost:8000/ServiceModelSamples/Service"/>
          </baseAddresses>
        </host>
        <endpoint address=""
                  binding="customBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex" />
        <!-- the mex endpoint is exposed at net.tcp://localhost:8000/ServiceModelSamples/service/mex -->
        <endpoint address="mex"
                  binding="mexTcpBinding"
                  contract="IMetadataExchange" />
      </service>
    </services>
    <bindings>
      <!-- configure a custom binding -->
      <customBinding>
        <binding name="Binding1">
          <security authenticationMode="SecureConversation"
                    requireSecurityContextCancellation="true">
          </security>
          <textMessageEncoding messageVersion="Soap12WSAddressing10"
                               writeEncoding="utf-8" />
          <sslStreamSecurity requireClientCertificate="false" />
          <tcpTransport />
        </binding>
      </customBinding>
    </bindings>
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceMetadata />
          <serviceDebug includeExceptionDetailInFaults="False" />
          <serviceCredentials>
            <serviceCertificate findValue="localhost"
                                storeLocation="LocalMachine"
                                storeName="My"
                                x509FindType="FindBySubjectName" />
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.SecurityElement>
- <xref:System.ServiceModel.Channels.SecurityBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [Como criar uma associação personalizada utilizando o SecurityBindingElement](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [Segurança de associação personalizada](../../../wcf/samples/custom-binding-security.md)
