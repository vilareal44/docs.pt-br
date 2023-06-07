---
title: <httpTransport>
ms.date: 03/30/2017
ms.assetid: 8b30c065-b32a-4fa3-8eb4-5537a9c6b897
ms.openlocfilehash: b3558db6018d79f0fad27ff28657bfadb5637467
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73736771"
---
# \<httpTransport>
Especifica um transporte HTTP para transmissão de mensagens SOAP para uma associação personalizada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<httpTransport>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<httpTransport allowCookies="Boolean"
               authenticationScheme="Digest/Negotiate/Ntlm/Basic/Anonymous"
               bypassProxyOnLocal="Boolean"
               hostnameComparisonMode="StrongWildcard/Exact/WeakWildcard"
               keepAliveEnabled="Boolean"
               maxBufferSize="Integer"
               proxyAddress="Uri"
               proxyAuthenticationScheme="None/Digest/Negotiate/Ntlm/Basic/Anonymous"
               realm="String"
               transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse"
               unsafeConnectionNtlmAuthentication="Boolean"
               useDefaultWebProxy="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|allowCookies|Um valor booliano que especifica se o cliente aceita cookies e os propaga em solicitações futuras. O padrão é `false`.<br /><br /> Você pode usar esse atributo ao interagir com serviços Web ASMX que usam cookies. Dessa forma, você pode ter certeza de que os cookies retornados do servidor são copiados automaticamente para todas as solicitações de cliente futuras para esse serviço.|  
|authenticationScheme|Especifica o protocolo usado para autenticar solicitações de cliente sendo processadas por um ouvinte HTTP. Os valores válidos incluem os seguintes:<br /><br /> -Digest: especifica a autenticação Resumida.<br />-Negotiate: negocia com o cliente para determinar o esquema de autenticação. Se o cliente e o servidor oferecem suporte ao Kerberos, ele é usado; caso contrário, o NTLM é usado.<br />-NTLM: especifica a autenticação NTLM.<br />-Básico: especifica a autenticação básica.<br />-Anônimo: especifica a autenticação anônima.<br /><br /> O padrão é anônimo. Esse atributo é do tipo <xref:System.Net.AuthenticationSchemes> . Este atributo só pode ser definido uma vez.|  
|bypassProxyOnLocal|Um valor booliano que indica se deve ignorar o servidor proxy para endereços locais. O padrão é `false`.<br /><br /> Um endereço local é aquele que está na LAN local ou na intranet.<br /><br /> Windows Communication Foundation (WCF) sempre ignora o proxy se o endereço do serviço começar com `http://localhost` .<br /><br /> Você deve usar o nome do host em vez de localhost se quiser que os clientes passem por um proxy ao conversar com os serviços no mesmo computador.|  
|hostnameComparisonMode|Especifica o modo de comparação de nome de host HTTP usado para analisar URIs. Os valores válidos são,<br /><br /> -StrongWildcard: ("+") corresponde a todos os nomes de host possíveis no contexto do esquema especificado, da porta e do URI relativo.<br />-Exato: não há curingas<br />-WeakWildcard: (" \* ") corresponde a todos os nomes de host possíveis no contexto do esquema especificado, da porta e da UIR relativa que não foram correspondidas explicitamente ou por meio do mecanismo curinga forte.<br /><br /> Esse atributo é do tipo <xref:System.ServiceModel.HostNameComparisonMode> . O padrão é <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>.|  
|keepAliveEnabled|Um valor booliano que especifica se uma conexão persistente deve ser feita ao recurso da Internet.|  
|maxBufferSize|Um inteiro positivo que especifica o tamanho máximo do buffer. O padrão é 524288|  
|proxyAddress|Um URI que especifica o endereço do proxy HTTP. Se `useSystemWebProxy` for `true` , essa configuração deverá ser `null` . O padrão é `null`.|  
|proxyAuthenticationScheme|Especifica o protocolo usado para autenticar solicitações de cliente sendo processadas por um proxy HTTP. Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: nenhuma autenticação é executada.<br />-Digest: especifica a autenticação Resumida.<br />-Negotiate: negocia com o cliente para determinar o esquema de autenticação. Se o cliente e o servidor oferecem suporte ao Kerberos, ele é usado; caso contrário, o NTLM é usado.<br />-NTLM: especifica a autenticação NTLM.<br />-Básico: especifica a autenticação básica.<br />-Anônimo: especifica a autenticação anônima.<br /><br /> O padrão é anônimo. Esse atributo é do tipo <xref:System.Net.AuthenticationSchemes> . Observe que o <xref:System.Net.AuthenticationSchemes.IntegratedWindowsAuthentication?displayProperty=nameWithType> não tem suporte.|  
|territórios|Uma cadeia de caracteres que especifica o realm a ser usado no proxy/servidor. O padrão é uma cadeia de caracteres vazia.<br /><br /> Os servidores usam territórios para particionar recursos protegidos. Cada partição pode ter seu próprio esquema de autenticação e/ou banco de dados de autorização. Os territórios são usados apenas para autenticação básica e resumida. Depois que um cliente é autenticado com êxito, a autenticação é válida para todos os recursos em um determinado realm. Para obter uma descrição detalhada dos territórios, consulte RFC 2617 no [site da IETF](https://www.ietf.org).|  
|transferMode|Especifica se as mensagens são armazenadas em buffer ou transmitidas ou uma solicitação ou resposta. Os valores válidos incluem os seguintes:<br /><br /> -Buffer: as mensagens de solicitação e resposta são armazenadas em buffer.<br />-Streamed: as mensagens de solicitação e resposta são transmitidas.<br />-StreamedRequest: a mensagem de solicitação é transmitida e a mensagem de resposta é armazenada em buffer.<br />-StreamedResponse: a mensagem de solicitação é armazenada em buffer e a mensagem de resposta é transmitida.<br /><br /> O padrão é armazenado em buffer. Esse atributo é do tipo <xref:System.ServiceModel.TransferMode> .|  
|unsafeConnectionNtlmAuthentication|Um valor booliano que especifica se o compartilhamento de conexão não segura está habilitado no servidor. O padrão é `false`. Se estiver habilitado, a autenticação NTLM será executada uma vez em cada conexão TCP.|  
|useDefaultWebProxy|Um valor booliano que especifica se as configurações de proxy de todo o computador são usadas em vez das configurações específicas do usuário. O padrão é `true`.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação da associação personalizada.|  
  
## <a name="remarks"></a>Comentários  
 O `httpTransport` elemento é o ponto de partida para criar uma associação personalizada que implementa o protocolo de transporte http. HTTP é o transporte primário usado para fins de interoperabilidade. Esse transporte é compatível com o Windows Communication Foundation (WCF) para garantir a interoperabilidade com outras pilhas de serviços Web não WCF.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.HttpTransportElement>
- <xref:System.ServiceModel.Channels.HttpTransportBindingElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Transportes](../../../wcf/feature-details/transports.md)
- [Selecionando um transporte](../../../wcf/feature-details/choosing-a-transport.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
