---
title: <connectionPoolSettings> de <tcpTransport>
ms.date: 03/30/2017
ms.assetid: 2fbc3aa7-fcc9-4193-99a3-85d31d60d3f7
ms.openlocfilehash: f9b0fff741c32c1a3d6f9461f478e89acc18114e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70398090"
---
# <a name="connectionpoolsettings-of-tcptransport"></a>\<connectionPoolSettings> de \<tcpTransport>
Especifica configurações de pool de conexões adicionais para um transporte TCP.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<tcpTransport>**](tcptransport.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<connectionPoolSettings>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<connectionPoolSettings groupName="String"
                        idleTimeout="TimeSpan"
                        leaseTimeout="TimeSpan"
                        maxOutboundConnectionsPerEndpoint="Integer" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`groupName`|Uma cadeia de caracteres que define o nome do pool de conexões usado para canais de saída. No modo de fluxo, as conexões não são compartilhadas, o que significa que o pool de conexões está desabilitado. O padrão é uma cadeia de caracteres "padrão". Você pode modificar esse valor para isolar as conexões de um cliente específico em grupos separados.|  
|`idleTimeout`|Um positivo <xref:System.TimeSpan> que especifica o tempo máximo que a conexão pode ficar ociosa antes de ser desconectada. O padrão é 00:02:00.|  
|`leaseTimeout`|Um <xref:System.TimeSpan> que especifica o tempo após o qual uma conexão ativa é fechada. O padrão é 00:05:00.<br /><br /> Uma conexão é fechada depois de ser retornada ao cache de conexão e não durante a transmissão ativa. O cache de conexão usado pelo transporte TCP cria novas conexões conforme necessário para cada ponto de extremidade, até o limite de cache definido por`maxOutboundConnectionsPerEndpoint.`|  
|`maxOutboundConnectionsPerEndpoint`|Um inteiro positivo que especifica o número máximo de conexões com um ponto de extremidade remoto iniciado pelo serviço. As conexões que excedem o limite são enfileiradas até que um espaço abaixo do limite fique disponível. O `idleTimeout` limita a duração em que as conexões permanecem enfileiradas antes que uma exceção seja lançada. O padrão é 10.<br /><br /> Esse atributo limita o número de conexões ativas simultâneas do cliente para um ponto de extremidade de serviço específico. Se esse valor for excedido por ter mais conexões de cliente ativas, o serviço poderá parecer sem resposta ao cliente. Nesse caso, esse valor deve ser ajustado para exceder o número máximo de conexões de cliente simultâneas esperadas para um ponto de extremidade específico.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<namedPipeTransport>](namedpipetransport.md)|Define um transporte que faz com que um canal transfira mensagens usando pipes nomeados.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.TcpConnectionPoolSettingsElement>
- <xref:System.ServiceModel.Channels.TcpTransportBindingElement.ConnectionPoolSettings%2A>
- <xref:System.ServiceModel.Channels.TcpConnectionPoolSettings>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Transportes](../../../wcf/feature-details/transports.md)
- [Selecionando um transporte](../../../wcf/feature-details/choosing-a-transport.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
