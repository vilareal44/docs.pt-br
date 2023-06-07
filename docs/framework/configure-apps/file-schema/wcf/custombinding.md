---
title: <customBinding>
ms.date: 03/30/2017
ms.assetid: 9da4f960-f64e-4d8a-894d-2b09eba5ce4b
ms.openlocfilehash: cdaaacf0dfa75209d001f6e8d6ac7175816048aa
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74140797"
---
# \<customBinding>

Fornece controle total sobre a pilha de mensagens para o usuário.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<customBinding>**  

## <a name="syntax"></a>Sintaxe

```xml
<customBinding>
  <binding name="String"
           closeTimeout="TimeSpan"
           openTimeout="TimeSpan"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan">
    <compositeDuplex clientBaseAddress="Uri" />
    <reliableSession acknowledgementInterval="TimeSpan"
                     advancedFlowControl="Boolean"
                     bufferedMessagesQuota="Integer"
                     inactivityTimeout="TimeSpan"
                     maxPendingChannels="Integer"
                     maxRetryCount="Integer"
                     ordered="Boolean" />
    <pnrpPeerResolver />
    <windowsStreamSecurity protectionLevel="None/Sign/EncryptAndSign" />
    <sslStreamSecurity requireClientCertificate="Boolean" />
    <transactionFlow transactionProtocol="OleTransactions/WSAtomicTransactionOctober2004" />
    <security defaultAlgorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
              authenticationMode="UserNameForAnonymous"
              contextMode="Cookie"
              defaultProtectionLevel="Sign"
              enableKeyDerivation="false"
              keyEntropyMode="ClientEntropy"
              messageProtectionOrder="SignBeforeEncryptAndEncryptSignature"
              securityVersion="WSSecurityXXX2005">
      <localClientSettings cacheCookies="false"
                           detectReplays="false"
                           maxCookieCachingTime="00:07:24" />
      <localServiceSettings replayCacheSize="9"
                            maxClockSkew="00:00:03"
                            replayWindow="00:07:22.2190000" />
    </security>
    <binaryMessageEncoding maxReadPoolSize="Integer"
                           maxWritePoolSize="Integer"
                           maxSessionSize="Integer" />
    <httpsTransport manualAddressing="Boolean"
                    maxMessageSize="Integer"
                    authenticationScheme="Negotiate"
                    bypassProxyOnLocal="Boolean"
                    hostNameComparisonMode="Exact"
                    mapAddressingHeadersToHttpHeaders="Boolean"
                    proxyaddress="Uri"
                    realm="String"
                    requireClientCertificate="Boolean" />
    <peerTransport manualAddressing="false"
                   maxMessageSize="20002"
                   listenIPAddress="202.10.1.9"
                   messageAuthentication="false"
                   peerNodeAuthenticationMode="None"
                   port="1000" />
    <security defaultAlgorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
              authenticationMode="UserNameForAnonymous"
              bootstrapBindingConfiguration="String"
              bootstrapBindingSectionName="String"
              defaultProtectionLevel="None/Sign/EncryptAndSign"
              requireDerivedKeys="Boolean"
              securityHeaderLayout="Strict/Lax/LaxTimestampFirst/LaxTimestampLast"
              includeTimestamp="Boolean"
              keyEntropyMode="ClientEntropy/ServerEntropy/CombinedEntropy"
              messageProtectionOrder="SignBeforeEncrypt/SignBeforeEncryptAndEncryptSignature/EncryptBeforeSign"
              protectTokens="Boolean"
              requireSecurityContextCancellation="Boolean"
              securityVersion=" WSSecurityJan2004/WSSecurityXXX2005"
              requireSignatureConfirmation="Boolean">
      <localClientSettings cacheCookies="Boolean"
                           detectReplays="Boolean"
                           replayCacheSize="Integer"
                           maxClockSkew="TimeSpan"
                           maxCookieCachingTime="TimeSpan"
                           replayWindow="TimeSpan"
                           sessionKeyRenewalInterval="TimeSpan"
                           sessionKeyRolloverInterval="TimeSpan"
                           reconnectOnTransportFailure="Boolean"
                           timestampValidityDuration="TimeSpan"
                           cookieRenewalThresholdPercentage="Integer" />
      <localServiceSettings detectReplays="Boolean"
                            issuedCookieLifeTime="TimeSpan"
                            maxStatefulNegotiations="Integer"
                            replayCacheSize="Integer"
                            maxClockSkew="TimeSpan"
                            negotiationTimeout="TimeSpan"
                            replayWindow="TimeSpan"
                            inactivityTimeout="TimeSpan"
                            sessionKeyRenewalInterval="TimeSpan"
                            sessionKeyRolloverInterval="TimeSpan"
                            reconnectOnTransportFailure="Boolean"
                            maxConcurrentSessions="Integer"
                            timestampValidityDuration="TimeSpan" />
      <federationParameters trustVersion="WSTrustApr2004/WSTrustFeb2005" />
    </security>
    <security defaultAlgorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
              authenticationMode="UserNameForAnonymous"
              bootstrapBindingConfiguration="String"
              bootstrapBindingSectionName="String"
              defaultProtectionLevel="None/Sign/EncryptAndSign"
              requireDerivedKeys="Boolean"
              securityHeaderLayout="Strict/Lax/LaxTimestampFirst/LaxTimestampLast"
              includeTimestamp="Boolean"
              keyEntropyMode="ClientEntropy/ServerEntropy/CombinedEntropy"
              messageProtectionOrder="SignBeforeEncrypt/SignBeforeEncryptAndEncryptSignature/EncryptBeforeSign"
              protectTokens="Boolean"
              requireSecurityContextCancellation="Boolean"
              securityVersion=" WSSecurityJan2004/WSSecurityXXX2005"
              requireSignatureConfirmation="Boolean" >
      <localClientSettings cacheCookies="Boolean"
                           detectReplays="Boolean"
                           replayCacheSize="Integer"
                           maxClockSkew="TimeSpan"
                           maxCookieCachingTime="TimeSpan"
                           replayWindow="TimeSpan"
                           sessionKeyRenewalInterval="TimeSpan"
                           sessionKeyRolloverInterval="TimeSpan"
                           reconnectOnTransportFailure="Boolean"
                           timestampValidityDuration="TimeSpan"
                           cookieRenewalThresholdPercentage="Integer" />
      <localServiceSettings detectReplays="Boolean"
                           issuedCookieLifeTime="TimeSpan"
                           maxStatefulNegotiations="Integer"
                           replayCacheSize="Integer"
                           maxClockSkew="TimeSpan"
                           negotiationTimeout="TimeSpan"
                           replayWindow="TimeSpan"
                           inactivityTimeout="TimeSpan"
                           sessionKeyRenewalInterval="TimeSpan"
                           sessionKeyRolloverInterval="TimeSpan"
                           reconnectOnTransportFailure="Boolean"
                           maxConcurrentSessions="Integer"
                           timestampValidityDuration="TimeSpan" />
      <federationParameters trustVersion="WSTrustApr2004/WSTrustFeb2005" />
      <genericIssuedTokenParameters>
        <localIssuerIssuedTokenParameters keyType="SymmetricKey/PublicKey"
                                          keySize="Integer"
                                          tokenType="String" />
        <issuedTokenParametersEndpointAddress address="URI"
                                              bindingConfiguration="String"
                                              binding="String" />
        <issuedTokenClient localIssuerChannelBehaviors="String"
                           cacheIssuedTokens="Boolean"
                           maxIssuedTokenCachingTime="TimeSpan"
                           keyEntropyMode="ClientEntropy/ServerEntropy/CombinedEntropy" />
        <issuedTokenClientBehavior issuerAddress="String"
                                   behaviorConfiguration="String" />
        <issuedTokenClientBehavior address="URI"
                                   bindingConfiguration="String"
                                   binding="String" />
      </genericIssuedTokenParameters>
    </security>
  </binding>
</customBinding>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|closeTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de fechamento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|
|name|Uma cadeia de caracteres que contém o nome da configuração da associação. Esse valor é uma cadeia de caracteres definida pelo usuário que atua como a cadeia de caracteres de identificação para a associação personalizada. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|
|openTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de abertura. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|
|receiveTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de recebimento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|
|sendTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de envio. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|

### <a name="child-elements"></a>Elementos filho

|Elemento|Descrição|
|-------------|-----------------|
|[\<compositeDuplex>](compositeduplex.md)|Especifica a mensagem de duas vias para a associação personalizada. Ele é usado com transportes que não permitem comunicações duplex nativamente, por exemplo, HTTP. O TCP, por outro lado, permite comunicações duplex nativamente e não requer o uso desse elemento de ligação para que o serviço envie mensagens de volta para um cliente.<br /><br /> O cliente deve expor um endereço para que o serviço faça contato e estabeleça uma conexão. Esse endereço de cliente é fornecido pelo `ClientBaseAddress` atributo.<br /><br /> Esse elemento é do tipo <xref:System.ServiceModel.Configuration.CompositeDuplexElement> .|
|[\<pnrpPeerResolver>](pnrppeerresolver.md)|Especifica um resolvedor de nome de par PNRP (Peer Name Resolution Protocol). Esse elemento é do tipo <xref:System.ServiceModel.Configuration.PnrpPeerResolverElement> .|
|[\<reliableSession>](reliablesession.md)|Especifica a configuração para mensagens WS-Reliable. Quando esse elemento é adicionado a uma associação personalizada, o canal resultante pode dar suporte a garantias de entrega exatamente uma vez. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.ReliableSessionElement> .|
|[\<security>](security-of-custombinding.md)|Especifica as opções de segurança da associação personalizada. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.SecurityElement> .|
|[\<sslStreamSecurity>](sslstreamsecurity.md)|Especifica as configurações de segurança para uma associação de fluxo SSL. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.SslStreamSecurityElement> .|
|[\<transactionFlow>](transactionflow.md)|Especifica que a associação dá suporte ao fluxo de transações e o protocolo a ser usado pelo `transactionProtocol` atributo. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.TransactionFlowElement> .|
|[\<windowsStreamSecurity>](windowsstreamsecurity.md)|Especifica as opções de segurança de streaming da associação personalizada. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.WindowsStreamSecurityElement> .|

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|associações|Contém todas as associações para aplicativos Windows Communication Foundation.|

## <a name="remarks"></a>Comentários

As associações personalizadas fornecem controle total sobre a pilha de mensagens do WCF. Associações especiais personalizadas podem ser criadas para adicionar os elementos de configuração para entidades específicas. Por exemplo, o usuário pode combinar a seção `httpsTransport` , `reliableSession` seção e a `security` seção para criar uma associação baseada em https confiável e segura.

Uma associação individual define a pilha de mensagens especificando os elementos de configuração dos elementos de pilha na ordem em que aparecem na pilha. Cada elemento define e configura o elemento de um da pilha. Deve haver apenas um elemento de transporte em cada associação personalizada. Sem esse elemento, a pilha de mensagens está incompleta.

A ordem na qual os elementos aparecem na pilha é importante, pois é a ordem na qual as operações são aplicadas à mensagem. A ordem recomendada de elementos de pilha é a seguinte:

1. Transações (opcional)

2. Mensagens confiáveis (opcional)

3. Segurança (opcional)

4. Transport

5. Codificador (opcional)

Use uma associação personalizada quando uma das associações fornecidas pelo sistema não atender aos requisitos do seu serviço. Uma associação personalizada poderia ser usada, por exemplo, para habilitar o uso de um novo transporte ou um novo codificador em um ponto de extremidade de serviço.

Uma associação personalizada é construída usando uma das <xref:System.ServiceModel.Channels.CustomBinding.%23ctor%2A> coleções de elementos de associação que são "empilhadas" em uma ordem específica:

- Na parte superior, há um opcional <xref:System.ServiceModel.Channels.TransactionFlowBindingElement> que permite transações de fluxo.

- Em seguida, é um opcional <xref:System.ServiceModel.Channels.ReliableSessionBindingElement> que fornece um mecanismo de sessão e ordenação, conforme definido na especificação WS-ReliableMessaging. Essa noção de uma sessão pode cruzar os intermediários de transporte e SOAP.

- A seguir está um elemento de associação de segurança opcional que fornece recursos de segurança como autorização, autenticação, proteção e confidencialidade. Os seguintes elementos de associação de segurança são fornecidos pelo Windows Communication Foundation (WCF):

  - <xref:System.ServiceModel.Channels.SecurityBindingElement>

  - <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>

  - <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>

  - <xref:System.ServiceModel.Channels.TransportSecurityBindingElement>

- Em seguida, estão os padrões de mensagem opcionais especificados por elementos de associação:

- <xref:System.ServiceModel.Channels.CompositeDuplexBindingElement>

- Em seguida, estão os elementos de associação de atualizações/auxiliares de transporte opcionais:

  - <xref:System.ServiceModel.Channels.PnrpPeerResolverBindingElement>

  - <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement>

  - <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement>

- A seguir está um elemento de associação de codificação de mensagem necessário. Você pode usar seu próprio transporte ou usar uma das seguintes associações de codificação de mensagem:

  - <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>

  - <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement>

  - <xref:System.ServiceModel.Channels.MtomMessageEncodingBindingElement>

- Na parte inferior, há um elemento Transport necessário. Você pode usar seu próprio transporte ou usar um dos elementos de associação de transporte fornecidos pelo Windows Communication Foundation (WCF):

  - <xref:System.ServiceModel.Channels.TcpTransportBindingElement>

  - <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>

  - <xref:System.ServiceModel.Channels.HttpTransportBindingElement>

  - <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>

  - <xref:System.ServiceModel.Channels.MsmqTransportBindingElement>

  - <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBindingElement>

  - <xref:System.ServiceModel.Channels.PeerTransportBindingElement>

A tabela a seguir resume as opções para cada camada.

|Camada|Opções|Obrigatório|
|-----------|-------------|--------------|
|Fluxo de transações|<xref:System.ServiceModel.Channels.TransactionFlowBindingElement>|Não|
|Confiabilidade|<xref:System.ServiceModel.Channels.ReliableSessionBindingElement>|Não|
|Segurança|Simétrico, assimétrico, nível de transporte|Não|
|Alteração de forma|<xref:System.ServiceModel.Channels.CompositeDuplexBindingElement>|Não|
|Atualizações de transporte|Fluxo SSL, fluxo do Windows, resolvedor de pares|Não|
|Codificação|Texto, binário, MTOM, personalizado|Sim|
|Transport|TCP, pipes nomeados, HTTP, HTTPS, tipos de MSMQ, personalizado|Sim|

Além disso, você pode definir seus próprios elementos de ligação e inseri-los entre as camadas definidas anteriormente.

Para obter uma discussão sobre como usar uma associação personalizada para modificar uma associação fornecida pelo sistema, consulte [como: personalizar uma associação fornecida pelo sistema](../../../wcf/extending/how-to-customize-a-system-provided-binding.md).

## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Channels.Binding>
- <xref:System.ServiceModel.Channels.BindingElement>
- <xref:System.ServiceModel.Configuration.BindingsSection>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [\<binding>](bindings.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [Elemento CustomBinding](custombinding.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
