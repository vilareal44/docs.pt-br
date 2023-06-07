---
title: <tcpTransport>
ms.date: 03/30/2017
ms.assetid: 8fcd18c1-9958-42e7-b442-7903f7bdb563
ms.openlocfilehash: f2c1335795ffd3cb395a7006bfaeb3cf7b39636b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "77448615"
---
# \<tcpTransport>
Define um transporte TCP que pode ser usado por um canal para transferir mensagens para uma associação personalizada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<tcpTransport>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<tcpTransport channelInitializationTimeout="TimeSpan"
              connectionBufferSize="Integer"
              hostNameComparisonMode="StrongWildcard/Exact/WeakWildcard"
              listenBacklog="Integer"
              manualAddressing="Boolean"
              maxBufferPoolSize="Integer"
              maxBufferSize="Integer"
              maxOutputDelay="TimeSpan"
              maxPendingAccepts="Integer"
              maxPendingConnections="Integer"
              maxReceivedMessageSize="Integer"
              portSharingEnabled="Boolean"
              teredoEnabled="Boolean"
              transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse" >
  <connectionPoolSettings groupName="String"
                          idleTimeout="TimeSpan"
                          leaseTimeout="TimeSpan"
                          maxOutboundConnectionsPerEndpoint="Integer" />
</tcpTransport>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|channelInitializationTimeout|Obtém ou define o limite de tempo para inicializar um canal a ser aceito.  O tempo máximo que um canal pode estar no estado de inicialização antes de ser desconectado em segundos. Essa cota inclui o tempo que uma conexão TCP pode tomar para se autenticar usando o protocolo de enquadramento de mensagens .NET. Um cliente precisa enviar alguns dados iniciais antes que o servidor tenha informações suficientes para executar a autenticação. O padrão é 30 segundos.|  
|connectionBufferSize|Obtém ou define o tamanho do buffer usado para transmitir uma parte da mensagem serializada na conexão do cliente ou do serviço.|  
|hostNameComparisonMode|Obtém ou define um valor que indica se o nome do host é usado para alcançar o serviço ao fazer correspondência no URI.|  
|listenBacklog|O número máximo de solicitações de conexão em fila que podem estar pendentes para um serviço Web. O `connectionLeaseTimeout` atributo limita a duração que o cliente aguardará para ser conectado antes de lançar uma exceção de conexão. Essa é uma propriedade de nível de soquete que controla o número máximo de solicitações de conexão em fila que podem estar pendentes para um serviço Web. Quando ListenBacklog for muito baixo, o WCF deixará de aceitar solicitações e, portanto, descartará novas conexões até que o servidor reconheça algumas das conexões em fila existentes. O padrão é 16 * número de processadores.|  
|manualAddressing|Obtém ou define um valor que indica se o endereçamento manual da mensagem é necessário.|  
|maxBufferPoolSize|Obtém ou define o tamanho máximo de qualquer pool de buffer usado pelo transporte.|  
|maxBufferSize|Obtém ou define o tamanho máximo do buffer a ser usado. Para mensagens transmitidas, este valor deve ser pelo menos o tamanho máximo possível dos cabeçalhos de mensagem, lidos em modo em buffer.|  
|maxOutputDelay|Obtém ou define o intervalo máximo de tempo que uma parte de uma mensagem ou uma mensagem completa pode permanecer armazenada em buffer na memória antes de ser enviada.|  
|maxPendingAccepts|Obtém ou define o número máximo de operações de aceitação assíncrona pendentes que estão disponíveis para processar conexões de entrada para o serviço.|  
|maxPendingConnections|Obtém ou define o número máximo de conexões aguardando a expedição no serviço.|  
|maxReceivedMessageSize|Obtém e define o tamanho máximo de mensagem permitido que pode ser recebido.|  
|portSharingEnabled|Um valor booliano que especifica se o compartilhamento de porta TCP está habilitado para esta conexão. Se isso for `false` , cada associação usará sua própria porta exclusiva. O padrão é `false`.<br /><br /> Essa configuração é relevante apenas para serviços do. Os clientes não são afetados.<br /><br /> O uso dessa configuração requer a habilitação do serviço de compartilhamento de porta TCP Windows Communication Foundation (WCF) alterando seu tipo de inicialização para manual ou automático|  
|teredoEnabled|Um valor booliano que especifica se Teredo (uma tecnologia para endereçar clientes que estão atrás de firewalls) está habilitado. O padrão é `false`.<br /><br /> Essa propriedade habilita a Teredo para o soquete TCP subjacente. Para obter mais informações, consulte [visão geral do Teredo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-xp/bb457011(v=technet.10)).<br /><br /> Essa propriedade é aplicável somente no Windows XP SP2 e no Windows Server 2003. O Windows Vista tem uma opção de configuração de todo o computador para Teredo, portanto, ao executar o vista, essa propriedade é ignorada. O Teredo requer que o cliente e as máquinas de serviço tenham a pilha do Microsoft IPv6 instalada e configurada corretamente para uso de Teredo.|  
|transferMode|Obtém ou define um valor que indica se as mensagens são armazenadas em buffer ou transmitidas com o transporte voltado para a conexão.|  
|connectionPoolSettings|Especifica configurações de pool de conexões adicionais para uma associação de pipe nomeado.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação da associação personalizada.|  
  
## <a name="remarks"></a>Comentários  
 Esse transporte usa URIs no formato "net. TCP://hostname: port/path". Outros componentes de URI são opcionais.  
  
 O `tcpTransport` elemento é o ponto de partida para criar uma associação personalizada que implementa o protocolo de transporte TCP. Esse transporte é otimizado para comunicação WCF para WCF.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.TcpTransportElement>
- <xref:System.ServiceModel.Channels.TcpTransportBindingElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Transportes](../../../wcf/feature-details/transports.md)
- [Selecionando um transporte](../../../wcf/feature-details/choosing-a-transport.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
