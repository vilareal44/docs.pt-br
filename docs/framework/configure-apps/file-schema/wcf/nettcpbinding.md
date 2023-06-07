---
title: <netTcpBinding>
description: Representa uma associação segura, confiável e otimizada, destinada apenas à comunicação entre computadores WCF usando TCP. O sistema de mensagens confiável está desativado por padrão.
ms.date: 03/30/2017
helpviewer_keywords:
- netTcpBinding Element
ms.assetid: 5c5104a7-8754-4335-8233-46a45322503e
ms.openlocfilehash: 95c2c691bf328050f3d189c790d111d2fdeb1bb0
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243991"
---
# \<netTcpBinding>

Especifica uma associação segura, confiável e otimizada adequada para comunicação entre computadores. Por padrão, ele gera uma pilha de comunicação de tempo de execução com segurança do Windows para segurança e autenticação de mensagens, TCP para entrega de mensagens e codificação de mensagens binárias.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<netTcpBinding>**  
  
## <a name="syntax"></a>Syntax  
  
```xml  
<netTcpBinding>
  <binding closeTimeout="TimeSpan"
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
    <security mode="None/Transport/Message/Both">
      <message clientCredentialType="None/Windows/UserName/Certificate/CardSpace"
               algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15" />
      <transport clientCredentialType="None/Windows/Certificate"
                 protectionLevel="None/Sign/EncryptAndSign" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</netTcpBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`closeTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de fechamento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`hostNameComparisonMode`|Especifica o modo de comparação de nome de host HTTP usado para analisar URIs. Esse atributo é do tipo <xref:System.ServiceModel.HostNameComparisonMode> , que indica se o nome do host é usado para acessar o serviço ao corresponder ao URI. O valor padrão é <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard> , que ignora o nome do host na correspondência.|  
|`listenBacklog`|Um inteiro positivo que especifica o número máximo de canais aguardando para serem aceitos no ouvinte. As conexões que excederem esse limite serão enfileiradas até que o espaço abaixo do limite fique disponível. O `connectionTimeout` atributo limita o tempo que um cliente aguardará para ser conectado antes de lançar uma exceção de conexão. O padrão é 10.|  
|`maxBufferPoolSize`|Um inteiro que especifica o tamanho máximo do pool de buffers para essa associação. O padrão é 512 * 1024 bytes. Muitas partes de Windows Communication Foundation (WCF) usam buffers. Criar e destruir buffers cada vez que eles são usados é caro, e a coleta de lixo para buffers também é dispendiosa. Com os pools de buffers, você pode pegar um buffer do pool, usá-lo e retorná-lo ao pool quando terminar. Portanto, a sobrecarga na criação e na destruição de buffers é evitada.|  
|`maxBufferSize`|Um inteiro positivo que especifica o tamanho máximo, em bytes, do buffer usado para armazenar mensagens na memória.<br /><br /> Se o `transferMode` atributo for igual a `Buffered` , esse atributo deverá ser igual ao `maxReceivedMessageSize` valor do atributo.<br /><br /> Se o `transferMode` atributo for igual a `Streamed` , esse atributo não poderá ser maior que o `maxReceivedMessageSize` valor do atributo e deverá ter pelo menos o tamanho dos cabeçalhos.<br /><br /> O padrão é 65536. Para obter mais informações, consulte <xref:System.ServiceModel.Configuration.NetNamedPipeBindingElement.MaxBufferSize%2A>.|  
|`maxConnections`|Um inteiro que especifica o número máximo de conexões de entrada e de saída que o serviço criará/aceitará. As conexões de entrada e saída são contadas em relação a um limite separado especificado por esse atributo.<br /><br /> As conexões de entrada excedentes ao limite são enfileiradas até que um espaço abaixo do limite fique disponível.<br /><br /> As conexões de saída excedentes ao limite são enfileiradas até que um espaço abaixo do limite fique disponível.<br /><br /> O padrão é 10.|  
|`maxReceivedMessageSize`|Um inteiro positivo que especifica o tamanho máximo da mensagem, em bytes, incluindo cabeçalhos, que pode ser recebido em um canal configurado com essa associação. O remetente de uma mensagem que excede esse limite receberá uma falha de SOAP. O receptor remove a mensagem e cria uma entrada do evento no log de rastreamento. O padrão é 65536.|  
|`name`|Uma cadeia de caracteres que contém o nome da configuração da associação. Esse valor deve ser exclusivo porque é usado como uma identificação para a associação. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|  
|`openTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de abertura. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`portSharingEnabled`|Um valor booliano que especifica se o compartilhamento de porta TCP está habilitado para esta conexão. Se isso for `false` , cada associação usará sua própria porta exclusiva. Essa configuração é relevante apenas para serviços, pois os clientes não são afetados.|  
|`receiveTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de recebimento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:10:00.|  
|`sendTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de envio. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`transactionFlow`|Um valor booliano que especifica se a associação dá suporte ao fluxo de WS-Transactions. O padrão é `false`.|  
|`transactionProtocol`|Especifica o protocolo de transação a ser usado com esta associação. Os valores válidos são<br /><br /> -OleTransactions<br />- WSAtomicTransactionOctober2004<br /><br /> O padrão é OleTransactions. Esse atributo é do tipo <xref:System.ServiceModel.TransactionProtocol> .|  
|`transferMode`|Um <xref:System.ServiceModel.TransferMode> valor que especifica se as mensagens são armazenadas em buffer ou transmitidas ou uma solicitação ou resposta.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Description|  
|-------------|-----------------|  
|[\<security>](security-of-nettcpbinding.md)|Define as configurações de segurança para a associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.NetTcpSecurityElement> .|  
|[\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Define as restrições sobre a complexidade de mensagens SOAP que podem ser processadas por pontos de extremidade configurados com essa associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> .|  
|[\<reliableSession>](https://docs.microsoft.com/previous-versions/ms731375(v=vs.90))|Especifica se as sessões confiáveis são estabelecidas entre os pontos de extremidade do canal.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Description|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|Esse elemento contém uma coleção de associações padrão e personalizadas.|  
  
## <a name="remarks"></a>Comentários

Essa associação gera uma pilha de comunicação de tempo de execução por padrão, que usa segurança de transporte, TCP para entrega de mensagem e uma codificação de mensagem binária. Essa associação é uma opção apropriada fornecida pelo sistema Windows Communication Foundation (WCF) para comunicação por uma intranet.  
  
 A configuração padrão do `netTcpBinding` é mais rápida do que a configuração fornecida pelo `wsHttpBinding` , mas destina-se apenas à comunicação do WCF. O comportamento de segurança é configurável usando o `securityMode` atributo opcional. O uso de WS-ReliableMessaging pode ser configurado usando o `reliableSessionEnabled` atributo opcional. Mas o sistema de mensagens confiável está desativado por padrão. Em geral, as associações fornecidas pelo sistema HTTP, como `wsHttpBinding` e, `basicHttpBinding` são configuradas para ativar as coisas por padrão, enquanto a `netTcpBinding` ligação desativa as coisas por padrão, para que você tenha que optar por obter suporte, por exemplo, para uma das especificações WS-*. Isso significa que a configuração padrão para TCP é mais rápida na troca de mensagens entre pontos de extremidade do que aquelas configuradas para as associações HTTP por padrão.  
  
## <a name="example"></a>Exemplo

A associação é especificada nos arquivos de configuração para o cliente e o serviço. O tipo de associação é especificado no `binding` atributo do `<endpoint>` elemento. Se você quiser configurar a associação netTcpbinding e alterar algumas de suas configurações, será necessário definir uma configuração de associação. O ponto de extremidade deve referenciar a configuração de associação com um `bindingConfiguration` atributo. No exemplo a seguir, uma configuração de associação é definida.  
  
```xml  
<services>
  <service name="Microsoft.ServiceModel.Samples.CalculatorService"
           behaviorConfiguration="CalculatorServiceBehavior">
    ...
    <endpoint address=""
              binding="netTcpBinding"
              contract="Microsoft.ServiceModel.Samples.ICalculator" />
    ...
  </service>
</services>
<bindings>
  <netTcpBinding>
    <binding closeTimeout="00:01:00"
             openTimeout="00:01:00"
             receiveTimeout="00:10:00"
             sendTimeout="00:01:00"
             transactionFlow="false"
             transferMode="Buffered"
             transactionProtocol="OleTransactions"
             hostNameComparisonMode="StrongWildcard"
             listenBacklog="10"
             maxBufferPoolSize="524288"
             maxBufferSize="65536"
             maxConnections="10"
             maxReceivedMessageSize="65536">
      <readerQuotas maxDepth="32"
                    maxStringContentLength="8192"
                    maxArrayLength="16384"
                    maxBytesPerRead="4096"
                    maxNameTableCharCount="16384" />
      <reliableSession ordered="true"
                       inactivityTimeout="00:10:00"
                       enabled="false" />
      <security mode="Transport">
        <transport clientCredentialType="Windows" protectionLevel="EncryptAndSign" />
      </security>
    </binding>
  </netTcpBinding>
</bindings>
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel.NetTcpBinding>
- <xref:System.ServiceModel.Configuration.NetTcpBindingElement>
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
