---
title: <ws2007FederationHttpBinding>
ms.date: 03/30/2017
ms.assetid: 9af4ec79-cdef-457e-9dca-09d5eb821594
ms.openlocfilehash: 20ba643fddbac8a488e5457f0195cc253d4d23f7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74139320"
---
# \<ws2007FederationHttpBinding>

Uma associação segura e interoperável que deriva de [\<wsFederationHttpBinding>](wsfederationhttpbinding.md) e dá suporte à segurança federada.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<ws2007FederationHttpBinding>**  
  
## <a name="syntax"></a>Sintaxe

```xml
<ws2007FederationHttpBinding>
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
           textEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding"
           transactionFlow="Boolean"
           useDefaultWebProxy="Boolean">
    <security mode="None/Message/TransportWithMessageCredential">
      <message negotiateServiceCredential="Boolean"
               algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               issuedTokenType="string"
               issuedKeyType="SymmetricKey/PublicKey">
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
</ws2007FederationHttpBinding>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

|Atributo|Descrição|
|---------------|-----------------|
|`bypassProxyOnLocal`|Um valor que indica se deve ignorar o servidor proxy para endereços locais. O padrão é `false`.|
|`closeTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de fechamento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|
|`hostNameComparisonMode`|Especifica o modo de comparação de nome de host HTTP usado para analisar URIs. Esse atributo é do tipo <xref:System.ServiceModel.HostNameComparisonMode> , que indica se o nome do host é usado para acessar o serviço ao corresponder ao URI. O valor padrão é <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard> , que ignora o nome do host na correspondência.|
|`maxBufferPoolSize`|O tamanho máximo do pool de buffers para essa associação. O padrão é 524.288 bytes (512 * 1024). Muitas partes de Windows Communication Foundation (WCF) usam buffers. Criar e destruir buffers cada vez que eles são usados é caro, e a coleta de lixo para buffers também é dispendiosa. Com os pools de buffers, você pode pegar um buffer do pool, usá-lo e retorná-lo ao pool quando terminar. Portanto, a sobrecarga na criação e na destruição de buffers é evitada.|
|`maxReceivedMessageSize`|O tamanho máximo da mensagem, em bytes, incluindo cabeçalhos, que pode ser recebido em um canal configurado com essa associação. O remetente de uma mensagem que excede esse limite recebe uma falha de SOAP. O receptor remove a mensagem e cria uma entrada do evento no log de rastreamento. O padrão é 65536.|
|`messageEncoding`|Define o codificador usado para codificar a mensagem. Os valores válidos incluem os seguintes:<br /><br /> -Text: Use um codificador de mensagem de texto.<br />-MTOM: usar um codificador MTOM (mecanismo de organização de transmissão de mensagens) 1,0.<br /><br /> O padrão é texto.<br /><br /> Esse atributo é do tipo <xref:System.ServiceModel.WSMessageEncoding> .|
|`name`|O nome da configuração da associação. Esse valor deve ser exclusivo porque é usado como uma identificação para a associação. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|
|`openTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de abertura. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|
|`privacyNoticeAt`|Um URI no qual o aviso de privacidade está localizado.|
|`privacyNoticeVersion`|A versão do aviso de privacidade atual.|
|`proxyAddress`|Um URI que especifica o endereço do proxy HTTP. Se `useDefaultWebProxy` for `true` , essa configuração deverá ser `null` . O padrão é `null`.|
|`receiveTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de recebimento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:10:00.|
|`sendTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de envio. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|
|`textEncoding`|Define a codificação do conjunto de caracteres a ser usada para emitir mensagens na associação. Os valores válidos incluem os seguintes:<br /><br /> -BigEndianUnicode: codificação Unicode big endian.<br />-Unicode: codificação de 16 bits.<br />-UTF8: codificação de 8 bits.<br /><br /> O padrão é UTF8. Esse atributo é do tipo <xref:System.Text.Encoding> .|
|`transactionFlow`|Um valor que especifica se a associação dá suporte ao fluxo de WS-Transactions. O padrão é `false`.|
|`useDefaultWebProxy`|Um valor que indica se o proxy HTTP autoconfigurado do sistema é usado. O endereço de proxy deve ser `null` (ou seja, não definido) se esse atributo for `true` . O padrão é `true`.|

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

A Federação é a capacidade de compartilhar identidades em várias empresas ou domínios de confiança para autenticação e autorização. Ele usa o protocolo WS-Trust para mapear a representação de identidade de um domínio de confiança para outro. A associação HTTP federada dá suporte à segurança SOAP, bem como à segurança de modo misto, mas não dá suporte à segurança de transporte. Os serviços configurados com essa associação devem usar o transporte HTTP. Para obter mais informações, confira [\<wsFederationHttpBinding>](wsfederationhttpbinding.md).

## <a name="example"></a>Exemplo

```xml
<configuration>
  <system.ServiceModel>
    <bindings>
      <ws2007FederationHttpBinding>
        <binding bypassProxyOnLocal="false"
                 transactionFlow="false"
                 hostNameComparisonMode="WeakWildcard"
                 maxReceivedMessageSize="1000"
                 messageEncoding="Mtom"
                 proxyAddress="http://www.contoso.com"
                 textEncoding="Utf16TextEncoding"
                 useDefaultWebProxy="false">
          <reliableSession ordered="false"
                           inactivityTimeout="00:02:00"
                           enabled="true" />
          <security mode="None">
            <message negotiateServiceCredential="false"
                     algorithmSuite="Aes128"
                     issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1"
                     issuedKeyType="PublicKey">
              <issuer address="http://localhost/Sts" />
            </message>
          </security>
        </binding>
      </ws2007FederationBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```

## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.WS2007FederationHttpBinding>
- <xref:System.ServiceModel.Configuration.WS2007FederationHttpBindingElement>
- [\<wsFederationHttpBinding>](wsfederationhttpbinding.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
