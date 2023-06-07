---
title: <wsFederationHttpBinding>
ms.date: 03/30/2017
helpviewer_keywords:
- wsFederationBinding element
ms.assetid: 9c3312b4-2137-4e71-bf3f-de1cf8e9be79
ms.openlocfilehash: 0a77c791d55c6009cf59d5a4b15f3b2a63b7ccf9
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74140465"
---
# \<wsFederationHttpBinding>

Define uma associação que oferece suporte ao WS-Federation.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<wsFederationHttpBinding>**  

## <a name="syntax"></a>Sintaxe

```xml
<wsFederationHttpBinding>
  <binding bypassProxyOnLocal="Boolean"
           closeTimeout="TimeSpan"
           hostNameComparisonMode="StrongWildcard/Exact/WeakWildcard"
           maxBufferPoolSize="integer"
           maxReceivedMessageSize="integer"
           messageEncoding="Text/Mtom"
           name="string"
           openTimeout="TimeSpan"
           privacyNoticeAt="Uri"
           privacyNoticeVersion="Integer"
           proxyAddress="Uri"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           textEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/ Utf8TextEncoding"
           transactionFlow="Boolean"
           useDefaultWebProxy="Boolean">
    <security mode="None/Message/TransportWithMessageCredential">
      <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               issuedTokenType="string"
               issuedKeyType="SymmetricKey/PublicKey"
               negotiateServiceCredential="Boolean">
        <claimTypeRequirements>
          <add claimType="URI"
               isOptional="Boolean" />
        </claimTypeRequirements>
        <issuer address="Uri" >
          <headers>
            <add name="String"
                 namespace="String" />
          </headers>
          <identity>
            <certificate encodedValue="String" />
            <certificateReference findValue="String"
                                  isChainIncluded="Boolean"
                                  storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
                                  storeLocation="LocalMachine/CurrentUser"
                                  X509FindType="System.Security.Cryptography.X509certificates.X509findtype" />
            <dns value="String" />
            <rsa value="String" />
            <servicePrincipalName value="String" />
            <usePrincipalName value="String" />
          </identity>
        </issuer>
        <issuerMetadata address="String">
          <headers>
            <add name="String"
                 namespace="String" />
          </headers>
          <identity>
            <certificate encodedValue="String" />
            <certificateReference findValue="String"
                                  isChainIncluded="Boolean"
                                  storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
                                  storeLocation="LocalMachine/CurrentUser"
                                  x509FindType="System.Security.Cryptography.X509certificates.X509findtype" />
            <dns value="String" />
            <rsa value="String" />
            <servicePrincipalName value="String" />
            <usePrincipalName value="String" />
          </identity>
        </issuerMetadata>
        <tokenRequestParameters>
          <xmlElement>
          </xmlElement>
        </tokenRequestParameters>
      </message>
    </security>
    <reliableSession ordered="Boolean"
                     inactivityTimeout="TimeSpan"
                     enabled="Boolean" />
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</wsFederationBinding>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|bypassProxyOnLocal|Um valor booliano que indica se deve ignorar o servidor proxy para endereços locais. O padrão é `false`.|
|closeTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de fechamento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|
|hostnameComparisonMode|Especifica o modo de comparação de nome de host HTTP usado para analisar URIs. Esse atributo é do tipo <xref:System.ServiceModel.HostNameComparisonMode> , que indica se o nome do host é usado para acessar o serviço ao corresponder ao URI. O valor padrão é <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard> , que ignora o nome do host na correspondência.|
|maxBufferPoolSize|Um inteiro que especifica o tamanho máximo do pool de buffers para essa associação. O padrão é 524.288 bytes (512 * 1024). Muitas partes de Windows Communication Foundation (WCF) usam buffers. Criar e destruir buffers cada vez que eles são usados é caro, e a coleta de lixo para buffers também é dispendiosa. Com os pools de buffers, você pode pegar um buffer do pool, usá-lo e retorná-lo ao pool quando terminar. Portanto, a sobrecarga na criação e na destruição de buffers é evitada.|
|maxReceivedMessageSize|Um inteiro positivo que especifica o tamanho máximo da mensagem, em bytes, incluindo cabeçalhos, que pode ser recebido em um canal configurado com essa associação. O remetente de uma mensagem que excede esse limite receberá uma falha de SOAP. O receptor remove a mensagem e cria uma entrada do evento no log de rastreamento. O padrão é 65536.|
|messageEncoding|Define o codificador usado para codificar a mensagem. Os valores válidos incluem os seguintes:<br /><br /> -Text: Use um codificador de mensagem de texto.<br />-MTOM: usar um codificador MTOM (mecanismo de organização de transmissão de mensagens) 1,0.<br /><br /> O padrão é texto.<br /><br /> Esse atributo é do tipo <xref:System.ServiceModel.WSMessageEncoding> .|
|name|Uma cadeia de caracteres que contém o nome da configuração da associação. Esse valor deve ser exclusivo porque é usado como uma identificação para a associação. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|
|openTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de abertura. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|
|privacyNoticeAt|Uma cadeia de caracteres que especifica um URI no qual o aviso de privacidade está localizado.|
|privacyNoticeVersion|Um inteiro que especifica a versão do aviso de privacidade atual.|
|proxyAddress|Um URI que especifica o endereço do proxy HTTP. Se `useDefaultWebProxy` for `true` , essa configuração deverá ser `null` . O padrão é `null`.|
|receiveTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de recebimento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:10:00.|
|sendTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de envio. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|
|codificação de|Define a codificação do conjunto de caracteres a ser usada para emitir mensagens na associação. Os valores válidos incluem os seguintes:<br /><br /> -BigEndianUnicode: codificação BigEndian Unicode.<br />-Unicode: codificação de 16 bits.<br />-UTF8: codificação de 8 bits<br /><br /> O padrão é UTF8. Este atributo é do tipo <xref:System.Text.Encoding> ..|
|transactionFlow|Um valor booliano que especifica se a associação dá suporte ao fluxo de WS-Transactions. O padrão é `false`.|
|useDefaultWebProxy|Um valor booliano que indica se o proxy HTTP autoconfigurado do sistema é usado. O endereço de proxy deve ser `null` (ou seja, não definido) se esse atributo for `true` . O padrão é `true`.|

### <a name="child-elements"></a>Elementos filho

|Elemento|Descrição|
|-------------|-----------------|
|[\<security>](security-of-wsfederationhttpbinding.md)|Define as configurações de segurança para a mensagem. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.WSFederationHttpSecurityElement> .|
|[\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Define as restrições sobre a complexidade de mensagens SOAP que podem ser processadas por pontos de extremidade configurados com essa associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> .|
|[\<reliableSession>](https://docs.microsoft.com/previous-versions/ms731375(v=vs.90))|Especifica se as sessões confiáveis são estabelecidas entre os pontos de extremidade do canal.|

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|
|-------------|-----------------|
|[\<bindings>](bindings.md)|Esse elemento contém uma coleção de associações padrão e personalizadas.|

## <a name="remarks"></a>Comentários

A Federação é a capacidade de compartilhar identidades em vários sistemas para autenticação e autorização. Essas identidades podem se referir a usuários ou a computadores. O HTTP federado dá suporte à segurança SOAP, bem como à segurança de modo misto, mas não dá suporte exclusivamente ao uso da segurança de transporte. Essa associação fornece suporte de Windows Communication Foundation (WCF) para o protocolo WS-Federation. Os serviços configurados com essa associação devem usar o transporte HTTP.

Associações consistem em uma pilha de elementos de associação. A pilha de elementos de associação em

`wsFederationHttpBinding`é o mesmo que o contido em`wsHttpBinding`

Quando [\<security>](security-of-wsfederationhttpbinding.md) é definido como o valor padrão de <xref:System.ServiceModel.WSFederationHttpSecurityMode.Message> .

O `wsFederationHttpBinding` controla os detalhes das configurações de segurança da mensagem no [\<message>](message-element-of-wsfederationhttpbinding.md) . Observe que o [\<security>](security-of-wsfederationhttpbinding.md) elemento fornece acesso Get somente quando a segurança usada pela associação não pode ser alterada depois que a associação é criada.

O `wsFederationHttpBinding` também fornece um atributo privacyNoticeAt para definir e recuperar o URI no qual o aviso de privacidade está localizado.

Manter a política segura é especialmente importante em cenários de Federação. A recomendação é usar alguma forma de segurança, como HTTPS, para proteger a política contra usuários mal-intencionados.

Em cenários de Federação que usam essa associação, a política de serviço potencialmente tem informações importantes, como a chave a ser usada para criptografar o token emitido (SAML), o tipo de declarações a serem colocadas no token e assim por diante. Se essa política for violada, um invasor poderá descobrir a chave do token emitido que leva à violação adicional, à divulgação de informações e a outros comportamentos mal-intencionados. Para ajudar a evitar isso, a política deve ser obtida com segurança (por exemplo, usando HTTPS) do serviço.

Para obter mais informações sobre essa ligação, consulte [como: criar um WSFederationHttpBinding](../../../wcf/feature-details/how-to-create-a-wsfederationhttpbinding.md).

## <a name="example"></a>Exemplo

```xml
<configuration>
  <system.ServiceModel>
    <bindings>
      <wsFederationHttpBinding>
        <binding bypassProxyOnLocal="false"
                 transactionFlow="false"
                 hostNameComparisonMode="WeakWildcard"
                 maxReceivedMessageSize="1000"
                 messageEncoding="Mtom"
                 proxyAddress="http://foo/bar"
                 textEncoding="Utf16TextEncoding"
                 useDefaultWebProxy="false">
          <reliableSession ordered="false"
                           inactivityTimeout="00:02:00"
                           enabled="true" />
          <security mode="None">
            <message negotiateServiceCredential="false"
                     algorithmSuite="Aes128"
                     issuedTokenType="saml"
                     issuedKeyType="PublicKey">
              <issuer address="http://localhost/Sts" />
            </message>
          </security>
        </binding>
      </wsFederationBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```

## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.WSFederationHttpBinding>
- <xref:System.ServiceModel.Configuration.WSFederationHttpBindingElement>
- [Como criar um WSFederationHttpBinding](../../../wcf/feature-details/how-to-create-a-wsfederationhttpbinding.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
