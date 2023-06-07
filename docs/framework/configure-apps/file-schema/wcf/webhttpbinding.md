---
title: <webHttpBinding>
ms.date: 03/30/2017
ms.assetid: 84179d77-825d-44b9-895a-ab08e7aa044d
ms.openlocfilehash: 0bb6d1f40fe38cb13ed8ab07957de5db22d48fcc
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74140492"
---
# \<webHttpBinding>
Define um elemento de associação que é usado para configurar pontos de extremidade para serviços da Web Windows Communication Foundation (WCF) que respondem a solicitações HTTP em vez de mensagens SOAP.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<webHttpBinding>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<webHttpBinding>
  <binding allowCookies="Boolean"
           bypassProxyOnLocal="Boolean"
           closeTimeout="TimeSpan"
           hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"
           maxBufferPoolSize="integer"
           maxBufferSize="integer"
           maxReceivedMessageSize="Integer"
           name="string"
           openTimeout="TimeSpan"
           proxyAddress="URI"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse"
           useDefaultWebProxy="Boolean"
           writeEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding">
    <security mode="None/Transport/TransportCredentialOnly">
      <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                 proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                 realm="string" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</webHttpBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|allowCookies|Um valor booliano que indica se o cliente aceita cookies e os propaga em solicitações futuras. O padrão é false.<br /><br /> Você pode usar essa propriedade ao interagir com serviços Web ASMX que usam cookies. Dessa forma, você pode ter certeza de que os cookies retornados do servidor são copiados automaticamente para todas as solicitações de cliente futuras para esse serviço.|  
|bypassProxyOnLocal|Um valor booliano que indica se deve ignorar o servidor proxy para endereços locais. O padrão é `false`.|  
|closeTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de fechamento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|hostnameComparisonMode|Especifica o modo de comparação de nome de host HTTP usado para analisar URIs. Esse atributo é do tipo <xref:System.ServiceModel.HostNameComparisonMode> , que indica se o nome do host é usado para acessar o serviço ao corresponder ao URI. O valor padrão é <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard> , que ignora o nome do host na correspondência.|  
|maxBufferPoolSize|Um inteiro que especifica o tamanho máximo do pool de buffers para essa associação. O padrão é 524.288 bytes (512 * 1024). Muitas partes de Windows Communication Foundation (WCF) usam buffers. Criar e destruir buffers cada vez que eles são usados é caro, e a coleta de lixo para buffers também é dispendiosa. Com os pools de buffers, você pode pegar um buffer do pool, usá-lo e retorná-lo ao pool quando terminar. Portanto, a sobrecarga na criação e na destruição de buffers é evitada.|  
|maxBufferSize|Um inteiro que especifica a quantidade máxima de memória alocada para uso pelo Gerenciador dos buffers de mensagens que recebem mensagens do canal. O valor padrão é 524.288 (0x80000) bytes.|  
|maxReceivedMessageSize|Um inteiro positivo que especifica o tamanho máximo da mensagem, em bytes, incluindo cabeçalhos, que pode ser recebido em um canal configurado com essa associação. O remetente de uma mensagem que excede esse limite receberá uma falha. O receptor remove a mensagem e cria uma entrada do evento no log de rastreamento. O padrão é 65536. **Observação:**  Aumentar este valor sozinho não é suficiente no modo compatível com ASP.NET. Você também deve aumentar o valor de `httpRuntime` (consulte o [elemento httpRuntime (esquema de configurações de ASP.net)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/e1f13641(v=vs.100))).|  
|name|Uma cadeia de caracteres que contém o nome da configuração da associação. Esse valor deve ser exclusivo porque é usado como uma identificação para a associação. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|  
|openTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de abertura. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|proxyAddress|Um URI que especifica o endereço do proxy HTTP. Se `useSystemWebProxy` for `true` , essa configuração deverá ser `null` . O padrão é `null`.|  
|receiveTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de recebimento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|sendTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de envio. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|TransferMode.|Um <xref:System.ServiceModel.TransferMode> valor que indica se o serviço configurado com a associação usa modos de fluxo ou em buffer (ou ambos) de transferência de mensagem. O padrão é `Buffered`.|  
|useDefaultWebProxy|Um valor booliano que especifica se o proxy HTTP autoconfigurado do sistema é usado. O padrão é `true`.|  
|writeEncoding|Especifica a codificação de caracteres usada para o texto da mensagem. Os valores válidos incluem os seguintes:<br /><br /> UnicodeFffeTextEncoding: codificação BigEndian Unicode.<br /><br /> Utf16TextEncoding: codificação de 16 bits.<br /><br /> Utf8TextEncoding: codificação de 8 bits.<br /><br /> O padrão é Utf8TextEncoding.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Define as restrições sobre a complexidade de mensagens POX que podem ser processadas por pontos de extremidade configurados com essa associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> .|  
|[\<security>](security-of-webhttpbinding.md)|Define as configurações de segurança para a associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.WebHttpSecurityElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|Esse elemento contém uma coleção de associações padrão e personalizadas.|  
  
## <a name="remarks"></a>Comentários  
 O modelo de programação Web do WCF permite que os desenvolvedores exponham os serviços Web WCF por meio de solicitações HTTP que usam mensagens de estilo "de" XML antigo "(POX) em vez de mensagens baseadas em SOAP. Para que os clientes se comuniquem com um serviço usando solicitações HTTP, um ponto de extremidade do serviço deve ser configurado com o [\<webHttpBinding>](webhttpbinding.md) que está \<WebHttpBehavior> anexado a ele.  
  
 Suporte no WCF para distribuição e ASP. A integração do AJAX é criada com base no modelo de programação da Web. Para obter mais informações sobre o modelo, consulte [modelo de programação do WCF Web http](../../../wcf/feature-details/wcf-web-http-programming-model.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Configuration.WebHttpBindingElement>
- [Modelo de programação WCF Web HTTP](../../../wcf/feature-details/wcf-web-http-programming-model.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
