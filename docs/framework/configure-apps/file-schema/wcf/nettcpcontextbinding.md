---
title: <netTcpContextBinding>
ms.date: 03/30/2017
ms.assetid: 1d4715e1-5fff-4c3d-a226-18f21d0b30c4
ms.openlocfilehash: 360cdf76e013d085022f45eba5f8549e11cd68d3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74140708"
---
# \<netTcpContextBinding>
Especifica um contexto para o <xref:System.ServiceModel.NetTcpBinding> que requer que o nível de proteção seja assinado. O contextExchangeMechanism para NetTcpContextBinding é SOAPHeader.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<netTcpContextBinding>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<netTcpContextBinding>
  <binding closeTimeout="TimeSpan"
           contextProtectionLevel="EncryptAndSign/None/Sign"
           hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"
           listenBacklog="Integer"
           maxBufferPoolSize="integer"
           maxBufferSize="Integer"
           maxConnections="Integer"
           maxReceivedMessageSize="Integer"
           name="string"
           openTimeout="TimeSpan"
           portSharingEnabled="Boolean"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           transactionFlow="Boolean"
           transactionProtocol="OleTransactions/WSAtomicTransactionOctober2004"
           transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse">
    <reliableSession ordered="Boolean"
                     inactivityTimeout="TimeSpan"
                     enabled="Boolean" />
    <security mode="Message/None/Transport/TransportWithCredential">
      <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                 proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                 realm="String"
                 defaultClientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                 defaultProxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                 defaultRealm="String" />
      <message clientCredentialType="Certificate/IssuedToken/None/UserName/Windows"
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
</netTcpContextBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|closeTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de fechamento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|contextProtectionLevel|Um <xref:System.Net.Security.ProtectionLevel> valor válido que especifica o nível de proteção desejado do cabeçalho SOAP usado para propagar as informações de contexto.  O valor padrão é <xref:System.Net.Security.ProtectionLevel.Sign>.|  
|hostnameComparisonMode|Especifica o modo de comparação de nome de host HTTP usado para analisar URIs. Esse atributo é do tipo <xref:System.ServiceModel.HostNameComparisonMode> , que indica se o nome do host é usado para acessar o serviço ao corresponder ao URI. O valor padrão é <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard> , que ignora o nome do host na correspondência.|  
|listenBacklog|Um inteiro positivo que especifica o número máximo de canais aguardando para serem aceitos no ouvinte. As conexões que excederem esse limite serão enfileiradas até que o espaço abaixo do limite fique disponível. O `connectionTimeout` atributo limita o tempo que um cliente aguardará para ser conectado antes de lançar uma exceção de conexão. O padrão é 10.|  
|maxBufferPoolSize|Um inteiro que especifica o tamanho máximo do pool de buffers para essa associação. O padrão é 512 * 1024 bytes. Muitas partes de Windows Communication Foundation (WCF) usam buffers. Criar e destruir buffers cada vez que eles são usados é caro, e a coleta de lixo para buffers também é dispendiosa. Com os pools de buffers, você pode pegar um buffer do pool, usá-lo e retorná-lo ao pool quando terminar. Portanto, a sobrecarga na criação e na destruição de buffers é evitada.|  
|maxBufferSize|Um inteiro positivo que especifica o tamanho máximo, em bytes, do buffer usado para armazenar mensagens na memória. Se o buffer estiver cheio, os dados em excesso permanecerão no soquete subjacente até que o buffer tenha espaço novamente. Este valor não pode ser menor que o `maxReceivedMessageSize` atributo. O padrão é 65536. Para obter mais informações, consulte <xref:System.ServiceModel.Configuration.NetNamedPipeBindingElement.MaxBufferSize%2A>.|  
|maxConnections|Um inteiro que especifica o número máximo de conexões de entrada e de saída que o serviço criará/aceitará. As conexões de entrada e saída são contadas em relação a um limite separado especificado por esse atributo.<br /><br /> As conexões de entrada excedentes ao limite são enfileiradas até que um espaço abaixo do limite fique disponível.<br /><br /> As conexões de saída excedentes ao limite são enfileiradas até que um espaço abaixo do limite fique disponível.<br /><br /> O padrão é 10.|  
|maxReceivedMessageSize|Um inteiro positivo que especifica o tamanho máximo da mensagem, em bytes, incluindo cabeçalhos, que pode ser recebido em um canal configurado com essa associação. O remetente de uma mensagem que excede esse limite receberá uma falha de SOAP. O receptor remove a mensagem e cria uma entrada do evento no log de rastreamento. O padrão é 65536.|  
|name|Uma cadeia de caracteres que contém o nome da configuração da associação. Esse valor deve ser exclusivo porque é usado como uma identificação para a associação. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|  
|openTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de abertura. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|portSharingEnabled|Um valor booliano que especifica se o compartilhamento de porta TCP está habilitado para esta conexão. Se isso for `false` , cada associação usará sua própria porta exclusiva. Essa configuração é relevante apenas para serviços, pois os clientes não são afetados.|  
|receiveTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de recebimento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:10:00.|  
|sendTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de envio. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|transactionFlow|Um valor booliano que especifica se a associação dá suporte ao fluxo de WS-Transactions. O padrão é `false`.|  
|transactionProtocol|Especifica o protocolo de transação a ser usado com esta associação. Os valores válidos são<br /><br /> -OleTransactions<br />- WSAtomicTransactionOctober2004<br /><br /> O padrão é OleTransactions. Esse atributo é do tipo <xref:System.ServiceModel.TransactionProtocol> .|  
|transferMode|Um <xref:System.ServiceModel.TransferMode> valor que especifica se as mensagens são armazenadas em buffer ou transmitidas ou uma solicitação ou resposta.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-nettcpbinding.md)|Define as configurações de segurança para a associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.NetTcpSecurityElement> .|  
|[\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Define as restrições sobre a complexidade de mensagens SOAP que podem ser processadas por pontos de extremidade configurados com essa associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> .|  
|[\<reliableSession>](https://docs.microsoft.com/previous-versions/ms731375(v=vs.90))|Especifica se as sessões confiáveis são estabelecidas entre os pontos de extremidade do canal.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|Esse elemento contém uma coleção de associações padrão e personalizadas.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.NetTcpBinding>
- <xref:System.ServiceModel.NetTcpContextBinding>
- <xref:System.ServiceModel.Configuration.NetTcpContextBindingElement>
- <xref:System.ServiceModel.Channels.ContextBindingElement>
- [\<netTcpBinding>](nettcpbinding.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
