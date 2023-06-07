---
title: <ws2007HttpBinding>
ms.date: 03/30/2017
ms.assetid: 8586ecc9-bdaa-44d6-8d4d-7038e4ea1741
ms.openlocfilehash: 379552f461a79415e3140a8084901e0c1d6b2c32
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74140484"
---
# \<ws2007HttpBinding>
Define uma associação interoperável que fornece suporte para as versões corretas <xref:System.ServiceModel.WSHttpBinding.Security%2A> dos <xref:System.ServiceModel.ReliableSession> elementos de <xref:System.ServiceModel.WSHttpBindingBase.TransactionFlow%2A> associação, e.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<ws2007HttpBinding>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<ws2007HttpBinding>
  <binding allowCookies="Boolean"
           bypassProxyOnLocal="Boolean"
           closeTimeout="TimeSpan"
           hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"
           maxBufferPoolSize="integer"
           maxReceivedMessageSize="Integer"
           messageEncoding="Text/Mtom"
           name="string"
           openTimeout="TimeSpan"
           proxyAddress="URI"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           textEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding"
           transactionFlow="Boolean"
           useDefaultWebProxy="Boolean">
    <reliableSession ordered="Boolean"
                     inactivityTimeout="TimeSpan"
                     enabled="Boolean" />
    <security mode="Message/None/Transport/TransportWithCredential">
      <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                 proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                 realm="string" />
        <message clientCredentialType ="Certificate/IssuedToken/None/UserName/Windows"
                 negotiateServiceCredential="Boolean"
                 algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
                 establishSecurityContext="Boolean"
                 negotiateServiceCredential="Boolean" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</ws2007HttpBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`allowCookies`|Um valor que indica se o cliente aceita cookies e os propaga em solicitações futuras. O padrão é `false`.<br /><br /> Você pode usar essa propriedade ao interagir com os serviços Web do ASP.NET (ASMX) que usam cookies. Isso garante que os cookies que o servidor retorna sejam copiados automaticamente para todas as solicitações de cliente futuras para esse serviço.|  
|`bypassProxyOnLocal`|Um valor que indica se deve ignorar o servidor proxy para endereços locais. O padrão é `false`.|  
|`closeTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo para a conclusão de uma operação de fechamento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`hostNameComparisonMode`|Especifica o modo de comparação de nome de host HTTP usado para analisar URIs (identificadores de recurso uniforme). Esse atributo é do tipo <xref:System.ServiceModel.HostNameComparisonMode> , que indica se o nome do host é usado para acessar o serviço ao corresponder ao URI. O valor padrão é <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard> , que ignora o nome do host na correspondência.|  
|`maxBufferPoolSize`|O tamanho máximo do pool de buffers para essa associação. O padrão é 524.288 bytes (512 × 1.024). Muitas partes de Windows Communication Foundation (WCF) usam buffers. Criar e destruir buffers cada vez que eles são usados é caro, assim como a coleta de lixo para buffers. Com os pools de buffers, você pode pegar um buffer do pool, usá-lo e retorná-lo ao pool quando terminar. Isso evita a sobrecarga na criação e destruição de buffers.|  
|`maxReceivedMessageSize`|O tamanho máximo da mensagem, em bytes, incluindo cabeçalhos, que um canal configurado com essa associação, pode receber. O remetente de uma mensagem que excede esse limite recebe uma falha de SOAP. O receptor remove a mensagem e cria uma entrada do evento no log de rastreamento. O padrão é 65536.|  
|`messageEncoding`|Define o codificador usado para codificar a mensagem. Os valores válidos incluem os seguintes:<br /><br /> -   `Text`: Use um codificador de mensagem de texto.<br />-   `Mtom`: Use um codificador MTOM (mecanismo de organização de transmissão de mensagens 1,0).<br /><br /> O padrão é `Text`.<br /><br /> Esse atributo é do tipo <xref:System.ServiceModel.WSMessageEncoding> .|  
|`name`|O nome da configuração da associação. Esse valor deve ser exclusivo porque é usado como uma identificação para a associação. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|  
|`openTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de abertura. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`proxyAddress`|Um URI que especifica o endereço do proxy HTTP. Se `useSystemWebProxy` for `true` , essa configuração deverá ser `null` . O padrão é `null`.|  
|`receiveTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de recebimento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`sendTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de envio. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`textEncoding`|Especifica a codificação do conjunto de caracteres a ser usada para emitir mensagens na associação. Os valores válidos incluem os seguintes:<br /><br /> -   `UnicodeFffeTextEncoding`: Codificação Big Endian Unicode.<br />-   `Utf16TextEncoding`: codificação de 16 bits.<br />-   `Utf8TextEncoding`: codificação de 8 bits.<br /><br /> O padrão é `Utf8TextEncoding`.<br /><br /> Esse atributo é do tipo <xref:System.Text.Encoding> .|  
|`transactionFlow`|Um valor que especifica se a associação dá suporte ao fluxo de WS-Transactions. O padrão é `false`.|  
|`useDefaultWebProxy`|Um valor que especifica se o proxy HTTP autoconfigurado do sistema é usado. O padrão é `true`.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-wshttpbinding.md)|Define as configurações de segurança para a associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.WSHttpSecurityElement> .|  
|[\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Define as restrições sobre a complexidade das mensagens SOAP que os pontos de extremidade configurados com essa associação podem processar. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> .|  
|[\<reliableSession>](https://docs.microsoft.com/previous-versions/ms731375(v=vs.90))|Especifica se as sessões confiáveis são estabelecidas entre os pontos de extremidade do canal.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|Esse elemento contém uma coleção de associações padrão e personalizadas.|  
  
## <a name="remarks"></a>Comentários  
 O `WS2007HttpBinding` adiciona uma associação fornecida pelo sistema semelhante a `WSHttpBinding` , mas usa a organização para o avanço das versões padrão do Oasis (padrões de informações estruturadas) dos protocolos ReliableSession, Security e TransactionFlow. Nenhuma alteração no modelo de objeto ou nas configurações padrão é necessária ao usar essa associação.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<configuration>
  <system.ServiceModel>
    <bindings>
      <ws2007HttpBinding>
        <binding closeTimeout="00:00:10"
                 openTimeout="00:00:20"
                 receiveTimeout="00:00:30"
                 sendTimeout="00:00:40"
                 bypassProxyOnLocal="false"
                 transactionFlow="false"
                 hostNameComparisonMode="WeakWildcard"
                 maxReceivedMessageSize="1000"
                 messageEncoding="Mtom"
                 proxyAddress="http://www.contoso.com"
                 textEncoding="utf-16"
                 useDefaultWebProxy="false">
          <reliableSession ordered="false"
                           inactivityTimeout="00:02:00"
                           enabled="true" />
          <security mode="Transport">
            <transport clientCredentialType="Digest"
                       proxyCredentialType="None"
                       realm="someRealm" />
            <message clientCredentialType="Windows"
                     negotiateServiceCredential="false"
                     algorithmSuite="Aes128"
                     defaultProtectionLevel="None" />
          </security>
        </binding>
      </ws2007HttpBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.WS2007HttpBinding>
- <xref:System.ServiceModel.Configuration.WS2007HttpBindingElement>
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
